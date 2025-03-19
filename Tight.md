# 1. Tight Binding Model

# Early Band Theory

- **Sommerfeld/Free Electron Model (~1927)**: Electrons treated as essentially free, ignoring detailed ionic potentials (**good for simple metals**).
- **Nearly Free Electron Model (1930s)**: Small periodic potential modifies free-electron dispersions, causing band gaps at **Brillouin zone boundaries** (**good for metals with weak ionic potentials**).

However, these methods are **ill-suited** for solids in which electrons are **strongly influenced by the lattice potential**, e.g.:

- **Covalently bonded semiconductors** (like **Si, Ge**)
- **Metals with partially filled $d$-bands** (transition metals)
- **Metals with $f$-bands** 

---

# Localized Atomic-Orbital Limit

Historically, **Heitler–London (1927)** first analyzed the **hydrogen molecule** by combining two atomic orbitals. This inspired the idea that, in a crystal, **electrons might remain fairly localized around atoms** and only **“hop” or overlap between neighboring atomic orbitals**. 

The **Tight-Binding approach** is the **complementary limit** to the nearly free electron model:

- **Nearly Free**: Start from **plane waves**, add a **small periodic potential**.
- **Tight-Binding**: Start from **localized atomic orbitals** and allow them to **overlap slightly from site to site**.

---

# Wider Implications

The **tight-binding model** remains **central** to describing:

- **Semiconductors**
- **Transition metals**
- **Superconducting materials**
- **Countless modern developments** (e.g., graphene, MoS$_2$, topological insulators)

where the concept of **“hopping” between localized orbitals** (e.g., **$p$-, $d$-, or $f$-like states**) is crucial.

Historically, **Bloch (1929)** and **Slater & Koster (1954)** refined these ideas for realistic crystals, culminating in **tight-binding band structure methods** that are widely used even now.

---

# 2. Core Idea: Electrons as Superpositions of Localized Orbitals

## 2.1. Bloch’s Theorem Revisited in an Atomic Orbital Basis
In a crystal with lattice vectors $\{ R \}$, each site hosts an atomic orbital $\phi(r - R)$. The electron wavefunctions must be Bloch functions:

$$
\psi_{n,k}(r) = \frac{1}{N} \sum_R e^{i k \cdot R} \phi_n(r - R),
$$

where:

- $n$ indexes the orbital type (e.g., $s$, $p$, or a combination).
- $k$ is the crystal wavevector within the first Brillouin zone.
- $N$ is the total number of lattice sites.

This **linear combination of atomic orbitals (LCAO)** ensures the periodic Bloch phase factor $e^{i k \cdot R}$. To learn more how Gaussian basis is computationally efficient to construct LCAO, click [here](Gaussian.md)

In tight-binding, we assume each $\phi(r - R)$ is quite localized around its site $R$, so overlap between distant sites is negligible. **Only nearest (or maybe next-nearest) neighbor overlaps matter**.

---

# 3. Hamiltonian and Matrix Elements

## 3.1. General Form of the Electronic Hamiltonian

$$
\hat{H} = -\frac{\hbar^2}{2m} \nabla^2 + V_{\text{ion}}(r) + V_{e-e}[\rho(r)],
$$

where:

- $V_{\text{ion}}$ is the ionic potential arranged periodically.
- $V_{e-e}$ is the electron-electron interaction (often simplified in a mean-field or **Hartree–Fock** approach, to learn more of Hatree-Fock click [here](Hartree-Fock_Theory.md)).

In the simplest tight-binding argument, one **lumps everything** into an effective single-particle potential $U_{\text{eff}}(r)$.

## 3.2. Matrix Elements in the Atomic Orbital Basis

Let $\phi_{\alpha}(r - R)$ represent an orbital labeled by $\alpha$ (could be $s$, $p_x$, etc.) on site $R$. We define the on-site and off-site **Hamiltonian matrix elements**:

### On-site energy:

$$
\varepsilon_{\alpha} = \langle R, \alpha | \hat{H} | R, \alpha \rangle
= \int d^3r \, \phi_{\alpha}^*(r - R) \hat{H} \phi_{\alpha}(r - R).
$$

This is the **energy of orbital $\alpha$** if the electron remains on the same site $R$.

### Hopping (transfer) integrals:

$$
t_{\alpha \beta}(R - R') = \langle R, \alpha | \hat{H} | R', \beta \rangle
= \int d^3r \, \phi_{\alpha}^*(r - R) \hat{H} \phi_{\beta}(r - R').
$$

If $R'$ is a **neighbor of** $R$, this integral might be **non-negligible**. Otherwise, it is often assumed small or zero in a simpler **“nearest-neighbor” tight-binding approach**.

### 3.2.1. Simplifications:
- **Nearest-Neighbor Approximation**: $t_{\alpha \beta}(R - R') \neq 0$ **only** if $R'$ is a **nearest neighbor** of $R$.
- **Orthogonality Assumption**:
  
$$
\langle R, \alpha | R', \beta \rangle = \delta_{R, R'} \delta_{\alpha, \beta}.
$$

---

# 4. Derivation of the Tight-Binding Band Structure

## 4.1. Constructing the Bloch State
We form a trial **Bloch wavefunction** as a superposition of local orbitals:

$$
\Psi_{\alpha, k}(r) = \frac{1}{N} \sum_R e^{i k \cdot R} \phi_{\alpha}(r - R).
$$

We want to solve the **time-independent Schrödinger equation**:

$$
\hat{H} \Psi_{\alpha, k}(r) = E_{\alpha}(k) \Psi_{\alpha, k}(r),
$$

for the **energy $E_{\alpha}(k)$**.

---

## 4.1.1. Single Orbital per Site (1D Example)
For clarity, let us do the simplest case:

- **1D chain** with lattice constant $a$.
- **One orbital** $\phi(r - n a)$ per site.
- **Nearest-neighbor hopping integral**: $-t$.
- **On-site energy**: $\varepsilon_0$.

Then:

$$
\Psi_k(x) = \frac{1}{N} \sum_{n=0}^{N-1} e^{i k n a} \phi(x - n a).
$$

Projecting $\hat{H}$ on $\langle m | \equiv \langle x | \phi(x - m a)$ and using orthogonality:

$$
\langle m | \hat{H} | \Psi_k \rangle = E(k) \langle m | \Psi_k \rangle = E(k) \frac{1}{N} e^{i k m a}.
$$

From the **tight-binding integrals**:

![image](https://github.com/user-attachments/assets/a47e1002-b530-41bd-8d1a-4ccbe29bdd0a)

Thus:

$$
(\varepsilon_0 + 2t \cos(ka)) \frac{1}{N} e^{i k m a} = E(k) \frac{1}{N} e^{i k m a}.
$$

### **Final Dispersion Relation:**

$$
E(k) = \varepsilon_0 + 2t \cos(ka).
$$

This is the **tight-binding dispersion** for a **1D chain** with **one orbital per site**.

---

## 4.1.2. Interpretation:
- If **$t = 0$**, each electron is **localized** at an atomic orbital with energy **$\varepsilon_0$** → No dispersion = No conduction.
- As **$t$ grows**, the **bandwidth increases**, allowing **electrons to move across sites more easily**.

---

# 4.2. Multiple Orbitals, 2D/3D Generalizations

For **multi-orbital** or **multi-dimensional lattices**, one obtains a **matrix equation** in orbital indices. The **dimension of the matrix** is:

- **Number of orbitals in the basis** or 
- **Number of atoms per unit cell $\times$ orbitals per atom**.

Solving this **matrix at each $k$** yields **multiple bands** $E_n(k)$. 

The **essence remains** that **each band arises from a set of atomic orbitals hybridizing via nearest-neighbor overlaps**.

# 5. Why Tight-Binding? Key Insights and Advantages

## Strong Lattice Potential:
- The electrons are **not free**; the wavefunctions revolve around atomic sites with strong binding. 
- **This is the opposite limit** of the nearly free electron model.

## Natural for Covalent Solids:
- In materials like **silicon or gallium arsenide**, electrons primarily occupy **localized bonding orbitals** ($sp^3$ hybrids, for instance). 
- The **TB approach** is physically intuitive and can yield accurate band structures with carefully chosen hopping integrals.

## Easily Extended:
- We can **add more neighbors** (2nd, 3rd nearest neighbors) to refine the model.
- We can **incorporate multi-orbital terms**: 
  - **$s$-, $p$-, or $d$-orbitals**, leading to larger **Hamiltonian matrices** that capture complex band structures (e.g., **transition-metal oxides**, **graphene’s $\pi$-bands** from carbon **$p_z$ orbitals**, etc.).

---

## 6. Comparison with Other Models

### **Drude / Sommerfeld Model:**
- **Overly simplistic** if electrons are not actually free-like.
- **Cannot handle** large band gaps or strongly localized states.

### **Nearly Free Electron Model:**
- **Good** for small potentials with **large overlap of wavefunctions**.
- **Fails** if wavefunctions are **strongly localized** around atomic cores or if the potential is **large**.

### **Tight-Binding Model:**
- **Shines** when atomic orbitals are **strongly localized**.
- **Straightforward** to see how local orbital energies **split** into bands.
- **Band gaps** appear naturally if hopping integrals are **overshadowed** by large differences in **on-site energies** or if there is **significant difference across orbitals**.

## Beyond Simplifications:
- **Real materials** may require **advanced tight-binding parameterization**.
- **Tight-Binding can be ab initio-like** if matrix elements are extracted from **quantum chemistry** or **DFT calculations**. 
- This is still widely used for **large-scale simulations** (e.g., **nano-scale devices, molecular solids**).

---

# 7. Detailed Equations Recap

## **Bloch Function in Atomic Basis**

$$
\psi_{n,k}(r) = \frac{1}{N} \sum_{R, \alpha} c_{n, \alpha}(k) e^{i k \cdot R} \phi_{\alpha}(r - R).
$$

## **Hamiltonian Matrix Elements**

$$
H_{R\alpha, R' \beta} = \langle R, \alpha | \hat{H} | R', \beta \rangle = t_{\alpha \beta}(R - R'),
$$

with the **on-site energy**:

$$
\varepsilon_{\alpha} = t_{\alpha \alpha}(0).
$$

---

## **Nearest-Neighbor 1D Single-Orbital Result**

$$
E(k) = \varepsilon_0 + 2t \cos(ka).
$$

This is the **prototypical tight-binding band dispersion**.

---

## **Multiple Orbitals → $k$-Dependent Matrix**
In general, for each **$k$**, we solve the **secular equation**:

$$
\det [ \hat{H}(k) - E(k) \hat{I} ] = 0,
$$

where **$\hat{H}(k)$** is a matrix whose dimension is the number of **distinct orbitals in the unit cell**.

Its elements come from sums of:

$$
t_{\alpha \beta}(R - R') e^{i k \cdot (R - R')}.
$$

---

# 8. Historical Significance and Ongoing Relevance

### **Heitler–London to Bloch**
- The idea that **atomic wavefunctions can combine** to form **molecular/solid states** dates back to **early quantum chemistry** on **diatomic molecules**, extended to **crystals by Bloch, Slater, Koster, and others**.

### **Solid-State Physics Cornerstone**
- The **tight-binding viewpoint** elegantly shows how **atomic levels broaden into bands** and how the **electron wavefunctions gain or lose dispersion** depending on overlap integrals.

## **Semiconductors, Transition Metals, Graphene**
- **Semiconductors**:  
  - **$sp^3$ tight-binding** used for **elemental (Si, Ge)** and **compound (GaAs, InP) semiconductors**.  
  - **Accurately predicts conduction/valence band structures**.
- **Transition Metals**:  
  - **$d$-bands** are often described in a tight-binding approach since **$d$-orbitals** are more **localized**.
- **Graphene**:  
  - A **honeycomb lattice** with **carbon $p_z$-orbitals** uses a **nearest-neighbor tight-binding** that yields the famous **Dirac cone dispersion** near the **K points**.

---

# 9. Modern Extensions

### **Tight-Binding DFT:**
- Replaces **complicated plane-wave expansions** in **density functional theory (DFT)** with **local orbitals**, enabling **large-scale calculations**.

### **Model Hamiltonians in Strongly Correlated Electron Systems:**
- **E.g., Hubbard model, t-J model** start from the **tight-binding picture** and add **electron-electron interactions** more explicitly.

---

# 10. Concluding Reflections

### **The Tight-Binding Model:**
- **Emphasizes localized atomic orbitals**.
- **Derives energy bands via overlap (hopping) integrals**.
- **Provides a powerful conceptual and practical framework** for materials where **electrons are not free** but remain **substantially bound** to atoms or molecules, bridging them into **extended Bloch states** via partial overlaps.

### **Final Thoughts:**
- From a **historical vantage**, it stands as the **complementary approach** to the **nearly free electron model**—together forming **two extremes** in the powerful **Band Theory of Solids**.
- It remains **indispensable** in **modern condensed matter and materials research**, from **modeling graphene** to **complex transition-metal oxides**.
- The **key lesson** is that **electron wavefunctions** in **real crystals** can be well described by expansions in either:
  - **Plane waves** (when the potential is weak).
  - **Local orbitals** (when the potential is strong or orbitals are well-defined).
- In **many materials**, a carefully **parameterized tight-binding model** gives an **excellent quantitative handle** on **electronic structures**, bridging **fundamental theory** and **experimental observables**.

