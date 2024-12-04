
# Deriving the Molecular Field Constant in Magnetism

## Abstract

Understanding the magnetic properties of materials requires bridging the gap between microscopic spin interactions and macroscopic magnetization. The molecular field theory, introduced by Pierre Weiss, provides a framework to explain ferromagnetism by considering an internal effective field acting on each magnetic moment. 

This document presents a detailed and rigorous derivation of the molecular field constant, starting from the Heisenberg exchange Hamiltonian and employing the mean field approximation. The derivation elucidates how the exchange interactions between atomic spins lead to spontaneous magnetization in ferromagnetic materials.

## Introduction

Ferromagnetism is a fundamental phenomenon where certain materials exhibit spontaneous magnetization even in the absence of an external magnetic field. This behavior arises from the alignment of atomic magnetic moments (spins) due to quantum mechanical exchange interactions. 

To model this effect, we use the Heisenberg exchange Hamiltonian, which describes the interactions between spins in a lattice. However, solving the Heisenberg model exactly is challenging due to the many-body nature of the problem.

To simplify the analysis, the mean field approximation (also known as the molecular field approximation) is employed. This approximation replaces the complex interactions of each spin with all others by an average or "molecular" field. The strength of this field is characterized by the molecular field constant ($\lambda$), which we aim to derive rigorously in this document.

---

## The Heisenberg Exchange Hamiltonian

The starting point of our derivation is the Heisenberg exchange Hamiltonian:

$$
\hat{H} = -2 \sum_{\langle i,j \rangle} J_{ij} \hat{S}_i \cdot \hat{S}_j
$$

Here:
- $\hat{H}$ is the Hamiltonian operator.
- $\hat{S}_i$ and $\hat{S}_j$ are the spin operators at lattice sites $i$ and $j$, respectively.
- $J_{ij}$ is the exchange integral between spins $i$ and $j$.
- The factor of 2 ensures that each pair is counted once (since the sum over $i$ and $j$ counts each pair twice).

The exchange integral $J_{ij}$ determines the nature of the interaction:
- $J_{ij} > 0$: Ferromagnetic interaction (spins prefer to align).
- $J_{ij} < 0$: Antiferromagnetic interaction (spins prefer to anti-align).

---

## Mean Field Approximation

### Separating Spin Operators

To make the problem tractable, we apply the mean field approximation, which assumes each spin interacts with an average field. We express each spin operator as the sum of its average value and a fluctuation:

$$
\hat{S}_i = \langle \hat{S}_i \rangle + \delta \hat{S}_i
$$

where:
- $\langle \hat{S}_i \rangle$ is the expectation value (average spin) at site $i$.
- $\delta \hat{S}_i$ represents fluctuations around the average.

### Approximating the Spin-Spin Interaction

The product of two spin operators becomes:

$$
\hat{S}_i \cdot \hat{S}_j = \langle \hat{S}_i \rangle \cdot \langle \hat{S}_j \rangle + \langle \hat{S}_i \rangle \cdot \delta \hat{S}_j + \delta \hat{S}_i \cdot \langle \hat{S}_j \rangle + \delta \hat{S}_i \cdot \delta \hat{S}_j
$$

In the mean field approximation, we neglect the term $\delta \hat{S}_i \cdot \delta \hat{S}_j$ since fluctuations are small. Simplifying:

$$
\hat{S}_i \cdot \hat{S}_j \approx \langle \hat{S}_i \rangle \cdot \hat{S}_j + \hat{S}_i \cdot \langle \hat{S}_j \rangle - \langle \hat{S}_i \rangle \cdot \langle \hat{S}_j \rangle
$$

For ferromagnetic materials in the absence of external fields, the average spins are the same at all sites ($\langle \hat{S}_i \rangle = \langle \hat{S} \rangle$). Thus:

$$
\hat{S}_i \cdot \hat{S}_j \approx 2 \langle \hat{S} \rangle \cdot \hat{S}_i - \langle \hat{S} \rangle \cdot \langle \hat{S} \rangle
$$

---

## Effective Hamiltonian

Substituting the mean field approximation into the Hamiltonian:

$$
\hat{H} \approx -2 \sum_{\langle i,j \rangle} J_{ij} \langle \hat{S} \rangle \cdot \hat{S}_i
$$

We define the effective exchange constant $J_{\text{eff}}$:

$$
J_{\text{eff}} = \sum_j J_{ij} \quad (\text{over neighbors of site } i)
$$

Assuming $J_{ij} = J$ for nearest neighbors and the lattice coordination number is $z$:

$$
J_{\text{eff}} = zJ
$$

Thus, the Hamiltonian simplifies to:

$$
\hat{H} \approx -2 J_{\text{eff}} \langle \hat{S} \rangle \cdot \sum_i \hat{S}_i
$$

---

## Defining the Molecular Field

The interaction of a magnetic moment $\mu_i$ with a magnetic field $\mathbf{H}$ is given by:

$$
\hat{H}_{\text{mag}} = -\sum_i \mu_i \cdot \mathbf{H}
$$

For electron spins:

$$
\mu_i = -g \mu_B \hat{S}_i
$$

Substituting, we define an effective molecular field $\mathbf{H}_{\text{mol}}$ such that:

![image](https://github.com/user-attachments/assets/14c91274-76b4-4926-a041-39e8860ff72c)


---

## Molecular Field Constant $\lambda$

By definition, the molecular field can be expressed as:

$$
\mathbf{H}_{\text{mol}} = \lambda \mathbf{M}
$$

Relating spin expectation value to magnetization $M$:

$$
M = n \langle \mu \rangle = -n g \mu_B \langle \hat{S} \rangle
$$

Substituting:

$$
\lambda = \frac{2 z J}{n (g \mu_B)^2}
$$

---

## Summary

Starting from the Heisenberg exchange Hamiltonian:
1. We applied the mean field approximation to simplify spin-spin interactions.
2. Derived an effective Hamiltonian.
3. Related the molecular field to the magnetization.
4. Expressed the molecular field constant $\lambda$ in terms of physical parameters.

---

## Implications of $\lambda$

- **Curie Temperature ($T_C$):**

$$
k_B T_C = \frac{2}{3} \lambda (g \mu_B S)^2
$$

- **Magnetic Susceptibility ($\chi$):**

$$
\chi = \frac{C}{T - T_C}
$$

where $C$ is the Curie constant.

This derivation highlights the central role of $\lambda$ in ferromagnetic behavior.

This derivation not only reinforces the theoretical foundation of ferromagnetism but also provides valuable insights for exploring magnetic materials and designing applications in data storage, spintronics, and quantum computing.

Understanding the molecular field constant enables physicists and material scientists to predict and manipulate the magnetic behavior of materials, paving the way for technological advancements that leverage magnetic phenomena.
