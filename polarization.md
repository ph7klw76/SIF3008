![image](https://github.com/user-attachments/assets/5e1f17c8-d3c5-4b18-92e1-a10c854e8671)


```python
import tkinter as tk
from tkinter import ttk
import math
import random
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg

# --- Constants ---
CANVAS_WIDTH = 400
CANVAS_HEIGHT = 300
NUM_PARTICLES = 5  # Number of representative particles/molecules per row
PARTICLE_RADIUS = 10
NUCLEUS_RADIUS = 4
ELECTRON_CLOUD_RADIUS = 10
ION_POS_RADIUS = 8
ION_NEG_RADIUS = 10
DIPOLE_LENGTH = 20
MAX_E_FIELD = 10.0 # Arbitrary units for electric field strength
# MAX_DISPLACEMENT_FACTOR = 0.4 # REMOVED - No longer needed for hard cap

# --- Simulation Parameters (Adjusted for linearity) ---
ELECTRONIC_POLARIZABILITY = 0.3 # Reduced slightly as it's now linear
IONIC_POLARIZABILITY = 0.6   # Reduced slightly as it's now linear
DIPOLE_MOMENT = 1.0
SATURATION_FIELD = 5.0 # Field strength for near-saturation in dipolar case
GRAPH_SCALING_FACTOR = 3.0 # Adjusted scaling for potentially larger linear P values

# --- Main Application Class ---
class PolarizationViewerApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Interactive Dielectric Polarization Viewer")
        self.root.resizable(False, False)

        self.material_type = tk.StringVar(value="Electronic")
        self.electric_field = tk.DoubleVar(value=0.0)
        self.particles = []
        self.polarization_history = {'E': [], 'P': []}

        self.setup_gui()
        self.setup_material()
        self.update_visualization()

    def setup_gui(self):
        # --- Main Frames ---
        control_frame = ttk.Frame(self.root, padding="10")
        control_frame.grid(row=0, column=0, sticky="nsew")

        vis_frame = ttk.Frame(self.root, padding="10")
        vis_frame.grid(row=1, column=0, sticky="nsew")

        graph_frame = ttk.Frame(self.root, padding="10")
        graph_frame.grid(row=0, column=1, rowspan=2, sticky="nsew")

        self.root.columnconfigure(0, weight=1)
        self.root.columnconfigure(1, weight=2)
        self.root.rowconfigure(1, weight=1)

        # --- Control Frame Widgets ---
        ttk.Label(control_frame, text="Material Type:").grid(row=0, column=0, sticky="w", pady=2)
        ttk.Radiobutton(control_frame, text="Electronic", variable=self.material_type, value="Electronic", command=self.setup_material).grid(row=1, column=0, sticky="w")
        ttk.Radiobutton(control_frame, text="Ionic", variable=self.material_type, value="Ionic", command=self.setup_material).grid(row=2, column=0, sticky="w")
        ttk.Radiobutton(control_frame, text="Dipolar", variable=self.material_type, value="Dipolar", command=self.setup_material).grid(row=3, column=0, sticky="w")

        ttk.Label(control_frame, text="External Electric Field (E):").grid(row=4, column=0, sticky="w", pady=(10, 2))
        self.e_field_slider = ttk.Scale(control_frame, from_=0.0, to=MAX_E_FIELD, orient=tk.HORIZONTAL, variable=self.electric_field, command=self.update_visualization, length=200)
        self.e_field_slider.grid(row=5, column=0, sticky="ew")
        self.e_field_label = ttk.Label(control_frame, text=f"E = {self.electric_field.get():.2f}")
        self.e_field_label.grid(row=6, column=0, sticky="w", pady=2)

        # --- Visualization Frame Widgets ---
        ttk.Label(vis_frame, text="Material Visualization:").pack(anchor="w")
        self.vis_canvas = tk.Canvas(vis_frame, width=CANVAS_WIDTH, height=CANVAS_HEIGHT, bg="white", relief=tk.SUNKEN, borderwidth=1)
        self.vis_canvas.pack(fill=tk.BOTH, expand=True)

        # --- Graph Frame Widgets ---
        ttk.Label(graph_frame, text="Polarization vs. Electric Field:").pack(anchor="w")
        self.fig, self.ax = plt.subplots(figsize=(5, 4))
        self.ax.set_xlabel("Electric Field (E)")
        self.ax.set_ylabel("Polarization (P) [Arbitrary Units]") # Clarified units
        self.ax.set_title("P vs E")
        self.ax.grid(True)
        self.ax.set_xlim(0, MAX_E_FIELD)
        self.line, = self.ax.plot([], [], 'b-o', markersize=4)

        self.graph_canvas = FigureCanvasTkAgg(self.fig, master=graph_frame)
        self.graph_canvas_widget = self.graph_canvas.get_tk_widget()
        self.graph_canvas_widget.pack(fill=tk.BOTH, expand=True)
        self.fig.tight_layout()

    def setup_material(self):
        self.particles = []
        self.vis_canvas.delete("all")
        # Reset graph history, ensuring E=0, P=0 is always the start
        self.polarization_history = {'E': [0.0], 'P': [0.0]}
        # Set slider to 0 when changing material type
        self.electric_field.set(0.0)

        mat_type = self.material_type.get()
        rows = int(math.sqrt(NUM_PARTICLES))
        cols = (NUM_PARTICLES + rows - 1) // rows
        spacing_x = CANVAS_WIDTH / (cols + 1)
        spacing_y = CANVAS_HEIGHT / (rows + 1)
        count = 0

        for r in range(rows):
            for c in range(cols):
                if count >= NUM_PARTICLES * NUM_PARTICLES: break
                center_x = spacing_x * (c + 1)
                center_y = spacing_y * (r + 1)

                if mat_type == "Electronic":
                    self.particles.append({'type': 'electronic', 'cx': center_x, 'cy': center_y, 'cloud_offset_x': 0})
                elif mat_type == "Ionic":
                    ion_type = 'pos' if (r + c) % 2 == 0 else 'neg'
                    self.particles.append({'type': 'ionic', 'ion': ion_type, 'ox': center_x, 'oy': center_y, 'cx': center_x, 'cy': center_y})
                elif mat_type == "Dipolar":
                    angle = random.uniform(0, 2 * math.pi)
                    # Store initial angle for consistent alignment behavior
                    self.particles.append({'type': 'dipolar', 'cx': center_x, 'cy': center_y, 'angle': angle, 'initial_angle': angle})
                count += 1

        self.update_visualization() # Draw initial state (E=0)
        self.update_graph() # Update graph with initial point

    def update_visualization(self, event=None):
        """Updates the visualization canvas based on the current E-field."""
        E = self.electric_field.get()
        self.e_field_label.config(text=f"E = {E:.2f}")
        self.vis_canvas.delete("all")

        # --- Draw Electric Field Lines ---
        if E > 0.1:
            num_lines = 5
            field_spacing = CANVAS_HEIGHT / (num_lines + 1)
            arrow_dir = tk.FIRST
            line_color = "lightgrey"
            line_width = max(1, E*0.5) # Make lines thicker with stronger field
            for i in range(num_lines):
                y = field_spacing * (i + 1)
                self.vis_canvas.create_line(0, y, CANVAS_WIDTH, y, arrow=arrow_dir, fill=line_color, width=line_width)


        # --- Draw Particles ---
        mat_type = self.material_type.get()
        total_polarization = 0

        for p in self.particles:
            cx, cy = p['cx'], p['cy'] # Use current center for electronic/dipolar

            if mat_type == "Electronic":
                # *** REVISED: Displacement is now linear with E ***
                # Calculate the displacement based purely on polarizability and field
                displacement = ELECTRONIC_POLARIZABILITY * E
                # Limit visual displacement only if absolutely necessary to prevent drawing errors
                # but calculation remains linear. Here, we assume MAX_E_FIELD and
                # polarizability are tuned so it looks reasonable.
                p['cloud_offset_x'] = -displacement # Shift left for positive E (opposite to E)

                nucleus_x = cx # Nucleus assumed fixed at the center
                cloud_center_x = cx + p['cloud_offset_x']

                self.vis_canvas.create_oval(nucleus_x - NUCLEUS_RADIUS, cy - NUCLEUS_RADIUS,
                                            nucleus_x + NUCLEUS_RADIUS, cy + NUCLEUS_RADIUS,
                                            fill="red", outline="black")
                self.vis_canvas.create_oval(cloud_center_x - ELECTRON_CLOUD_RADIUS, cy - ELECTRON_CLOUD_RADIUS,
                                            cloud_center_x + ELECTRON_CLOUD_RADIUS, cy + ELECTRON_CLOUD_RADIUS,
                                            fill="lightblue", outline="blue", stipple="gray50")

                # Contribution to polarization is the linear displacement (charge*displacement)
                # We use absolute displacement as a proxy for dipole moment magnitude
                total_polarization += abs(displacement)

            elif mat_type == "Ionic":
                ox, oy = p['ox'], p['oy'] # Original positions
                # *** REVISED: Displacement is now linear with E ***
                displacement = IONIC_POLARIZABILITY * E

                if p['ion'] == 'pos':
                    p['cx'] = ox + displacement # Shift right (with E)
                    radius = ION_POS_RADIUS
                    color = "orange"
                    total_polarization += displacement # Dipole moment proportional to displacement
                else: # 'neg'
                    p['cx'] = ox - displacement # Shift left (against E)
                    radius = ION_NEG_RADIUS
                    color = "purple"
                    total_polarization += displacement # Add magnitude of opposing displacement

                self.vis_canvas.create_oval(p['cx'] - radius, p['cy'] - radius,
                                            p['cx'] + radius, p['cy'] + radius,
                                            fill=color, outline="black")

            elif mat_type == "Dipolar":
                # Dipoles align - this part remains the same (saturation model)
                initial_angle = p['initial_angle'] # Use stored initial angle

                alignment_factor = math.tanh(E / SATURATION_FIELD)
                target_angle = 0 # Align with E field (horizontal right)
                delta_angle = target_angle - initial_angle
                delta_angle = (delta_angle + math.pi) % (2 * math.pi) - math.pi # Normalize [-pi, pi]
                p['angle'] = initial_angle + delta_angle * alignment_factor

                half_len = DIPOLE_LENGTH / 2
                end1_x = cx - half_len * math.cos(p['angle'])
                end1_y = cy - half_len * math.sin(p['angle'])
                end2_x = cx + half_len * math.cos(p['angle'])
                end2_y = cy + half_len * math.sin(p['angle'])

                self.vis_canvas.create_line(end1_x, end1_y, end2_x, end2_y, width=3, fill="darkgreen")
                self.vis_canvas.create_oval(end1_x-3, end1_y-3, end1_x+3, end1_y+3, fill="red")   # (-) end
                self.vis_canvas.create_oval(end2_x-3, end2_y-3, end2_x+3, end2_y+3, fill="blue") # (+) end

                # Contribution: projection of dipole moment along E field
                total_polarization += DIPOLE_MOMENT * math.cos(p['angle'])


        # --- Calculate and Store Polarization ---
        if len(self.particles) > 0:
             avg_polarization = total_polarization / len(self.particles)
             # Apply scaling factor for graph visibility
             scaled_polarization = avg_polarization * GRAPH_SCALING_FACTOR
        else:
             scaled_polarization = 0

        # --- Update History ---
        # Find if E already exists (approximately)
        found_index = -1
        for i, hist_e in enumerate(self.polarization_history['E']):
            if abs(E - hist_e) < 0.01: # Check for closeness
                found_index = i
                break

        if found_index != -1:
            # Update existing P value if E is very close
            self.polarization_history['P'][found_index] = scaled_polarization
        # Only add if E changed significantly from the *last added point* or is an endpoint
        elif not self.polarization_history['E'] or abs(E - self.polarization_history['E'][-1]) > 0.05 or E == 0 or abs(E - MAX_E_FIELD) < 0.01 :
             self.polarization_history['E'].append(E)
             self.polarization_history['P'].append(scaled_polarization)

        self.update_graph() # Update graph regardless of history addition for smoothness


    def update_graph(self):
        """Updates the Matplotlib graph."""
        mat_type = self.material_type.get()

        # Ensure E=0, P=0 is present and first
        if 0.0 not in self.polarization_history['E']:
            self.polarization_history['E'].insert(0, 0.0)
            self.polarization_history['P'].insert(0, 0.0)
        else:
            zero_idx = self.polarization_history['E'].index(0.0)
            self.polarization_history['P'][zero_idx] = 0.0 # Ensure P is exactly 0 at E=0

        E_vals = self.polarization_history['E']
        P_vals = self.polarization_history['P']

        sorted_indices = np.argsort(E_vals)
        E_sorted = np.array(E_vals)[sorted_indices]
        P_sorted = np.array(P_vals)[sorted_indices]

        self.line.set_data(E_sorted, P_sorted)

        if len(P_sorted) > 0:
            max_p = max(0.1, np.max(P_sorted) * 1.1) # Add 10% buffer top
            min_p = min(-0.1, np.min(P_sorted) * 1.1) # Add 10% buffer bottom
            # Make y-axis slightly larger than max P for linear cases
            if mat_type != "Dipolar":
                 max_p = max(max_p, abs(min_p)) # Ensure positive range is sufficient
                 min_p = -max_p # Make symmetric for linear cases maybe? Or just ensure range.
            self.ax.set_ylim(min_p, max_p) # Auto adjust y-axis
        else:
            self.ax.set_ylim(-1, 10) # Default range

        self.ax.set_xlim(0, MAX_E_FIELD)

        linearity = " (Linear)" if mat_type != "Dipolar" else " (Non-Linear/Saturating)"
        self.ax.set_title(f"P vs E - {mat_type}{linearity}")

        self.graph_canvas.draw()

# --- Main Execution ---
if __name__ == "__main__":
    root = tk.Tk()
    app = PolarizationViewerApp(root)
    root.mainloop()

```
