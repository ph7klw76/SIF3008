# 1. Physical Model: 2D Tight-Binding on a Square Lattice

## 1.1. Nearest-Neighbor Tight-Binding Hamiltonian

In a nearest-neighbor tight-binding model on a 2D square lattice, each site $r_i$ has hopping only to its four nearest neighbors. The Hamiltonian takes the form:

$$
\hat{H} = -t \sum_{\langle i, j \rangle} \left( c_i^\dagger c_j + c_j^\dagger c_i \right),
$$

where:

- $c_i^\dagger$, $c_i$ are creation and annihilation operators for an electron at lattice site $i$  
- $t$ is the hopping parameter (real, positive) describing the amplitude for an electron to hop between neighboring sites $\langle i, j \rangle$

---

## 1.2. Reciprocal-Space Representation and Dispersion

Switching to reciprocal space (momentum $k$), the single-particle energies of the tight-binding model on a square lattice (lattice constant $a$) become:

$$
E(k) = -2t \left[ \cos(k_x a) + \cos(k_y a) \right]
$$

In the code, we set $t = 1$ and $a = 1$ for simplicity, so:

$$
E(k) = -2 \left[ \cos(k_x) + \cos(k_y) \right]
$$

This dispersion ranges from $-4$ (at $k_x = \pm \pi$, $k_y = \pm \pi$) to $+4$ (at $k_x = 0$, $k_y = 0$) in energy units of $t$. The code then shifts this expression upward by $+4$ so that the minimum becomes $E_{\text{min}} = 0$ and the maximum becomes $E_{\text{max}} = 8$.

Mathematically, the shifted dispersion in the code is:

$$
E_{\text{shifted}}(k) = 2 \left( 2 - \cos(k_x) - \cos(k_y) \right)
$$

- **Minimum:** 0  
- **Maximum:** 8

---

## 1.3. Brillouin Zone (BZ)

For a 2D square lattice of lattice constant $a = 1$, the reciprocal lattice vectors have magnitude:

$$
\frac{2\pi}{a} = 2\pi
$$

Hence, the (first) Brillouin zone is typically taken as the set of wave vectors $k = (k_x, k_y)$ with:

- $-\pi < k_x \leq \pi$  
- $-\pi < k_y \leq \pi$

In the code, we discretize this square domain into `N_POINTS × N_POINTS` to form a grid $(KX, KY)$.

---

# 2. Code Implementation Overview

## 2.1. K-space Grid and Energy Calculation

```python
N_POINTS = 100
BZ_BOUNDARY = np.pi

kx = np.linspace(-BZ_BOUNDARY, BZ_BOUNDARY, N_POINTS)
ky = np.linspace(-BZ_BOUNDARY, BZ_BOUNDARY, N_POINTS)
KX, KY = np.meshgrid(kx, ky)

ENERGY = 2.0 * (2.0 - np.cos(KX) - np.cos(KY))
```
We set up a 2D grid of $k$-points spanning $[-\pi, \pi]$ in both $k_x$ and $k_y$.

We compute $E(k)$ on this grid, storing the result in `ENERGY`.

---

## 2.2. Density of States (DOS) via Histogram

### 2.2.1. DOS Definition

The density of states $g(E)$ in 2D for a discrete model can be computed by counting how many states (i.e., how many $k$-points) occupy a given energy interval $dE$:

$$
g(E) = \frac{\Delta N(E)}{\Delta E},
$$

where $\Delta N(E)$ is the number of states in an energy window around $E$.

In the code:

- Flatten the 2D energy array into a 1D list of energies  
- Create a histogram (many bins) to approximate $\Delta N(E)$  
- Normalize by the total number of points and the bin width $\Delta E$

Hence, the DOS is approximately:

$$
g(E_i) \approx \frac{\text{number of grid points with } E_k \text{ in bin } i}{N_{\text{total}} \, \Delta E}
$$

---

### 2.2.2. Code Implementation

```python
energy_flat = ENERGY.flatten()
bins = int(np.sqrt(N_POINTS*N_POINTS)) * 2
dos_counts, dos_edges = np.histogram(energy_flat, bins=bins,
                                     range=(MIN_ENERGY - 0.1, MAX_ENERGY + 0.1))
dos_energies = (dos_edges[:-1] + dos_edges[1:]) / 2
dos_normalized = dos_counts / (N_POINTS*N_POINTS * (dos_edges[1] - dos_edges[0]))

# Smooth the DOS
dos_smoothed = gaussian_filter1d(dos_normalized, sigma=1.5)
```

### Histogram

We use `np.histogram` with enough bins to capture fine energy details.

### Normalization

We divide by $N\_POINTS^2 \times \Delta E$ to get a DOS-like function.

### Smoothing

The `gaussian_filter1d` call gives a smoother curve.

---

## 3. Graphical User Interface (GUI)

The code uses Tkinter to create an interactive window with:

- **K-space Plot:** Shows the Brillouin zone $[-\pi, \pi] \times [-\pi, \pi]$ and contours of constant energy.
- **DOS Plot:** Shows $g(E)$ vs. $E$.
- **Slider:** Allows you to vary the Fermi energy $E_F$.

As you move the slider:

- K-space is “filled” wherever $E(k) \leq E_F$.
- A contour line is drawn where $E(k) = E_F$ (the Fermi “surface” in 2D).
- On the DOS plot, the portion of the DOS up to $E_F$ is shaded, and a vertical line indicates the current $E_F$.

---

### 3.1. Plotting the Filled Region

The fill in K-space is done via:

```python
fill_plot = ax_kspace.contourf(
    KX, KY, ENERGY,
    levels=np.linspace(MIN_ENERGY - 1e-9, fermi_energy, 10),
    colors=['#cccccc'], alpha=0.8, zorder=2
)
```

Conceptually, `contourf` finds the region in $k$-space for which $E(k) \leq E_F$.  
This highlights the occupied states (assuming a fermionic picture at zero temperature).

---

### 3.2. Drawing the Fermi Surface

The “Fermi surface” in 2D is all $k$ such that $E(k) = E_F$.  
Numerically, the code calls:

```python
line_plot = ax_kspace.contour(
    KX, KY, ENERGY,
    levels=[fermi_energy],
    colors=['blue'], linewidths=1.5, zorder=3
)
```
This draws a contour line exactly at $\{k : E(k) = E_F\}$.

---

# 4. Equations and Rationale

### **Tight-Binding Dispersion:**

$$
E(k) = -2t \left( \cos(k_x a) + \cos(k_y a) \right) \Rightarrow \text{Shifted to } [0, 8] \text{ via } E(k) \mapsto 2 \left( 2 - \cos(k_x) - \cos(k_y) \right)
$$

---

### **Brillouin Zone:**

$-\pi \leq k_x \leq \pi$,  
$-\pi \leq k_y \leq \pi$

---

### **Density of States (Histogram Approximation):**

- Flatten energies: $\{ E(k_i) \}$
- Bin energies to estimate: $\Delta N(E) \approx$ count in bin
- Divide by total number of $k$-points ($N^2$) and bin width to get a DOS-like function:

$$
g(E) \approx \frac{\Delta N(E)}{\Delta E \times N^2}
$$

---

### **Fermi Energy and Occupancy:**

- **Occupied region:** $\{k : E(k) \leq E_F\}$
- **Fermi surface:** $\{k : E(k) = E_F\}$

---

### **When you run the code:**

A Tkinter window appears, containing two subplots:

#### **K-space Plot:**

- Shows a square representing the Brillouin zone ($-\pi \leq k_x, k_y \leq \pi$)
- Gray fill for occupied states if $E(k) \leq E_F$
- A blue contour line indicating the Fermi “surface” for $E(k) = E_F$

#### **Density of States Plot:**

- A black line for $g(E)$ vs. $E$
- Gray fill for energies below $E_F$
- A red vertical dashed line indicating $E_F$

---

A slider at the bottom of the window lets you vary $E_F$ from $\min E = 0$ to $\max E = 8$.

As you change $E_F$:

- The filling region in K-space changes (larger $E_F$ means more of K-space is “filled”)
- The DOS filling on the plot adjusts accordingly, and the vertical line moves

This real-time visualization provides an intuitive understanding of how the Fermi surface evolves and how states up to $E_F$ are occupied in the 2D tight-binding model.

