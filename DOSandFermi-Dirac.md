

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.constants import k, hbar, m_e, e

# Constants
T = [100, 300, 600]  # Temperatures in Kelvin
E = np.linspace(-0.5, 1.5, 1000) * e  # Energy range in Joules
Ef = 0  # Fermi level in Joules

# Density of States (DOS) for different materials
def dos_free_electron(E):
    """Density of states for a free electron gas (3D)."""
    E = np.maximum(E, 0)  # Ensure non-negative energy values
    return (1 / (2 * np.pi**2)) * ((2 * m_e / hbar**2)**(3/2)) * np.sqrt(E)

def dos_semiconductor(E, Eg):
    """Density of states for a semiconductor with bandgap Eg."""
    E_shifted = E - Eg
    E_shifted = np.maximum(E_shifted, 0)  # Ensure non-negative energy values
    return (1 / (2 * np.pi**2)) * ((2 * m_e / hbar**2)**(3/2)) * np.sqrt(E_shifted)

# Fermi-Dirac Distribution
def fermi_dirac(E, T, Ef):
    """Fermi-Dirac distribution function."""
    return 1 / (np.exp((E - Ef) / (k * T)) + 1)

# Calculate DOS and Fermi-Dirac distribution for different materials
dos_metal = dos_free_electron(E)
dos_semi = dos_semiconductor(E, 1.1 * e)  # Example bandgap of 1.1 eV

# Plot results
plt.figure(figsize=(18, 6))

# Plot DOS for metal and semiconductor
plt.subplot(1, 3, 1)
plt.plot(E / e, dos_metal, label='Metal')
plt.plot(E / e, dos_semi, label='Semiconductor')
plt.title('Density of States')
plt.xlabel('Energy (eV)')
plt.ylabel('DOS (states/eV)')
plt.legend()
plt.grid(True)

# Plot Fermi-Dirac distribution at different temperatures
plt.subplot(1, 3, 2)
for temp in T:
    fd_dist = fermi_dirac(E, temp, Ef)
    plt.plot(E / e, fd_dist, label=f'T = {temp} K')
plt.title('Fermi-Dirac Distribution')
plt.xlabel('Energy (eV)')
plt.ylabel('Occupation Probability')
plt.legend()
plt.grid(True)

# Plot electron occupation for metal and semiconductor at different temperatures
plt.subplot(1, 3, 3)
for temp in T:
    occupation_metal = dos_metal * fermi_dirac(E, temp, Ef)
    occupation_semi = dos_semi * fermi_dirac(E, temp, Ef)
    plt.plot(E / e, occupation_metal, label=f'Metal T = {temp} K')
    plt.plot(E / e, occupation_semi, label=f'Semiconductor T = {temp} K', linestyle='--')
plt.title('Electron Occupation')
plt.xlabel('Energy (eV)')
plt.ylabel('Occupation (states/eV)')
plt.legend()
plt.grid(True)

plt.tight_layout()
plt.show()
```

## Explanation of the Physics in the Code for Density of States and Fermi-Dirac Distribution

### Introduction
The code provided simulates the density of states (DOS) for metals and semiconductors and visualizes how the Fermi-Dirac distribution governs electron occupation at various temperatures. This blog will delve deeply into the mathematical derivation of the DOS for different materials, explain the Fermi-Dirac statistics, and interpret the results in a physical context.

### 1. Density of States (DOS) for a Free Electron Gas (Metal)

#### Mathematical Derivation
The density of states $g(E)$ describes the number of available electronic states per unit energy range in a system. For a three-dimensional free electron gas (idealized model for metals), the energy dispersion relation is given by:

$$
E = \frac{\hbar^2 k^2}{2m_e},
$$

where:

- $E$ is the energy of the electron.
- $\hbar$ is the reduced Planck's constant.
- $k$ is the wave vector.
- $m_e$ is the electron mass.

The number of states in a spherical shell of radius $k$ and thickness $dk$ in $k$-space is given by:

$$
dN = \frac{V}{(2\pi)^3} \cdot 4\pi k^2 dk,
$$

where $V$ is the volume of the system. The density of states $g(E)$ is obtained by changing variables from $k$ to $E$ using the Jacobian of the transformation:

$$
g(E) = \frac{dN}{dE} = \frac{V}{(2\pi)^3} \cdot 4\pi k^2 \cdot \frac{dk}{dE}.
$$

Using the dispersion relation:

$$
k = \sqrt{\frac{2m_e E}{\hbar^2}}, \quad \frac{dk}{dE} = \frac{m_e}{\hbar^2 k}.
$$

Substituting these into the expression for $g(E)$ gives:

$$
g(E) = \frac{V}{2\pi^2} \left(\frac{2m_e}{\hbar^2}\right)^{3/2} \sqrt{E}.
$$

This is implemented in the `dos_free_electron` function in the code.

#### Physical Interpretation
- Metals have a continuous density of states starting from $E = 0$ (assuming a parabolic band structure).
- The density of states increases as $\sqrt{E}$, reflecting the increasing number of available quantum states as energy increases.

### 2. Density of States for a Semiconductor

#### Mathematical Derivation with Bandgap
For a semiconductor, the density of states near the conduction band edge (assuming a simple parabolic band) is similar to that of a free electron gas but shifted by the bandgap energy $E_g$. The DOS is given by:

$$
g(E) = \frac{1}{2\pi^2} \left(\frac{2m_e}{\hbar^2}\right)^{3/2} \sqrt{E - E_g} \quad \text{for } E \geq E_g,
$$

and

$$
g(E) = 0 \quad \text{for } E < E_g.
$$

This shift represents the energy required to promote an electron from the valence band to the conduction band.

This is implemented in the `dos_semiconductor` function in the code.

#### Physical Interpretation
- The DOS for a semiconductor is zero below the conduction band edge (i.e., below $E_g$) and then rises as $\sqrt{E - E_g}$ for energies above the bandgap.
- This reflects the lack of available states in the bandgap and the rapid increase in states as electrons enter the conduction band.

### 3. Fermi-Dirac Distribution

#### Mathematical Formulation
The Fermi-Dirac distribution function gives the probability that a quantum state of energy $E$ is occupied by an electron at temperature $T$:

$$
f(E, T) = \frac{1}{\exp\left(\frac{E - E_f}{k_B T}\right) + 1},
$$

where:

- $E_f$ is the Fermi energy (energy level where the probability of occupation is 50% at $T = 0$ K).
- $k_B$ is the Boltzmann constant.
- $T$ is the absolute temperature.

#### Physical Interpretation
- At $T = 0$ K, all states with $E < E_f$ are occupied ($f = 1$) and all states with $E > E_f$ are unoccupied ($f = 0$).
- As $T$ increases, the distribution "smears out," and states above $E_f$ have a non-zero probability of being occupied.

This is implemented in the `fermi_dirac` function in the code.

