```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

def construct_hamiltonian_2d(Nx, Ny, t, epsilon):
    N = Nx * Ny
    H = np.zeros((N, N))
    for x in range(Nx):
        for y in range(Ny):
            i = x * Ny + y
            H[i, i] = epsilon
            if x > 0:
                j = (x - 1) * Ny + y
                H[i, j] = -t
                H[j, i] = -t
            if y > 0:
                j = x * Ny + (y - 1)
                H[i, j] = -t
                H[j, i] = -t
    return H

def solve_hamiltonian(H):
    eigenvalues, eigenvectors = np.linalg.eigh(H)
    return eigenvalues, eigenvectors

def plot_band_structure_3d(kx_values, ky_values, energy_values):
    KX, KY = np.meshgrid(kx_values, ky_values)
    fig = plt.figure(figsize=(12, 8))
    ax = fig.add_subplot(111, projection='3d')
    ax.plot_surface(KX, KY, energy_values, cmap='viridis')
    ax.set_xlabel('Wavevector kx')
    ax.set_ylabel('Wavevector ky')
    ax.set_zlabel('Energy (E)')
    ax.set_title('3D Band Structure of 2D Tight-Binding Model')
    ax.scatter([0, np.pi, -np.pi], [0, np.pi, -np.pi], [0, 0, 0], color='red', s=50, label='Special Symmetry Points')
    ax.legend()
    plt.show()

def plot_contour_energy(kx_values, ky_values, energy_values):
    plt.figure(figsize=(8, 6))
    plt.contourf(kx_values, ky_values, energy_values, levels=50, cmap='viridis')
    plt.colorbar(label='Energy (E)')
    plt.xlabel('Wavevector kx')
    plt.ylabel('Wavevector ky')
    plt.title('Energy Contour Plot')
    plt.grid(True)
    plt.show()

def plot_density_of_states(eigenvalues):
    plt.figure(figsize=(8, 6))
    plt.hist(eigenvalues, bins=50, density=True, alpha=0.7, color='blue')
    plt.xlabel('Energy (E)')
    plt.ylabel('Density of States (DOS)')
    plt.title('Density of States (DOS)')
    plt.grid(True)
    plt.show()


def main():
    global Nx, Ny, t, epsilon, kx_values, ky_values
    Nx = 30  # Number of atoms in the x direction
    Ny = 30  # Number of atoms in the y direction
    t = 1.0  # Initial hopping parameter
    epsilon = 0.0  # On-site energy

    # Construct Hamiltonian and solve for eigenvalues/eigenvectors
    H = construct_hamiltonian_2d(Nx, Ny, t, epsilon)
    eigenvalues, eigenvectors = solve_hamiltonian(H)

    # Generate k-space values
    kx_values = np.linspace(-np.pi, np.pi, Nx)
    ky_values = np.linspace(-np.pi, np.pi, Ny)
    energy_values = np.zeros((Nx, Ny))
    for i, kx in enumerate(kx_values):
        for j, ky in enumerate(ky_values):
            energy_values[i, j] = -2 * t * (np.cos(kx) + np.cos(ky)) + epsilon

    # Initial plots
    plot_band_structure_3d(kx_values, ky_values, energy_values)
    plot_contour_energy(kx_values, ky_values, energy_values)
    plot_density_of_states(eigenvalues)


if __name__ == "__main__":
    main()

```
