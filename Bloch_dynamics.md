```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation

# ======================
# Physical Parameters (Natural Units)
# ======================
a = 1.0            # Lattice constant
J = 1.0            # Hopping energy
E_field = 2.0      # Electric field (adjusted for better visualization)
e = 1.0            # Elementary charge
ħ = 1.0            # Reduced Planck constant
num_frames = 300    # Increased number of frames
T = 4*np.pi        # Simulation time for 2 full oscillations

# ======================
# Derived Parameters
# ======================
ω_B = e*E_field*a/ħ  # Bloch frequency
v_max = 2*J*a/ħ      # Maximum group velocity
x_max = 2*J/(e*E_field)  # Theoretical oscillation amplitude

# ======================
# Computational Setup
# ======================
x = np.linspace(0, 10*a, 1000)  # Extended spatial grid
times = np.linspace(0, T, num_frames)
dt = times[1] - times[0]

# Periodic potential modulation
u_x = 1 + 0.5*np.sin(2*np.pi*x/a)  # Adjusted amplitude for better visibility

# Pre-calculate dynamics using analytical solutions
k_values = (e*E_field/ħ) * times % (2*np.pi/a)
phase_terms = (2*J/(e*E_field*a)) * (1 - np.cos(e*E_field*a*times/ħ))
positions = (2*J/(e*E_field)) * (1 - np.cos(ω_B*times))

# ======================
# Visualization Setup
# ======================
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(12, 8), gridspec_kw={'height_ratios': [2, 1]})

# Wavefunction plot
line1, = ax1.plot(x/a, np.zeros_like(x), lw=1.5, color='tab:blue')
ax1.set_ylim(-2.2, 2.2)
ax1.set_ylabel('Re[ψ(x,t)]', fontsize=12)
ax1.grid(True, alpha=0.3)
ax1.set_title(f'Bloch Wave Dynamics (E = {E_field} a.u.)', fontsize=14)

# Position plot
line2, = ax2.plot([], [], 'r-', lw=1.5)
ax2.set_xlim(0, T)
ax2.set_ylim(-0.2*x_max, 2.2*x_max)
ax2.set_xlabel('Time (a.u.)', fontsize=12)
ax2.set_ylabel('Position (a.u.)', fontsize=12)
ax2.grid(True, alpha=0.3)
ax2.set_title('Semiclassical Trajectory', fontsize=14)
ax2.annotate(f'Theoretical amplitude: {x_max:.2f} a.u.',
             xy=(0.7, 0.9), xycoords='axes fraction',
             fontsize=10, color='darkred')

# Time indicator
time_text = ax2.text(0.02, 0.9, '', transform=ax2.transAxes, fontsize=10)

def animate(i):
    # Current crystal momentum (wrapped in BZ)
    k = k_values[i]
    
    # Wavefunction calculation
    plane_wave = np.cos(k*x - phase_terms[i])
    psi_real = u_x * plane_wave
    
    # Update plots
    line1.set_ydata(psi_real)
    line2.set_data(times[:i+1], positions[:i+1])
    time_text.set_text(f't = {times[i]:.2f} a.u. ({(times[i]/T)*100:.1f}% of period)')
    
    # Dynamic color change for position plot
    if positions[i] > 0.9*x_max:
        line2.set_color('darkred')
    elif positions[i] < -0.9*x_max:
        line2.set_color('darkblue')
    else:
        line2.set_color('tab:red')
    
    return line1, line2, time_text

# Initialize animation
def init():
    line1.set_ydata(np.zeros_like(x))
    line2.set_data([], [])
    time_text.set_text('')
    return line1, line2, time_text

# Create animation
ani = FuncAnimation(fig, animate, frames=num_frames,
                    init_func=init, interval=30, blit=True)

plt.tight_layout()
plt.show()
```

# Bloch Dynamics in Crystalline Solids: From Theory to Numerical Simulation

In this post, we explore the fascinating phenomenon of **Bloch oscillations**—an effect arising when electrons move through a crystalline lattice under the influence of a constant electric field. We will derive key relationships from first principles and then walk through a complete Python simulation that animates the dynamics of Bloch waves and their corresponding semiclassical trajectories.

Below, we break down the simulation code into its theoretical foundations and computational steps.

## 1. Theoretical Background

### 1.1 Bloch’s Theorem and Bloch Oscillations

Bloch’s theorem tells us that in a periodic potential, such as that found in a crystalline solid, the electronic wavefunctions can be written as a plane wave multiplied by a periodic function. When an external electric field is applied, the crystal momentum $k$ evolves over time. Semiclassically, this is given by:

$$
\hbar \frac{dk}{dt} = -eE
$$

Assuming a constant field $E$, integrating yields:

$$
k(t) = k(0) - \frac{eE}{\hbar}t
$$

Due to the periodicity of the Brillouin zone, $k(t)$ “wraps around” leading to an oscillatory behavior in the electron dynamics—this is known as **Bloch oscillations**.

### 1.2 Semiclassical Equations of Motion

Within the tight-binding model, the energy dispersion is given by:

$$
E(k) = -2J \cos(ka)
$$

where:

- $J$ is the hopping energy,  
- $a$ is the lattice constant.

The group velocity of the electron is:

$$
v_g = \frac{1}{\hbar} \frac{dE(k)}{dk} = \frac{2Ja}{\hbar} \sin(ka)
$$

The maximum velocity occurs when $\sin(ka) = 1$:

$$
v_{\text{max}} = \frac{2Ja}{\hbar}
$$

Integrating the group velocity over time gives the electron's position. In our simulation, the position is expressed as:

$$
x(t) = \frac{2J}{eE} \left[1 - \cos\left( \frac{eEa}{\hbar} t \right)\right]
$$

This oscillatory behavior captures the essence of **Bloch dynamics**.

### 1.3 Derived Parameters in the Simulation

Several key parameters are computed from the above theory:

**Bloch Frequency:**

$$
\omega_B = \frac{eEa}{\hbar}
$$

**Oscillation Amplitude:**

$$
x_{\text{max}} = \frac{2J}{eE}
$$

These expressions are used to set up the simulation time, space grid, and wavefunction evolution.

## 2. Numerical Simulation Setup

The code is organized to simulate and animate the dynamics of an electron wavepacket in a one-dimensional crystal.

### 2.1 Physical Parameters and Natural Units

The simulation begins by defining physical constants in natural units (where $\hbar = 1$, $e = 1$, etc.):

```python
a = 1.0            # Lattice constant
J = 1.0            # Hopping energy
E_field = 2.0      # Electric field
e = 1.0            # Elementary charge
ħ = 1.0            # Reduced Planck constant
```

Setting these values to unity (or simple numbers) simplifies our calculations and makes the physical insights clearer.

### 2.2 Computational Grid and Potential Modulation

The spatial domain is discretized using:

```python
x = np.linspace(0, 10*a, 1000)
```

Setting these values to unity (or simple numbers) simplifies our calculations and makes the physical insights clearer.

### 2.2 Computational Grid and Potential Modulation

The spatial domain is discretized using:


```python
x = np.linspace(0, 10*a, 1000)
```

This grid is chosen to be wide enough to capture the extended behavior of the wavefunction.

A periodic modulation is applied to mimic the effect of a crystalline potential:

```python
u_x = 1 + 0.5*np.sin(2*np.pi*x/a)
```
Here, $u(x)$ modulates the amplitude of the wavefunction, representing the periodic potential in the lattice.

### 2.3 Analytical Solution Components

**For the time evolution:**

**Time Grid:**  
The simulation runs over two full Bloch oscillations:

```python
T = 4*np.pi
times = np.linspace(0, T, num_frames)
```

**Crystal Momentum $k$:**  
The evolution of $k$ is calculated and wrapped within the Brillouin zone.

```python
k_values = (e*E_field/ħ) * times % (2*np.pi/a)
```

**Phase Terms:**  
The phase accumulated by the wavefunction is derived from the tight-binding dispersion.

```python
phase_terms = (2*J/(e*E_field*a)) * (1 - np.cos(e*E_field*a*times/ħ))
```

**Position (Semiclassical Trajectory):**  
Derived from integrating the group velocity:

```python
positions = (2*J/(e*E_field)) * (1 - np.cos(ω_B*times))
```

## 3. Visualization and Animation

The simulation uses Matplotlib’s `FuncAnimation` to create a dynamic visualization comprising two panels:

### 3.1 Wavefunction Evolution

The top subplot displays the real part of the Bloch wave:

The wavefunction is a cosine function modulated by the periodic potential $u(x)$.  
At each frame, the phase of the cosine wave is updated according to the current $k$ and phase term.

### 3.2 Semiclassical Trajectory

The bottom subplot traces the semiclassical trajectory:

It plots the evolution of the electron’s position as a function of time.  
A dynamic color change is implemented to indicate when the position is near the theoretical bounds (close to $\pm x_{\text{max}}$).

## 4. Detailed Code Walkthrough

Let’s step through the code to understand each section in detail.

### 4.1 Importing Libraries

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
```

## 3. Visualization and Animation

The simulation uses Matplotlib’s `FuncAnimation` to create a dynamic visualization comprising two panels:

### 3.1 Wavefunction Evolution

The top subplot displays the real part of the Bloch wave:

The wavefunction is a cosine function modulated by the periodic potential $u(x)$.  
At each frame, the phase of the cosine wave is updated according to the current $k$ and phase term.

### 3.2 Semiclassical Trajectory

The bottom subplot traces the semiclassical trajectory:

It plots the evolution of the electron’s position as a function of time.  
A dynamic color change is implemented to indicate when the position is near the theoretical bounds (close to $\pm x_{\text{max}}$).

## 4. Detailed Code Walkthrough

Let’s step through the code to understand each section in detail.

### 4.1 Importing Libraries


```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
```
We use:

- NumPy for numerical operations and array handling.  
- Matplotlib for plotting and animations.  
- `FuncAnimation` to update the plots dynamically.

### 4.2 Parameter Setup and Derived Quantities

The physical and computational parameters are defined next:

Physical constants such as the lattice constant $a$, hopping energy $J$, electric field $E$, and natural units are set.  
Derived parameters like the Bloch frequency $\omega_B$, maximum velocity $v_{\text{max}}$, and oscillation amplitude $x_{\text{max}}$ are computed directly from the theory.

### 4.3 Computation of Wavefunction Dynamics

A spatial grid `x` is defined over a domain $[0, 10a]$ and the potential modulation $u(x)$ is introduced:

```python
u_x = 1 + 0.5*np.sin(2*np.pi*x/a)
```

The analytical evolution for k, phase, and positions are computed over a time grid:

```python
k_values = (e*E_field/ħ) * times % (2*np.pi/a)
phase_terms = (2*J/(e*E_field*a)) * (1 - np.cos(e*E_field*a*times/ħ))
positions = (2*J/(e*E_field)) * (1 - np.cos(ω_B*times))
```

These equations reflect our earlier derivations and ensure that the simulation follows the predicted dynamics.

### 4.4 Animation Routine and Dynamic Plot Updating

The animation is handled by the `animate` function:

```python
def animate(i):
    k = k_values[i]
    plane_wave = np.cos(k*x - phase_terms[i])
    psi_real = u_x * plane_wave  # Modulated wavefunction
    
    # Update the wavefunction plot
    line1.set_ydata(psi_real)
    # Update the semiclassical trajectory plot
    line2.set_data(times[:i+1], positions[:i+1])
    time_text.set_text(f't = {times[i]:.2f} a.u. ({(times[i]/T)*100:.1f}% of period)')
    
    # Dynamic color changes near oscillation amplitude boundaries
    if positions[i] > 0.9*x_max:
        line2.set_color('darkred')
    elif positions[i] < -0.9*x_max:
        line2.set_color('darkblue')
    else:
        line2.set_color('tab:red')
    
    return line1, line2, time_text
```
**Key Points:**

- The wavefunction is updated at each frame by recalculating the cosine term with the current crystal momentum $k$ and phase.  
- The trajectory is plotted cumulatively, illustrating how the electron moves over time.  
- **Dynamic feedback:** The plot color changes as the electron approaches the theoretical amplitude limits.

The animation is initialized with:

```python
def init():
    line1.set_ydata(np.zeros_like(x))
    line2.set_data([], [])
    time_text.set_text('')
    return line1, line2, time_text
```
And finally, the animation is created with:
``` python
ani = FuncAnimation(fig, animate, frames=num_frames,
                    init_func=init, interval=30, blit=True)
plt.tight_layout()
plt.show()
```
This part ties together the plots and makes the simulation interactive.

## 5. Conclusion

In this blog post, we have connected theory with practice by simulating Bloch oscillations in a crystalline solid. We derived the key equations from Bloch’s theorem and the tight-binding model, computed essential parameters, and implemented an animation in Python. This simulation not only visualizes the real-space oscillations of an electron wavepacket but also demonstrates how semiclassical physics can be captured in numerical experiments.

Understanding and simulating such phenomena are critical for modern condensed matter physics and materials science, as they provide insights into electron transport and the behavior of solids under external fields.


![image](https://github.com/user-attachments/assets/f4886c03-5d1c-4465-a6d4-d4fbc87e58dd)










