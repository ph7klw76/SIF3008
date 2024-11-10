```python
import numpy as np
import matplotlib.pyplot as plt

# Constants
epsilon_0 = 8.854187817e-12  # Vacuum permittivity (F/m)
frequencies = np.logspace(1, 15, 1000)  # Frequency range from 10 Hz to 10^15 Hz

# Material-specific parameters (example values)
# Electronic polarization parameters
omega_e = 1e15  # Resonance frequency for electronic polarization (Hz)
gamma_e = 1e13  # Damping factor for electronic polarization (Hz)
epsilon_inf = 2.0  # High-frequency permittivity

# Ionic polarization parameters
omega_i = 1e12  # Resonance frequency for ionic polarization (Hz)
gamma_i = 1e10  # Damping factor for ionic polarization (Hz)
epsilon_s = 10.0  # Static permittivity

# Dipolar polarization parameters
tau_d = 1e-9  # Relaxation time for dipolar polarization (s)
epsilon_d = 5.0  # Contribution to permittivity from dipolar polarization

# Calculate electronic polarization contribution
def electronic_polarization(frequencies, omega_e, gamma_e, epsilon_inf):
    omega = 2 * np.pi * frequencies
    chi_e = (epsilon_inf - 1) * omega_e**2 / (omega_e**2 - omega**2 + 1j * gamma_e * omega)
    return chi_e

# Calculate ionic polarization contribution
def ionic_polarization(frequencies, omega_i, gamma_i, epsilon_s, epsilon_inf):
    omega = 2 * np.pi * frequencies
    chi_i = (epsilon_s - epsilon_inf) * omega_i**2 / (omega_i**2 - omega**2 + 1j * gamma_i * omega)
    return chi_i

# Calculate dipolar polarization contribution
def dipolar_polarization(frequencies, tau_d, epsilon_d):
    omega = 2 * np.pi * frequencies
    chi_d = epsilon_d / (1 + 1j * omega * tau_d)
    return chi_d

# Calculate total complex permittivity
chi_e = electronic_polarization(frequencies, omega_e, gamma_e, epsilon_inf)
chi_i = ionic_polarization(frequencies, omega_i, gamma_i, epsilon_s, epsilon_inf)
chi_d = dipolar_polarization(frequencies, tau_d, epsilon_d)

epsilon_complex = 1 + chi_e + chi_i + chi_d

# Plot results
plt.figure(figsize=(12, 6))

# Plot real part of complex permittivity
plt.subplot(1, 2, 1)
plt.loglog(frequencies, epsilon_complex.real, label='Real Part')
plt.xlabel('Frequency (Hz)')
plt.ylabel('Real Permittivity')
plt.title('Real Part of Complex Permittivity')
plt.grid(True)
plt.legend()

# Plot imaginary part of complex permittivity
plt.subplot(1, 2, 2)
plt.plot(frequencies, epsilon_complex.imag, label='Imaginary Part', color='r')
plt.xscale('log')
plt.xlabel('Frequency (Hz)')
plt.ylabel('Imaginary Permittivity')
plt.title('Imaginary Part of Complex Permittivity')
plt.grid(True)
plt.legend()

plt.tight_layout()
plt.show()

from matplotlib.widgets import Slider

# Define slider for electronic resonance frequency
ax_slider = plt.axes([0.2, 0.02, 0.65, 0.03])
slider_omega_e = Slider(ax_slider, 'omega_e', 1e14, 2e15, valinit=omega_e)

# Update function for the plot
def update(val):
    new_omega_e = slider_omega_e.val
    chi_e = electronic_polarization(frequencies, new_omega_e, gamma_e, epsilon_inf)
    epsilon_complex = 1 + chi_e + chi_i + chi_d
    plt.subplot(1, 2, 1)
    plt.cla()
    plt.loglog(frequencies, epsilon_complex.real, label='Real Part')
    plt.xlabel('Frequency (Hz)')
    plt.ylabel('Real Permittivity')
    plt.title('Real Part of Complex Permittivity')
    plt.grid(True)
    plt.legend()
    
    plt.subplot(1, 2, 2)
    plt.cla()
    plt.plot(frequencies, epsilon_complex.imag, label='Imaginary Part', color='r')
    plt.xscale('log')
    plt.xlabel('Frequency (Hz)')
    plt.ylabel('Imaginary Permittivity')
    plt.title('Imaginary Part of Complex Permittivity')
    plt.grid(True)
    plt.legend() 
    plt.draw()
slider_omega_e.on_changed(update)
plt.show()
```

## Frequency-Dependent Polarization and Complex Permittivity in Dielectric Materials

### Introduction
The code provided simulates the frequency-dependent behavior of the complex permittivity of a dielectric material by accounting for different polarization mechanisms: electronic, ionic, and dipolar polarization. Each of these polarization processes responds differently to an applied electric field, and their contributions vary across different frequency ranges. This blog provides a detailed and rigorous explanation of the underlying physics, the mathematical derivations for each contribution to permittivity, and the physical meaning of the results.

### 1. The Complex Permittivity of a Material
The complex permittivity $\epsilon(\omega)$ of a dielectric material describes how it responds to an oscillating electric field at a given angular frequency $\omega$. It can be expressed as:

$$
\epsilon(\omega) = \epsilon'(\omega) - i \epsilon''(\omega),
$$

where:

- $\epsilon'(\omega)$ is the real part, representing the material's ability to store electrical energy.
- $\epsilon''(\omega)$ is the imaginary part, representing energy losses due to various mechanisms such as damping and absorption.

The total complex permittivity is determined by different polarization mechanisms that contribute to the dielectric response across different frequency ranges.

### 2. Electronic Polarization

#### Physics and Derivation
Electronic polarization arises from the displacement of electron clouds relative to the nucleus under an applied electric field. This effect dominates at high frequencies, typically in the optical and ultraviolet range. The resonance behavior of electronic polarization is characterized by a resonance frequency $\omega_e$ and a damping term $\gamma_e$.

The susceptibility due to electronic polarization $\chi_e$ is given by:

$$
\chi_e(\omega) = (\epsilon_\infty - 1) \frac{\omega_e^2}{\omega_e^2 - \omega^2 + i \gamma_e \omega},
$$

where:

- $\epsilon_\infty$ is the high-frequency permittivity (when $\omega \rightarrow \infty$).
- $\omega_e$ is the resonance frequency of electronic polarization.
- $\gamma_e$ is the damping coefficient, accounting for energy loss.

The term $\frac{\omega_e^2}{\omega_e^2 - \omega^2 + i \gamma_e \omega}$ represents a damped harmonic oscillator response, capturing the resonance behavior of electrons within atoms.

**Implementation**  
The function `electronic_polarization` computes the frequency-dependent susceptibility for electronic polarization based on the given parameters. The contribution is added to the total complex permittivity.

### 3. Ionic Polarization

#### Physics and Derivation
Ionic polarization arises from the relative displacement of positive and negative ions in a crystal lattice under an applied electric field. This effect is significant at lower frequencies, typically in the infrared range. The resonance frequency $\omega_i$ and damping term $\gamma_i$ characterize ionic vibrations.

The susceptibility due to ionic polarization $\chi_i$ is given by:

$$
\chi_i(\omega) = (\epsilon_s - \epsilon_\infty) \frac{\omega_i^2}{\omega_i^2 - \omega^2 + i \gamma_i \omega},
$$

where:

- $\epsilon_s$ is the static permittivity (at $\omega = 0$).
- $\epsilon_\infty$ is the high-frequency permittivity.
- $\omega_i$ is the resonance frequency for ionic motion.
- $\gamma_i$ is the damping factor for ionic motion.

This expression also follows the form of a damped harmonic oscillator, representing the oscillatory motion of ions in response to an external field.

**Implementation**  
The function `ionic_polarization` computes the frequency-dependent susceptibility for ionic polarization, capturing its contribution to the overall permittivity.

### 4. Dipolar (Orientational) Polarization

#### Physics and Derivation
Dipolar polarization occurs due to the orientation of permanent dipole moments in an applied electric field. This process is dominant at low frequencies (radiofrequency and below) and has a characteristic relaxation time $\tau_d$, which dictates how quickly the dipoles can respond to changes in the electric field.

The susceptibility due to dipolar polarization $\chi_d$ is given by:

$$
\chi_d(\omega) = \frac{\epsilon_d}{1 + i \omega \tau_d},
$$

where:

- $\epsilon_d$ is the contribution to permittivity from dipolar polarization.
- $\tau_d$ is the relaxation time.

This form represents a Debye-type relaxation, where the response of the dipole moments lags behind the applied field.

**Implementation**  
The function `dipolar_polarization` calculates the susceptibility for dipolar polarization, adding to the total complex permittivity.

### 5. Total Complex Permittivity
The total complex permittivity is the sum of the contributions from electronic, ionic, and dipolar polarizations:

$$
\epsilon(\omega) = 1 + \chi_e(\omega) + \chi_i(\omega) + \chi_d(\omega).
$$

This summation reflects the superposition of different polarization mechanisms, each dominating in specific frequency ranges.

### Visualization
The code plots the real and imaginary parts of the complex permittivity as functions of frequency, showing how different mechanisms contribute to the dielectric response across a wide range of frequencies.

- **Real Part $\epsilon'(\omega$)**: Shows how the material's ability to store electric energy varies with frequency.
- **Imaginary Part $\epsilon''(\omega)$**: Represents energy dissipation due to different damping mechanisms.

### 6. Interactive Slider for Electronic Resonance Frequency
The code also includes an interactive slider to vary the electronic resonance frequency $\omega_e$, allowing users to observe how changes in this parameter affect the complex permittivity. This helps illustrate the sensitivity of the dielectric response to the underlying material properties.

![image](https://github.com/user-attachments/assets/edcf0ceb-6389-44c5-a6cb-9bfbf1296602)
