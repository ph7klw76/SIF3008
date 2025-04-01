# 1. The Essence of Band Folding

## 1.1. Bloch’s Theorem and the Brillouin Zone

By Bloch’s theorem, the electronic wavefunctions in a crystal can be written as

$$
\psi_{k,n}(r) = e^{i k \cdot r} \, u_{k,n}(r),
$$

where $u_{k,n}(r)$ has the same periodicity as the crystal lattice. The wavevector $k$ is typically taken within the first Brillouin zone (BZ), which is the Wigner–Seitz cell in reciprocal space. For a 1D crystal of lattice constant $a$, the first BZ is usually defined as $-\pi/a \leq k < \pi/a$. Generalizing this to higher dimensions yields an analogous region in $k$-space.

## 1.2. Extending vs. Folding Bands

A standard approach to understanding band structures is to plot the energy $E_n(k)$ versus $k$ within the first BZ (the reduced zone scheme). However, you can also display bands in an extended zone scheme, repeating them in reciprocal lattice vectors $G$. That is, for $k$ outside the first BZ, one may define an equivalent wavevector $k' = k - G$ inside the first BZ.

When a real-space periodicity changes, the reciprocal lattice vectors also change, which typically shrinks or expands the BZ. Consequently, the existing band structure in the old scheme must be re-cast—folded—into the new BZ.

### **Physical Origin of Band Folding**

When a crystal’s lattice constant (or basis) is effectively changed—e.g., by forming a superlattice or a surface reconstruction—a new (larger) real-space unit cell emerges. In reciprocal space, that means the Brillouin zone shrinks. Energy bands that previously dispersed across a larger BZ are now mapped into a smaller region, which mathematically appears like multiple replicas of the band folding onto one another.

---

# 2. Mathematical Description

## 2.1. Superlattice and Reduced Reciprocal Lattice Vectors

Consider a 1D crystal whose original lattice constant is $a$. Now suppose you create a new unit cell of size $Na$ (for example, by repeating a motif $N$ times in real space). The real-space lattice vector changes from $a$ to $Na$.

In reciprocal space, the corresponding reciprocal lattice vector changes from $G = 2\pi/a$ to:

$$
G_{\text{new}} = \frac{2\pi}{Na}
$$

The new first Brillouin zone extends from $-\pi/(Na)$ to $+\pi/(Na)$.

If you had a band structure described in the old reciprocal lattice with wavevectors $k \in [-\pi/a, \pi/a]$, you must now recast $k$ within $[-\pi/(Na), \pi/(Na)]$. Any wavevector outside this range can be folded back by subtracting or adding integer multiples of $G_{\text{new}} = 2\pi/(Na)$. The new wavevector is:

$$
k_{\text{folded}} = k - m \left( \frac{2\pi}{Na} \right),
$$

where $m$ is chosen so that $k_{\text{folded}}$ lies in the new, smaller BZ. The band energies $E(k)$ remain physically the same, but they are simply re-labeled and “folded” into the smaller range of wavevector space.

## 2.2. Example in 1D (Doubling the Lattice Constant)

- **Original lattice constant:** $a$
- **First Brillouin zone:** $-\pi/a \leq k < \pi/a$
- **New (superlattice) lattice constant:** $2a$
- **First Brillouin zone:** $-\pi/(2a) \leq k < \pi/(2a)$

Any wavevector $k_0$ that originally lay in $[\pi/(2a), \pi/a]$ is outside the new first BZ. It gets folded back by subtracting $G_{\text{new}} = \pi/a$:

$$
k_{\text{folded}} = k_0 - \frac{\pi}{a}
$$

which places it in $[-\pi/(2a), \pi/(2a)]$. Numerically, a band that peaked near $k_0 = \pm \pi/a$ would now appear at $\pm \pi/(2a)$ in the new zone. Graphically, this shows up as two lines (folded versions) that occupy half the original $k$-range but exhibit more band crossings or anti-crossings due to zone folding.

---

# 3. Physical Scenarios Where Band Folding Arises

## 3.1. Superlattices and Heterostructures

Superlattices—for instance, a periodic arrangement of two different materials A and B with thicknesses $d_A$ and $d_B$—create a longer periodicity along the growth direction. The new lattice constant along that direction becomes $d_A + d_B$.

In the simplest model (1D Kronig-Penney–like approach), wavevectors along the growth axis must be considered in the smaller BZ determined by the superlattice period. This compresses the original BZ and folds higher-zone states back into the new, smaller BZ. Consequently, the band structure in the superlattice often reveals minigaps or additional band edges that originate purely from folding (and potential coupling at the new Bragg planes).

## 3.2. Charge Density Waves or Structural Distortions

In certain correlated electron systems, a charge density wave (CDW) can spontaneously break translational symmetry by doubling (or otherwise multiplying) the lattice constant. This modifies the reciprocal lattice vectors and causes the original conduction or valence bands to fold. Experimentally, one sees new band gaps open at wavevectors that used to be allowed but are now at the new Brillouin zone boundary.

## 3.3. Surface Reconstructions

Surfaces of crystals often reconstruct to minimize surface free energy. For instance, a (100) surface of Si can dimerize or form complex patterns that effectively double or quadruple the surface unit cell. In angle-resolved photoemission spectroscopy (ARPES) of such surfaces, the measured dispersions show states that appear at folded $k$-locations relative to the bulk band structure. This effect is again explained by the smaller reciprocal lattice of the reconstructed surface.

---

# 4. Illustrative Examples

## 4.1. Simple 1D Model: Doubling the Lattice

- **Initial system:** 1D chain with lattice constant $a$
- **Hopping Hamiltonian:**

$$
\hat{H} = -t \sum_i (c_i^\dagger c_{i+1} + \text{h.c.})
$$

- **Dispersion:**

$$
E(k) = -2t \cos(ka), \quad -\pi/a \leq k \leq \pi/a
$$

If you artificially create a new period $2a$, you must fold the band:

- **Original zone:** from $-\pi/a$ to $\pi/a$
- **New zone:** from $-\pi/(2a)$ to $\pi/(2a)$

The single cosine band now appears twice in the smaller zone: one copy in $[-\pi/(2a), \pi/(2a)]$, and another copy mirroring the portion that got folded in. Graphically, you see two branches that can hybridize if an additional potential couples them.

## 4.2. Semiconductor Superlattice

Take GaAs/AlGaAs superlattices grown by molecular beam epitaxy:

- Each GaAs layer has thickness $d_{\text{GaAs}}$
- Each AlGaAs barrier layer has thickness $d_{\text{AlGaAs}}$
- **Superlattice period:** $D = d_{\text{GaAs}} + d_{\text{AlGaAs}}$

In the direction perpendicular to the layers, the crystal periodicity is replaced by $D$. The band structure must be plotted in a BZ of size $\pm \pi/D$.

States that used to lie at multiples of $\pi/a_{\text{bulk}}$ are folded back into $\pm \pi/D$. This leads to mini-bands and mini-gaps characteristic of superlattices.

---

# 5. Consequences and Signatures of Band Folding

- **Zone Boundary Gaps:** Where folded bands intersect at the new zone boundary, additional gaps can open if the system has a nontrivial potential (e.g., superlattice potential or CDW).

- **Replica Bands:** Experimentally, in ARPES or electron diffraction, you might see satellite features or new band maxima/minima that did not exist in the simpler, smaller real-space cell.

- **Reduced Symmetry:** If the reason for folding is some lower symmetry (say, a specific structural distortion), new selection rules or gap openings appear that are absent in the higher-symmetry parent structure.

---

# 6. Conclusion

Band folding is a ubiquitous concept in solid state physics that describes how electronic bands appear in reciprocal space when the real-space lattice changes. Mathematically, it can be understood as re-labelling (folding) wavevectors from a larger Brillouin zone into a smaller one.

Physically, it manifests whenever the crystal’s real-space periodicity is modified—by superlattices, charge density waves, doubling lattice constants, or surface reconstructions. The resulting band structure often exhibits new features such as mini-band gaps, replica bands, and altered dispersions that are direct signatures of the new periodic potential.

---

### **Key Points to Remember:**

- Band folding does **not** by itself change the energies of the original band dispersion; rather, it changes how those dispersions are arranged within a new Brillouin zone.
- Additional interactions or potentials in the new superlattice can cause further splitting or hybridization of folded bands.
- Experimental observations of folded bands are strong evidence of new periodicities in the crystal, whether from structural, chemical, or electronic modifications.

**Overall**, recognizing and interpreting band folding is crucial for analyzing any system where the periodicity is larger or more complex than that of the primitive cell—ranging from advanced semiconductor heterostructures to exotic correlated materials.



![image](https://github.com/user-attachments/assets/d1b1b037-91e6-4700-9347-886d3b82d865)

. The code implements a 2D nearest-neighbor tight-binding model on a square lattice, then constructs an 𝑁×N supercell and visualizes the folded band structure along a chosen high-symmetry path. We also compute an approximate density of states (DOS) from the path. 

```python
import tkinter as tk
from tkinter import ttk
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg
from scipy.ndimage import gaussian_filter1d
import time

# --- Configuration & Physics Model ---

# Primitive Cell Tight-Binding Model (Nearest Neighbor Square Lattice)
# E(kx, ky) = E0 - 2t(cos(kx*a) + cos(ky*a))
# E = 4 - 2*(cos(kx) + cos(ky)) --> Range [0, 8] (t=1, a=1, E0=2)
def calculate_primitive_energy(kx, ky):
    return 4.0 - 2.0 * (np.cos(kx) + np.cos(ky))

MIN_ENERGY = 0.0
MAX_ENERGY = 8.0

# K-path generation parameters
NUM_KPOINTS_PER_SEGMENT = 30 # Increase for smoother bands, but slower calc

# Slider range for N (Supercell size NxN)
MIN_N = 1
MAX_N = 20 # Keep relatively low for interactivity

# --- K-Path Generation ---
def generate_k_path(N, num_points_per_segment):
    """Generates k-points and distances for Γ-X-M-Γ path in the small BZ."""
    if N == 0: N = 1 # Avoid division by zero

    gamma = np.array([0, 0])
    x = np.array([np.pi / N, 0])
    m = np.array([np.pi / N, np.pi / N])

    path_gx = np.linspace(gamma, x, num_points_per_segment)
    path_xm = np.linspace(x, m, num_points_per_segment)
    path_mg = np.linspace(m, gamma, num_points_per_segment)

    # Ensure endpoints are included exactly once where segments meet
    k_points = np.vstack((path_gx[:-1], path_xm[:-1], path_mg))

    distances = [0]
    total_dist = 0
    current_pos = k_points[0]
    for i in range(1, len(k_points)):
        segment_dist = np.linalg.norm(k_points[i] - k_points[i-1]) # Dist between consecutive points
        total_dist += segment_dist
        distances.append(total_dist)

    # Distances for high-symmetry points (relative to start)
    dist_gamma = 0
    dist_x = np.linalg.norm(x - gamma) * (num_points_per_segment - 1) / num_points_per_segment * len(path_gx[:-1]) / (num_points_per_segment -1) # Approximate based on segments
    dist_m = distances[len(path_gx[:-1]) + len(path_xm[:-1]) -1] # Index of M point
    dist_gamma2 = distances[-1] # Total distance

    sym_point_distances = [dist_gamma, distances[len(path_gx)-1], distances[len(path_gx[:-1]) + len(path_xm)-1], dist_gamma2]
    sym_point_labels = ['Γ', 'X', 'M', 'Γ']

    return k_points, np.array(distances), sym_point_distances, sym_point_labels

# --- Folded Band Calculation ---
def calculate_folded_bands(k_point, N):
    """Calculates the N*N folded energy bands for a given k in the small BZ."""
    kx, ky = k_point
    indices_i = np.arange(N)
    indices_j = np.arange(N)
    II, JJ = np.meshgrid(indices_i, indices_j)

    Kx_folded = kx + 2 * np.pi * II.flatten() / N
    Ky_folded = ky + 2 * np.pi * JJ.flatten() / N

    folded_energies = calculate_primitive_energy(Kx_folded, Ky_folded)
    return folded_energies

# --- GUI Setup ---
root = tk.Tk()
root.title(f"Band Folding Visualization (Up to N={MAX_N})")

fig = plt.figure(figsize=(10, 5))
gs = fig.add_gridspec(1, 2, width_ratios=[3, 1], wspace=0.3)
ax_bands = fig.add_subplot(gs[0])
ax_dos = fig.add_subplot(gs[1])
ax_dos.tick_params(axis='y', which='both', left=False, right=False, labelleft=False) # Remove y-axis ticks/labels for DOS

# Embed the plots in Tkinter window
canvas = FigureCanvasTkAgg(fig, master=root)
canvas_widget = canvas.get_tk_widget()
canvas_widget.pack(side=tk.TOP, fill=tk.BOTH, expand=1)

# --- Update Function ---
def update_plots(val_str):
    start_time = time.time()
    try:
        N = int(float(val_str))
        if N < MIN_N: N = MIN_N
    except ValueError:
        return

    # --- Recalculate Bands ---
    k_points, k_distances, sym_dists, sym_labels = generate_k_path(N, NUM_KPOINTS_PER_SEGMENT)
    num_bands = N * N
    total_k_points = len(k_points)
    all_bands = np.zeros((total_k_points, num_bands))

    print(f"Calculating for N={N} ({num_bands} bands, {total_k_points} k-points)...")
    for i, k_vec in enumerate(k_points):
        all_bands[i, :] = calculate_folded_bands(k_vec, N)
    calc_duration = time.time() - start_time
    print(f"Calculation for N={N} took {calc_duration:.3f} seconds.")

    # --- Update Band Plot ---
    ax_bands.cla() # Clear axes completely
    # Faster plotting: plot all bands at once
    # Adjust alpha based on N for better visibility with many bands
    alpha_val = max(0.05, 0.7 - N*0.02)
    ax_bands.plot(k_distances, all_bands, 'b-', lw=0.8, alpha=alpha_val)

    # Reset plot limits and labels
    ax_bands.set_ylabel("Energy (E)")
    ax_bands.set_ylim(MIN_ENERGY - 0.5, MAX_ENERGY + 0.5)
    ax_bands.set_xlim(0, k_distances[-1])
    ax_bands.grid(True, axis='y', linestyle='--', alpha=0.6)

    # Add vertical lines and labels for high-symmetry points
    ax_bands.set_xticks(sym_dists)
    ax_bands.set_xticklabels(sym_labels)
    for dist in sym_dists:
        ax_bands.axvline(x=dist, color='k', linestyle='-', lw=0.7)

    ax_bands.set_title(f"Folded Band Structure ({N}x{N} Supercell)")

    # --- Calculate and Update DOS from displayed bands ---
    ax_dos.cla() # Clear DOS axis

    # Flatten all calculated energy points along the path
    energies_on_path = all_bands.flatten()

    # Create histogram (this is the "DOS" based *only* on the path points)
    n_bins_dos = 75 # Adjust number of bins as needed
    dos_counts_path, dos_edges_path = np.histogram(energies_on_path, bins=n_bins_dos, range=(MIN_ENERGY - 0.1, MAX_ENERGY + 0.1))
    dos_energies_path = (dos_edges_path[:-1] + dos_edges_path[1:]) / 2

    # Normalize counts to represent density (counts per energy interval)
    # Normalization here is arbitrary in absolute scale, focuses on shape
    delta_e = dos_edges_path[1] - dos_edges_path[0]
    # Avoid division by zero if delta_e is somehow zero
    dos_values_path = dos_counts_path / delta_e if delta_e > 0 else dos_counts_path

    # Smooth for aesthetics (optional, might obscure spikiness)
    dos_smoothed_path = gaussian_filter1d(dos_values_path, sigma=1.0) # Less smoothing maybe

    # Plot the new "DOS"
    ax_dos.plot(dos_smoothed_path, dos_energies_path, 'r-') # Plot DOS with Energy on Y-axis

    # Configure DOS plot axes
    ax_dos.set_xlabel("DOS (from path, arb. units)") # Emphasize origin
    ax_dos.set_ylim(MIN_ENERGY - 0.5, MAX_ENERGY + 0.5)
    # Dynamically set x-limit based on calculated DOS values
    max_dos_val_path = np.max(dos_smoothed_path) * 1.1 if np.max(dos_smoothed_path) > 0 else 1.0
    ax_dos.set_xlim(0, max_dos_val_path)
    ax_dos.set_yticks([]) # Remove energy ticks on y-axis
    ax_dos.set_xticks([]) # Remove DOS value ticks if desired
    ax_dos.set_title("DOS (from path)") # Clarify title
    ax_dos.grid(True, axis='x', linestyle='--', alpha=0.6)


    # Redraw the canvas
    redraw_start_time = time.time()
    canvas.draw_idle()
    print(f"Redraw took {time.time() - redraw_start_time:.3f} seconds.")


# --- Slider ---
controls_frame = tk.Frame(root)
controls_frame.pack(side=tk.BOTTOM, fill=tk.X, padx=10, pady=5)

slider_label = tk.Label(controls_frame, text="Supercell Size (N):")
slider_label.pack(side=tk.LEFT, padx=5)

slider_var = tk.IntVar(value=MIN_N)

n_value_label = tk.Label(controls_frame, text=f"N = {MIN_N}", width=8)
n_value_label.pack(side=tk.LEFT, padx=5)

# Label for status/timing
status_label = tk.Label(controls_frame, text="", width=30)
status_label.pack(side=tk.RIGHT, padx=5)

def slider_cmd_wrapper(val_str):
    """Updates label, status, and calls the main plot update."""
    try:
        N = int(float(val_str))
        n_value_label.config(text=f"N = {N}")
        status_label.config(text=f"Calculating N={N}...")
        root.update_idletasks() # Update GUI to show status message immediately
        update_plots(val_str)
        status_label.config(text="Ready") # Update status when done
    except ValueError:
        status_label.config(text="Invalid N")


slider = ttk.Scale(controls_frame, from_=MIN_N, to=MAX_N, orient=tk.HORIZONTAL,
                   variable=slider_var,
                   command=lambda val: slider_cmd_wrapper(str(int(float(val)))), # Ensure integer steps
                   length=400)
slider.pack(side=tk.LEFT, fill=tk.X, expand=True, padx=5, pady=10)


# --- Run GUI ---
root.update_idletasks()
slider_cmd_wrapper(str(slider_var.get())) # Call initial plot using wrapper

root.mainloop()
```

