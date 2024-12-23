# 1. Cooper Pair Formation: The Essence

One of Leon Cooper’s great insights was to show that even a tiny attractive interaction between electrons near the Fermi surface can lead to a bound state of two electrons (now called a “Cooper pair”). This result underpins the BCS (Bardeen–Cooper–Schrieffer) theory of superconductivity.

---

## 1.1 Simplified Picture of the Problem

### Start with a Filled Fermi Sea
Imagine a metal at $T=0$ where all single-particle states with energies $E_k \leq E_F$ are occupied (the Fermi sea). Here $E_F$ is the Fermi energy, and $k$ is the momentum quantum number.

### Add Two Extra Electrons
Now consider placing two extra electrons (with spin up and spin down) into unoccupied states just above the Fermi surface (i.e., states with $E_k > E_F$). Ordinarily, you might think these electrons would simply stay unbound.

### Introduce a Weak Attraction
Suppose there is a small but attractive interaction $-\lvert V \rvert$ (where $\lvert V \rvert$ is a positive constant) that only acts for electrons near the Fermi surface (i.e., for $\lvert E_k - E_F \rvert \leq \hbar \omega_D$, where $\omega_D$ is on the order of the Debye frequency).

#### Question: Does this tiny attraction matter?

Cooper’s calculation shows that any nonzero attractive interaction can create a bound state of these two electrons, lowering the energy below $2 E_F$. This pair is then referred to as a “Cooper pair.”

---

## 1.2 The Reduced Hamiltonian for Cooper’s Problem

To see why, let’s write down (in second-quantized form) a reduced Hamiltonian that focuses on electrons near the Fermi surface:

$$
\hat{H} = \sum_{k, \sigma} \epsilon_k c_{k, \sigma}^\dagger c_{k, \sigma} + \sum_{k, k'} V_{k, k'} c_{k', \uparrow}^\dagger c_{-k', \downarrow}^\dagger c_{-k, \downarrow} c_{k, \uparrow}.
$$

Here:
- $\epsilon_k \equiv E_k - E_F$ is the energy relative to the Fermi level.
- $c_{k, \sigma}^\dagger$ and $c_{k, \sigma}$ are fermionic creation and annihilation operators.
- $V_{k, k'}$ is negative (attractive) and nonzero only for $k, k'$ near $\lvert k \rvert = k_F$.

### Trial Wavefunction for Two Electrons
Consider a trial wavefunction for two electrons in the $(k, -k)$ pair state:

$$
\lvert \Psi_{\text{pair}} \rangle = \sum_k \alpha_k c_{k, \uparrow}^\dagger c_{-k, \downarrow}^\dagger \lvert \text{Fermi sea} \rangle.
$$

Here, $\alpha_k$ are coefficients to be determined. The Fermi sea (all states with $\lvert k \rvert < k_F$ filled) serves as the base, and we add two electrons in states above the Fermi surface.

---

## 1.3 The Bound State Solution (Sketch)

By plugging $\lvert \Psi_{\text{pair}} \rangle$ into the Schrödinger equation $\hat{H} \lvert \Psi_{\text{pair}} \rangle = E \lvert \Psi_{\text{pair}} \rangle$, one obtains an integral equation for $\alpha_k$. Cooper found:

- A bound-state energy $E = 2 \epsilon_F + \delta E$, with $\delta E < 0$.
- The magnitude of $\delta E$ is exponentially small in $1 / \lvert V \rvert \rho(\epsilon_F)$, where $\rho(\epsilon_F)$ is the density of states at the Fermi level. Formally:
- 
$$
\lvert \delta E \rvert \sim \hbar \omega_D \exp\left(-\frac{1}{\lvert V \rvert \rho(E_F)}\right).
$$
  
- The wavefunction in momentum space, $\alpha_k$, peaks around the Fermi surface, ensuring $k \approx k_F$ and $-k \approx -k_F$.

This singular result—that any attractive potential leads to a bound pair—opened the door for BCS to propose that all electrons near the Fermi surface can form such pairs simultaneously.

---

## 1.4 Physical Interpretation

1. **Spin-1/2 Electrons**: The pair formed is typically a singlet state ($\uparrow \downarrow - \downarrow \uparrow$) with total spin zero.
2. **Momentum**: Because the two electrons have momenta $k$ and $-k$, the total momentum of the pair is zero.
3. **Why a Sea of Pairs?**: In a real superconductor, you don’t just have one pair. You have a macroscopic number of these pairs forming a collective condensate (the BCS ground state).

This is the **essence** of Cooper pairing: a small attraction, restricted to states near the Fermi surface, results in a bound pair with a small (but nonzero) binding energy. BCS builds upon this to show that all such pairs can condense, causing superconductivity.

---

# 2. The BCS Hamiltonian

Once Cooper showed that two electrons form a bound pair, the next question Bardeen, Cooper, and Schrieffer asked was: How do we describe the entire many-electron system in which numerous pairs form at once?

---

## 2.1 Reduced (Model) Hamiltonian for Pairing

The BCS approach proposes a reduced Hamiltonian that focuses on the pairing of electrons with opposite momenta and spins near the Fermi surface:

![image](https://github.com/user-attachments/assets/dd6ef1c4-1a90-49fe-b95d-7e8ec863c732)


### Key Points

- $\epsilon_k \equiv E_k - \mu$ is the single-particle energy measured from the chemical potential $\mu$. Near the Fermi surface, $\mu \approx E_F$.
- The interaction is attractive ($-\lvert V \rvert$), but only when both $k$ and $k'$ lie in a thin shell (width $\hbar \omega_D$) around the Fermi energy.
- The sum over spins $\uparrow, \downarrow$ is crucial—pairs form between electrons with opposite spins.

---

## 2.2 Why Does This Hamiltonian Produce Superconductivity?

1. **Pauli Principle**: Electrons are fermions, so they fill available states. However, once pairing sets in, it becomes energetically favorable to “pair up” opposite-momentum states.
2. **Energy Gap**: Forming a pair lowers the total energy, so excitations of the system (which break pairs) cost a finite amount of energy—the energy gap.
3. **Macroscopic Occupation**: Instead of a normal Fermi sea, many electrons “condense” into the same pairing state (or a coherent superposition of pair states), leading to a superconducting ground state.

---

## 2.4 Mathematical Summary of the BCS Hamiltonian

### Kinetic / Free Part:

![image](https://github.com/user-attachments/assets/7fbb9536-2a64-498d-9179-9fb54676d612)


### Interaction Part (Reduced):

![image](https://github.com/user-attachments/assets/10cb9d76-30b6-472e-b974-bc1b2a2d0156)

### Total Hamiltonian:

![image](https://github.com/user-attachments/assets/6c65909c-e770-4ec9-8bdd-6f27d1f0a536)


**Core Idea**: The negative sign ($-\lvert V \rvert$) ensures an attractive interaction for opposite-spin electrons with momenta $k, -k$. Under the right conditions (low $T$, near the Fermi level), this leads to a paired superconducting ground state.

In the full BCS theory, a mean-field approximation introduces the “gap function” $\Delta$, resulting in a diagonalized Hamiltonian with “Bogoliubov quasiparticles.” These quasiparticles explain:

- The superconducting gap $\Delta$,
- An exponential formula for $T_c$,
- Zero electrical resistance and the Meissner effect, as each Cooper pair contributes to the supercurrent.

# 3. The Bardeen–Cooper–Schrieffer (BCS) Ground State

The core of the BCS approach is that the superconducting ground state is a coherent superposition of electron pairs $(k, -k)$. This section explains how that ansatz arises and what it implies about the electron distribution.

---

## 3.1 Why a Paired Ground State?

From Cooper’s analysis, we know that any attractive interaction between electrons near the Fermi surface can lead to bound pairs (Cooper pairs). In a real superconductor at sufficiently low temperature, we expect many pairs to form simultaneously, condensing into a new ground state with lower energy than the unpaired Fermi sea.

---

## 3.2 BCS Variational Ansatz

To describe a many-electron system in which each momentum state $k$ (above the Fermi surface) could be paired with its partner $-k$, Bardeen, Cooper, and Schrieffer proposed the following variational wavefunction (in second quantization):

$$
\lvert \Psi_{\text{BCS}} \rangle = \prod_k (u_k + v_k c_{k, \uparrow}^\dagger c_{-k, \downarrow}^\dagger) \lvert 0 \rangle.
$$

Here:
- $c_{k, \sigma}^\dagger$ is the fermionic creation operator that creates an electron in momentum state $k$ with spin $\sigma \in \{\uparrow, \downarrow\}$.
- $\lvert 0 \rangle$ is the vacuum state (i.e., no electrons).  
  Strictly, in some treatments, $\lvert 0 \rangle$ is replaced by a filled Fermi sea of lower-energy states, but for the sake of the pairing argument, one typically redefines the reference state so that “unoccupied” means “above the Fermi surface.”

---

### 3.2.1 The Coefficients $u_k, v_k$

These are variational parameters we must determine by minimizing the expectation value of the Hamiltonian $\hat{H}_{\text{BCS}}$. They must satisfy the normalization condition:

$$
\lvert u_k \rvert^2 + \lvert v_k \rvert^2 = 1.
$$

This ensures that the state $(u_k + v_k c_{k, \uparrow}^\dagger c_{-k, \downarrow}^\dagger) \lvert 0 \rangle$ is properly normalized for each $k$.

---

### 3.2.2 Physical Meaning of $v_k$

- $\lvert v_k \rvert^2$ is interpreted as the probability that the momentum state $k, \uparrow$ and $-k, \downarrow$ are both occupied (i.e., that a pair is present in these two states).
- Conversely, $\lvert u_k \rvert^2$ is the probability that neither of these states is occupied.

---

### 3.2.3 Simplifying Assumptions

- **Spin-Singlet Pairing**: We assume s-wave (isotropic) pairing with opposite spins in the pair. This ensures total spin $S = 0$.
- **Momentum $k$ Only**: The pair has total momentum zero, so the two electrons come in states $k$ and $-k$.

Thus, we see that the BCS ground state is a product over $k$ of “two-state systems,” each describing the occupation (paired vs. unoccupied) of $(k, -k)$. This is the simplest expression of a superconducting condensate.

---

# 4. Mean-Field BCS Equations

To derive the parameters $u_k$ and $v_k$ self-consistently, BCS invoked a **mean-field approximation**. This is often done in two main steps:

1. Rewrite the interaction (pairing) term in a simpler, bilinear form.
2. Define the pairing gap $\Delta$ and solve for it self-consistently.

Below, we show how these steps lead to the famous BCS gap equation and the quasiparticle energies.

---

## 4.1 The BCS (Reduced) Hamiltonian

The “reduced” BCS Hamiltonian can be written (in second quantization) as:

![image](https://github.com/user-attachments/assets/efcc87ae-4ace-4ef8-ae06-1358c3f7ba3e)


where:
- $\epsilon_k = E_k - \mu$ is the single-particle energy relative to the chemical potential $\mu$.
- The interaction $-\lvert V \rvert$ is effectively a constant only for $k$ within a thin shell $\pm \hbar \omega_D$ around the Fermi surface. Outside that shell, the interaction is zero.

---

## 4.2 Mean-Field Decomposition and the Gap $\Delta$

### 2.2.1 Pairing Operator

Define the pair creation operator:

![image](https://github.com/user-attachments/assets/74e2135c-c01d-4930-822d-006128c33ead)


The interaction term can be written as:

![image](https://github.com/user-attachments/assets/67114ebb-7dbe-4c72-8c1f-15e8a16e7647)


---

### 4.2.2 The Mean-Field Approximation

In the mean-field or Hartree–Fock–Bogoliubov approach, we replace the four-fermion term by an approximation:

![image](https://github.com/user-attachments/assets/81ae817d-6e77-442e-a06b-b86d60ae0b1b)


dropping the “fluctuation” part that is typically small in the superconducting state. Physically, we assume the operator $\hat{b}_k$ can be replaced by its expectation value plus small fluctuations.

---

### 4.2.3 Defining the Gap $\Delta$

We define the gap parameter $\Delta$ as:

$$
\Delta \equiv \lvert V \rvert \sum_{k'} \langle c_{-k', \downarrow} c_{k', \uparrow} \rangle = \lvert V \rvert \sum_{k'} \langle \hat{b}_{k'} \rangle.
$$

Because pairing is strongest for $\lvert \epsilon_k \rvert < \hbar \omega_D$ near the Fermi surface, the sum is restricted to that region. In practice, BCS often treats $\Delta$ as a constant (independent of $k$), at least for s-wave superconductors.

---

### 4.4 Self-Consistency: The Gap Equation

Substituting the forms for $u_k^2$ and $v_k^2$ into the sum yields (at $T = 0$):

$$
\Delta = \lvert V \rvert \sum_k \frac{\Delta}{\sqrt{\epsilon_k^2 + \Delta^2}} \quad \Rightarrow \quad 1 = \lvert V \rvert \rho(\epsilon_F) \int_0^{\hbar \omega_D} \frac{d\xi}{\sqrt{\xi^2 + \Delta^2}}.
$$

Solving gives:

$$
\Delta \approx 2 \hbar \omega_D \exp\left(-\frac{1}{\lvert V \rvert \rho(\epsilon_F)}\right),
$$

which is exponentially small in $1 / \lvert V \rvert \rho(\epsilon_F)$.

# 5. Finite Temperature: The BCS Gap and $T_c$

---

## 5.1 Thermal Occupations

At finite temperature $T$, the average occupation of Bogoliubov quasiparticles is governed by the Fermi–Dirac distribution:

$$
\langle \gamma_{k, \sigma}^\dagger \gamma_{k, \sigma} \rangle = f(E_k) = \frac{1}{\exp(E_k / k_B T) + 1}.
$$

Hence, the gap equation becomes:

$$
1 = \lvert V \rvert \sum_k \frac{1 - 2 f(E_k)}{2 E_k}.
$$

In the continuum limit:

$$
1 = \lvert V \rvert \rho(\epsilon_F) \int_0^{\hbar \omega_D} \frac{d\xi}{\sqrt{\xi^2 + \Delta^2(T)}} \left[ 1 - 2 f\left(\sqrt{\xi^2 + \Delta^2(T)}\right) \right].
$$

---

## 5.2 Critical Temperature $T_c$

At the critical temperature, $\Delta \to 0$. Thus:

$$
1 = \lvert V \rvert \rho(\epsilon_F) \int_0^{\hbar \omega_D} \frac{d\xi}{\xi} \left[ 1 - 2 f(\xi) \right]_{\Delta = 0}.
$$

Solving yields the approximate relation:

$$
k_B T_c \approx 1.13 \hbar \omega_D \exp\left(-\frac{1}{\lvert V \rvert \rho(\epsilon_F)}\right).
$$

Thus, BCS predicts:

$$
\Delta(0) = 1.76 k_B T_c,
$$

which matches experimental data for many conventional superconductors.

# BCS Ground State

A coherent superposition of paired states $(k, -k)$. The variational parameters $(u_k, v_k)$ represent the probability amplitudes for the pairs to be occupied vs. unoccupied.

---

## Mean-Field Hamiltonian

By introducing the gap $\Delta$, we decouple the four-fermion interaction into a simpler bilinear form. The resulting Hamiltonian describes Bogoliubov quasiparticles with an energy dispersion:

$$
E_k = \sqrt{\epsilon_k^2 + \Delta^2}.
$$

---

## Self-Consistency (Gap Equation)

The value of $\Delta$ must satisfy the integral equation that captures the net effect of pairing near the Fermi surface. The solution reveals an energy gap $\Delta$ that opens up at the Fermi level, explaining zero-resistance flow and other superconducting phenomena.

---

## Exponential Dependence

The gap (and the critical temperature $T_c$) depends on:

$$
\exp\left[-\frac{1}{\lvert V \rvert \rho(E_F)}\right],
$$

showcasing how even a small attractive potential can produce a robust pairing phenomenon.

---

## Final Remarks

1. The **BCS wavefunction** is an elegant way of capturing the essential “all-electrons-paired” picture.
2. The **mean-field approach** allows a complicated many-body problem (four-fermion interactions) to be simplified to a problem of noninteracting quasiparticles—a standard tactic in condensed matter (akin to Hartree-Fock in atomic physics).
3. The **gap equation** not only predicts the magnitude of the superconducting energy gap but also the characteristic temperature $T_c$ at which superconductivity disappears.
4. Throughout, the key insight is that **Cooper pairing** (first argued by Leon Cooper for two electrons) extends to a macroscopic scale in superconductors: a huge number of electrons near the Fermi surface form a condensate of pairs with zero net momentum, leading to superconductivity.


