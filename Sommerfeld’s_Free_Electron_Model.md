# Details of Sommerfeld’s Free Electron Model

This section details how Arnold Sommerfeld’s Free Electron Model (often called the Sommerfeld model) derives the electrical conductivity $\sigma$. In stark contrast to Drude’s purely classical derivation, Sommerfeld’s approach employs:

- **Quantum mechanical energy levels**
  
$$ E = \frac{\hbar^2 k^2}{2m} $$
  
- **Fermi–Dirac statistics** (the Pauli exclusion principle with occupation up to the Fermi surface).

Although the final functional form of the conductivity looks similar to Drude’s, the **physical interpretation and numerical consistency at low temperatures** are fundamentally improved.

---

# 1. Historical Prelude and Motivation

### **Drude’s Shortcomings:**
Paul Drude (1900) first succeeded in explaining the order of magnitude of conductivity using **classical kinetic ideas**: collisions, a relaxation time $\tau$, and a classical electron gas. However, Drude’s model **predicted the wrong magnitude** for the electronic heat capacity and sometimes gave **incorrect signs or magnitudes** for the Hall coefficient.

### **Quantum Breakthrough (1920s):**
With the establishment of quantum mechanics, **Arnold Sommerfeld** realized that electrons in a metal must obey the **Pauli exclusion principle** and that they fill up quantum states up to a characteristic **Fermi energy** $E_F$. Only electrons near $E_F$ can respond to small electric fields or temperature changes, **reconciling Drude’s anomalies** with experimental reality.

### **Boltzmann Transport Equation (Quantum Version):**
Sommerfeld’s derivation effectively used (or paralleled) the **Boltzmann transport equation** but with the **Fermi–Dirac equilibrium distribution** $f_0(k)$. This is essential for deriving how an electric field shifts the electron occupation slightly from equilibrium, thereby generating a net current.

---

# 2. Setting Up the Sommerfeld Model

## 2.1. Free Electrons in a Box (Periodic Boundary Conditions)
We imagine $N$ electrons occupying a volume $V$. The electron wavefunctions under periodic boundary conditions are:

$$
\psi_k (r) = \frac{1}{V} e^{i k \cdot r},
$$

with energies:

$$
E(k) = \frac{\hbar^2 k^2}{2m}.
$$

---

## 2.2. Fermi–Dirac Distribution
At temperature $T$, electrons obey:

$$
f_0(E) = \frac{1}{\exp \left( \frac{E - \mu}{k_B T} \right) + 1},
$$

where $\mu \approx E_F$ at low $T$. At $T=0$, all states with $E(k) < E_F$ are occupied; those above are empty.

---

## 2.3. Relaxation-Time Approximation
Sommerfeld (and subsequent treatments) introduced a **scattering time** $\tau$ to model collisions. In the presence of an electric field $E$, electrons accelerate between collisions, but each collision randomizes (or partially relaxes) their momentum. This is often encapsulated in the **Boltzmann transport equation** with a collision term:

$$
-\frac{f - f_0}{\tau}.
$$

---

# 3. Boltzmann Transport Equation: Step-by-Step

A more rigorous route (historically emerging in the 1920s and 1930s) is to solve the **Boltzmann equation** for electrons, treating them as quantum particles with a classical phase-space distribution:

$$
\frac{\partial f}{\partial t} + \dot{r} \cdot \nabla_r f + \dot{k} \cdot \nabla_k f = \left( \frac{\partial f}{\partial t} \right)_{\text{collisions}}.
$$

To understand more of the above equation click [here](https://github.com/ph7klw76/SIF3008/blob/main/Boltzmann_Transport_Equation.md) and [here](https://github.com/ph7klw76/SIF3008/blob/main/Boltzmann_%20Transport_Equation2.md) more in depth. 
In the Sommerfeld model (uniform metal, uniform electric field, no magnetic field):

$$
\dot{r} = v_k = \frac{\partial E}{\hbar \partial k} = \frac{\hbar k}{m}.
$$

$$
\dot{k} = -\frac{e}{\hbar} E.
$$

$$
\left( \frac{\partial f}{\partial t} \right)_{\text{coll}} \approx -\frac{f - f_0}{\tau} \quad \text{(relaxation-time approximation).}
$$

**Steady State** implies $\frac{\partial f}{\partial t} = 0$. Also, $\nabla_r f = 0$ for a uniform system. Thus:

$$
0 + v_k \cdot 0 + \left(-\frac{e}{\hbar} E\right) \cdot \nabla_k f = -\frac{f - f_0}{\tau}.
$$

Hence,

$$
-\frac{e}{\hbar} E \cdot \nabla_k f = -\frac{f - f_0}{\tau}.
$$

---

## 3.1. Linearized Solution
Assume the departure from equilibrium $f - f_0$ is small, so we write:

$$
f(k) = f_0(k) - \frac{e \tau}{\hbar} E \cdot \nabla_k f_0(k).
$$

We took:

$$
\frac{f - f_0}{\tau} \approx \frac{e}{\hbar} E \cdot \nabla_k f_0.
$$

Since:

$$
\nabla_k f_0 (k) = \frac{d f_0}{dE} \nabla_k E(k),
$$

and:

$$
E(k) = \frac{\hbar^2 k^2}{2m}, \quad \Rightarrow \quad \nabla_k E = \frac{\hbar^2 k}{m},
$$

also,

$$
\frac{d f_0}{dE} = -\frac{\partial f_0}{\partial E} \approx \delta (E - E_F) \quad \text{(at } T=0 \text{)},
$$

which is crucial: **only states near the Fermi surface matter for conduction**.

---

# 4. Current Density and Electrical Conductivity

## 4.1. Expression for $J$

The current density is:

$$
J = -2e \int \frac{d^3 k}{(2\pi)^3} v_k f(k).
$$

The factor of 2 accounts for spin $\frac{1}{2}$ electrons (up & down). Since:
To understand more of the above equation click [here](https://github.com/ph7klw76/SIF3008/blob/main/Current_Density.md)
$$
v_k = \frac{\hbar k}{m},
$$

and $f(k)$ is the perturbed distribution from the previous step.

---

## 4.2. Substituting the Linearized Distribution
Substituting:

$$
f(k) \approx f_0(k) - \frac{e \tau}{\hbar} E \cdot \nabla_k f_0(k).
$$

Since $f_0(k)$ is isotropic around $k=0$, its integral over all $k$-space at equilibrium **yields no net current**. The first non-zero contribution comes from:

$$
-\frac{e \tau}{\hbar} E \cdot \nabla_k f_0.
$$

Hence,

$$
J = 2 e^2 \tau \int \frac{d^3 k}{(2\pi)^3} v_k \left[ E \cdot \nabla_k f_0(k) \right].
$$

Rearrange:

$$
J = 2 e^2 \tau E \int \frac{d^3 k}{(2\pi)^3} \left[ v_k v_k \right] \left( -\frac{\partial f_0}{\partial E} \right).
$$

Since:

$$
\nabla_k E = \frac{\hbar^2 k}{m} = \hbar v_k,
$$

we get:

$$
J = \left(\text{constant}\right) E \left[ \frac{n e^2 \tau}{m} \right].
$$

Thus, to leading order:

$$
\sigma = \frac{n e^2 \tau}{m}.
$$

**Crucially, the relevant velocity (and states) are at the Fermi surface.** Unlike Drude, where **thermal velocity** dominates, Sommerfeld showed that conduction arises from **electrons with velocity near**:

$$
v_F = \sqrt{\frac{2 E_F}{m}}.
$$

Despite its quantum modifications, **the final functional form of $\sigma$ remains identical to Drude’s formula**—but the interpretation and temperature dependence are profoundly different.

# 5. Discussion of Sommerfeld’s Key Innovations

### **Quantum Statistics:**
Sommerfeld replaced Drude’s classical **Maxwell–Boltzmann distribution** with the **Fermi–Dirac distribution**. This ensures only electrons within **$\sim k_B T$** of **$E_F$** are thermally excited and can respond to an external field at low **$T$**.

### **Fermi Velocity:**
At **$T=0$**, conduction electrons effectively move at **Fermi velocity $v_F$**, which can be significantly larger than the naive **“thermal velocity.”**

### **Correct Low T Specific Heat:**
Since the vast majority of electrons are in states **far below the Fermi surface** (and blocked by **Pauli exclusion** from excitations), the model **resolves Drude’s erroneous large specific heat.**

### **Better Agreement with Experiments:**
Sommerfeld’s approach yields (after slightly more refined treatments of scattering) the **correct Wiedemann–Franz ratio**, the **temperature dependence of resistivity at low $T$**, and **consistent signs** for the Hall effect in many (but not all) simple metals.

---

# 6. Final Conductivity Expression and Its Meaning

### **Putting the results together:**

$$
\sigma_{\text{Sommerfeld}} = \frac{n e^2 \tau}{m}
$$

- **Looks the same as Drude:** The same algebraic structure arises from a **different microscopic rationale**.  
- **Sommerfeld identifies** $n$ as the **total conduction electron density**, but **only those near the Fermi surface** are dynamically relevant for conduction.

### **Temperature Dependence:**  
Sommerfeld’s derivation clarifies that **$\tau$** or other terms (e.g., scattering rates) can have a **more subtle dependence on temperature**, especially for **$T \ll T_F$**.

### **Effective Mass:**  
In real crystals, the electron’s n band structure modifies m to m*. The **free electron model sets** m* = m, but in actual metals (with some periodic potential), the **conduction band** can yield an **effective mass** different from the bare electron mass.

---

# 7. Historical Significance

### **Convergence of Classical and Quantum Worlds:**  
Sommerfeld showed that, on the surface, the formula:

$$
\sigma = \frac{n e^2 \tau}{m}
$$

survives quantum corrections. However, the **internal justification is dramatically different.**

### **Resolution of Drude’s Paradoxes:**

- **Heat Capacity:** No classical **equipartition fiasco**—only **electrons near $E_F$** are excited at low **$T$**.  
- **Better Hall Coefficients:** The **distribution near the Fermi surface** influences **sign and magnitude** in ways a purely classical picture **could not replicate fully**.  
- **Wiedemann–Franz:** The correct ratio **$\kappa / \sigma \propto T$** emerges with a **more accurate Lorenz number** than Drude’s estimate.

### **Foundation for Band Theory:**
Sommerfeld’s method was the **quantum free-electron baseline**. The next step was **adding the periodic potential** of the lattice (**Bloch’s theorem, band structure**) to explain **semiconductors, insulators, and the effective masses in real metals**.

---

# 8. Summary and Key Takeaways

- **Sommerfeld’s primary step** was to apply the **Pauli principle** and **Fermi–Dirac statistics**, keeping Drude’s **relaxation-time ($\tau$) concept** for collisions.
- **Mathematically,** the **Boltzmann transport equation** with **Fermi–Dirac equilibrium distribution** yields the **same overall shape** for $\sigma$ as Drude:

$$
\sigma = \frac{n e^2 \tau}{m}.
$$

- The **crucial quantum difference** is that **conduction is dominated by the Fermi surface electrons**.

- **Historically,** this overcame the **biggest Drude-era puzzle**: why the **electronic specific heat is so small at low $T$**.  
  - Sommerfeld pinned it on the **“blocking” effect**—only electrons near **$E_F$** can be excited.

- **Physically,** $\tau$ and $m$ in modern theory are often replaced by:  
  - **$\tau(\varepsilon_F)$** (energy-dependent scattering times)  
  - **$m^*$** (effective mass), bridging to **real crystal band structures** and **more precise predictions**.

- **Sommerfeld’s derivation**—though seemingly straightforward from a modern vantage—was a **landmark** in applying **quantum statistics** to a **many-electron system** and bridging the gap from **Drude’s classical guess** to the **more refined, realistic pictures** that followed.

