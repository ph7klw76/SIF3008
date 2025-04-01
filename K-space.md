#  Physical Model: 2D Tight-Binding on a Square Lattice

This code demonstrates the core ideas of a 2D tight-binding band structure, from the discrete lattice Hamiltonian to reciprocal-space energies and a numerical density of states. By embedding Matplotlib plots inside a Tkinter GUI, it becomes possible to interactively explore the Fermi level’s influence on both band filling and the DOS. Even though the model here is quite simple (nearest-neighbor hopping, no spin), the core ideas extend to more complex lattice geometries and hopping configurations.

### **Key Equations:**

$$
E(k) = -2t \left( \cos k_x + \cos k_y \right),
$$

$$
g(E) \approx \frac{\text{Histogram}[E(k)]}{N^2 \, \Delta E}
$$

### **Main Physical Concept:**

As the Fermi energy changes, the occupied region in the Brillouin zone and the portion of the density of states lying below $E_F$ both adjust in real time.

This completes our technical overview of how the code—and the underlying physics—work in tandem to visualize a 2D tight-binding electron system.

![image](https://github.com/user-attachments/assets/fcd967ca-21b4-4239-a655-32379de974a0)

```python
import tkinter as tk
from tkinter import ttk
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg
from scipy.ndimage import gaussian_filter1d # For smoothing DOS

# --- Configuration & Physics Model ---
N_POINTS = 100  # Grid resolution for K-space (NxN)
BZ_BOUNDARY = np.pi # Brillouin Zone boundaries (+/- pi/a, setting a=1)

# Create K-space grid
kx = np.linspace(-BZ_BOUNDARY, BZ_BOUNDARY, N_POINTS)
ky = np.linspace(-BZ_BOUNDARY, BZ_BOUNDARY, N_POINTS)
KX, KY = np.meshgrid(kx, ky)

# Simple 2D Tight-Binding Energy Model (shifted so min energy is 0)
# E(kx, ky) = -2*t*(cos(kx*a) + cos(ky*a)). Let t=1, a=1.
# Original range: -4 to 4. Shifted range: 0 to 8.
ENERGY = 2.0 * (2.0 - np.cos(KX) - np.cos(KY))
MIN_ENERGY = np.min(ENERGY) # Should be 0
MAX_ENERGY = np.max(ENERGY) # Should be 8

# Calculate Density of States (DOS) numerically from the energy grid
# Flatten the energy array and create a histogram
energy_flat = ENERGY.flatten()
# Increase bins for better resolution, adjust range slightly for inclusion
bins = int(np.sqrt(N_POINTS*N_POINTS)) * 2
dos_counts, dos_edges = np.histogram(energy_flat, bins=bins, range=(MIN_ENERGY - 0.1, MAX_ENERGY + 0.1))
dos_energies = (dos_edges[:-1] + dos_edges[1:]) / 2
# Normalize DOS (optional, makes y-axis scale consistent)
dos_normalized = dos_counts / (N_POINTS*N_POINTS * (dos_edges[1] - dos_edges[0]))
# Smooth the DOS slightly for a nicer look
dos_smoothed = gaussian_filter1d(dos_normalized, sigma=1.5) # Adjust sigma as needed

# --- GUI Setup ---
root = tk.Tk()
root.title("K-space Filling and Density of States")

# Matplotlib Figure and Axes
fig, (ax_kspace, ax_dos) = plt.subplots(1, 2, figsize=(10, 5))
fig.tight_layout(pad=4.0, h_pad=3.0) # Add padding between plots and window edge

# --- Plot Setup ---

# K-space plot
ax_kspace.set_title("K-space Occupancy")
ax_kspace.set_xlabel("$k_x$ (units of $\pi/a$)")
ax_kspace.set_ylabel("$k_y$ (units of $\pi/a$)")
ax_kspace.set_aspect('equal', adjustable='box')
# Draw Brillouin Zone Boundary
bz_lines = [
    ([-BZ_BOUNDARY, BZ_BOUNDARY], [-BZ_BOUNDARY, -BZ_BOUNDARY]),
    ([-BZ_BOUNDARY, BZ_BOUNDARY], [BZ_BOUNDARY, BZ_BOUNDARY]),
    ([-BZ_BOUNDARY, -BZ_BOUNDARY], [-BZ_BOUNDARY, BZ_BOUNDARY]),
    ([BZ_BOUNDARY, BZ_BOUNDARY], [-BZ_BOUNDARY, BZ_BOUNDARY]),
]
for line in bz_lines:
    ax_kspace.plot(line[0], line[1], 'k-', lw=1.5, zorder=1) # Use 'k-' for black solid line

# Set K-space plot limits slightly larger than BZ
ax_kspace.set_xlim(-BZ_BOUNDARY * 1.1, BZ_BOUNDARY * 1.1)
ax_kspace.set_ylim(-BZ_BOUNDARY * 1.1, BZ_BOUNDARY * 1.1)
# Use pi symbols for ticks
tick_vals = [-np.pi, -np.pi/2, 0, np.pi/2, np.pi]
tick_labels = ['$-\pi$', '$-\pi/2$', '0', '$\pi/2$', '$\pi$']
ax_kspace.set_xticks(tick_vals)
ax_kspace.set_xticklabels(tick_labels)
ax_kspace.set_yticks(tick_vals)
ax_kspace.set_yticklabels(tick_labels)
ax_kspace.grid(True, linestyle='--', alpha=0.5)


# DOS plot
ax_dos.set_title("Density of States g(E)")
ax_dos.set_xlabel("Energy (E)")
ax_dos.set_ylabel("g(E) (arb. units)")
dos_line, = ax_dos.plot(dos_energies, dos_smoothed, 'k-', label='g(E)', zorder=1) # Base DOS line
ax_dos.set_ylim(bottom=0)
ax_dos.set_xlim(MIN_ENERGY, MAX_ENERGY)
ax_dos.grid(True, axis='y', linestyle='--', alpha=0.6)

# --- Placeholders for dynamic plot elements ---
# Use lists to hold artists that need clearing. This is slightly more robust
# than single variables if multiple contours/fills were ever needed.
kspace_fill_artists = []
kspace_line_artists = []
dos_fill_artist = None # fill_between returns a Polygon, not a list
dos_line_artist = None # vlines returns a LineCollection

# Embed the plots in Tkinter window
canvas = FigureCanvasTkAgg(fig, master=root)
canvas_widget = canvas.get_tk_widget()
canvas_widget.pack(side=tk.TOP, fill=tk.BOTH, expand=1)

# --- Update Function ---
def update_plots(val_str):
    # Note: slider command passes value as string, convert explicitly
    global kspace_fill_artists, kspace_line_artists, dos_fill_artist, dos_line_artist

    try:
        fermi_energy = float(val_str)
    except ValueError:
        print(f"Invalid slider value: {val_str}")
        return # Ignore if conversion fails

    # --- Clear previous dynamic K-space elements ---
    # Remove collections associated with contourf
    for coll in kspace_fill_artists:
        coll.remove()
    kspace_fill_artists.clear() # Clear the list

    # Remove collections associated with contour
    for coll in kspace_line_artists:
        coll.remove()
    kspace_line_artists.clear() # Clear the list

    # --- Update K-space plot ---
    # Draw new filled region (if Fermi energy > min energy)
    if fermi_energy > MIN_ENERGY:
        # Use levels to ensure only area <= E_F is colored. Add small epsilon to MIN_ENERGY
        # to handle the case E_F == MIN_ENERGY gracefully for contourf.
        fill_plot = ax_kspace.contourf(KX, KY, ENERGY, levels=np.linspace(MIN_ENERGY - 1e-9, fermi_energy, 10), colors=['#cccccc'], alpha=0.8, zorder=2)
        # Store the collections of the new contourf plot
        kspace_fill_artists.extend(fill_plot.collections)

    # Draw new Fermi surface line (only if E_F is within the actual data range)
    if MIN_ENERGY < fermi_energy < MAX_ENERGY:
         line_plot = ax_kspace.contour(KX, KY, ENERGY, levels=[fermi_energy], colors=['blue'], linewidths=1.5, zorder=3)
         # Store the collections of the new contour plot
         kspace_line_artists.extend(line_plot.collections)


    # --- Clear previous dynamic DOS elements ---
    if dos_fill_artist:
        dos_fill_artist.remove()
        dos_fill_artist = None # Reset placeholder
    if dos_line_artist:
        dos_line_artist.remove()
        dos_line_artist = None # Reset placeholder

    # --- Update DOS plot ---
    # Create new fill
    fill_mask = dos_energies <= fermi_energy
    valid_indices = np.where(fill_mask)[0]
    if len(valid_indices) > 0:
         # Make sure we use the correct corresponding y-values
         fill_energies = dos_energies[valid_indices]
         fill_dos_values = dos_smoothed[valid_indices]
         # Use zorder to ensure fill is below the main DOS line if desired, or above grid
         dos_fill_artist = ax_dos.fill_between(fill_energies, 0, fill_dos_values, color='#cccccc', alpha=0.8, zorder=2) # Store the Polygon object

    # Draw Fermi level line
    max_dos_val = np.max(dos_smoothed) * 1.1 if np.max(dos_smoothed) > 0 else 1.0 # Avoid 0 limit
    max_dos_val = max(max_dos_val, 0.1) # Ensure a minimum height for the line
    # Use zorder to ensure line is prominent
    dos_line_artist = ax_dos.vlines(fermi_energy, 0, max_dos_val, color='r', linestyle='--', lw=1.5, label=f'$E_F={fermi_energy:.2f}$', zorder=3) # Store the LineCollection
    ax_dos.set_ylim(0, max_dos_val) # Adjust y-limit

    # Update title
    ax_dos.set_title(f"Density of States (E_F = {fermi_energy:.2f})")

    # Redraw the canvas
    canvas.draw_idle() # Use draw_idle for potentially better performance

# --- Slider ---
slider_frame = tk.Frame(root)
slider_frame.pack(side=tk.BOTTOM, fill=tk.X, padx=10, pady=5)

slider_label = tk.Label(slider_frame, text="Fermi Energy (E_F):")
slider_label.pack(side=tk.LEFT, padx=5)

# Use a variable for the slider value for easier getting/setting
slider_var = tk.DoubleVar(value=MIN_ENERGY)

slider = ttk.Scale(slider_frame, from_=MIN_ENERGY, to=MAX_ENERGY, orient=tk.HORIZONTAL,
                   variable=slider_var, command=update_plots, length=400)
slider.pack(side=tk.LEFT, fill=tk.X, expand=1, padx=5, pady=10)

# Add a label to display the current slider value (optional but helpful)
value_label = tk.Label(slider_frame, text=f"{MIN_ENERGY:.2f}", width=5)
value_label.pack(side=tk.RIGHT, padx=5)

# Function to update the value label when slider moves
def update_value_label(val_str):
    try:
        value_label.config(text=f"{float(val_str):.2f}")
    except ValueError:
        pass # Ignore errors during rapid sliding if value is briefly invalid

# Link the label update to the slider variable trace
slider_var.trace_add("write", lambda name, index, mode: update_value_label(slider_var.get()))


# --- Run GUI ---


# Perform an initial update *after* the main window is created and visible
root.update_idletasks() # Ensure widgets are created
update_plots(slider_var.get()) # Call with initial value (as float string)

root.mainloop()
```
