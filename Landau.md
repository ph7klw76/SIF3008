# Introduction to Landau Theory

Landau theory, developed by Lev Davidovich Landau, is a powerful theoretical framework used to describe phase transitions in physical systems. It provides a macroscopic view of how the free energy of a system changes as it undergoes a transition from one phase to another, such as from a liquid to a solid, or a paramagnetic to a ferromagnetic state. Central to Landau theory is the concept of the order parameter, a quantity that measures the degree of order in a system and changes during a phase transition.

## The Order Parameter

In Landau theory, the order parameter $\eta$ quantifies the degree of symmetry breaking during a phase transition:

- For example, in a ferromagnetic system, the order parameter is the magnetization $M$, which is zero in the high-temperature paramagnetic phase and non-zero in the low-temperature ferromagnetic phase.
- For ferroelectric, the order parameter is the polarization. 
- For a liquid-solid transition, the order parameter represents the density difference between the liquid and the solid phases.

The order parameter is zero in the symmetric phase (high symmetry) and non-zero in the symmetry-broken phase (low symmetry).

## The Landau Free Energy Expansion

The key idea behind Landau theory is that near a phase transition, the free energy $F$ of a system can be expressed as a power series expansion in terms of the order parameter $\eta$:

$$
F(\eta, T) = F_0(T) + \frac{1}{2}a(T)\eta^2 + \frac{1}{4}b\eta^4 + \frac{1}{6}c\eta^6 + \cdots
$$

- $F_0(T)$ is the free energy of the reference state (symmetric phase).
- $a(T)$, $b$, and $c$ are coefficients that depend on temperature and material properties.
- The terms in the expansion represent different contributions to the free energy as a function of the order parameter.

## Temperature Dependence of the Coefficients

The coefficient $a(T)$ is assumed to vary linearly with temperature, often expressed as:

$$
a(T) = a_0 (T - T_c)
$$

where:

- $a_0 > 0$ is a constant.
- $T_c$ is the critical temperature at which the phase transition occurs.

The coefficients $b$ and $c$ are typically assumed to be temperature-independent, with $b > 0$ to ensure stability of the free energy at large values of $\eta$. Higher-order terms (like $c\eta^6$) are included when necessary to maintain stability or to describe more complex transitions.

## Minimization of the Free Energy

To determine the equilibrium state of the system, we find the value of the order parameter $\eta$ that minimizes the free energy $F$. This involves taking the derivative of $F$ with respect to $\eta$ and setting it to zero:

$$
\frac{\partial F}{\partial \eta} = 0
$$

Substituting the Landau expansion, we obtain:

$$
a(T)\eta + b\eta^3 + c\eta^5 = 0
$$

## Types of Phase Transitions

### Second-Order (Continuous) Phase Transitions

For a second-order phase transition:

- The order parameter changes continuously from zero as $T$ decreases below $T_c$.
- $b > 0$ ensures that the transition is smooth, with no discontinuous jump in the order parameter.

Solving the minimization condition:

$$
\eta = 0 \quad \text{(symmetric phase)}
$$

$$
a(T) + b\eta^2 = 0
$$

For $T < T_c$, we find:

$$
\eta^2 = -\frac{a(T)}{b} \Rightarrow \eta = \pm \sqrt{-\frac{a(T)}{b}}
$$

Thus, the order parameter grows continuously as the temperature is lowered below $T_c$, indicating a second-order transition.

### First-Order (Discontinuous) Phase Transitions

For a first-order transition:

- $b < 0$ and $c > 0$ (the next higher-order term is needed to stabilize the free energy).
- The free energy exhibits a double-well structure, leading to a discontinuous jump in the order parameter.

Minimizing the free energy involves finding solutions that correspond to local minima and examining their stability. A first-order transition occurs when the free energy of one minimum becomes lower than the other, causing a discontinuous change in the order parameter.

## Limitations of Landau Theory

While Landau theory provides deep insights into phase transitions, it has limitations:

- It assumes a mean-field approximation, neglecting fluctuations that can be significant near critical points.
- It may not accurately describe systems with strong coupling or long-range interactions without modifications.

Despite these limitations, Landau theory remains a cornerstone of condensed matter physics and continues to offer valuable insights into the behavior of complex systems.

# Introduction to Ferroelectricity and Landau Theory

Ferroelectric materials are characterized by a spontaneous electric polarization that can be reversed by applying an external electric field. These materials undergo a phase transition from a high-temperature paraelectric phase (where polarization is zero) to a low-temperature ferroelectric phase (where spontaneous polarization appears). Landau theory, which describes phase transitions through an order parameter, offers a robust framework to understand and analyze the behavior of ferroelectrics near the phase transition temperature.

## The Order Parameter in Ferroelectric Materials


![image](https://github.com/user-attachments/assets/0ee1d8fb-2192-407c-b76b-ceca51a78449)


In the context of ferroelectrics, the order parameter is a physical quantity that characterizes the degree of order in the system. For ferroelectric materials, the order parameter is typically chosen as the macroscopic polarization $P$. Above the transition temperature $T_c$ (in the paraelectric phase), the order parameter $P$ is zero, while below $T_c$, the system develops a nonzero spontaneous polarization.

The change in the order parameter is indicative of a phase transition and is key to describing the behavior of ferroelectric materials.

## Landau Theory and Free Energy Expansion

The Landau theory describes the free energy of a system near a phase transition using a power series expansion in terms of the order parameter $P$:

$$
F(P) = F_0 + \frac{1}{2} \alpha P^2 + \frac{1}{4} \beta P^4 + \frac{1}{6} \gamma P^6 + \cdots - EP
$$

- $F_0$ is the free energy of the reference (paraelectric) state. Can be referenced as zero
- $\alpha, \beta, \gamma$ are temperature-dependent expansion coefficients.
- $E$ is the applied electric field.

The coefficient $\alpha$ varies with temperature as:

$$
\alpha = \alpha_0 (T - T_c)
$$

where:

- $\alpha_0 > 0$ is a constant.
- $T_c$ is the critical temperature at which the ferroelectric transition occurs.

The equilibrium state is determined by minimizing the free energy with respect to the polarization $P$, leading to:

$$
\frac{\partial F}{\partial P} = \alpha P + \beta P^3 + \gamma P^5 - E = 0
$$

## Phase Transitions in Ferroelectrics

### Second-Order Phase Transition

For a second-order (continuous) transition, $\beta > 0$ and higher-order terms can be neglected. The polarization smoothly develops below $T_c$, as indicated by:

$$
P^2 = -\frac{\alpha}{\beta} \quad \text{for} \, \alpha < 0
$$

This corresponds to a spontaneous symmetry-breaking transition, where the system moves from a symmetric paraelectric phase (zero polarization) to an asymmetric ferroelectric phase.

### First-Order Phase Transition

If $\beta < 0$ and $\gamma > 0$, the transition becomes first-order, characterized by a discontinuous jump in the polarization. In this case, the free energy develops a double-well structure below $T_c$, and the system exhibits a metastable state.

## Soft Mode Theory and Phonon Vibrations in Ferroelectrics

To understand the microscopic mechanism underlying the phase transition in ferroelectrics, we turn to soft mode theory and phonon vibrations.

### Phonons and Lattice Vibrations

Phonons are quantized modes of vibrations in a crystal lattice. In a ferroelectric material, the atoms can oscillate around their equilibrium positions, creating lattice vibrations that contribute to the material's dielectric properties.

# Ferroelectric Phase Transitions and Soft Modes

Ferroelectric phase transitions are often driven by changes in the lattice dynamics of the crystal structure. At the heart of this transition is the concept of a soft mode: a vibrational mode in the crystal lattice whose frequency decreases (softens) as the temperature approaches the critical temperature $T_c$. This softening of the vibrational mode is a key precursor to the emergence of a spontaneous polarization, leading to a ferroelectric state.

## Understanding Lattice Vibrations in Crystals

In any crystal, atoms are arranged in a periodic lattice and can undergo vibrational motion about their equilibrium positions. These vibrations can be described by phonons, which are quantized normal modes of the lattice vibrations. Phonons can be broadly categorized as:
- **Acoustic phonons**: Low-energy vibrations that propagate sound waves.
- **Optical phonons**: Higher-energy vibrations where atoms within the basis oscillate relative to each other.

## The Role of Optical Phonons in Ferroelectrics

In many ferroelectric materials, the relevant soft mode is an optical phonon mode, where atoms or ions within the unit cell move relative to each other, often in a manner that affects the dipole moment of the material. For example:
- In materials like BaTiO$_3$ (barium titanate) or PbTiO$_3$ (lead titanate), the ferroelectric phase transition is associated with the displacement of cations (e.g., Ti) relative to the surrounding anions (e.g., oxygen ions), creating a net electric polarization.

## Mechanism of Soft Mode Softening (second-order (continuous) phase transitions)

As the temperature of a ferroelectric material decreases and approaches the transition temperature $T_c$, several key factors contribute to the softening of the optical phonon mode:

### Anharmonic Potential Energy Well

- In the paraelectric phase (above $T_c$), the lattice potential energy that governs the position of ions is typically symmetric and can be approximated by a parabolic (harmonic) potential for small displacements.
- As the temperature decreases and approaches $T_c$, the shape of this potential becomes increasingly anharmonic due to changes in interatomic forces. This anharmonicity flattens the potential energy well near its minimum, reducing the restoring force for ionic displacements.
- The flattening of the potential well leads to a decrease in the frequency of the corresponding vibrational mode (i.e., the soft mode), since the frequency $\omega$ of a vibration is related to the square root of the curvature (second derivative) of the potential energy $V(x)$ at equilibrium:

$$
  \omega \propto \sqrt{\frac{\partial^2 V}{\partial x^2}}
$$

  When the curvature decreases, the frequency decreases, resulting in a soft mode with lower frequency.

### Coupling to Macroscopic Polarization

- In ferroelectrics, the soft mode is strongly coupled to the polarization of the material. As the temperature decreases and approaches $T_c$, the lattice instability associated with the soft mode allows for a spontaneous polarization to develop.
- The collective motion of ions (associated with the soft mode) effectively reduces the energy barrier for polarization, leading to a lower frequency mode.
- This coupling also makes the system more susceptible to external perturbations (e.g., electric fields), leading to the large dielectric permittivity observed near the transition.

### Dynamic Lattice Instability

- The soft mode represents a dynamic instability of the crystal lattice. Near $T_c$, the energy associated with this mode approaches zero, meaning that the lattice has a nearly "free" oscillatory motion along the direction of the mode. This dynamic instability drives the system from the paraelectric state (where there is no net polarization) to the ferroelectric state (where there is a net polarization).
- When the frequency of this mode becomes zero at $T_c$, the system undergoes a second-order phase transition. The zero-frequency behavior indicates a critical slowing down of the lattice dynamics, meaning that any small perturbation can easily induce a change in polarization.

## Soft Mode Behavior in Real Materials: An Example

### BaTiO3

![image](https://github.com/user-attachments/assets/b24a6944-a9c5-411a-90d2-08129371a766)

- In the paraelectric phase (above $T_c$), the Ti atoms are symmetrically positioned within the oxygen octahedra, leading to no net dipole moment.
- As the temperature decreases, the soft mode (involving the relative displacement of Ti ions) lowers in frequency, and the lattice becomes unstable.
- Below $T_c$, the Ti ions shift off-center, creating a spontaneous polarization due to their relative displacement.

### PbTiO3

- Similarly, in PbTiO$_3$, the soft mode involves the displacement of Pb and Ti ions relative to the oxygen sublattice, leading to a ferroelectric phase transition when the mode softens.
