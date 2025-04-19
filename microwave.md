## Introduction
Microwaves are a part of the electromagnetic spectrum with frequencies typically ranging from 300 MHz to 300 GHz. The microwave ovens used in our daily lives operate at a specific frequency of 2.45 GHz. The primary mechanism by which microwaves heat food is through their interaction with water molecules, causing them to vibrate and generate heat. This process relies on a phenomenon known as dielectric heating, which is underpinned by complex physics.

## Electromagnetic Waves and Their Interaction with Matter

### Maxwell’s Equations
To understand how microwaves heat water molecules, we must first describe the electromagnetic fields that form microwaves using Maxwell’s equations:

$$
\nabla \cdot \mathbf{E} = \frac{\rho}{\epsilon_0}
$$
$$
\nabla \cdot \mathbf{B} = 0
$$
$$
\nabla \times \mathbf{E} = - \frac{\partial \mathbf{B}}{\partial t}
$$
$$
\nabla \times \mathbf{B} = \mu_0 \mathbf{J} + \mu_0 \epsilon_0 \frac{\partial \mathbf{E}}{\partial t}
$$

Here, $\mathbf{E}$ represents the electric field, $\mathbf{B}$ is the magnetic field, $\rho$ is the charge density, $\mathbf{J}$ is the current density, $\epsilon_0$ is the permittivity of free space, and $\mu_0$ is the permeability of free space. When microwaves pass through a medium containing water molecules, their electric fields exert forces on the charged and polar molecules in the medium.

## Dielectric Heating Mechanism
Water molecules are polar, meaning they possess a dipole moment due to the uneven distribution of electron density between the oxygen and hydrogen atoms. The electric field component of the microwave interacts with these dipole moments, causing the molecules to attempt to align themselves with the rapidly oscillating field. This continual reorientation results in molecular friction and, consequently, heat generation.

### Mathematical Description of Dielectric Heating

#### Polarization and Dipole Rotation
The interaction of a dipole with an oscillating electric field $\mathbf{E}(t) = E_0 \cos(\omega t)$ can be expressed mathematically as:

$$
\mathbf{P} = \epsilon_0 \chi_e \mathbf{E}
$$

where $\mathbf{P}$ is the polarization of the medium, $\chi_e$ is the electric susceptibility, and $\omega$ is the angular frequency of the microwaves. The reorientation of water molecules (which have a dipole moment $\mu$) leads to energy absorption. The power dissipated due to dielectric losses is given by:

$$
P = \omega \epsilon_0 \epsilon'' |E|^2
$$

where $\epsilon''$ is the imaginary part of the permittivity (dielectric loss factor), representing the efficiency of microwave absorption and conversion into heat.

### Heat Generation and Temperature Increase
The power absorbed per unit volume by the water molecules is converted into heat, described by the following equation:

$$
Q = \sigma |E|^2
$$

Here, $\sigma$ is the electrical conductivity, and $Q$ is the heat generated per unit volume. In practice, the absorbed energy causes an increase in the kinetic energy of the water molecules, raising the temperature of the water through molecular collisions.

## Detailed Mechanism of Heating

### Microwave Frequency and Resonance
Microwaves operate at a frequency of 2.45 GHz, which does not precisely match the natural vibrational frequency of water molecules. This prevents direct resonance absorption, allowing energy transfer to occur more gradually, which is optimal for efficient heating.

### Debye Relaxation Model
The response of water molecules to an oscillating field can be modeled using the Debye relaxation model, which describes the frequency-dependent dielectric response. The relaxation time $\tau$ of water molecules (approximately 8.27 picoseconds) dictates how quickly they can align with the oscillating electric field. The dielectric loss factor $\epsilon''$ reaches a peak when $\omega \tau \approx 1$, indicating maximal energy absorption near this frequency.

$$
\epsilon''(\omega) = \frac{\epsilon_s - \epsilon_\infty}{1 + (\omega \tau)^2}
$$

where $\epsilon_s$ and $\epsilon_\infty$ are the static and high-frequency permittivities, respectively.

## Quantum Mechanical Description of Microwave Heating of Water Molecules

### Introduction
The quantum mechanical perspective focuses on how microwaves interact with the discrete energy states of water molecules at the molecular level. Unlike visible light, microwaves interact primarily with the rotational states of polar molecules such as water. Here, we will explore the relevant quantum mechanical principles, including the quantization of rotational energy levels and transitions induced by electromagnetic fields.

### 1. Rotational Energy Levels of Water Molecules
In quantum mechanics, the rotational energy of a molecule is quantized and described by the rigid rotor model. The rotational energy levels $E_J$ for a linear (or approximately rigid) molecule like water are given by:

$$
E_J = \frac{\hbar^2 J(J + 1)}{2I}
$$

where:

- $J$ is the rotational quantum number ($J = 0, 1, 2, \ldots$)
- $\hbar$ is the reduced Planck's constant
- $I$ is the moment of inertia of the molecule

For water, which has a bent geometry, its rotational energy levels can be more complex due to asymmetry, but the quantization principle remains similar. Each energy level is associated with a specific rotational state.

### 2. Selection Rules for Rotational Transitions
Microwave radiation induces transitions between these quantized rotational states when the energy of the absorbed photon matches the energy difference between two levels. This process follows the selection rule:

$$
\Delta J = \pm 1
$$

The energy of the absorbed photon is given by:

$$
\Delta E = E_{J+1} - E_J = \hbar \omega
$$

where $\omega$ is the angular frequency of the microwave radiation. This resonance condition ensures that energy is efficiently transferred to the water molecules, causing rotational excitation.

### 3. Interaction of Microwaves with Molecular Dipoles
Microwaves are part of the electromagnetic spectrum and consist of oscillating electric and magnetic fields. When the oscillating electric field interacts with the permanent dipole moment $\mu$ of a water molecule, the molecule experiences a torque. Quantum mechanically, this interaction is represented by the Hamiltonian $H_{\text{int}}$ that describes the interaction between the dipole moment and the electric field:

$$
H_{\text{int}} = -\mu \cdot E(t)
$$

In a microwave field with electric field $E(t) = E_0 \cos(\omega t)$, the dipole transitions between rotational states, leading to energy absorption.

### 4. Time-Dependent Schrödinger Equation for Dipole Rotation
The evolution of the quantum state of the water molecule under the influence of the microwave field can be described by the time-dependent Schrödinger equation:

$$
i \hbar \frac{\partial \psi(t)}{\partial t} = (H_0 + H_{\text{int}}) \psi(t)
$$

where:

- $H_0$ is the Hamiltonian for the rotational states of the molecule in the absence of an external field
- $H_{\text{int}}$ is the interaction term as described above

Solving this equation for a given initial state $\psi(0)$ provides the time evolution of the system, including the probability of transitions between different rotational states.

### 5. Absorption and Energy Dissipation Mechanism
The absorbed energy from microwaves leads to an increase in the rotational kinetic energy of the water molecules. However, the quantum states of individual molecules are continually perturbed by collisions with neighboring molecules, causing energy transfer between rotational and translational degrees of freedom. This energy transfer manifests as heat, effectively increasing the thermal energy of the system. Quantum mechanically, this process is described as relaxation of the excited states back to thermal equilibrium through dipole-dipole interactions and collisions.

### 6. Quantum Description of Dielectric Loss
The dielectric loss factor $\epsilon''$ in the classical description corresponds to the quantum mechanical probability of transitions between rotational states due to the external field. This can be computed using perturbation theory, specifically Fermi’s Golden Rule, which gives the transition rate $W$ for a system interacting with a perturbative field:

$$
W = \frac{2\pi}{\hbar} |\langle \psi_f | H_{\text{int}} | \psi_i \rangle|^2 \rho(E_f)
$$

where:

- $|\psi_i\rangle$ and $|\psi_f\rangle$ are the initial and final quantum states
- $\rho(E_f)$ is the density of final states

The transition rate determines the amount of energy absorbed by the system, contributing to the heating effect.

### 7. Temperature Increase and Statistical Mechanics
At the molecular level, the absorbed microwave energy results in a distribution of rotational and vibrational states following Boltzmann statistics. The thermal energy $\langle E \rangle$ of the system is given by:

$$
\langle E \rangle = \frac{\sum_i E_i \exp(-E_i / k_B T)}{Z}
$$

where $Z$ is the partition function, $k_B$ is the Boltzmann constant, and $T$ is the temperature. The absorbed energy redistributes among translational, rotational, and vibrational modes, increasing the temperature.

## Mathematical Formulation of the Debye Model

The Debye model describes the frequency dependence of the complex permittivity as:

$$
\epsilon(\omega) = \epsilon_\infty + \frac{\epsilon_s - \epsilon_\infty}{1 + (i\omega\tau)}
$$

where:

- $\epsilon_s$ is the static permittivity (permittivity at zero frequency).
- $\epsilon_\infty$ is the permittivity at infinite frequency.
- $\omega = 2\pi f$ is the angular frequency of the applied field.
- $i$ is the imaginary unit ($i^2 = -1$).

### Real and Imaginary Components of Permittivity

**Dielectric Constant :** 
$\epsilon'(\omega)$

The real part of the complex permittivity is given by:

$$
\epsilon'(\omega) = \epsilon_\infty + \frac{\epsilon_s - \epsilon_\infty}{1 + (\omega \tau)^2}
$$

This term represents the ability of the material to store electric energy. As the frequency increases, the dipoles cannot reorient as quickly, leading to a decrease in $\epsilon'(\omega)$.

**Dielectric Loss :** 
$\epsilon''(\omega)$

The imaginary part of the complex permittivity is given by:

$$
\epsilon''(\omega) = \frac{(\epsilon_s - \epsilon_\infty) \omega \tau}{1 + (\omega \tau)^2}
$$

This term represents the energy dissipated as heat due to the lagging response of the dipoles relative to the applied electric field.

### Loss Factor and Energy Dissipation

The dielectric loss factor $\epsilon''(\omega)$ reaches its maximum when $\omega \tau = 1$, indicating maximum energy absorption (and heat generation). This peak occurs because, at this frequency, the oscillation of the electric field closely matches the natural reorientation time of the dipoles, leading to maximum friction and energy dissipation.

### Interpretation of the Debye Model

**Low-Frequency Behavior :** ($\omega \rightarrow 0$)

At low frequencies (or static fields), the dipoles have enough time to align with the applied field, resulting in high polarization. 

$$
\epsilon'(\omega) \approx \epsilon_s, \quad \text{and} \quad \epsilon''(\omega) \approx 0
$$

This means that the dielectric loss is minimal, and most of the energy is stored.

**High-Frequency Behavior :** ($\omega \rightarrow \infty$)

At high frequencies, the dipoles cannot keep up with the rapid oscillations of the field, resulting in a low dielectric constant.

$$
\epsilon'(\omega) \approx \epsilon_\infty, \quad \text{and} \quad \epsilon''(\omega) \approx 0
$$

indicating little energy storage or dissipation.

**Intermediate Frequencies (Resonance):**

At intermediate frequencies around $\omega \approx \frac{1}{\tau}$, the dielectric loss $\epsilon''(\omega)$ is maximized. This frequency range is crucial for applications like microwave heating, as it represents efficient energy transfer to the medium.

![output (9)](https://github.com/user-attachments/assets/e198ad67-d1ba-47a0-96c3-9c8b0100a90d)

The plot illustrates how the dielectric loss (represented by $\epsilon''$) and the energy dissipation (represented by $\omega \epsilon''$) of water molecules change as a function of frequency. The Debye relaxation model shows that the dielectric loss peaks near a characteristic frequency related to the relaxation time $\tau$, which signifies maximal energy absorption by the water molecules. This absorbed energy, when subjected to an oscillating electric field (like microwaves), results in heat generation and subsequent heating of water.


```python
import numpy as np
import matplotlib.pyplot as plt

# Parameters for the Debye relaxation model
epsilon_s = 80  # Static permittivity (relative permittivity of water at low frequencies)
epsilon_inf = 4  # High-frequency permittivity (relative permittivity at very high frequencies)
tau = 8.27e-12  # Relaxation time for water (in seconds)
frequency = np.logspace(6, 12, 500)  # Frequency range from 1 MHz to 1 THz
omega = 2 * np.pi * frequency  # Angular frequency

# Debye model for dielectric loss
epsilon_prime = epsilon_inf + (epsilon_s - epsilon_inf) / (1 + (omega * tau)**2)
epsilon_double_prime = (epsilon_s - epsilon_inf) * omega * tau / (1 + (omega * tau)**2)

# Calculate energy dissipation (loss) as a function of frequency
loss_factor = omega * epsilon_double_prime

# Plot the dielectric loss
plt.figure(figsize=(12, 6))
plt.loglog(frequency, epsilon_double_prime, label='Dielectric Loss (ε\'\')')
plt.loglog(frequency, loss_factor, label='Energy Dissipation (ωε\'\')', linestyle='--')
plt.xlabel('Frequency (Hz)')
plt.ylabel('Magnitude')
plt.title('Dielectric Loss and Energy Dissipation in Water (Debye Model)')
plt.grid(True, which='both', linestyle='--', linewidth=0.5)
plt.legend()
plt.show()
```



## Microwave and water interaction


![image](https://github.com/user-attachments/assets/497c0f22-bec4-4c21-9fb1-cf08add31ebd)

```python
import sys
import math
import random
from PyQt5.QtWidgets import (QApplication, QWidget, QPushButton, QVBoxLayout, QLabel, QSizePolicy)
from PyQt5.QtGui import QPainter, QColor, QPen, QBrush, QFont
from PyQt5.QtCore import Qt, QTimer, QPointF, QRectF

# --- Constants ---
# Physical constants (mostly for reference, simulation uses scaled values)
FREQ_HZ = 2.45e9  # 2.45 GHz (Actual microwave frequency)
OMEGA_RAD_S = 2 * math.pi * FREQ_HZ # Actual angular frequency
DIPOLE_MOMENT_SI = 6.17e-30 # C*m (Actual dipole moment)

# Simulation parameters
NUM_MOLECULES = 50
WINDOW_WIDTH = 800
WINDOW_HEIGHT = 600
MOLECULE_SIZE = 15  # Visual size of oxygen atom
BOND_LENGTH = MOLECULE_SIZE * 1.3
H_SIZE_FACTOR = 0.6 # Hydrogen atom size relative to oxygen

# --- Simulation Dynamics Parameters (Tunable) ---
SIM_UPDATE_MS = 20 # Update interval in milliseconds (50 FPS)
DT = SIM_UPDATE_MS / 1000.0 # Time step for simulation updates

# Scaled E-field parameters for visualization
SIM_OMEGA = 2 * math.pi * 1.0 # Visual oscillation frequency (e.g., 1 Hz)
SIM_E0_MAX = 50.0 # Maximum strength of the visual E-field (arbitrary units)

# Molecule behavior parameters (Tunable)
ALIGNMENT_STRENGTH = 50.0   # How strongly dipoles try to align with E-field
DAMPING_FACTOR = 0.90      # Reduces angular velocity (simulates friction)
BASE_JITTER_ANGLE = 0.1    # Base random rotation per step (radians)
BASE_JITTER_POS = 0.5      # Base random movement per step (pixels)
HEATING_FACTOR_INCREASE = 1.5 # How much jitter increases when microwave is on
TEMPERATURE_RAMP_RATE = 0.05 # How fast the "temperature factor" changes

# --- Water Molecule Class ---
class WaterMolecule:
    def __init__(self, x, y, angle_deg):
        self.pos = QPointF(x, y)
        self.angle = math.radians(angle_deg) # Angle of the dipole moment (O -> H midpoint) w.r.t positive x-axis
        self.angular_velocity = 0.0
        self.color_o = QColor(200, 0, 0) # Red for Oxygen
        self.color_h = QColor(200, 200, 200) # Light grey for Hydrogen
        self.dipole_strength = 1.0 # Scaled for simulation torque calculation

        # Calculate H positions relative to O based on angle
        # H-O-H angle is approx 104.5 degrees
        h_angle_offset = math.radians(104.5 / 2.0)
        self.h1_offset = QPointF(BOND_LENGTH * math.cos(h_angle_offset),
                                 BOND_LENGTH * math.sin(h_angle_offset))
        self.h2_offset = QPointF(BOND_LENGTH * math.cos(-h_angle_offset),
                                 BOND_LENGTH * math.sin(-h_angle_offset))

    def get_dipole_vector(self):
        # Dipole points from O towards midpoint between H's
        # For simplicity, aligned with the molecule's angle
        return QPointF(math.cos(self.angle), math.sin(self.angle))

    def update(self, dt, E_field_x, is_microwave_on, temperature_factor):
        # --- 1. Calculate Torque from E-field (Simplified Eq. 1) ---
        torque = 0.0
        if is_microwave_on and abs(E_field_x) > 1e-6:
            # Torque = mu x E = |mu| |E| sin(phi) * z_hat
            # phi is the angle between dipole and E-field (along x-axis)
            # Dipole vector = (cos(self.angle), sin(self.angle))
            # E vector = (E_field_x, 0)
            # Cross product magnitude in 2D: mu_x*E_y - mu_y*E_x
            dipole_vec = self.get_dipole_vector() * self.dipole_strength
            torque = dipole_vec.x() * 0 - dipole_vec.y() * E_field_x
            torque *= -ALIGNMENT_STRENGTH # Scale torque effect

        # --- 2. Apply Rotational Dynamics (Simplified Eq. 2) ---
        # Ignore inertia (I=1) for simplicity, apply torque directly to angular velocity
        # Add damping and stochastic torque (jitter)
        self.angular_velocity += torque * dt
        self.angular_velocity *= DAMPING_FACTOR # Rotational friction

        # Add thermal jitter (stochastic torque effect)
        random_torque = random.uniform(-1, 1) * BASE_JITTER_ANGLE * temperature_factor
        self.angular_velocity += random_torque * dt # Instantaneous random kick

        # Update angle
        self.angle += self.angular_velocity * dt
        self.angle = self.angle % (2 * math.pi) # Keep angle in [0, 2pi)

        # --- 3. Apply Translational Dynamics (Jitter) ---
        # Random walk representing thermal motion
        vx = random.uniform(-1, 1) * BASE_JITTER_POS * temperature_factor
        vy = random.uniform(-1, 1) * BASE_JITTER_POS * temperature_factor
        self.pos += QPointF(vx * dt * 50, vy * dt * 50) # Scale dt effect

        # --- 4. Boundary Conditions (Wrap around) ---
        if self.pos.x() < -MOLECULE_SIZE: self.pos.setX(WINDOW_WIDTH + MOLECULE_SIZE)
        if self.pos.x() > WINDOW_WIDTH + MOLECULE_SIZE: self.pos.setX(-MOLECULE_SIZE)
        if self.pos.y() < -MOLECULE_SIZE: self.pos.setY(WINDOW_HEIGHT + MOLECULE_SIZE)
        if self.pos.y() > WINDOW_HEIGHT + MOLECULE_SIZE: self.pos.setY(-MOLECULE_SIZE)


    def draw(self, painter):
        painter.save()
        painter.translate(self.pos)
        painter.rotate(math.degrees(self.angle)) # Rotate coordinate system

        # Draw Oxygen (at origin after translation)
        painter.setBrush(QBrush(self.color_o))
        painter.setPen(Qt.NoPen)
        painter.drawEllipse(QPointF(0, 0), MOLECULE_SIZE, MOLECULE_SIZE)

        # Calculate Hydrogen positions in the rotated frame
        h1_pos = self.h1_offset
        h2_pos = self.h2_offset
        h_radius = MOLECULE_SIZE * H_SIZE_FACTOR

        # Draw Hydrogens
        painter.setBrush(QBrush(self.color_h))
        painter.drawEllipse(h1_pos, h_radius, h_radius)
        painter.drawEllipse(h2_pos, h_radius, h_radius)

        # Draw Bonds
        painter.setPen(QPen(Qt.black, 1))
        painter.drawLine(QPointF(0, 0), h1_pos)
        painter.drawLine(QPointF(0, 0), h2_pos)

        # Optional: Draw dipole moment vector (for debugging/visualization)
        # painter.setPen(QPen(Qt.blue, 2, Qt.DashLine))
        # painter.drawLine(QPointF(0,0), self.get_dipole_vector() * MOLECULE_SIZE * 1.5)

        painter.restore()

# --- Simulation Widget ---
class MicrowaveWidget(QWidget):
    def __init__(self):
        super().__init__()
        self.molecules = []
        self.microwave_on = False
        self.simulation_time = 0.0
        self.current_E_field = 0.0
        self.temperature_factor = 1.0 # Represents thermal agitation level
        self.target_temperature_factor = 1.0

        self.init_molecules()
        self.init_timer()

    def init_molecules(self):
        self.molecules = []
        for _ in range(NUM_MOLECULES):
            x = random.uniform(MOLECULE_SIZE, WINDOW_WIDTH - MOLECULE_SIZE)
            y = random.uniform(MOLECULE_SIZE, WINDOW_HEIGHT - MOLECULE_SIZE)
            angle = random.uniform(0, 360)
            self.molecules.append(WaterMolecule(x, y, angle))

    def init_timer(self):
        self.timer = QTimer(self)
        self.timer.timeout.connect(self.update_simulation)
        self.timer.start(SIM_UPDATE_MS)

    def toggle_microwave(self, state):
        self.microwave_on = state
        if self.microwave_on:
            self.target_temperature_factor = HEATING_FACTOR_INCREASE
            print("Microwave ON")
        else:
            self.target_temperature_factor = 1.0 # Cool down to base level
            self.current_E_field = 0.0 # Ensure field is off immediately
            print("Microwave OFF")

    def update_simulation(self):
        self.simulation_time += DT

        # Update E-field based on microwave state
        if self.microwave_on:
            # E(t) = E0 * cos(omega*t) along x-axis
            self.current_E_field = SIM_E0_MAX * math.cos(SIM_OMEGA * self.simulation_time)
        else:
            self.current_E_field = 0.0

        # Smoothly adjust temperature factor towards target
        if abs(self.temperature_factor - self.target_temperature_factor) > 0.01:
             diff = self.target_temperature_factor - self.temperature_factor
             self.temperature_factor += diff * TEMPERATURE_RAMP_RATE
        else:
            self.temperature_factor = self.target_temperature_factor


        # Update each molecule
        for molecule in self.molecules:
            molecule.update(DT, self.current_E_field, self.microwave_on, self.temperature_factor)

        # Trigger repaint
        self.update()

    # --- CORRECTED paintEvent Method ---
    def paintEvent(self, event):
        painter = QPainter(self)
        painter.setRenderHint(QPainter.Antialiasing)

        # Background
        painter.fillRect(self.rect(), QColor(240, 240, 255)) # Light blue background

        # Draw Molecules
        for molecule in self.molecules:
            molecule.draw(painter)

        # Draw E-field indicator (optional)
        if self.microwave_on:
            arrow_length = self.current_E_field * 2 # Scale for visibility
            arrow_y = 30 # Keep Y as integer is fine
            painter.setPen(QPen(QColor(0, 150, 0), 2))
            painter.setBrush(QBrush(QColor(0, 150, 0)))

            # Base line - Use integer casting just in case (though // should make it int)
            base_start_x = WINDOW_WIDTH // 2 - 100
            base_end_x = WINDOW_WIDTH // 2 + 100
            painter.drawLine(int(base_start_x), int(arrow_y), int(base_end_x), int(arrow_y))

            # Arrow head indicating direction and strength
            start_x = WINDOW_WIDTH // 2 # This is likely an int or float needing casting
            end_x = start_x + arrow_length # end_x is definitely float

            # *** FIX: Cast coordinates to integers ***
            painter.drawLine(int(start_x), int(arrow_y), int(end_x), int(arrow_y))

            # Arrow tips
            if abs(arrow_length) > 1:
                direction = 1 if arrow_length > 0 else -1
                tip_len = 8
                tip_angle = math.radians(20)

                # *** FIX: Cast coordinates to integers ***
                tip1_x = int(end_x - direction * tip_len * math.cos(tip_angle))
                tip1_y = int(arrow_y + tip_len * math.sin(tip_angle))
                tip2_x = int(end_x - direction * tip_len * math.cos(tip_angle)) # x is same for both tips
                tip2_y = int(arrow_y - tip_len * math.sin(tip_angle)) # y-coordinate for the other tip line

                # Draw the two lines forming the arrowhead
                painter.drawLine(int(end_x), int(arrow_y), tip1_x, tip1_y)
                painter.drawLine(int(end_x), int(arrow_y), tip2_x, tip2_y) # Use tip2_x, tip2_y

            painter.setFont(QFont("Arial", 10))
            # Position text relative to the base line start
            painter.drawText(base_start_x, arrow_y - 10, "E-Field (x-axis)")


        # Draw Status Text
        painter.setPen(Qt.black)
        painter.setFont(QFont("Arial", 12))
        status = "Microwave: ON" if self.microwave_on else "Microwave: OFF"
        painter.drawText(10, 20, status)
        painter.drawText(10, 40, f"Agitation Factor: {self.temperature_factor:.2f}")
    # --- End of corrected paintEvent ---


# --- Main Application Window ---
class MainWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Microwave Water Heating Simulation")
        # Make window slightly larger vertically to accommodate button without overlap
        self.setGeometry(100, 100, WINDOW_WIDTH, WINDOW_HEIGHT + 60) # Extra height for button

        self.layout = QVBoxLayout()
        self.setLayout(self.layout)

        # Simulation Area
        self.simulation_widget = MicrowaveWidget()
        # Make sure the widget takes up available space
        self.simulation_widget.setSizePolicy(QSizePolicy.Expanding, QSizePolicy.Expanding)
        self.layout.addWidget(self.simulation_widget)

        # Control Button
        self.toggle_button = QPushButton("Turn Microwave ON")
        self.toggle_button.setCheckable(True) # Make it a toggle button
        self.toggle_button.clicked.connect(self.on_toggle_button)
        self.layout.addWidget(self.toggle_button) # Add button below simulation

    def on_toggle_button(self, checked):
        self.simulation_widget.toggle_microwave(checked)
        if checked:
            self.toggle_button.setText("Turn Microwave OFF")
        else:
            self.toggle_button.setText("Turn Microwave ON")


# --- Run the Application ---
if __name__ == '__main__':
    app = QApplication(sys.argv)
    main_window = MainWindow()
    main_window.show()
    sys.exit(app.exec_())
```
