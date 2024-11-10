
```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
from matplotlib.widgets import Slider

def free_electron_energy(kx, ky, hbar=1, m=1):
    return (hbar**2 / (2 * m)) * (kx**2 + ky**2)

def plot_fermi_surface_2d(ax, kx_values, ky_values, energy_values, fermi_energy):
    ax.clear()
    KX, KY = np.meshgrid(kx_values, ky_values)
    ax.plot_surface(KX, KY, energy_values, cmap='viridis', alpha=0.7)
    ax.contour(KX, KY, energy_values, levels=[fermi_energy], colors='r', linewidths=2)
    ax.set_xlabel('Wavevector kx')
    ax.set_ylabel('Wavevector ky')
    ax.set_zlabel('Energy (E)')
    ax.set_title('Fermi Surface in 2D Free Electron Model')

def plot_brillouin_zone():
    fig, ax = plt.subplots()
    ax.set_aspect('equal')
    ax.plot([-np.pi, np.pi, np.pi, -np.pi, -np.pi], [-np.pi, -np.pi, np.pi, np.pi, -np.pi], 'k-')
    ax.set_xlabel('kx')
    ax.set_ylabel('ky')
    ax.set_title('First Brillouin Zone of 2D Square Lattice')
    plt.show()

def update(val):
    fermi_energy = slider.val
    plot_fermi_surface_2d(ax, kx_values, ky_values, energy_values, fermi_energy)
    fig.canvas.draw_idle()

# Parameters
Nx = 100  # Number of points in the kx direction
Ny = 100  # Number of points in the ky direction
hbar = 1.0  # Reduced Planck's constant
m = 1.0  # Electron mass

# Calculate energy values
kx_values = np.linspace(-np.pi, np.pi, Nx)
ky_values = np.linspace(-np.pi, np.pi, Ny)
energy_values = np.zeros((Nx, Ny))
for i, kx in enumerate(kx_values):
    for j, ky in enumerate(ky_values):
        energy_values[i, j] = free_electron_energy(kx, ky, hbar, m)

# Initial Fermi energy
initial_fermi_energy = np.max(energy_values) / 2

# Create figure and axis
fig = plt.figure(figsize=(10, 7))
ax = fig.add_subplot(111, projection='3d')

# Plot initial Fermi surface
plot_fermi_surface_2d(ax, kx_values, ky_values, energy_values, initial_fermi_energy)

# Create slider axis and slider
ax_slider = plt.axes([0.25, 0.02, 0.50, 0.03], facecolor='lightgoldenrodyellow')
slider = Slider(ax_slider, 'Fermi Energy', 0, np.max(energy_values), valinit=initial_fermi_energy)

# Update plot on slider change
slider.on_changed(update)

# Display plot and slider
plt.show()

# Plot Brillouin zone
plot_brillouin_zone()
```

## Fermi Surface Visualization in a 2D Free Electron Model with Brillouin Zone Representation

### Introduction
The provided code simulates and visualizes the Fermi surface in a two-dimensional free electron model using the concept of the energy dispersion relation of free electrons and the Fermi energy. This blog will explore the underlying physics of free electron models, Fermi surfaces, Brillouin zones, and their implications in condensed matter physics, with detailed mathematical derivations for each concept illustrated in the code.

### 1. Free Electron Model and Energy Dispersion Relation

#### Physics of the Free Electron Model
The free electron model is a simple approximation used to describe the behavior of conduction electrons in a solid. In this model, electrons are treated as free particles that move in a potential-free space within the material, experiencing no interactions except for being constrained by the boundaries of the crystal.

The energy dispersion relation for a free electron in two dimensions is given by:

$$
E(k) = \frac{\hbar^2}{2m} (k_x^2 + k_y^2),
$$

where:

- $E(k)$ is the energy of the electron as a function of the wavevector $k = (k_x, k_y)$.
- $\hbar$ is the reduced Planck's constant.
- $m$ is the effective mass of the electron.
- $k_x$ and $k_y$ are the components of the wavevector along the $x$ and $y$ directions.

The above expression describes a parabolic energy surface in $k$-space (momentum space).

#### Implementation
The function `free_electron_energy(kx, ky, hbar=1, m=1)` calculates the energy of an electron given the wavevector components $k_x$ and $k_y$. The default values of $\hbar = 1$ and $m = 1$ are used for simplicity in this code, though they can be adjusted to reflect actual physical constants.

### 2. Fermi Energy and the Fermi Surface

#### Definition and Significance
The Fermi energy $E_F$ is defined as the highest occupied energy level at absolute zero temperature ($T = 0$ K). The Fermi surface is a geometric representation in $k$-space of all points where the electron energy equals the Fermi energy:

$$
E(k) = E_F.
$$

The shape and properties of the Fermi surface play a crucial role in determining the electronic properties of metals and semiconductors, including electrical conductivity and heat capacity.

#### Visualization
- The code creates a meshgrid of $k_x$ and $k_y$ values and calculates the corresponding energy values using the `free_electron_energy` function.
- The `plot_fermi_surface_2d` function plots the energy surface in 3D and highlights the contour where the energy equals the Fermi energy (using a red contour line). This contour represents the 2D Fermi surface.

### 3. Brillouin Zone and Its Importance

#### Definition
The Brillouin zone is a fundamental concept in solid-state physics, representing the first region in reciprocal space that contains all unique wavevectors $k$. For a two-dimensional square lattice, the first Brillouin zone is a square bounded by $-\pi \leq k_x, k_y \leq \pi$ (assuming a lattice constant of unity).

#### Physical Interpretation
- The Brillouin zone helps to define the periodicity and symmetry properties of the crystal lattice in reciprocal space.
- Electron wavevectors are typically analyzed within the first Brillouin zone due to the periodic nature of the lattice.

#### Visualization
The `plot_brillouin_zone` function creates a 2D plot showing the boundaries of the first Brillouin zone for a square lattice. This helps to illustrate the range of possible $k$-values considered in the simulation.

### 4. Interactive Slider for Fermi Energy
The code includes an interactive slider to adjust the Fermi energy, which allows for real-time visualization of how the Fermi surface changes with varying Fermi energy levels. This demonstrates the sensitivity of electronic properties to changes in the Fermi level, which can occur due to doping, temperature variations, or other factors.

### Mathematical Derivations and Analysis

#### Energy Dispersion Relation
The energy of a free electron is given by:

$$
E = \frac{\hbar^2}{2m} (k_x^2 + k_y^2).
$$

This equation represents a paraboloid in 2D $k$-space.

#### Fermi Surface Condition
The Fermi surface is defined by the equation:

$$
E_F = \frac{\hbar^2}{2m} (k_x^2 + k_y^2).
$$

Rearranging terms gives:

$$
k_x^2 + k_y^2 = \frac{2m E_F}{\hbar^2}.
$$

This equation describes a circle in 2D $k$-space, with a radius proportional to $\sqrt{E_F}$. As $E_F$ changes, the size of this circle changes, reflecting changes in the occupied states at the Fermi energy.

#### Brillouin Zone Boundaries
The first Brillouin zone for a square lattice extends from $-\pi$ to $\pi$ in both $k_x$ and $k_y$ directions. This region captures all unique electron states due to the periodicity of the lattice.
