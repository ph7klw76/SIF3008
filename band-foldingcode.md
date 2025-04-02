The code implements a 2D nearest-neighbor tight-binding model on a square lattice, then constructs an $N \times N$ supercell and visualizes the folded band structure along a chosen high-symmetry path. We also compute an approximate density of states (DOS) from the path. Let us dissect both the physical model and mathematical details of how the band folding is carried out.

---

# 1. Physical Model: 2D Nearest-Neighbor Tight-Binding

## 1.1. Primitive Cell Dispersion

The code defines a function:

```python
def calculate_primitive_energy(kx, ky):
    return 4.0 - 2.0 * (np.cos(kx) + np.cos(ky))
```
This corresponds to a tight-binding dispersion on a square lattice with lattice constant $a = 1$ (units of length) and hopping parameter $t = 1$.

Typically, for a nearest-neighbor Hamiltonian:

$$
\hat{H} = -t \sum_{\langle i, j \rangle} \left( c_i^\dagger c_j + c_j^\dagger c_i \right),
$$

the energy in momentum space (with on-site energy $E_0 = 2$ included) is:

$$
E(k) = E_0 - 2t \left[ \cos(k_x a) + \cos(k_y a) \right]
$$

In code units (setting $a = 1$, $t = 1$, and $E_0 = 2$), this becomes:

$$
E(k_x, k_y) = 2 - 2 \left[ \cos(k_x) + \cos(k_y) \right]
$$

Then the code’s function rearranges it slightly to:

$$
E(k_x, k_y) = 4 - 2 \left[ \cos(k_x) + \cos(k_y) \right]
$$

### **Energy Range:**

You can easily confirm that:

- $\min E(k_x, k_y)$ is **0** (occurring at $k_x = \pm \pi$ and $k_y = \pm \pi$)
- $\max E(k_x, k_y)$ is **8** (at $k_x = 0$ and $k_y = 0$)

Thus:

- `MIN_ENERGY = 0`
- `MAX_ENERGY = 8`

---

# 2. Band Folding in an $N \times N$ Supercell

## 2.1. Supercell in Real Space

A supercell approach modifies the real-space periodicity to a larger unit cell, effectively repeating the primitive cell $N$ times in each direction.

If the original (primitive) lattice constant is $a = 1$, then an $N \times N$ supercell has side length $N$.

---

## 2.2. Brillouin Zone Shrinkage in Reciprocal Space

Changing the real-space period to $N$ means that the reciprocal lattice vectors become:

$$
\tilde{G}_x = \frac{2\pi}{N}, \quad \tilde{G}_y = \frac{2\pi}{N}
$$

instead of $2\pi$ for the primitive lattice (with $a = 1$). Hence, the new (folded) Brillouin zone (BZ) is smaller by a factor of $N$.

Instead of spanning $[-\pi, \pi]$ in each direction, the folded BZ extends over:

$$
\left[ -\frac{\pi}{N}, \frac{\pi}{N} \right]
$$

---

## 2.3. Wavevector Folding: $k + G$

Even though the new BZ is smaller, physically all primitive-lattice wavevectors $k_x, k_y$ are still possible solutions.

The standard approach is to “fold” all wavevectors back into the smaller BZ by subtracting or adding reciprocal-lattice vectors:

$$
G = \left( \frac{2\pi}{N} i, \frac{2\pi}{N} j \right)
$$

where $i, j$ are integers $0, 1, 2, \dots, N - 1$.

Concretely, if $k \in$ Folded BZ, then:

$$
k + G_{i,j} = \left( k_x + \frac{2\pi}{N} i, \, k_y + \frac{2\pi}{N} j \right)
$$

lives in the original, larger Brillouin zone.

We then evaluate the primitive cell dispersion $E(k_x + G_x, \, k_y + G_y)$ for all $(i, j)$.

This gives $N^2$ energies for each $k$ inside the new BZ.

These are interpreted as the folded bands for the $N \times N$ supercell.

---

## 2.4. Code Implementation of Band Folding

In the function:
```python
def calculate_folded_bands(k_point, N):
    kx, ky = k_point
    indices_i = np.arange(N)
    indices_j = np.arange(N)
    II, JJ = np.meshgrid(indices_i, indices_j)

    # Shift wavevector by multiples of 2*pi/N in both directions
    Kx_folded = kx + 2 * np.pi * II.flatten() / N
    Ky_folded = ky + 2 * np.pi * JJ.flatten() / N

    folded_energies = calculate_primitive_energy(Kx_folded, Ky_folded)
    return folded_energies
```
**Input:** A wavevector $k \in \left[ -\frac{\pi}{N}, \frac{\pi}{N} \right]^2$ in the folded BZ.

**Output:** A list/array of length $N^2$ containing:

$$
E\left(k_x + \frac{2\pi i}{N},\ k_y + \frac{2\pi j}{N}\right)
$$

for $i, j = 0, \dots, N - 1$.

These $N^2$ values are precisely the folded band energies at the chosen $k$.

---

# 3. Path Through the Reduced BZ: $\Gamma \rightarrow X \rightarrow M \rightarrow \Gamma$

## 3.1. High-Symmetry Points in the Folded BZ

For a square-lattice BZ reduced by a factor of $N$, the corners of the smaller BZ are at:

$$
\pm \frac{\pi}{N},\ \pm \frac{\pi}{N}
$$

The code chooses a path in this smaller BZ:

- $\Gamma = (0, 0)$  
- $X = \left( \frac{\pi}{N},\ 0 \right)$  
- $M = \left( \frac{\pi}{N},\ \frac{\pi}{N} \right)$  
- back to $\Gamma = (0, 0)$

---

The function:
```python
def generate_k_path(N, num_points_per_segment):
    gamma = np.array([0, 0])
    x     = np.array([np.pi / N, 0])
    m     = np.array([np.pi / N, np.pi / N])
    ...
```
**Segments:** Each segment $\Gamma \rightarrow X$, $X \rightarrow M$, and $M \rightarrow \Gamma$ is interpolated with `num_points_per_segment` points (linear interpolation in $k$-space).

The code then merges these segments (avoiding double-counting end-points) into a single array of $k$-points $\{k_i\}$.

---

## 3.2. Distance Along the Path

The code calculates a cumulative distance along the path, letting us plot the band energies vs. “distance” from $\Gamma$ on the horizontal axis.  
This is standard for band-structure plots: you see a piecewise linear path in $k$-space, but the abscissa is a one-dimensional “arc length” along that path.

---

# 4. Plotting the Folded Bands and DOS

## 4.1. Folded Bands vs. $k$-Distance

For each $k_i$ on the path, we call:

![image](https://github.com/user-attachments/assets/7f2584f6-4392-43c6-92ba-312fad5d483d)


where $\alpha = 1 \dots N^2$ indexes the $N^2$ possible folded energies.

The code then plots $E(i, \alpha)$ as lines (one line per $\alpha$) vs. the cumulative distance at point $i$.

```python
for i, k_vec in enumerate(k_points):
    all_bands[i, :] = calculate_folded_bands(k_vec, N)
# ...
ax_bands.plot(k_distances, all_bands, 'b-', lw=0.8, alpha=alpha_val)
```

Because there are $N^2$ “sub-bands” in the supercell, as $N$ grows, the plot becomes dense with lines.

The code adjusts the plotting transparency ($\alpha_{\text{val}}$) so we can still discern overall trends at higher $N$.

---

## 4.2. Approximate DOS from the Path

After computing $\{ E(i, \alpha) \}$, the code flattens them into a 1D array:

$$
\{ E(i, \alpha) \mid i = 1, \dots, n_k,\ \alpha = 1, \dots, N^2 \}
$$

where $n_k$ is the total number of sampled $k$-points along $\Gamma \rightarrow X \rightarrow M \rightarrow \Gamma$.

A histogram is then used to approximate a DOS:

```python
energies_on_path = all_bands.flatten()
dos_counts_path, dos_edges_path = np.histogram(
    energies_on_path, bins=..., range=(MIN_ENERGY-0.1, MAX_ENERGY+0.1)
)
dos_energies_path = (dos_edges_path[:-1] + dos_edges_path[1:]) / 2
...
dos_smoothed_path = gaussian_filter1d(dos_values_path, sigma=1.0)
```
This is not the true 2D DOS, because we are only sampling energies along a single path in the BZ (plus folding). Nonetheless, it provides a rough or partial view of how the energies are distributed.

The code then plots DOS vs. energy (on the same figure, but in a separate subplot).

---

# 5. Equations Summarizing the Process

### Primitive Cell Energy:

$$
E(k) = 4 - 2 \left[ \cos(k_x) + \cos(k_y) \right]
$$

### Supercell Reciprocal-Lattice Shift:

![image](https://github.com/user-attachments/assets/be0846c1-a8f0-4e05-bf5b-a0b6a8a2df91)

### Folded Energies:

![image](https://github.com/user-attachments/assets/e8233c38-0256-4b3f-acb5-95b6f6d6516b)

Each $k$ in the new BZ yields $N^2$ band energies in the folded scheme.

---

### Path in Reduced BZ:

- $\Gamma$: $(0, 0)$  
- $X$: $\left( \frac{\pi}{N}, 0 \right)$  
- $M$: $\left( \frac{\pi}{N}, \frac{\pi}{N} \right)$

---

### DOS from Path (Histogram approach):

$$
g_{\text{path}}(E) \approx \frac{\Delta N(E)}{\Delta E}, \quad \Delta N(E) = \text{(number of states with } E(i, \alpha) \text{ in bin)}
$$

This ignores the full 2D integration, but gives an approximate distribution of energies.

---

# 6. Practical Steps in the Code

- **Tkinter + Matplotlib:** The code embeds Matplotlib plots in a Tkinter GUI.
- **Slider:** A Tkinter slider (`Scale`) controls the integer $N$. As $N$ changes, the code:
  - Generates the new high-symmetry path $\Gamma \rightarrow X \rightarrow M \rightarrow \Gamma$ in the smaller BZ.
  - For each point on that path, computes all $N^2$ energies by shifting $k$ with the reciprocal-lattice offsets.
  - Plots these energies vs. distance along the path.
  - Histograms these energies to produce the approximate DOS.

**Performance:** The code prints calculation and redraw times. For large $N$, the number of states is $n_k \times N^2$, which can grow quickly.  
The code tries to keep $N \leq 20$ for interactivity.

---

# 7. Physical Interpretation

- At $N = 1$:  
  We recover the primitive cell result.  
  The path is $\Gamma \rightarrow X \rightarrow M \rightarrow \Gamma$ in the original BZ.  
  We see the original tight-binding band along those directions, typically just one or two bands (in a single-orbital model).

- At $N > 1$:  
  The BZ shrinks by a factor of $N$.  
  Each point in the smaller BZ can be seen as a superposition of $k$ points from the original BZ.  
  This yields $N^2$ folded bands.  
  The “lowest” band might mostly replicate the primitive band near $\Gamma$, while the higher bands are essentially the folded copies from corners (or edges) of the original BZ.

**DOS:**  
Because so many states get folded into a smaller BZ, the energies at each path point can start to cluster.  
You might see more dense lines or additional band crossings that do not appear for $N = 1$.

---

# 8. Conclusion

This code is an interactive band-folding tool for a 2D nearest-neighbor square-lattice tight-binding model.  
The essential physics is that:

- **Large Real-Space Supercell** ⟹ **Smaller Brillouin Zone**
- **Folding:** Every wavevector in the new (small) BZ is mapped to $N^2$ wavevectors in the old (primitive) BZ.
- **Folded Band Structure:** Evaluate the primitive dispersion at those $N^2$ wavevectors, and interpret them as “sub-bands” in the supercell.
- **Path + Histogram:** Plotting along $\Gamma \rightarrow X \rightarrow M \rightarrow \Gamma$ plus a histogram of energies yields a graphical depiction of the multiple folded bands and a rough DOS.

---

### **Key Equations:**

![image](https://github.com/user-attachments/assets/f3a08d21-75ce-4bf6-8db3-62d39591d154)

---

Because tight-binding modeling and band folding are central concepts in solid-state physics,  
the code—and the techniques it demonstrates—are broadly applicable to any scenario  
where a superlattice or repeated pattern modifies the original crystal’s periodicity,  
leading to new or “replica” bands in the folded Brillouin zone.

