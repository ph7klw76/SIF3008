

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
