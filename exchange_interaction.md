# Exchange Interaction Energy and Its Integral in Magnetism in Solids

## Introduction
Magnetism in solids arises from the collective behavior of electron spins and their interactions. At the heart of this phenomenon is the exchange interaction energy, a quantum mechanical effect that governs the alignment of electron spins in materials. This blog delves into the detailed principles and mathematical derivations of exchange interaction energy, providing realistic examples and calculations involving real ferromagnetic and paramagnetic materials.

## Quantum Mechanical Foundation of Magnetism

### Electron Spin and the Pauli Exclusion Principle
Electrons possess an intrinsic angular momentum called spin ($S$), with a magnitude of $s = \frac{1}{2}$ and magnetic moment $\mu = -g_s \mu_B S$, where $g_s \approx 2$ is the electron spin g-factor and $\mu_B$ is the Bohr magneton.

The Pauli Exclusion Principle dictates that no two electrons can occupy the same quantum state simultaneously. This principle, combined with electron spin, leads to significant consequences for the electronic structure and magnetic properties of materials.

### Antisymmetry of Fermionic Wave Functions
For a system of identical fermions (e.g., electrons), the total wave function must be antisymmetric under particle exchange:

$$
\Psi(r_1, \sigma_1; r_2, \sigma_2) = -\Psi(r_2, \sigma_2; r_1, \sigma_1)
$$

where $r_i$ and $\sigma_i$ are the spatial coordinates and spin states of electron $i$, respectively.

## Exchange Interaction Energy

### Origin of Exchange Interaction
The exchange interaction arises due to the combination of Coulomb repulsion and the antisymmetry requirement of the fermionic wave function. It leads to an effective interaction between electron spins, favoring certain spin alignments to minimize the system's energy.

### Heisenberg Model and Exchange Hamiltonian
In solids, the exchange interaction is often modeled using the Heisenberg exchange Hamiltonian:

$$
\hat{H} = -\sum_{i \neq j} J_{ij} \hat{S}_i \cdot \hat{S}_j
$$

where:
- $\hat{S}_i$ and $\hat{S}_j$ are spin operators at sites $i$ and $j$.
- $J_{ij}$ is the exchange integral between spins at sites $i$ and $j$.

A positive $J_{ij}$ favors parallel alignment (ferromagnetism), while a negative $J_{ij}$ favors antiparallel alignment (antiferromagnetism).

### Mathematical Derivation of Exchange Integral

#### Two-Electron System
Consider two electrons in orbitals $\phi_a(r)$ and $\phi_b(r)$, interacting via the Coulomb potential.

#### Coulomb Potential Operator
$$
\hat{V} = \frac{e^2}{4 \pi \varepsilon_0 r_{12}}
$$

where $r_{12} = |r_1 - r_2|$.

#### Constructing Symmetric and Antisymmetric Spatial Wave Functions
- **Symmetric (Spin Singlet):**

$$
\psi_S(r_1, r_2) = \frac{1}{\sqrt{2(1+S^2)}} [\phi_a(r_1) \phi_b(r_2) + \phi_b(r_1) \phi_a(r_2)]
$$

- **Antisymmetric (Spin Triplet):**

$$
\psi_A(r_1, r_2) = \frac{1}{\sqrt{2(1-S^2)}} [\phi_a(r_1) \phi_b(r_2) - \phi_b(r_1) \phi_a(r_2)]
$$

$S$ is the overlap integral:

$$
S = \int \phi_a^*(r) \phi_b(r) \, dr
$$

#### Energy Expectation Values
The total Hamiltonian is:

$$
\hat{H} = \hat{H}_1 + \hat{H}_2 + \hat{V}
$$

The energy expectation values for the symmetric and antisymmetric states are:

$$
E_S = \langle \psi_S | \hat{H} | \psi_S \rangle
$$

$$
E_A = \langle \psi_A | \hat{H} | \psi_A \rangle
$$

#### Evaluating the Exchange Integral
The exchange integral $J_{ex}$ is given by:

![image](https://github.com/user-attachments/assets/facf8ef4-c498-439f-b956-f4b50e58df15)

The energy difference due to exchange interaction is:

$$
E_A - E_S = -2 J_{ex}
$$

Therefore, the exchange constant $J$ is:

$$
J = 2 J_{ex}
$$

# Curie Temperature and Exchange Interaction

## Curie Temperature
The Curie temperature $T_C$ is the temperature above which a ferromagnetic material becomes paramagnetic.

For iron, $T_C \approx 1043 \, \text{K}$.

The relationship between $J$ and $T_C$ in the mean-field approximation is:

$$
k_B T_C = \frac{2}{3} z J S(S+1)
$$

where:
- $k_B$ is the Boltzmann constant,
- $z$ is the number of nearest neighbors (8 for BCC lattice),
- $S = \frac{1}{2}$.

### Calculation
The exchange constant $J$ is:

$$
J = \frac{3 k_B T_C}{2 z S(S+1)} = \frac{3 \times (1.38 \times 10^{-23} \, \text{J/K}) \times 1043 \, \text{K}}{2 \times 8 \times \frac{1}{2} \times (\frac{1}{2} + 1)}
$$

Simplify the calculation:

$$
J = \frac{3 \times 1.43 \times 10^{-20} \, \text{J}}{2 \times 8 \times \frac{1}{2} \times \frac{3}{2}} = \frac{4.29 \times 10^{-20} \, \text{J}}{12} = 3.58 \times 10^{-21} \, \text{J}
$$

Convert $J$ to electronvolts:

$$
J = \frac{3.58 \times 10^{-21} \, \text{J}}{1.602 \times 10^{-19} \, \text{J/eV}} \approx 0.022 \, \text{eV}
$$

This estimated $J$ is smaller than the exchange splitting derived from band structure, indicating that multiple exchange interactions contribute to $T_C$.

---

## Antiferromagnetic Materials: Manganese Oxide (MnO)

MnO is a prototypical antiferromagnetic material with a rocksalt structure.

### Exchange Interaction in MnO
- $\text{Mn}^{2+}$ ions have a high-spin $d^5$ configuration.
- Superexchange interactions via oxygen ions lead to antiparallel spin alignment.

#### Exchange Constant Estimation
The Néel temperature $T_N$ (analogous to $T_C$ in antiferromagnets) for MnO is $T_N \approx 122 \, \text{K}$.

Using:

$$
k_B T_N = \frac{2}{3} z |J| S(S+1)
$$

Assuming $z = 6$ (octahedral coordination) and $S = \frac{5}{2}$:

### Calculation
$$
|J| = \frac{3 k_B T_N}{2 z S(S+1)} = \frac{3 \times (1.38 \times 10^{-23} \, \text{J/K}) \times 122 \, \text{K}}{2 \times 6 \times \frac{5}{2} \times (\frac{5}{2} + 1)}
$$

Simplify:

$$
|J| = \frac{5.05 \times 10^{-21} \, \text{J}}{2 \times 6 \times \frac{5}{2} \times \frac{7}{2}} = \frac{5.05 \times 10^{-21} \, \text{J}}{105} = 4.81 \times 10^{-23} \, \text{J}
$$

Convert $J$ to electronvolts:

$$
|J| = \frac{4.81 \times 10^{-23} \, \text{J}}{1.602 \times 10^{-19} \, \text{J/eV}} \approx 0.0003 \, \text{eV}
$$

This small $J$ reflects the weak antiferromagnetic exchange interaction in MnO.

# The Hydrogen Molecule Ion ($H_2^+$) (ADVANCED STUFF)

The hydrogen molecule ion ($H_2^+$) is the simplest molecular system, consisting of two protons and a single electron. Despite its simplicity, it provides profound insights into the nature of chemical bonding and the role of exchange interactions. In this detailed example, we will:

1. Perform a complete mathematical derivation of the exchange integral for $H_2^+$.
2. Describe each mathematical step comprehensively.
3. Implement the calculation using a Python program.

This example will illustrate how exchange interactions lead to the energy splitting between molecular orbitals, a fundamental concept in quantum chemistry and solid-state physics.

---

## Mathematical Derivation

### Atomic Orbitals
In the Linear Combination of Atomic Orbitals (LCAO) approximation, we consider atomic orbitals centered on nuclei $A$ and $B$:

$$
\phi_A(r) = \phi_{1s}(r - R_A)
$$

$$
\phi_B(r) = \phi_{1s}(r - R_B)
$$

where:
- $\phi_{1s}(r)$ is the hydrogen 1s orbital,
- $R_A$ and $R_B$ are the positions of nuclei $A$ and $B$,
- $r$ is the electron coordinate.

The hydrogen 1s orbital is given by:

$$
\phi_{1s}(r) = \frac{1}{\pi a_0^3} e^{-r / a_0}
$$

where $a_0$ is the Bohr radius.

---

### Molecular Orbitals
The molecular orbitals are formed as symmetric and antisymmetric combinations of the atomic orbitals:

#### Bonding Orbital ($\psi_+$):
$$
\psi_+(r) = \frac{1}{\sqrt{2(1+S)}} \left[ \phi_A(r) + \phi_B(r) \right]
$$

#### Antibonding Orbital ($\psi_-$):
$$
\psi_-(r) = \frac{1}{\sqrt{2(1-S)}} \left[ \phi_A(r) - \phi_B(r) \right]
$$

where $S$ is the overlap integral:

$$
S = \int \phi_A^*(r) \phi_B(r) \, dr
$$

---

### Overlap Integral ($S$)
The overlap integral quantifies the overlap between the atomic orbitals:

$$
S = \int \phi_A^*(r) \phi_B(r) \, dr
$$

Given the symmetry and real nature of the hydrogen 1s orbitals, the integral simplifies to:

$$
S = \int \phi_A(r) \phi_B(r) \, dr
$$

---

### Hamiltonian and Matrix Elements
The Hamiltonian for the electron in $H_2^+$ is:

$$
\hat{H} = -\frac{\hbar^2}{2m_e} \nabla^2 - \frac{e^2}{4 \pi \varepsilon_0} \left( \frac{1}{r_A} + \frac{1}{r_B} \right) + \frac{e^2}{4 \pi \varepsilon_0 R}
$$

where:
- $m_e$ is the electron mass,
- $r_A = |r - R_A|$ and $r_B = |r - R_B|$,
- $R = |R_A - R_B|$ is the internuclear distance.

The Hamiltonian matrix elements are:

#### Coulomb Integral ($H_{AA}$):
$$
H_{AA} = \int \phi_A^*(r) \hat{H} \phi_A(r) \, dr
$$

#### Exchange Integral ($H_{AB}$):
$$
H_{AB} = \int \phi_A^*(r) \hat{H} \phi_B(r) \, dr
$$

Due to symmetry:
$H_{AA} = H_{BB}, \quad H_{AB} = H_{BA}$

---

### Evaluating $H_{AA}$ and $H_{AB}$

#### Coulomb Integral ($H_{AA}$):
The Coulomb integral represents the energy expectation value of an electron in atomic orbital $\phi_A$:

$$
H_{AA} = E_0 + J
$$

where:
- $E_0$ is the energy of the hydrogen 1s orbital ($E_0 = -\frac{e^2}{8 \pi \varepsilon_0 a_0} = -13.6 \, \text{eV}$),
- $J$ accounts for the interaction of the electron with the other proton:

$$
J = \int \phi_A(r) \left( -\frac{e^2}{4 \pi \varepsilon_0 r_B} \right) \phi_A(r) \, dr
$$

#### Exchange Integral ($H_{AB}$):
The exchange integral represents the coupling between atomic orbitals $\phi_A$ and $\phi_B$:

$$
H_{AB} = S E_0 + K
$$

where $K$ is:

$$
K = \int \phi_A(r) \left( -\frac{e^2}{4 \pi \varepsilon_0} \left( \frac{1}{r_A} + \frac{1}{r_B} \right) \right) \phi_B(r) \, dr
$$

---

### Secular Equations
The energy eigenvalues are obtained by solving:

$$
\begin{vmatrix}
H_{AA} - E & H_{AB} - E S \\
H_{AB} - E S & H_{AA} - E
\end{vmatrix} = 0
$$

Simplifying:

$$
(H_{AA} - E)^2 - (H_{AB} - E S)^2 = 0
$$

Solving for $E$:

$$
E = \frac{H_{AA} \pm H_{AB}}{1 \pm S}
$$

The $+$ sign corresponds to the bonding orbital ($E_+$).  
The $-$ sign corresponds to the antibonding orbital ($E_-$).

---

### Analytical Expressions for Integrals

#### Overlap Integral ($S$):
For hydrogen 1s orbitals separated by distance $R$:

$$
S = e^{-R / a_0} \left( 1 + \frac{R}{a_0} + \frac{R^2}{3 a_0^2} \right)
$$

#### Coulomb Integral ($J$):
$$
J = -E_0 \left( e^{-2R / a_0} \left( 1 + \frac{R}{a_0} \right) \right)
$$

#### Exchange Integral ($K$):
$$
K = -E_0 \left( e^{-R / a_0} \left( 1 + \frac{R}{a_0} \right) \right)
$$

---

### Final Energy Expressions

#### Bonding Orbital Energy ($E_+$):
$$
E_+ = \frac{H_{AA} + H_{AB}}{1 + S}
$$

#### Antibonding Orbital Energy ($E_-$):
$$
E_- = \frac{H_{AA} - H_{AB}}{1 - S}
$$

---

## Python Implementation
We will now implement the calculation using Python, utilizing the `numpy` and `scipy` libraries for numerical computations and integrations.
```python
import numpy as np
from scipy import integrate
from scipy.constants import physical_constants, pi, e, epsilon_0

# Physical constants
a0 = physical_constants['Bohr radius'][0]         # Bohr radius (m)
E0 = -physical_constants['Rydberg constant times hc in eV'][0]  # Hydrogen ground state energy (eV)
eV_to_J = physical_constants['electron volt'][0]  # eV to Joules conversion factor

# Internuclear distance (adjust as needed)
R = 2.0 * a0  # in meters

def phi_1s(r):
    return (1 / np.sqrt(pi * a0**3)) * np.exp(-r / a0)

def overlap_integral(R):
    def integrand(r, theta):
        r1 = r
        r2 = np.sqrt(r1**2 + R**2 - 2 * r1 * R * np.cos(theta))
        return (r1**2 * np.sin(theta) *
                phi_1s(r1) * phi_1s(r2))
    integral = integrate.dblquad(
        integrand, 0, pi, lambda theta: 0, lambda theta: 10 * a0)
    return 2 * pi * integral[0]  # Multiply by 2π for azimuthal symmetry

def coulomb_integral(R):
    def integrand(r, theta):
        r1 = r
        r2 = np.sqrt(r1**2 + R**2 - 2 * r1 * R * np.cos(theta))
        V_B = -e**2 / (4 * pi * epsilon_0 * r2)
        return (r1**2 * np.sin(theta) *
                phi_1s(r1)**2 * V_B)
    integral = integrate.dblquad(
        integrand, 0, pi, lambda theta: 0, lambda theta: 10 * a0)
    return 2 * pi * integral[0] / eV_to_J  # Convert to eV

def exchange_integral(R):
    def integrand(r, theta):
        r1 = r
        r2 = np.sqrt(r1**2 + R**2 - 2 * r1 * R * np.cos(theta))
        V_B = -e**2 / (4 * pi * epsilon_0 * r2)
        return (r1**2 * np.sin(theta) *
                phi_1s(r1) * phi_1s(r2) * V_B)
    integral = integrate.dblquad(
        integrand, 0, pi, lambda theta: 0, lambda theta: 10 * a0)
    return 2 * pi * integral[0] / eV_to_J  # Convert to eV

# Compute integrals
S = overlap_integral(R)
J = coulomb_integral(R)
K = exchange_integral(R)

# Hamiltonian matrix elements
H_AA = E0 + J
H_AB = E0 * S + K

# Energies
E_plus = (H_AA + H_AB) / (1 + S)
E_minus = (H_AA - H_AB) / (1 - S)

# Display results
print(f"Overlap integral S: {S:.6f}")
print(f"Coulomb integral J: {J:.6f} eV")
print(f"Exchange integral K: {K:.6f} eV")
print(f"Bonding orbital energy E+: {E_plus:.6f} eV")
print(f"Antibonding orbital energy E-: {E_minus:.6f} eV")
```
