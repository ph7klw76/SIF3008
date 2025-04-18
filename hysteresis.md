
# Simulating First‑Order Ferroelectric Hysteresis in Python

*A step‑by‑step walk‑through of the physics, the mathematics and the GUI that brings it all to life.*

---

### 1  |  Why ferroelectrics show “square” loops

A ferroelectric crystal possesses two (or more) energetically‑equivalent directions in which its electric polarization $P$ can point. At zero field the crystal picks one of them, just as an iron magnet chooses a magnetization direction. Sweep an external field $E$ and the polarization stays put—until a critical field $E_c$ punches it over the barrier into the opposite state. The result is a hysteresis loop: after one full $E$ cycle the crystal remembers its history and ends up with a finite remanent polarization $P_r$.

Whether the phase transition that gives rise to this double‑well potential is first‑order (abrupt) or second‑order (continuous) is fixed by the material’s Landau free‑energy coefficients. BaTiO₃, Pb(Zr,Ti)O₃ and many organic ferroelectrics are first‑order: a cubic term with negative coefficient creates a metastable well and a latent heat, producing the experimentally familiar, nearly rectangular loops.

---

### 2  |  Landau–Devonshire free energy: where the double well comes from

#### 2.1 Order‑parameter expansion

Landau’s mean‑field philosophy is to write the Gibbs free‑energy density $G$ as a power series in the order parameter—in a ferroelectric, the scalar polarisation $P$:

tag{2.1}

$$
G(P,E,T) = \frac{1}{2} \alpha(T)P^2 + \frac{1}{4} \beta P^4 + \frac{1}{6} \gamma P^6 - EP. 
$$

$$
\alpha(T) = \alpha_0 (T - T_0)
$$

encodes how thermal agitation destabilises the polar phase.

$\beta$ and $\gamma$ are temperature‑independent (to lowest order) “stiffness” coefficients that must keep $G$ bounded for large $\lvert P \rvert$.

The final term $-EP$ is the Legendre transform that imposes an external field $E$.

---

#### 2.2 First‑ versus second‑order criteria

A second‑order transition requires every even power to have a positive coefficient so that the single‑minimum at $P = 0$ bifurcates continuously.  
A first‑order transition instead appears when the quartic term turns negative but stability is rescued by the positive sixth‑order term:

tag{2.2}

$$
\beta < 0, \quad \gamma > 0 \
$$

This combination produces two degenerate minima below a certain temperature, i.e. the celebrated “W”‑shaped potential *Wikipedia TU Graz*.

---

#### 2.3 Minima and metastability

Set $\partial G / \partial P = 0$ to obtain the equation of state

tag{2.3}

$$
E = \alpha P + \beta P^3 + \gamma P^5. \
$$

For a fixed $T < T_c$ and $\beta < 0$ this curve is S‑shaped:

- Outer branches: stable domains (polarisation “up” or “down”).
- Middle branch: unstable (negative curvature).

At zero field the system chooses either well. As you sweep $E$ forward, the chosen minimum remains a local minimum until a spinodal point where

tag{2.4}

$$
\frac{\partial^2 G}{\partial P^2} = \alpha + 3\beta P^2 + 5\gamma P^4 = 0 
$$

collapses. The sudden jump to the opposite branch is the microscopic origin of the macroscopic coercive field $E_c$.

---

#### 2.4 Temperature slider ⇒ loop width

Increasing $\alpha(T)$ (moving the GUI’s $\hat{\alpha}$‑slider) shallows the wells and lowers the spinodal, so the loop narrows; at $\hat{\alpha} \approx 1$ the two minima merge and hysteresis vanishes—exactly what you see when warming BaTiO₃ through its Curie point.

---

### 3  |  Landau–Khalatnikov dynamics: turning statics into hysteresis

#### 3.1 Time‑dependent Ginzburg–Landau

Landau’s static theory can’t say how fast domains switch. For a non‑conserved order parameter one assumes overdamped relaxational kinetics,

tag{3.1}

$$
\rho \frac{dP}{dt} = -\frac{\partial G}{\partial P}, \
$$

where $\rho$ (units F m⁻¹ s) is a viscous damping constant. This postulate, due to L. D. Landau and I. Khalatnikov (1954), is equivalent to minimising $G$ in the presence of a Rayleigh dissipation function. Substituting Eq. (2.1) gives

tag{3.2}

$$
\rho \frac{dP}{dt} = E - \alpha P - \beta P^3 - \gamma P^5. \
$$

Equation (3.2) is the Landau–Khalatnikov (LK) equation that every modern phase‑field and SPICE ferroelectric model ultimately solves *JKPS ScienceDirect*.

---

#### 3.2 Non‑dimensionalisation (what the code really integrates)

Define characteristic scales

$$
P_0 = \frac{\lvert \beta \rvert}{2\gamma}, \quad
E_0 = \frac{\lvert \beta \rvert^{5/2}}{(2)^{3/2} \gamma^{3/2}}, \quad
t_0 = \frac{\rho}{\gamma E_0}
$$

and reduced variables

$$
p = \frac{P}{P_0}, \quad e = \frac{E}{E_0}, \quad \tau = \frac{t}{t_0}, \quad \hat{\alpha} = \frac{2\alpha\gamma}{\lvert \beta \rvert^2}
$$

Equation (3.2) shrinks to the elegant one‑liner used in the script:

tag{3.3}

$$
\frac{dp}{d\tau} = e(\tau) - \hat{\alpha} p + p^3 - p^5. \
$$

All material details hide in two constants: $\hat{\alpha}(T)$ and the sign of $\beta$ (here $-1$). Everything plotted in the GUI is therefore generic; you retrofit experimental units afterwards by multiplying $p$ by $P_0$ and $e$ by $E_0$.

---

#### 3.3 Numerical integration: 4‑th order Runge–Kutta

The LK equation is stiff when $p$ approaches the walls of the well, but an explicit 4‑stage Runge–Kutta (RK4) remains stable if the timestep satisfies

$$
\Delta t \lesssim 5 \times 10^{-3} f^{-1},
$$

where $f$ is the drive frequency. The code therefore builds an equally‑spaced time array and loops:

- Evaluate RHS at four staggered points $k_{1…4}$
- Take the weighted average to advance $p$
- Clip the result if it wanders beyond $\lvert p \rvert = 10$ (purely numerical guard)

Because $\rho$ only sets the overall time scale $t_0$, any drive frequency in reduced units is legitimate.

---

#### 3.4 Why hysteresis emerges automatically

If you set $\Delta t$ so small that $dp/d\tau \approx 0$ at every step, Eq. (3.3) collapses back to the static curve (2.3) and the loop degenerates into its ideal, infinitely thin rectangle. At finite frequency $p$ lags behind $e$—just like a damped pendulum driven near its natural frequency—and the loop acquires a measurable area proportional to energy dissipation per cycle.

---

#### 3.5 Reading the GUI in dynamical terms

- Drive frequency slider → increases the phase lag $\Delta\phi \approx \tan^{-1}(\omega \tau_0)$, shrinking loop area.
- $E_\text{max}$ slider → if $e_{\max} < E_c / E_0$ you obtain a minor loop trapped in one well; once $e_{\max}$ exceeds that threshold the trajectory jumps branches and the red point sweeps out the full loop.
- Animation → the red dot literally integrates Eq. (3.3) in real time, while the arrow grid visualises the changing mixture of $+p$ and $-p$ microscopic domains (sigmoid mapping).

---

### Key take‑aways

| Concept                   | Equation            | What you see                                     |
|--------------------------|---------------------|--------------------------------------------------|
| Double‑well potential     | Eq. (2.1) with β < 0 | Two minima in $G(P)$ ⇒ bistability               |
| Static equation of state | Eq. (2.3)           | S‑curve, coercive field from spinodal            |
| Dynamic LK equation      | Eq. (3.2)/(3.3)     | Time evolution, rate‑dependent loop              |
| RK4 integrator           | Code lines 59–77    | Stable explicit solution                         |
| Pr, Ec extraction        | Sign‑change interp. | Green stars on the plot                          |

Armed with this physical roadmap, every widget in the Tkinter window ceases to be a black box and becomes a tangible lever on the underlying thermodynamics.



![image](https://github.com/user-attachments/assets/71899ca6-a768-44f0-bcd8-1ebae11f9b88)

```python
import tkinter as tk
from tkinter import ttk
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg
from matplotlib.figure import Figure
import matplotlib.gridspec as gridspec

# Keep the original simulation function
def simulate_hysteresis(alpha_hat=0.1, e_max=0.2, freq=0.01, dt=1e-3, n_cycles=2, p0=0.0):
    """
    Integrate the reduced Landau–Khalatnikov ODE for a first‑order ferroelectric:
        dp/dt = e(t) - α̂ p + p³ - p⁵
    with a sinusoidal driving field e(t) = e_max * sin(2π f t).

    Parameters
    ----------
    alpha_hat : float
        Reduced temperature coefficient (α̂ = 2αγ/|β|²). Must be > 0 for 1st order.
    e_max : float
        Peak reduced electric field.
    freq : float
        Drive frequency (cycles per unit time).
    dt : float
        Time step for the RK4 integrator.
    n_cycles : int
        Number of drive cycles to simulate.
    p0 : float
        Initial polarization.

    Returns
    -------
    t : ndarray
        Time samples.
    E : ndarray
        Electric field samples.
    P : ndarray
        Polarisation samples.
    """
    if alpha_hat <= 0:
        print("Warning: alpha_hat should be > 0 for a first-order transition model.")
        # Prevent division by zero or unstable behavior later if needed,
        # although this specific ODE might handle it. Clamp it?
        alpha_hat = 1e-6 # Small positive value if needed

    omega = 2 * np.pi * freq
    t_end = n_cycles / freq
    # Ensure simulation runs long enough for full cycles
    num_steps = int(np.ceil(t_end / dt))
    t = np.linspace(0, t_end, num_steps + 1)
    actual_dt = t[1] - t[0] # Use the actual dt from linspace

    # Allocate arrays
    p = np.zeros_like(t)
    p[0] = p0 # Set initial condition

    # Right‑hand side of the ODE
    def rhs(p_val, e_val):
        # Clamp p_val to prevent potential overflow with high powers if needed
        p_val_clamped = np.clip(p_val, -10, 10) # Adjust bounds as necessary
        # The p^3 - p^5 term represents the double-well potential
        # For a first-order transition (alpha_hat > 0), this form is standard.
        # The potential is -(alpha_hat/2)p^2 + (1/4)p^4 - (1/6)p^6
        return e_val - alpha_hat * p_val_clamped + p_val_clamped**3 - p_val_clamped**5

    # Fourth‑order Runge–Kutta time stepping
    for i in range(len(t) - 1):
        e1 = e_max * np.sin(omega * t[i])
        k1 = rhs(p[i], e1)

        e2 = e_max * np.sin(omega * (t[i] + actual_dt / 2))
        k2 = rhs(p[i] + 0.5 * actual_dt * k1, e2)
        k3 = rhs(p[i] + 0.5 * actual_dt * k2, e2)

        e4 = e_max * np.sin(omega * (t[i] + actual_dt))
        k4 = rhs(p[i] + actual_dt * k3, e4)

        p_next = p[i] + actual_dt * (k1 + 2 * k2 + 2 * k3 + k4) / 6

        # Basic stability check (optional)
        if not np.isfinite(p_next):
            print(f"Warning: Non-finite polarization detected at step {i}. Stopping simulation.")
            # Trim arrays to the last valid point
            t = t[:i+1]
            p = p[:i+1]
            break
        p[i + 1] = p_next


    E = e_max * np.sin(omega * t)
    return t, E, p

# --- Find Pr and Ec ---
def find_pr_ec(t, E, P, freq):
    """Find Remanent Polarization (Pr) and Coercive Field (Ec) from data."""
    pr_values = []
    ec_values = []
    p_max = np.max(np.abs(P))
    if p_max < 1e-6: # Avoid issues if polarization is essentially zero
        return 0, 0, 0, 0, p_max

    # Look only at the last cycle for stable loop values
    t_one_cycle = 1.0 / freq
    # Ensure we don't index beyond the array start if t is very short
    start_index_last_cycle = max(0, np.searchsorted(t, t[-1] - t_one_cycle) -1)
    last_cycle_mask = np.zeros_like(t, dtype=bool)
    last_cycle_mask[start_index_last_cycle:] = True

    # Apply the mask, ensure there are enough points
    t_lc = t[last_cycle_mask]
    E_lc = E[last_cycle_mask]
    P_lc = P[last_cycle_mask]

    if len(t_lc) < 3: # Need enough points for interpolation/finding crossings
         return 0, 0, 0, 0, p_max

    # Find Pr (P when E crosses 0)
    # Find indices where E crosses zero
    zero_crossings_E = np.where(np.diff(np.sign(E_lc)))[0]

    for idx in zero_crossings_E:
         # Check if it's a real crossing (not just touching zero)
         # Need to compare signs on both sides relative to the crossing index `idx`
         if idx > 0 and (idx + 1) < len(E_lc): # Ensure valid indices
            if np.sign(E_lc[idx]) != np.sign(E_lc[idx+1]): # Check sign change
                 # Interpolate P at E=0
                 e1, e2 = E_lc[idx], E_lc[idx+1]
                 p1, p2 = P_lc[idx], P_lc[idx+1]
                 if not np.isclose(e1, e2): # Avoid division by zero
                     pr_interp = p1 + (p2 - p1) * (0 - e1) / (e2 - e1)
                     pr_values.append(pr_interp)

    # Find Ec (E when P crosses 0, during switching)
    # Find indices where P crosses zero
    zero_crossings_P = np.where(np.diff(np.sign(P_lc)))[0]

    # Need to know direction of E field during P crossing for +/- Ec
    for idx in zero_crossings_P:
        # Check if P is actually switching significantly & crossing zero
        if idx > 0 and (idx + 1) < len(P_lc): # Ensure valid indices
            if np.sign(P_lc[idx]) != np.sign(P_lc[idx+1]): # Check sign change
                # Interpolate E at P=0
                p1, p2 = P_lc[idx], P_lc[idx+1]
                e1, e2 = E_lc[idx], E_lc[idx+1]
                if not np.isclose(p1, p2): # Avoid division by zero
                    ec_interp = e1 + (e2 - e1) * (0 - p1) / (p2 - p1)
                    ec_values.append(ec_interp)


    # Process found values (average positive and negative magnitudes)
    pos_pr = [p for p in pr_values if p > 0]
    neg_pr = [p for p in pr_values if p < 0]
    pos_ec = [e for e in ec_values if e > 0]
    neg_ec = [e for e in ec_values if e < 0]

    # Calculate means carefully, handling cases where none are found
    Pr_pos = np.mean(pos_pr) if pos_pr else 0
    Pr_neg = np.mean(neg_pr) if neg_pr else 0
    Ec_pos = np.mean(pos_ec) if pos_ec else 0
    Ec_neg = np.mean(neg_ec) if neg_ec else 0

    # Often Pr and Ec are quoted as positive magnitudes
    # Pr_mag = (Pr_pos + abs(Pr_neg)) / 2 if (Pr_pos != 0 and Pr_neg != 0) else (Pr_pos or abs(Pr_neg) or 0)
    # Ec_mag = (Ec_pos + abs(Ec_neg)) / 2 if (Ec_pos != 0 and Ec_neg != 0) else (Ec_pos or abs(Ec_neg) or 0)


    # Return the distinct positive and negative values found, and overall P_max
    return Pr_pos, Pr_neg, Ec_pos, Ec_neg, p_max


# --- Tkinter Application Class ---
class FerroelectricApp:
    def __init__(self, master):
        self.master = master
        master.title("Ferroelectric Hysteresis & Domain Switching Simulation")
        master.geometry("1000x750") # Increased height slightly for equation label

        self.master.protocol("WM_DELETE_WINDOW", self._quit) # Handle closing window

        # --- Simulation Parameters ---
        self.alpha_hat = tk.DoubleVar(value=0.1)
        self.e_max = tk.DoubleVar(value=0.2)
        self.freq = tk.DoubleVar(value=0.01)
        self.n_cycles = tk.IntVar(value=2)
        self.dt = tk.DoubleVar(value=1e-3)

        # --- Simulation Data ---
        self.t_sim = None
        self.E_sim = None
        self.P_sim = None
        self.Pr_pos = 0
        self.Pr_neg = 0
        self.Ec_pos = 0
        self.Ec_neg = 0
        self.P_max = 0

        # --- Animation State ---
        self.anim_running = False
        self.anim_job = None
        self.current_step = 0
        self.anim_delay = 20  # ms between frames

        # --- Domain Visualization Parameters ---
        self.domain_grid_size = 10
        self.domain_X, self.domain_Y = np.meshgrid(
            np.arange(0, self.domain_grid_size),
            np.arange(0, self.domain_grid_size)
        )
        # Initialize domain orientations (U=0, V=1 for 'up')
        self.domain_U = np.zeros((self.domain_grid_size, self.domain_grid_size))
        self.domain_V = np.ones((self.domain_grid_size, self.domain_grid_size)) * 0.1 # Small initial state


        # --- GUI Layout ---
        # Top frame for controls
        self.control_frame = ttk.Frame(master, padding="10")
        self.control_frame.pack(side=tk.TOP, fill=tk.X, pady=(0, 5)) # Added bottom padding

        # Frame for the equation (NEW)
        self.equation_frame = ttk.Frame(master, padding="5")
        self.equation_frame.pack(side=tk.TOP, fill=tk.X)

        # Bottom frame for plots
        self.plot_frame = ttk.Frame(master, padding="5")
        self.plot_frame.pack(side=tk.TOP, fill=tk.BOTH, expand=True) # Changed side to TOP

        # --- Controls (in control_frame) ---
        row_idx = 0
        ttk.Label(self.control_frame, text="α̂ (Temp Coeff):").grid(row=row_idx, column=0, sticky=tk.W, padx=5)
        ttk.Scale(self.control_frame, from_=0.01, to=1.0, variable=self.alpha_hat, orient=tk.HORIZONTAL, length=150).grid(row=row_idx, column=1, padx=5)
        ttk.Label(self.control_frame, textvariable=self.alpha_hat, width=5).grid(row=row_idx, column=2, sticky=tk.W)

        ttk.Label(self.control_frame, text="E_max (Peak Field):").grid(row=row_idx, column=3, sticky=tk.W, padx=5)
        ttk.Scale(self.control_frame, from_=0.01, to=1.0, variable=self.e_max, orient=tk.HORIZONTAL, length=150).grid(row=row_idx, column=4, padx=5)
        ttk.Label(self.control_frame, textvariable=self.e_max, width=5).grid(row=row_idx, column=5, sticky=tk.W)

        row_idx += 1
        ttk.Label(self.control_frame, text="Frequency:").grid(row=row_idx, column=0, sticky=tk.W, padx=5)
        ttk.Scale(self.control_frame, from_=0.001, to=0.1, variable=self.freq, orient=tk.HORIZONTAL, length=150).grid(row=row_idx, column=1, padx=5)
        ttk.Label(self.control_frame, textvariable=self.freq, width=5).grid(row=row_idx, column=2, sticky=tk.W)

        ttk.Label(self.control_frame, text="Cycles:").grid(row=row_idx, column=3, sticky=tk.W, padx=5)
        ttk.Scale(self.control_frame, from_=1, to=10, variable=self.n_cycles, orient=tk.HORIZONTAL, length=150, command=lambda v: self.n_cycles.set(int(float(v)))).grid(row=row_idx, column=4, padx=5) # Ensure int
        ttk.Label(self.control_frame, textvariable=self.n_cycles, width=5).grid(row=row_idx, column=5, sticky=tk.W)

        # Buttons
        row_idx += 1
        self.run_button = ttk.Button(self.control_frame, text="Run Simulation", command=self._run_simulation)
        self.run_button.grid(row=row_idx, column=0, columnspan=2, pady=10, padx=5)

        self.anim_button = ttk.Button(self.control_frame, text="Start Animation", command=self._toggle_animation, state=tk.DISABLED)
        self.anim_button.grid(row=row_idx, column=2, columnspan=2, pady=10, padx=5)

        # Status Label
        self.status_var = tk.StringVar(value="Adjust parameters and run simulation.")
        ttk.Label(self.control_frame, textvariable=self.status_var, relief=tk.SUNKEN, anchor=tk.W).grid(row=row_idx, column=4, columnspan=3, sticky=tk.EW, padx=5)

        # --- Equation Label (in equation_frame) (NEW) ---
        # Using Unicode characters for better appearance: α̂ (\u03b1\u0302), ³ (\u00b3), ⁵ (\u2075)
        equation_text = "Governing Equation (Reduced Landau-Khalatnikov):\n" \
                        "dp/dt = e(t) - α\u0302 p + p\u00b3 - p\u2075        " \
                        "[e(t) = e_{max} sin(ωt), ω = 2πf]"
        self.equation_label = ttk.Label(self.equation_frame, text=equation_text, justify=tk.CENTER, font=('TkDefaultFont', 10))
        self.equation_label.pack(pady=(0, 5)) # Add some bottom padding


        # --- Matplotlib Figure and Axes (in plot_frame) ---
        self.fig = Figure(figsize=(10, 6), dpi=100)
        gs = gridspec.GridSpec(1, 2, width_ratios=[2, 1], wspace=0.3) # Hysteresis plot wider

        # P-E Hysteresis Plot
        self.ax_hysteresis = self.fig.add_subplot(gs[0])
        self.ax_hysteresis.set_xlabel("Reduced electric field, $e$")
        self.ax_hysteresis.set_ylabel("Reduced polarisation, $p$")
        self.ax_hysteresis.set_title("P-E Hysteresis Loop")
        self.ax_hysteresis.grid(True)
        self.line_hysteresis, = self.ax_hysteresis.plot([], [], 'b-', lw=1.5, label="Full Loop") # Full loop
        self.point_hysteresis, = self.ax_hysteresis.plot([], [], 'ro', markersize=8, label="Current State") # Animated point
        self.pr_ec_points, = self.ax_hysteresis.plot([], [], 'g*', markersize=10, ls='None', label='Pr/Ec') # Pr/Ec markers
        self.ax_hysteresis.legend(fontsize='small')

        # Domain Visualization Plot
        self.ax_domains = self.fig.add_subplot(gs[1])
        self.ax_domains.set_title("Microscopic Domains (Conceptual)")
        self.ax_domains.set_xticks([])
        self.ax_domains.set_yticks([])
        self.ax_domains.set_aspect('equal', adjustable='box')
        # Create the quiver object for domains
        self.quiver_domains = self.ax_domains.quiver(
            self.domain_X, self.domain_Y, self.domain_U, self.domain_V,
            pivot='mid', scale=self.domain_grid_size*1.5, # Adjust scale as needed
            headwidth=5, headlength=5, width=0.005 # Arrow appearance
        )
        self.ax_domains.set_xlim(-1, self.domain_grid_size)
        self.ax_domains.set_ylim(-1, self.domain_grid_size)


        # --- Canvas Embedding ---
        self.canvas = FigureCanvasTkAgg(self.fig, master=self.plot_frame)
        self.canvas_widget = self.canvas.get_tk_widget()
        self.canvas_widget.pack(fill=tk.BOTH, expand=True)
        self.canvas.draw()


    def _run_simulation(self):
        """Runs the simulation with current parameters."""
        if self.anim_running:
            self._toggle_animation() # Stop animation if running

        self.status_var.set("Running simulation...")
        self.master.update_idletasks() # Update GUI to show status

        alpha = self.alpha_hat.get()
        emax = self.e_max.get()
        freq = self.freq.get()
        dt_val = self.dt.get() # Get dt value (though fixed in this version)
        n_cyc = self.n_cycles.get()

        try:
            self.t_sim, self.E_sim, self.P_sim = simulate_hysteresis(
                alpha_hat=alpha, e_max=emax, freq=freq, dt=dt_val, n_cycles=n_cyc
            )
        except Exception as e:
            self.status_var.set(f"Simulation Error: {e}")
            self.anim_button.config(state=tk.DISABLED)
            return

        if self.E_sim is None or len(self.E_sim) < 2:
             self.status_var.set("Simulation failed or produced no data.")
             self.anim_button.config(state=tk.DISABLED)
             return


        # Find Pr and Ec from the *last* cycle
        self.Pr_pos, self.Pr_neg, self.Ec_pos, self.Ec_neg, self.P_max = find_pr_ec(
            self.t_sim, self.E_sim, self.P_sim, freq
        )

        # Use the magnitudes for display if needed, or the signed values
        pr_display = (abs(self.Pr_pos) + abs(self.Pr_neg))/2 if (self.Pr_pos != 0 or self.Pr_neg != 0) else 0
        ec_display = (abs(self.Ec_pos) + abs(self.Ec_neg))/2 if (self.Ec_pos != 0 or self.Ec_neg != 0) else 0
        self.status_var.set(f"Simulation complete. Pr≈{pr_display:.3f}, Ec≈{ec_display:.3f}")

        # Update the hysteresis plot with the full loop
        self.line_hysteresis.set_data(self.E_sim, self.P_sim)
        self.ax_hysteresis.relim()
        self.ax_hysteresis.autoscale_view()

        # Plot Pr/Ec points using the signed values found
        pr_ec_e_vals = []
        pr_ec_p_vals = []
        # Plot points where E is near 0 (for Pr)
        if self.Pr_pos != 0: pr_ec_p_vals.append(self.Pr_pos); pr_ec_e_vals.append(0)
        if self.Pr_neg != 0: pr_ec_p_vals.append(self.Pr_neg); pr_ec_e_vals.append(0)
        # Plot points where P is near 0 (for Ec)
        if self.Ec_pos != 0: pr_ec_e_vals.append(self.Ec_pos); pr_ec_p_vals.append(0)
        if self.Ec_neg != 0: pr_ec_e_vals.append(self.Ec_neg); pr_ec_p_vals.append(0)

        self.pr_ec_points.set_data(pr_ec_e_vals, pr_ec_p_vals)


        # Reset animation state
        self.current_step = 0
        self.point_hysteresis.set_data([], []) # Clear animated point

        # Update domain visualization for the initial state (P[0])
        self._update_domains(self.P_sim[0])
        self.quiver_domains.set_UVC(self.domain_U, self.domain_V) # Update quiver data


        self.canvas.draw()
        self.anim_button.config(state=tk.NORMAL) # Enable animation button


    def _toggle_animation(self):
        """Starts or stops the animation."""
        if self.anim_running:
            self.anim_running = False
            if self.anim_job is not None:
                self.master.after_cancel(self.anim_job)
                self.anim_job = None
            self.anim_button.config(text="Start Animation")
            self.status_var.set("Animation stopped.")
        else:
            if self.E_sim is None or len(self.E_sim) == 0:
                self.status_var.set("Run simulation first.")
                return

            self.anim_running = True
            self.anim_button.config(text="Stop Animation")
            self.status_var.set("Animation running...")
            # Restart animation from the beginning if stopped
            if self.current_step >= len(self.t_sim) -1:
                 self.current_step = 0
            self._animate_step()


    def _animate_step(self):
        """Performs one step of the animation."""
        if not self.anim_running or self.E_sim is None:
            return

        if self.current_step >= len(self.t_sim):
            # Loop the animation
            self.current_step = 0

        # Get current data point
        current_e = self.E_sim[self.current_step]
        current_p = self.P_sim[self.current_step]

        # Update hysteresis plot point
        self.point_hysteresis.set_data([current_e], [current_p])

        # Update domain visualization
        self._update_domains(current_p)
        self.quiver_domains.set_UVC(self.domain_U, self.domain_V)

        # Highlight Pr/Ec proximity (using calculated signed values)
        highlight_info = ""
        # Use a tolerance relative to the max values or a small absolute value
        e_tol = max(abs(self.Ec_neg), abs(self.Ec_pos), 0.01) / 5.0
        p_tol = max(abs(self.Pr_neg), abs(self.Pr_pos), 0.01) / 5.0

        if np.isclose(current_e, 0, atol=e_tol): # Near E=0
             if current_p > p_tol and self.Pr_pos != 0: highlight_info = " Near +Pr"
             elif current_p < -p_tol and self.Pr_neg != 0: highlight_info = " Near -Pr"
        elif np.isclose(current_p, 0, atol=p_tol): # Near P=0
             if current_e > e_tol and self.Ec_pos != 0: highlight_info = " Near +Ec"
             elif current_e < -e_tol and self.Ec_neg != 0: highlight_info = " Near -Ec"

        self.status_var.set(f"Animating: t={self.t_sim[self.current_step]:.2f}, E={current_e:.3f}, P={current_p:.3f}{highlight_info}")


        # Redraw the canvas
        self.canvas.draw_idle()

        # Increment step and schedule next frame
        self.current_step += 1
        self.anim_job = self.master.after(self.anim_delay, self._animate_step)


    def _update_domains(self, current_p):
        """
        Updates the domain orientations based on the current polarization P.
        This is a *conceptual* representation.
        """
        if self.P_max == 0: # Avoid division by zero if P is always zero
            self.domain_U = np.zeros_like(self.domain_U)
            self.domain_V = np.zeros_like(self.domain_V)
            return

        # Normalize polarization: ranges roughly from -1 to 1
        norm_p = np.clip(current_p / (self.P_max + 1e-9), -1.0, 1.0)

        # Calculate the fraction of domains aligned 'up' (positive P) using sigmoid
        k = 10 # Steepness of switching
        fraction_up = 1 / (1 + np.exp(-k * norm_p))

        num_domains_total = self.domain_grid_size * self.domain_grid_size
        num_up = int(fraction_up * num_domains_total)

        # Create a flat array representing domain states (1=up, -1=down)
        domain_states = np.full(num_domains_total, -1) # Start all down
        # Randomly select 'num_up' indices to set to 'up'
        up_indices = np.random.choice(num_domains_total, num_up, replace=False)
        domain_states[up_indices] = 1

        # Reshape back to grid and set U, V components for quiver
        domain_states_grid = domain_states.reshape((self.domain_grid_size, self.domain_grid_size))

        self.domain_U = np.zeros_like(self.domain_U) # U component is zero for vertical arrows
        self.domain_V = domain_states_grid * 0.8 # V component determines direction and length


    def _quit(self):
        """Cleanly exit the application."""
        if self.anim_running:
            self.anim_running = False
            if self.anim_job is not None:
                self.master.after_cancel(self.anim_job)
        self.master.quit()
        self.master.destroy()


# --- Main Execution ---
if __name__ == "__main__":
    root = tk.Tk()
    app = FerroelectricApp(root)
    root.mainloop()
```
