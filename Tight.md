# 1. Tight Binding Model

## Early Band Theory

- **Sommerfeld/Free Electron Model (~1927)**: Electrons treated as essentially free, ignoring detailed ionic potentials (good for simple metals).
- **Nearly Free Electron Model (1930s)**: Small periodic potential modifies free-electron dispersions, causing band gaps at Brillouin zone boundaries (good for metals with weak ionic potentials).

However, these methods are ill-suited for solids in which electrons are strongly influenced by the lattice potential, e.g., **covalently bonded semiconductors** (like Si, Ge) or metals with **partially filled $d$-bands** (transition metals) or **$f$-bands**.

---

## Localized Atomic-Orbital Limit

Historically, **Heitler–London (1927)** first analyzed the hydrogen molecule by combining two atomic orbitals. This inspired the idea that in a crystal, **electrons might remain fairly localized around atoms** and only **“hop”** or overlap between neighboring atomic orbitals. The **Tight-Binding approach** thus is the complementary limit to the nearly free electron model:

- **Nearly Free**: Start from plane waves, add a small periodic potential.
- **Tight-Binding**: Start from localized atomic orbitals and allow them to overlap slightly from site to site.

### Wider Implications

The **tight-binding model** remains central to describing:

- **Semiconductors**
- **Transition metals**
- **Superconducting materials**
- **Modern developments (graphene, MoS$_2$, topological insulators)**

where the concept of **“hopping” between localized orbitals** (e.g., $p$-, $d$-, or $f$-like states) is crucial.

Historically, **Bloch (1929)** and **Slater & Koster (1954)** refined these ideas for realistic crystals, culminating in **tight-binding band structure methods** that are widely used even now.

---

# 2. Core Idea: Electrons as Superpositions of Localized Orbitals

## 2.1 Bloch’s Theorem Revisited in an Atomic Orbital Basis

In a crystal with lattice vectors $\{ R \}$, each site hosts an atomic orbital $\phi(r - R)$. The electron wavefunctions must be **Bloch functions**:

$$
\psi_{n,k} (r) = \frac{1}{N} \sum_R e^{i k \cdot R} \phi_n (r - R),
$$

where:

- $n$ indexes the orbital type (e.g., $s$, $p$, or a combination).
- $k$ is the crystal wavevector within the first Brillouin zone.
- $N$ is the total number of lattice sites.

This **linear combination of atomic orbitals (LCAO)** ensures the periodic Bloch phase factor $e^{i k \cdot R}$.

In **tight-binding**, we assume each $\phi(r - R)$ is quite **localized** around its site $R$, so **overlap between distant sites is negligible**. Only **nearest (or maybe next-nearest) neighbor overlaps matter**.

---

# 3. Hamiltonian and Matrix Elements

## 3.1 General Form of the Electronic Hamiltonian

$$
\hat{H} = - \frac{\hbar^2}{2m} \nabla^2 + V_{\text{ion}}(r) + V_{\text{e-e}}[\rho(r)],
$$

where:

- $V_{\text{ion}}$ is the **ionic potential** arranged periodically.
- $V_{\text{e-e}}$ is the **electron-electron interaction** (often simplified in a mean-field or Hartree–Fock approach).

In the simplest tight-binding argument, one **lumps everything into an effective single-particle potential** $U_{\text{eff}}(r)$.

---

## 3.2 Matrix Elements in the Atomic Orbital Basis

Let $\phi_\alpha (r - R)$ represent an **orbital labeled by** $\alpha$ (could be $s$, $p_x$, etc.) on site $R$. We define the **on-site and off-site Hamiltonian matrix elements**:

### On-site energy:

$$
\varepsilon_\alpha = \langle R, \alpha | \hat{H} | R, \alpha \rangle
= \int d^3 r \, \phi_\alpha^* (r - R) \hat{H} \phi_\alpha (r - R).
$$

This is the **energy of orbital $\alpha$ if the electron remains on the same site $R$**.

### Hopping (transfer) integrals:

$$
t_{\alpha \beta} (R - R') = \langle R, \alpha | \hat{H} | R', \beta \rangle
= \int d^3 r \, \phi_\alpha^* (r - R) \hat{H} \phi_\beta (r - R').
$$

- If $R'$ is a **neighbor of $R$**, this integral might be **non-negligible**.
- Otherwise, it is often assumed **small or zero** in a simpler **nearest-neighbor tight-binding approach**.

---

# 4. Derivation of the Tight-Binding Band Structure

## 4.1 Constructing the Bloch State

We form a trial **Bloch wavefunction** as a **superposition of local orbitals**:

$$
\Psi_{\alpha, k} (r) = \frac{1}{N} \sum_R e^{i k \cdot R} \phi_\alpha (r - R).
$$

We want to solve the **time-independent Schrödinger equation**:

$$
\hat{H} \Psi_{\alpha, k} (r) = E_\alpha (k) \Psi_{\alpha, k} (r),
$$

for the **energy $E_\alpha (k)$**.

### 4.1.1 Single Orbital per Site (1D Example)

For clarity, let us do the simplest case:

- **1D chain** with lattice constant $a$.
- One orbital $\phi (r - n a)$ per site.
- **Nearest-neighbor hopping integral**: $-t$.
- **On-site energy**: $\varepsilon_0$.

Then:

$$
\Psi_k (x) = \frac{1}{N} \sum_{n=0}^{N-1} e^{i k n a} \phi (x - n a).
$$

Projecting $\hat{H}$ on $\langle m | \equiv \langle x | \phi (x - m a)$ and using orthogonality:

$$
\langle m | \hat{H} | \Psi_k \rangle = E(k) \langle m | \Psi_k \rangle = E(k) \frac{1}{N} e^{i k m a}.
$$

From the **TB integrals**:

$$
\langle m | \hat{H} | n \rangle =
\begin{cases}
\varepsilon_0, & m = n, \\
- t, & m = n \pm 1, \\
0, & |m - n| > 1.
\end{cases}
$$

Thus, solving for energy:

$$
E(k) = \varepsilon_0 + 2t \cos(ka).
$$

This is the **tight-binding dispersion for a 1D chain** with one orbital per site.

---

# 5. Why Tight-Binding? Key Insights and Advantages

- **Strong Lattice Potential**: Electrons are **not free** but bound to atoms.
- **Natural for Covalent Solids**: Useful for semiconductors, transition metals.
- **Easily Extended**:
  - More neighbors (2nd, 3rd nearest neighbors).
  - Multi-orbital terms ($s$, $p$, $d$ orbitals).
  - **Graphene’s $\pi$-bands** arise from **nearest-neighbor hopping of $p_z$ orbitals**.

---

# 6. Final Tight-Binding Equations Recap

### Bloch Function in Atomic Basis

$$
\psi_{n,k} (r) = \frac{1}{N} \sum_{R,\alpha} c_{n,\alpha} (k) e^{i k \cdot R} \phi_\alpha (r - R).
$$

### Nearest-Neighbor 1D Single-Orbital Result

$$
E(k) = \varepsilon_0 + 2t \cos(ka).
$$

This is the **prototypical tight-binding band dispersion**.
