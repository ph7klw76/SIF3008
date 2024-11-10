```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.constants import hbar, m_e, e, k
from mpl_toolkits.mplot3d import Axes3D

# Constants
m = 0.26 * m_e  # Effective mass of electron in silicon
T = 300  # Temperature in Kelvin
kT = k * T  # Thermal energy

# Quantum Tunneling
def transmission_coefficient(E, V0, a):
    """Calculate the transmission coefficient for a potential barrier."""
    if E < V0:
        kappa = np.sqrt(2 * m * (V0 - E)) / hbar
        return np.exp(-2 * kappa * a)
    else:
        return 1

# Doping in Semiconductors
def carrier_concentration(Nd, Na, ni):
    """Calculate the electron and hole concentration in a doped semiconductor."""
    n = (Nd - Na + np.sqrt((Nd - Na)**2 + 4 * ni**2)) / 2
    p = ni**2 / n
    return n, p

def conductivity(n, p, mu_n, mu_p):
    """Calculate the conductivity of a semiconductor."""
    return e * (n * mu_n + p * mu_p)

# Parameters for quantum tunneling
V0 = 1 * e  # Potential barrier height in Joules
a = 1e-9  # Barrier width in meters
E = np.linspace(0, 2 * V0, 1000)  # Energy range

# Calculate transmission coefficient
T_coeff = np.array([transmission_coefficient(e, V0, a) for e in E])

# Parameters for doping in semiconductors
ni = 1.5e10  # Intrinsic carrier concentration in silicon at 300K (cm^-3)
Nd = 1e16  # Donor concentration (cm^-3)
Na = 0  # Acceptor concentration (cm^-3)
mu_n = 1350  # Electron mobility (cm^2/Vs)
mu_p = 480  # Hole mobility (cm^2/Vs)

# Calculate carrier concentration and conductivity
n, p = carrier_concentration(Nd, Na, ni)
sigma = conductivity(n, p, mu_n, mu_p)

# Plot results
plt.figure(figsize=(12, 6))

# Plot transmission coefficient
plt.subplot(1, 2, 1)
plt.plot(E / e, T_coeff)
plt.title('Quantum Tunneling')
plt.xlabel('Energy (eV)')
plt.ylabel('Transmission Coefficient')
plt.grid(True)

# Plot carrier concentration and conductivity
plt.subplot(1, 2, 2)
plt.bar(['n (electrons)', 'p (holes)'], [n, p])
plt.title('Carrier Concentration and Conductivity')
plt.ylabel('Concentration (cm^-3)')
plt.text(1, p, f'Conductivity: {sigma:.2e} S/cm', ha='center', va='bottom')

plt.tight_layout()
plt.show()

# Quantum Tunneling Probability Distribution
V0_values = np.linspace(0.1 * e, 2 * e, 500)  # Range of barrier heights
a_values = np.linspace(0.1e-9, 2e-9, 500)  # Range of barrier widths
E = 1 * e  # Fixed energy

# Calculate transmission coefficients for each combination of V0 and a
T_coeff_matrix = np.zeros((len(V0_values), len(a_values)))

for i, V0 in enumerate(V0_values):
    for j, a in enumerate(a_values):
        T_coeff_matrix[i, j] = transmission_coefficient(E, V0, a)

# Plot heatmap
plt.figure(figsize=(12, 6))
plt.subplot(1, 2, 1)
plt.imshow(T_coeff_matrix, extent=[a_values.min(), a_values.max(), V0_values.min(), V0_values.max()],
           aspect='auto', origin='lower', cmap='viridis')
plt.colorbar(label='Transmission Coefficient')
plt.title('Transmission Coefficient Heatmap')
plt.xlabel('Barrier Width (m)')
plt.ylabel('Barrier Height (J)')

# Plot 3D surface
ax = plt.subplot(1, 2, 2, projection='3d')
A, V = np.meshgrid(a_values, V0_values)
ax.plot_surface(A, V, T_coeff_matrix, cmap='viridis')
ax.set_title('Transmission Coefficient 3D Plot')
ax.set_xlabel('Barrier Width (m)')
ax.set_ylabel('Barrier Height (J)')
ax.set_zlabel('Transmission Coefficient')

plt.tight_layout()
plt.show()
```
