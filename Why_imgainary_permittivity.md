## Classical Harmonic Oscillator Model for Dielectric Response

Consider a simple model where each atom or molecule in a dielectric medium behaves like a damped harmonic oscillator under the influence of an applied electric field. The dynamics of this system can be described using the following classical differential equation:

$$
m \frac{d^2x}{dt^2} + \gamma \frac{dx}{dt} + kx = qE(t)
$$

where:

- $m$ is the mass of the oscillating charge.
- $x$ is the displacement of the charge from its equilibrium position.
- $\gamma$ is the damping coefficient, representing resistive losses (friction).
- $k$ is the spring constant of the restoring force.
- $q$ is the charge of the particle.
- $E(t)$ is the time-varying electric field.

### Solution for a Time-Harmonic Field
Assume an oscillatory electric field of the form:

$$
E(t) = E_0 e^{j\omega t}
$$

We seek a solution for the displacement $x(t)$ that also oscillates at the same frequency:

$$
x(t) = x_0 e^{j\omega t}
$$

Substituting $x(t)$ and $E(t)$ into the differential equation gives:

$$
-m\omega^2 x_0 e^{j\omega t} + j\omega \gamma x_0 e^{j\omega t} + kx_0 e^{j\omega t} = qE_0 e^{j\omega t}
$$

Factoring out common terms:

$$
(-m\omega^2 + j\omega \gamma + k)x_0 = qE_0
$$

Solving for $x_0$:

$$
x_0 = \frac{qE_0}{k - m\omega^2 + j\omega \gamma}
$$

### Polarization and Electric Susceptibility
The polarization $P$ of the material is given by the dipole moment per unit volume:

$$
P = Nqx_0 = Nq \left( \frac{qE_0}{k - m\omega^2 + j\omega \gamma} \right)
$$

where $N$ is the number density of oscillators. We can relate the polarization to the electric field through the electric susceptibility $\chi_e$:

$$
P = \epsilon_0 \chi_e E
$$

Thus, the electric susceptibility is:

$$
\chi_e(\omega) = \frac{Nq^2}{\epsilon_0 (k - m\omega^2 + j\omega \gamma)}
$$

### Complex Permittivity
The relative permittivity $\epsilon_r$ is related to the susceptibility as:

$$
\epsilon_r(\omega) = 1 + \chi_e(\omega) = 1 + \frac{Nq^2}{\epsilon_0 (k - m\omega^2 + j\omega \gamma)}
$$

Splitting this into real and imaginary parts gives:

$$
\epsilon_r(\omega) = \epsilon'(\omega) - j\epsilon''(\omega)
$$

where:

- $\epsilon'(\omega)$ represents the real part of permittivity, associated with energy storage.
- $\epsilon''(\omega)$ represents the imaginary part of permittivity, associated with energy loss (dissipation).

- ### Given Expression

The relative permittivity is given by:

$$
\epsilon_r(\omega) = 1 + \frac{Nq^2}{\epsilon_0 (k - m\omega^2 + j\omega \gamma)}
$$

Let:

$$
A = k - m\omega^2 \quad \text{and} \quad B = \omega \gamma
$$

So, we can rewrite the expression as:

$$
\epsilon_r(\omega) = 1 + \frac{Nq^2}{\epsilon_0 (A + jB)}
$$

### Separating the Real and Imaginary Parts

To separate the real and imaginary parts, we multiply the numerator and denominator by the complex conjugate of the denominator:

$$
\epsilon_r(\omega) = 1 + \frac{Nq^2}{\epsilon_0} \cdot \frac{A - jB}{A^2 + B^2}
$$

Expanding this, we get:

$$
\epsilon_r(\omega) = 1 + \frac{Nq^2}{\epsilon_0} \left( \frac{A}{A^2 + B^2} - j \frac{B}{A^2 + B^2} \right)
$$

Thus, we have:

### Real Part of $\epsilon_r(\omega)$

The real part of $\epsilon_r(\omega)$ is:

$$
\epsilon'(\omega) = 1 + \frac{Nq^2}{\epsilon_0} \cdot \frac{A}{A^2 + B^2} = 1 + \frac{Nq^2}{\epsilon_0} \cdot \frac{k - m\omega^2}{(k - m\omega^2)^2 + (\omega \gamma)^2}
$$

### Imaginary Part of $\epsilon_r(\omega)$

The imaginary part of $\epsilon_r(\omega)$ is:

$$
\epsilon''(\omega) = \frac{Nq^2}{\epsilon_0} \cdot \frac{B}{A^2 + B^2} = \frac{Nq^2}{\epsilon_0} \cdot \frac{\omega \gamma}{(k - m\omega^2)^2 + (\omega \gamma)^2}
$$


### Physical Interpretation of the Imaginary Permittivity
The imaginary part $\epsilon''(\omega)$ arises due to the damping term $\gamma$ in the differential equation, which accounts for energy losses in the material. This loss corresponds to the phase lag between the applied electric field $E$ and the induced polarization $P$, leading to a frequency-dependent dissipative behavior in the material. The energy dissipation is manifested as heat or other forms of energy transfer, which is why $\epsilon''(\omega)$ is commonly associated with dielectric losses.

## Electric Displacement Field and Phase Difference with Electric Field

In electromagnetism, the electric displacement field $D$ and the electric field $E$ are related through the material's response, which is characterized by its permittivity $\epsilon$. In an ideal vacuum, $D$ and $E$ are in phase because there is no material polarization to consider. However, in certain materials, $D$ can exhibit a phase difference with $E$ due to the frequency-dependent response of the material's dielectric properties. This phase difference is particularly important in contexts such as dielectric relaxation, alternating current (AC) fields, and complex permittivity analysis.

### Mathematical Background
The relationship between the electric displacement field $D$ and the electric field $E$ in a linear, isotropic, and homogeneous material is given by:

$$
D = \epsilon E
$$

Here, $\epsilon$ is the permittivity of the material, which may vary with frequency when dealing with time-varying fields. For a time-harmonic electric field of the form:

$$
E(t) = E_0 e^{j\omega t}
$$

where:

- $E_0$ is the amplitude of the electric field.
- $\omega$ is the angular frequency.
- $j = \sqrt{-1}$ represents the imaginary unit.

The permittivity $\epsilon$ can be expressed as a complex number:

$$
\epsilon(\omega) = \epsilon'(\omega) - j \epsilon''(\omega)
$$

where:

- $\epsilon'(\omega)$ is the real part (describing the stored energy).
- $\epsilon''(\omega)$ is the imaginary part (describing the energy loss).

The displacement field $D$ then becomes:

$$
D(t) = \epsilon(\omega) E_0 e^{j\omega t} = [\epsilon'(\omega) - j \epsilon''(\omega)] E_0 e^{j\omega t}
$$

Rewriting this, we obtain:

$$
D(t) = E_0 [\epsilon'(\omega) e^{j\omega t} - j \epsilon''(\omega) e^{j\omega t}] = E_0 |\epsilon(\omega)| e^{j(\omega t + \phi)}
$$

where

$$
\phi = \tan^{-1} \left( \frac{\epsilon''(\omega)}{\epsilon'(\omega)} \right)
$$

is the phase difference between $D$ and $E$.

### Explanation of Phase Difference
The phase difference arises due to the complex permittivity of the material, reflecting both the reactive (storage) and resistive (dissipative) behavior. When $\epsilon''(\omega)$ is significant, $D$ lags $E$ because energy dissipation causes a delay in the material's polarization response.


The program below demonstrates how the damping coefficient 𝛾 impacts energy storage and loss (represented by the real and imaginary components of permittivity, respectively) in a material system subjected to an oscillating electric field. This helps to visualize the physical significance of γ in determining the material's response characteristics.

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.widgets import Slider

# Constants
m = 1.0  # Mass of the particle (arbitrary units)
k = 1.0  # Spring constant (arbitrary units)
q = 1.0  # Charge of the particle (arbitrary units)
N = 1e3  # Number density of oscillators
epsilon_0 = 1.0  # Permittivity of free space (arbitrary units)

# Function to calculate real and imaginary parts of permittivity
def calculate_permittivity(omega, gamma):
    A = k - m * omega**2
    B = omega * gamma
    real_part = 1 + (N * q**2 / epsilon_0) * (A / (A**2 + B**2))
    imag_part = (N * q**2 / epsilon_0) * (B / (A**2 + B**2))
    return real_part, imag_part

# Initial parameters
gamma_init = 0.1
omega_values = np.linspace(0.1, 3.0, 1000)  # Range of angular frequencies

# Calculate initial values
real_parts = []
imag_parts = []
for omega in omega_values:
    real_part, imag_part = calculate_permittivity(omega, gamma_init)
    real_parts.append(real_part)
    imag_parts.append(imag_part)

# Plotting
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(8, 6))
plt.subplots_adjust(left=0.1, bottom=0.3)

# Plot real part of permittivity
line1, = ax1.plot(omega_values, real_parts, label='Real Part of Permittivity')
ax1.set_xlabel('Angular Frequency (ω)')
ax1.set_ylabel('Real Part')
ax1.set_title('Real Part of Permittivity vs Angular Frequency')
ax1.grid(True)
ax1.legend()

# Plot imaginary part of permittivity
line2, = ax2.plot(omega_values, imag_parts, label='Imaginary Part of Permittivity', color='tab:orange')
ax2.set_xlabel('Angular Frequency (ω)')
ax2.set_ylabel('Imaginary Part')
ax2.set_title('Imaginary Part of Permittivity vs Angular Frequency')
ax2.grid(True)
ax2.legend()

# Slider for gamma
ax_gamma = plt.axes([0.2, 0.15, 0.65, 0.03], facecolor='lightgray')
gamma_slider = Slider(ax_gamma, 'Damping (γ)', 0.01, 2.0, valinit=gamma_init)

# Update function for slider
def update(val):
    gamma = gamma_slider.val
    real_parts = []
    imag_parts = []
    for omega in omega_values:
        real_part, imag_part = calculate_permittivity(omega, gamma)
        real_parts.append(real_part)
        imag_parts.append(imag_part)
    line1.set_ydata(real_parts)
    line2.set_ydata(imag_parts)
    ax1.set_title(f'Real Part of Permittivity vs Angular Frequency (γ={gamma:.2f})')
    ax2.set_title(f'Imaginary Part of Permittivity vs Angular Frequency (γ={gamma:.2f})')
    fig.canvas.draw_idle()

gamma_slider.on_changed(update)

plt.show()
```
![image](https://github.com/user-attachments/assets/8f32da49-a7ae-4a7d-af5c-a4a9124b20f8)

### Top Plot (Real Part of Permittivity vs. Angular Frequency)

**Behavior**:  
The real part of the permittivity $\epsilon'(\omega)$ exhibits a significant change around a certain angular frequency. This behavior is characteristic of resonance phenomena in systems described by a damped harmonic oscillator model.

**Resonance Dip**:  
Near the resonance frequency, where $\omega$ is close to $\sqrt{\frac{k}{m}}$, the real part experiences a rapid transition. This transition represents the system's shift from storing energy (dominated by elastic restoring forces) to being dominated by dissipative effects due to the damping term.

**High-Frequency Limit**:  
As the angular frequency increases beyond the resonance region, the real part of the permittivity stabilizes to a lower value. This indicates that the system's ability to store energy diminishes as $\omega$ moves far away from the resonance frequency, where it behaves more like a high-frequency response of a damped material.

### Bottom Plot (Imaginary Part of Permittivity vs. Angular Frequency)

**Peak Behavior**:  
The imaginary part of the permittivity $\epsilon''(\omega)$ exhibits a pronounced peak at the resonance frequency. This peak indicates the maximum energy dissipation in the system, which corresponds to the frequency where the system absorbs and dissipates the most energy. This is a hallmark of resonance in damped systems.

**Damping and Loss**:  
The height and width of the peak depend on the damping coefficient $\gamma$. For larger values of $\gamma$, the peak would be lower and broader, indicating more significant dissipation over a wider range of frequencies. Conversely, for smaller values of $\gamma$, the peak would be higher and narrower, showing sharper resonance behavior.

