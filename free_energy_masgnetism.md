# First Principles and Landau-Lifshitz Equation

We will start from first principles, proceed through the definitions of each energy contribution, and ultimately show how these lead to the effective magnetic field and, finally, the Landau-Lifshitz equation of motion for the magnetization.

## 1. Context and Motivation

Magnetic materials organize their magnetization $M(r, t)$ into domains to minimize their total free energy. The equilibrium domain structure and the response of the magnetization to external perturbations (applied fields, stress, etc.) can be understood by considering a free-energy functional. The Landau-Lifshitz free energy functional is a cornerstone of micromagnetic theory, combining several energy contributions that depend on the spatial distribution of $M$.

At equilibrium, the magnetization configuration is one that minimizes the total free energy. For dynamical processes, the time evolution of $M$ can be described by a Landau-Lifshitz-type equation of motion derived from the same free-energy functional. Thus, understanding the free energy is essential for both static and dynamic properties of magnetic materials.

## 2. Landau-Lifshitz Free-Energy Functional

The total free energy $E$ of a ferromagnetic body can typically be expressed as:

$$
E = E_{ex} + E_D + E_\lambda + E_k + E_H,
$$

where each term represents a different physical interaction or contribution:

- **Exchange Energy**: $E_{ex}$
- **Magnetostatic (Demagnetizing) Energy**: $E_D$
- **Magnetoelastic (Magnetostrictive) Energy**: $E_\lambda$
- **Magnetocrystalline Anisotropy Energy**: $E_k$
- **Zeeman Energy**: $E_H$

We will detail each term below.

### 2.1 Exchange Energy ($E_{ex}$)

**Physical Origin**:  
Exchange interactions arise from the quantum mechanical overlap of electronic wavefunctions, favoring parallel alignment of neighboring spins. This leads to a tendency for the magnetization to be spatially uniform, avoiding abrupt changes in direction.

**Typical Form**:  
For a continuous medium, the exchange energy is written as an integral over the volume $V$:

$$
E_{ex} = \int_V d^3r \, A (\nabla m)^2,
$$

where $m(r) = M(r)/M_s$ is the normalized magnetization ($M_s = |M|$ at saturation), and $A$ is the exchange stiffness (a positive constant with units of J/m).

Expanding $(\nabla m)^2 = (\partial_x m_x)^2 + (\partial_x m_y)^2 + \dots$ for all spatial derivatives and vector components, this term penalizes spatial inhomogeneities in $m$. Minimizing $E_{ex}$ alone would favor a uniform magnetization state.

### 2.2 Magnetostatic (Demagnetizing) Energy ($E_D$)

**Physical Origin**:  
A magnetized body creates a magnetic field in space. The portion of this field outside the body is called the stray field, and it costs energy. The system can lower this energy by forming domains that reduce free magnetic poles on surfaces.

**Formulation**:  
If $H_{demag}$ is the demagnetizing field produced by the magnetization distribution $M$, the magnetostatic energy is:

$$
E_D = \frac{\mu_0}{2} \int_V d^3r \, M \cdot H_{demag}.
$$

Here, $H_{demag}$ is determined by the magnetostatic Maxwell’s equations:

$$
\nabla \cdot B = 0, \quad \nabla \times H = 0, \quad \text{and } B = \mu_0 (H + M).
$$

From these, one finds $H_{demag} = -\nabla \Phi$, where $\Phi$ is a scalar potential satisfying:

$$
\nabla^2 \Phi = \nabla \cdot M.
$$

Solving this Poisson equation for a given $M$ gives the demagnetizing field, and thus the magnetostatic energy.

### 2.3 Magnetoelastic Energy ($E_\lambda$)

**Physical Origin**:  
Magnetostriction couples the material’s elastic strain to its magnetization. When magnetized, a crystal may deform slightly, and conversely, external stress can influence the preferred magnetization direction.

**Typical Form**:  
For small strains $\epsilon_{ij}$, the magnetoelastic energy density can be expanded as:

$$
f_\lambda \sim B_1 \sum_i \epsilon_{ii} m_i^2 + B_2 (\epsilon_{xy} m_x m_y + \dots),
$$

where $B_1, B_2$ are magnetoelastic coupling constants and $m_i$ are the direction cosines of $m$. The total energy is:

$$
E_\lambda = \int_V d^3r \, f_\lambda(m, \epsilon_{ij}).
$$

Minimizing $E_\lambda$ favors magnetization directions that best relieve or accommodate internal stresses.

### 2.4 Magnetocrystalline Anisotropy Energy ($E_k$)

**Physical Origin**:  
Crystalline symmetry leads to certain directions (easy axes) along which it is energetically easier to magnetize. This arises from spin-orbit coupling and the symmetry of the crystal lattice.

**Common Simplified Form**:  
For a uniaxial crystal with an easy axis along $\hat{z}$, the anisotropy energy density can be written as:

$$
f_k = K \sin^2 \theta,
$$

where $K$ is the anisotropy constant, and $\theta$ is the angle between $m$ and the easy axis. The total anisotropy energy is:

$$
E_k = \int_V d^3r \, f_k(m).
$$

Minimization of $E_k$ favors magnetization along specified crystallographic directions.

### 2.5 Zeeman Energy ($E_H$)

**Physical Origin**:  
An external magnetic field $H_{ext}$ applied to the system interacts with the magnetization, lowering the energy when $M$ aligns with $H_{ext}$.

**Form**:  
The Zeeman energy is:

$$
E_H = -\mu_0 \int_V d^3r \, M \cdot H_{ext}.
$$

This term is minimized by orienting $M$ parallel to $H_{ext}$.

## 3. Total Energy and Variational Principle

The total energy is:

$$
E[M] = \int_V d^3r \, \left[ A (\nabla m)^2 + f_D(M) + f_\lambda(m, \epsilon_{ij}) + f_k(m) - \mu_0 M \cdot H_{ext} \right].
$$

At equilibrium, the magnetization configuration $M(r)$ satisfies:

$$
\frac{\delta E}{\delta M} = 0.
$$

This condition ensures a stationary point of the energy functional.

**Effective Field**:  
The effective magnetic field $H_{eff}$ is defined as:

$$
H_{eff}(r) = -\frac{\delta E}{\delta M(r)}.
$$

The equilibrium condition becomes $H_{eff} = 0$ in the absence of dynamics.

## 4. Landau-Lifshitz Equation of Motion

The Landau-Lifshitz equation describes the temporal evolution of $M$:

$$
\frac{dM}{dt} = -\gamma M \times H_{eff},
$$

where $\gamma$ is the gyromagnetic ratio.

**With Damping**:  
Including dissipation, the equation becomes:

$$
\frac{dM}{dt} = -\gamma M \times H_{eff} - \frac{\lambda}{M_s} M \times (M \times H_{eff}),
$$

where $\lambda$ is the damping parameter.

**Landau-Lifshitz-Gilbert Form**:

$$
\frac{dM}{dt} = -\gamma M \times H_{eff} + \frac{\alpha}{M_s} \left( M \times \frac{dM}{dt} \right),
$$

where $\alpha$ is the Gilbert damping constant.

## 5. Summary and Physical Insight

- The Landau-Lifshitz free energy governs magnetization configurations, combining exchange, magnetostatic, magnetoelastic, anisotropy, and Zeeman contributions.
- Minimization of this energy yields equilibrium domain structures and magnetization patterns.
- The Landau-Lifshitz equation and its Gilbert extension describe magnetization dynamics.
