# Landau-Lifshitz Theory: Free Energy and Dynamics

## Introduction

We will start from first principles, proceed through the definitions of each energy contribution, and ultimately show how these lead to the effective magnetic field and, finally, the Landau-Lifshitz equation of motion for the magnetization.

---

### 1. Context and Motivation

Magnetic materials organize their magnetization $\mathbf{M}(\mathbf{r}, t)$ into domains to minimize their total free energy. The equilibrium domain structure and the response of the magnetization to external perturbations (applied fields, stress, etc.) can be understood by considering a free-energy functional. The Landau-Lifshitz free energy functional is a cornerstone of micromagnetic theory, combining several energy contributions that depend on the spatial distribution of $\mathbf{M}$.

- **Equilibrium**: The magnetization configuration minimizes the total free energy.
- **Dynamics**: The time evolution of $\mathbf{M}$ can be described by a Landau-Lifshitz-type equation derived from the free-energy functional.

---

### 2. Landau-Lifshitz Free-Energy Functional

The total free energy $E$ of a ferromagnetic body is typically expressed as:

$$
E = E_\text{ex} + E_D + E_\lambda + E_k + E_H,
$$

where:

- $E_\text{ex}$: Exchange energy
- $E_D$: Magnetostatic (demagnetizing) energy
- $E_\lambda$: Magnetoelastic (magnetostrictive) energy
- $E_k$: Magnetocrystalline anisotropy energy
- $E_H$: Zeeman energy

We will detail each term below.

---

### 2.1 Exchange Energy ($E_\text{ex}$)

**Physical Origin**:  
Exchange interactions arise from the quantum mechanical overlap of electronic wavefunctions, favoring parallel alignment of neighboring spins.

**Form**:  
For a continuous medium, the exchange energy is:

$$
E_\text{ex} = \int_V d^3r \, A (\nabla \mathbf{m})^2,
$$

where:

- $\mathbf{m}(\mathbf{r}) = \mathbf{M}(\mathbf{r}) / M_s$ is the normalized magnetization.
- $A$ is the exchange stiffness (units: J/m).
- $(\nabla \mathbf{m})^2 = (\partial_x m_x)^2 + (\partial_y m_y)^2 + \dots$ penalizes spatial inhomogeneities.

---

### 2.2 Magnetostatic (Demagnetizing) Energy ($E_D$)

**Physical Origin**:  
A magnetized body creates a stray field outside its volume, costing energy. This energy can be reduced by forming domains.

**Form**:  
If $\mathbf{H}_\text{demag}$ is the demagnetizing field produced by the magnetization $\mathbf{M}$, the magnetostatic energy is:

$$
E_D = \frac{\mu_0}{2} \int_V d^3r \, \mathbf{M} \cdot \mathbf{H}_\text{demag}.
$$

**Field Calculation**:  
$\mathbf{H}_\text{demag}$ satisfies Maxwell’s equations:

$$
\nabla \cdot \mathbf{B} = 0, \quad \nabla \times \mathbf{H} = 0, \quad \mathbf{B} = \mu_0 (\mathbf{H} + \mathbf{M}),
$$

leading to:

$$
\mathbf{H}_\text{demag} = -\nabla \Phi, \quad \nabla^2 \Phi = \nabla \cdot \mathbf{M}.
$$

Solving for $\Phi$ provides $\mathbf{H}_\text{demag}$ and hence $E_D$.

---

### 2.3 Magnetoelastic Energy ($E_\lambda$)

**Physical Origin**:  
Magnetostriction couples a material's strain to its magnetization.

**Form**:  
The magnetoelastic energy density is:

$$
f_\lambda \sim B_1 \sum_i \epsilon_{ii} m_i^2 + B_2 (\epsilon_{xy} m_x m_y + \dots),
$$

where:

- $B_1, B_2$: Magnetoelastic coupling constants.
- $\epsilon_{ij}$: Strain tensor components.
- $\mathbf{m}$: Magnetization direction cosines.

The total magnetoelastic energy is:

$$
E_\lambda = \int_V d^3r \, f_\lambda(\mathbf{m}, \epsilon_{ij}).
$$

---

### 2.4 Magnetocrystalline Anisotropy Energy ($E_k$)

**Physical Origin**:  
Crystal symmetry introduces easy magnetization axes due to spin-orbit coupling.

**Form**:  
For a uniaxial crystal:

$$
f_k = K \sin^2 \theta,
$$

where:

- $K$: Anisotropy constant.
- $\theta$: Angle between $\mathbf{m}$ and the easy axis.

The total anisotropy energy is:

$$
E_k = \int_V d^3r \, f_k(\mathbf{m}).
$$

---

### 2.5 Zeeman Energy ($E_H$)

**Physical Origin**:  
An external magnetic field $\mathbf{H}_\text{ext}$ interacts with $\mathbf{M}$, favoring alignment.

**Form**:  
The Zeeman energy is:

$$
E_H = -\mu_0 \int_V d^3r \, \mathbf{M} \cdot \mathbf{H}_\text{ext}.
$$

---

### 3. Total Energy and Variational Principle

The total energy functional is:

$$
E[\mathbf{M}] = \int_V d^3r \, \left[ A (\nabla \mathbf{m})^2 + f_D(\mathbf{M}) + f_\lambda(\mathbf{m}, \epsilon_{ij}) + f_k(\mathbf{m}) - \mu_0 \mathbf{M} \cdot \mathbf{H}_\text{ext} \right].
$$

**Equilibrium Condition**:  
At equilibrium, the magnetization configuration $\mathbf{M}(\mathbf{r})$ satisfies:

$$
\frac{\delta E}{\delta \mathbf{M}} = 0.
$$

**Effective Field**:  
Define the effective field $\mathbf{H}_\text{eff}$ as:

$$
\mathbf{H}_\text{eff}(\mathbf{r}) = -\frac{\delta E}{\delta \mathbf{M}(\mathbf{r})}.
$$

---

### 4. Landau-Lifshitz Equation of Motion

The Landau-Lifshitz equation describes the time evolution of $\mathbf{M}$:

$$
\frac{d\mathbf{M}}{dt} = -\gamma \mathbf{M} \times \mathbf{H}_\text{eff},
$$

where $\gamma$ is the gyromagnetic ratio. Introducing damping, the equation becomes:

$$
\frac{d\mathbf{M}}{dt} = -\gamma \mathbf{M} \times \mathbf{H}_\text{eff} - \frac{\lambda}{M_s} \mathbf{M} \times (\mathbf{M} \times \mathbf{H}_\text{eff}),
$$

where $\lambda$ is the damping parameter.

---

### 5. Summary and Physical Insight

- **Energy Contributions**: Exchange, magnetostatic, magnetoelastic, anisotropy, and Zeeman terms shape the total free energy.
- **Effective Field**: Derived from functional derivatives of the free energy.
- **Dynamics**: Governed by the Landau-Lifshitz equation, incorporating precession and damping.

This framework underpins micromagnetic theory, explaining domain structures, spin-wave dynamics, and magnetization switching in magnetic materials.
