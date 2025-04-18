```python
import tkinter as tk
from tkinter import ttk
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg
from mpl_toolkits.mplot3d import Axes3D
import copy # To keep original atom positions safe

# --- Material Properties (Approximate values) ---
# Units: d33: pm/V (or pC/N), g33: Vm/N * 10^-3, Y: GPa
# Added 'shifting_atoms' key to specify which atoms move in the schematic.
MATERIALS = {
    "Quartz (SiO2)": {
        "d33": 2.3,
        "g33": 50,
        "Y": 70.0,
        "structure": "trigonal",
        # Schematic Trigonal Cell - highly simplified. Reality is complex helix.
        # Representing Si atoms slightly off-center, O forming a frame.
        "atoms": {
            'Si': [[0.45, 0.5, 0.33], [0.55, 0.5, 0.66]], # Slightly off center Z
            'O': [[0.25, 0.25, 0.75], [0.75, 0.25, 0.25], [0.25, 0.75, 0.25], [0.75, 0.75, 0.75]]
        },
        "colors": {'Si': 'blue', 'O': 'red'},
        "shift_vector": np.array([0, 0, 1.0]), # Si moves relative to O along Z
        "shifting_atoms": ['Si'], # Specify which atom types shift
        "fixed_atoms": ['O'] # Specify which are stationary in schematic
    },
    "PZT (Lead Zirconate Titanate)": {
        "d33": 250.0, # Moderate PZT
        "g33": 25.0,
        "Y": 65.0,
        "structure": "perovskite",
        # Schematic Tetragonal PZT (Pb corners, O face-centers, Zr/Ti body-center)
        # Ensuring positions are relative to a 0-1 cell
        "atoms": {
            'Pb': [[0,0,0], [1,0,0], [0,1,0], [0,0,1], [1,1,0], [1,0,1], [0,1,1], [1,1,1]],
            'O': [[0.5,0.5,0], [0.5,0,0.5], [0,0.5,0.5], [1,0.5,0.5], [0.5,1,0.5], [0.5,0.5,1]],
            'Zr/Ti': [[0.5, 0.5, 0.5]] # The key shifting ion
        },
        "colors": {'Pb': 'gray', 'O': 'red', 'Zr/Ti': 'green'},
        "shift_vector": np.array([0, 0, 1.0]), # Central ion shifts along Z (polar axis)
        "shifting_atoms": ['Zr/Ti'],
        "fixed_atoms": ['Pb', 'O']
    }
}
DEFAULT_THICKNESS_MM = 5.0

# --- Constants ---
MAX_STRESS_MPA = 100
MAX_VOLTAGE_V = 500
VIS_EXAGGERATION_MACRO = 500 # Exaggeration for macroscopic crystal block deformation
VIS_EXAGGERATION_MICRO = 150 # Exaggeration for atomic shifts in unit cell (Adjust!)
CHARGE_DENSITY_FACTOR = 1e-6
UPDATE_THRESHOLD = 0.01 # Sensitivity for adding points to history

# --- Main Application Class ---
class PiezoSimulatorApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Piezoelectric Simulator (Revised)")
        self.root.state('zoomed') # Make window large

        # --- Variables ---
        self.material_name = tk.StringVar(value=list(MATERIALS.keys())[0])
        self.effect_type = tk.StringVar(value="Direct")
        self.applied_stress_mpa = tk.DoubleVar(value=0.0)
        self.applied_voltage_v = tk.DoubleVar(value=0.0)
        self.crystal_thickness_mm = tk.DoubleVar(value=DEFAULT_THICKNESS_MM)

        # Store original atom positions separately
        self.original_atom_positions = {}

        # Data for plots
        self.stress_history = [0.0]
        self.polarization_history = [0.0]
        self.voltage_history = [0.0]
        self.strain_history = [0.0]

        # --- GUI Setup ---
        self.setup_gui()

        # --- Initial Load ---
        self.load_material_params()
        self.update_simulation()

    def setup_gui(self):
        # Main frame using grid layout
        main_frame = ttk.Frame(self.root, padding="10")
        main_frame.grid(row=0, column=0, sticky="nsew")
        self.root.columnconfigure(0, weight=1)
        self.root.rowconfigure(0, weight=1)

        # Configure grid weights for resizing
        main_frame.columnconfigure(0, weight=1) # Controls + Graphs column
        main_frame.columnconfigure(1, weight=2) # Visualizations column (wider)
        main_frame.rowconfigure(0, weight=1)    # Controls + Macro row
        main_frame.rowconfigure(1, weight=2)    # Graphs + Micro row (taller)

        # --- Column 0: Controls & Graphs ---
        col0_frame = ttk.Frame(main_frame)
        col0_frame.grid(row=0, column=0, rowspan=2, sticky="nsew", padx=5, pady=5)
        col0_frame.rowconfigure(0, weight=0) # Controls area fixed size
        col0_frame.rowconfigure(1, weight=1) # Graphs area expands

        # Controls Frame (Top-Left)
        controls_frame = ttk.LabelFrame(col0_frame, text="Controls", padding="10")
        controls_frame.grid(row=0, column=0, sticky="new") # Stick to top, expand width
        self._create_controls(controls_frame) # Helper function for controls

        # Graphs Frame (Bottom-Left)
        graphs_frame = ttk.LabelFrame(col0_frame, text="Graphs", padding="10")
        graphs_frame.grid(row=1, column=0, sticky="nsew") # Expand in all directions
        graphs_frame.columnconfigure(0, weight=1)
        graphs_frame.rowconfigure(0, weight=1) # Strain/Voltage graph
        graphs_frame.rowconfigure(1, weight=1) # Stress/Polarization graph
        self._create_graphs(graphs_frame) # Helper function for graphs


        # --- Column 1: Visualizations ---
        col1_frame = ttk.Frame(main_frame)
        col1_frame.grid(row=0, column=1, rowspan=2, sticky="nsew", padx=5, pady=5)
        col1_frame.rowconfigure(0, weight=1) # Macro view
        col1_frame.rowconfigure(1, weight=1) # Micro view

        # Macro View (Top-Right)
        macro_frame = ttk.LabelFrame(col1_frame, text="Macroscopic View", padding="5")
        macro_frame.grid(row=0, column=0, sticky="nsew", pady=(0, 5))
        macro_frame.columnconfigure(0, weight=1)
        macro_frame.rowconfigure(0, weight=1)
        self.macro_canvas = tk.Canvas(macro_frame, bg="lightgrey", width=350, height=300) # Slightly adjusted default size
        self.macro_canvas.grid(row=0, column=0, sticky="nsew")

        # Micro View (Bottom-Right)
        micro_frame = ttk.LabelFrame(col1_frame, text="Unit Cell View (Schematic)", padding="5")
        micro_frame.grid(row=1, column=0, sticky="nsew", pady=(5, 0))
        micro_frame.columnconfigure(0, weight=1)
        micro_frame.rowconfigure(0, weight=1)
        self.fig_3d = plt.figure(figsize=(4, 4)) # Adjusted size for better fit
        self.ax_3d = self.fig_3d.add_subplot(111, projection='3d')
        self.unit_cell_canvas = FigureCanvasTkAgg(self.fig_3d, master=micro_frame)
        self.unit_cell_canvas_widget = self.unit_cell_canvas.get_tk_widget()
        self.unit_cell_canvas_widget.grid(row=0, column=0, sticky="nsew")
        self.fig_3d.tight_layout(pad=0.1)

        # --- Initial state update ---
        self.update_control_state()


    def _create_controls(self, parent_frame):
        """Helper to create control widgets."""
        ttk.Label(parent_frame, text="Material:").grid(row=0, column=0, sticky="w", padx=5, pady=2)
        self.material_combo = ttk.Combobox(parent_frame, textvariable=self.material_name, values=list(MATERIALS.keys()), state="readonly", width=25)
        self.material_combo.grid(row=0, column=1, columnspan=2, sticky="ew", padx=5, pady=2)
        self.material_combo.bind("<<ComboboxSelected>>", self.on_material_change)

        ttk.Label(parent_frame, text="Effect Type:").grid(row=1, column=0, sticky="w", padx=5, pady=5)
        ttk.Radiobutton(parent_frame, text="Direct (Apply Stress)", variable=self.effect_type, value="Direct", command=self.on_effect_change).grid(row=1, column=1, sticky="w", padx=5)
        ttk.Radiobutton(parent_frame, text="Converse (Apply Voltage)", variable=self.effect_type, value="Converse", command=self.on_effect_change).grid(row=1, column=2, sticky="w", padx=5)

        # Stress Slider (Direct Effect)
        self.stress_label = ttk.Label(parent_frame, text=f"Applied Stress (σ): {self.applied_stress_mpa.get():.1f} MPa")
        self.stress_label.grid(row=2, column=0, columnspan=3, sticky="w", padx=5, pady=(10, 0))
        self.stress_slider = ttk.Scale(parent_frame, from_=-MAX_STRESS_MPA, to=MAX_STRESS_MPA, orient=tk.HORIZONTAL, variable=self.applied_stress_mpa, command=self.on_slider_change, length=300)
        self.stress_slider.grid(row=3, column=0, columnspan=3, sticky="ew", padx=5, pady=2)

        # Voltage Slider (Converse Effect)
        self.voltage_label = ttk.Label(parent_frame, text=f"Applied Voltage (V): {self.applied_voltage_v.get():.0f} V")
        self.voltage_label.grid(row=4, column=0, columnspan=3, sticky="w", padx=5, pady=(10, 0))
        self.voltage_slider = ttk.Scale(parent_frame, from_=-MAX_VOLTAGE_V, to=MAX_VOLTAGE_V, orient=tk.HORIZONTAL, variable=self.applied_voltage_v, command=self.on_slider_change, length=300)
        self.voltage_slider.grid(row=5, column=0, columnspan=3, sticky="ew", padx=5, pady=2)

        # Thickness Entry
        ttk.Label(parent_frame, text="Thickness (t):").grid(row=6, column=0, sticky="w", padx=5, pady=(10,2))
        self.thickness_entry = ttk.Entry(parent_frame, textvariable=self.crystal_thickness_mm, width=8)
        self.thickness_entry.grid(row=6, column=1, sticky="w", padx=5, pady=2)
        ttk.Label(parent_frame, text="mm").grid(row=6, column=2, sticky="w", padx=0, pady=2)
        self.thickness_entry.bind("<KeyRelease>", self.on_thickness_change)

    def _create_graphs(self, parent_frame):
        """Helper to create graph widgets."""
        # Strain vs Voltage Graph
        sv_frame = ttk.Frame(parent_frame) # No border needed inside LabelFrame
        sv_frame.grid(row=0, column=0, sticky="nsew", padx=2, pady=2)
        sv_frame.columnconfigure(0, weight=1)
        sv_frame.rowconfigure(0, weight=1)
        self.fig_sv, self.ax_sv = plt.subplots(figsize=(4, 3)) # Adjust figsize as needed
        self.line_sv, = self.ax_sv.plot([], [], 'b-o', markersize=3)
        self.ax_sv.set_xlabel("Voltage (V)")
        self.ax_sv.set_ylabel("Strain")
        self.ax_sv.set_title("Strain vs. Voltage", fontsize=10)
        self.ax_sv.grid(True)
        self.fig_sv.tight_layout(pad=0.5)
        self.sv_canvas = FigureCanvasTkAgg(self.fig_sv, master=sv_frame)
        self.sv_canvas_widget = self.sv_canvas.get_tk_widget()
        self.sv_canvas_widget.grid(row=0, column=0, sticky="nsew")

        # Stress vs Polarization Graph
        sp_frame = ttk.Frame(parent_frame)
        sp_frame.grid(row=1, column=0, sticky="nsew", padx=2, pady=2)
        sp_frame.columnconfigure(0, weight=1)
        sp_frame.rowconfigure(0, weight=1)
        self.fig_sp, self.ax_sp = plt.subplots(figsize=(4, 3)) # Adjust figsize as needed
        self.line_sp, = self.ax_sp.plot([], [], 'r-o', markersize=3)
        self.ax_sp.set_xlabel("Stress (MPa)")
        self.ax_sp.set_ylabel("Polarization (µC/m²)")
        self.ax_sp.set_title("Stress vs. Polarization", fontsize=10)
        self.ax_sp.grid(True)
        self.fig_sp.tight_layout(pad=0.5)
        self.sp_canvas = FigureCanvasTkAgg(self.fig_sp, master=sp_frame)
        self.sp_canvas_widget = self.sp_canvas.get_tk_widget()
        self.sp_canvas_widget.grid(row=0, column=0, sticky="nsew")

    def load_material_params(self):
        """Loads parameters and stores original atom positions."""
        mat_name = self.material_name.get()
        self.params = MATERIALS[mat_name]
        # Deep copy original positions to prevent modification
        self.original_atom_positions = copy.deepcopy(self.params["atoms"])
        # Clear history when material changes
        self._reset_history()

    def _reset_history(self):
        """Resets plot history data."""
        self.stress_history = [0.0]
        self.polarization_history = [0.0]
        self.voltage_history = [0.0]
        self.strain_history = [0.0]
        # Reset sliders visually as well
        self.applied_stress_mpa.set(0.0)
        self.applied_voltage_v.set(0.0)
        self.stress_label.config(text=f"Applied Stress (σ): 0.0 MPa")
        self.voltage_label.config(text=f"Applied Voltage (V): 0 V")


    def update_control_state(self):
        """Enable/disable sliders based on selected effect type."""
        if self.effect_type.get() == "Direct":
            self.stress_slider.config(state="normal")
            self.voltage_slider.config(state="disabled")
            self.applied_voltage_v.set(0) # Reset inactive slider's value
        else: # Converse
            self.stress_slider.config(state="disabled")
            self.voltage_slider.config(state="normal")
            self.applied_stress_mpa.set(0) # Reset inactive slider's value
        self.stress_label.config(text=f"Applied Stress (σ): {self.applied_stress_mpa.get():.1f} MPa")
        self.voltage_label.config(text=f"Applied Voltage (V): {self.applied_voltage_v.get():.0f} V")


    def on_material_change(self, event=None):
        self.load_material_params()
        self.update_simulation()

    def on_effect_change(self):
        self.update_control_state()
        self._reset_history() # Reset history and sliders when changing mode
        self.update_simulation()

    def on_slider_change(self, event=None):
        self.update_control_state() # Update labels immediately
        self.update_simulation()

    def on_thickness_change(self, event=None):
        try:
            t = self.crystal_thickness_mm.get()
            if t > 0:
                self.update_simulation()
            else:
                 pass # Ignore non-positive thickness
        except tk.TclError:
            pass # Ignore non-numeric input during typing


    def update_simulation(self):
        """Performs calculations and updates all visualizations and graphs."""
        try:
            stress_mpa = self.applied_stress_mpa.get()
            voltage_v = self.applied_voltage_v.get()
            thickness_mm = self.crystal_thickness_mm.get()
            if thickness_mm <= 0: thickness_mm = DEFAULT_THICKNESS_MM # Fallback
            thickness_m = thickness_mm / 1000.0

            # --- Get Material Parameters ---
            d33 = self.params["d33"] * 1e-12 # pm/V or pC/N -> m/V or C/N
            g33 = self.params["g33"] * 1e-3  # mVm/N -> Vm/N
            Y_gpa = self.params["Y"]
            Y_pa = Y_gpa * 1e9 if Y_gpa else 0 # GPa -> Pa, handle Y=0 case
            stress_pa = stress_mpa * 1e6     # MPa -> Pa

            # --- Core Calculations ---
            calculated_strain = 0.0
            calculated_polarization_C_m2 = 0.0
            calculated_voltage = 0.0
            derived_stress_pa = stress_pa # For graph consistency
            derived_voltage_v = voltage_v # For graph consistency

            if self.effect_type.get() == "Direct":
                # Strain is caused by stress
                calculated_strain = stress_pa / Y_pa if Y_pa > 1e-9 else 0.0
                # Polarization is induced by stress
                calculated_polarization_C_m2 = d33 * stress_pa
                # Voltage is generated by stress
                calculated_voltage = g33 * stress_pa * thickness_m
                derived_voltage_v = calculated_voltage # Use calculated V for S-V plot

            else: # Converse
                electric_field_V_m = voltage_v / thickness_m if thickness_m > 1e-9 else 0.0
                # Strain is caused by electric field
                calculated_strain = d33 * electric_field_V_m
                # Polarization is related to applied field (P = epsilon_0*(er-1)*E)
                # For consistency with d33, estimate P from equivalent stress P = d*sigma = d*Y*S
                equiv_stress_pa = Y_pa * calculated_strain if Y_pa > 1e-9 else 0.0
                calculated_polarization_C_m2 = d33 * equiv_stress_pa
                derived_stress_pa = equiv_stress_pa # Use equivalent stress for P-S plot

            # Convert Polarization for plotting
            calculated_polarization_uC_m2 = calculated_polarization_C_m2 * 1e6

            # --- Update History ---
            current_stress_mpa = derived_stress_pa / 1e6
            current_voltage = derived_voltage_v

            # Add to history if changed significantly
            if abs(current_stress_mpa - self.stress_history[-1]) > UPDATE_THRESHOLD or \
               abs(calculated_polarization_uC_m2 - self.polarization_history[-1]) > UPDATE_THRESHOLD:
                self.stress_history.append(current_stress_mpa)
                self.polarization_history.append(calculated_polarization_uC_m2)

            if abs(current_voltage - self.voltage_history[-1]) > UPDATE_THRESHOLD * 10 or \
               abs(calculated_strain - self.strain_history[-1]) > UPDATE_THRESHOLD * 1e-6:
                self.voltage_history.append(current_voltage)
                self.strain_history.append(calculated_strain)

            # --- Update Visualizations ---
            self.draw_macroscopic(calculated_strain, calculated_polarization_C_m2, current_voltage)
            # Pass strain to unit cell as the primary driver for geometric change
            self.draw_unit_cell(calculated_strain)
            self.update_graphs()

        except tk.TclError:
            print("Warning: Invalid numeric input detected.") # Handle TclError during get()
        except Exception as e:
            print(f"An error occurred during simulation update: {e}")


    def draw_macroscopic(self, strain, polarization_C_m2, voltage):
        """Updates the macroscopic view canvas."""
        self.macro_canvas.delete("all")
        # Use actual canvas dimensions after window is drawn
        self.macro_canvas.update_idletasks()
        w = self.macro_canvas.winfo_width()
        h = self.macro_canvas.winfo_height()
        if w < 20 or h < 20: return # Avoid drawing if canvas too small initially

        base_w = w * 0.4
        base_h = h * 0.6
        center_x = w / 2
        top_y = h * 0.15 # Adjusted margins
        bottom_y = top_y + base_h

        # Apply strain (exaggerated along height)
        # Strain is unitless deformation (delta_L / L)
        delta_h = base_h * strain * VIS_EXAGGERATION_MACRO
        current_h = base_h + delta_h
        # Adjust top/bottom y based on center change
        current_top_y = top_y - delta_h / 2
        current_bottom_y = bottom_y + delta_h / 2

        # Draw crystal body
        self.macro_canvas.create_rectangle(center_x - base_w/2, current_top_y,
                                           center_x + base_w/2, current_bottom_y,
                                           fill="lightblue", outline="black", width=2)

        # Draw stress/force indicators
        stress = self.applied_stress_mpa.get()
        if self.effect_type.get() == "Direct" and abs(stress) > 0.1:
            arrow_len = h * 0.12
            arrow_width = max(1, min(8, abs(stress) * 0.1)) # Cap max width
            color = "red" if stress > 0 else "green" # Compression/Tension
            if stress > 0: # Compression
                self.macro_canvas.create_line(center_x, current_top_y - arrow_len, center_x, current_top_y, arrow=tk.LAST, fill=color, width=arrow_width)
                self.macro_canvas.create_line(center_x, current_bottom_y + arrow_len, center_x, current_bottom_y, arrow=tk.FIRST, fill=color, width=arrow_width)
            else: # Tension
                 self.macro_canvas.create_line(center_x, current_top_y - arrow_len, center_x, current_top_y, arrow=tk.FIRST, fill=color, width=arrow_width)
                 self.macro_canvas.create_line(center_x, current_bottom_y + arrow_len, center_x, current_bottom_y, arrow=tk.LAST, fill=color, width=arrow_width)

        # Determine sign for charge display based on mode and sign conventions
        # Assuming positive d33: Compression (+) -> Top (-), Bottom (+) Polarization -> Positive Voltage
        # Positive Voltage -> Top (-), Bottom (+) E-field -> Positive Strain (Elongation)
        display_sign = 0
        if self.effect_type.get() == "Direct":
             display_sign = np.sign(voltage) # Voltage sign reflects polarization sign
        else: # Converse
             display_sign = np.sign(self.applied_voltage_v.get()) # Use applied voltage sign

        # Draw charge/voltage indicators
        charge_magnitude = abs(polarization_C_m2 if self.effect_type.get() == "Direct" else self.applied_voltage_v.get())
        charge_density = charge_magnitude * CHARGE_DENSITY_FACTOR * (w * 0.6)
        num_charges = min(int(charge_density * 50), 50)

        if num_charges > 1 :
            charge_spacing = base_w / (num_charges + 1)
            for i in range(num_charges):
                x_pos = center_x - base_w/2 + charge_spacing * (i + 1)
                if display_sign > 0:
                    # Top surface negative, bottom positive
                    self.macro_canvas.create_text(x_pos, current_top_y - 10, text="-", font=("Arial", 12, "bold"), fill="blue")
                    self.macro_canvas.create_text(x_pos, current_bottom_y + 10, text="+", font=("Arial", 12, "bold"), fill="red")
                elif display_sign < 0:
                    # Top surface positive, bottom negative
                    self.macro_canvas.create_text(x_pos, current_top_y - 10, text="+", font=("Arial", 12, "bold"), fill="red")
                    self.macro_canvas.create_text(x_pos, current_bottom_y + 10, text="-", font=("Arial", 12, "bold"), fill="blue")

        # Display calculated V (direct) or Strain (converse)
        info_text = f"Strain: {strain:.3e}" if self.effect_type.get() == "Converse" else f"Voltage: {voltage:.3f} V"
        self.macro_canvas.create_text(10, 10, anchor="nw", font=("Arial", 9), fill="black", text=info_text)
        # Add polarization display
        pol_text = f"Polarization: {polarization_C_m2*1e6:.2f} µC/m²"
        self.macro_canvas.create_text(w-10, 10, anchor="ne", font=("Arial", 9), fill="black", text=pol_text)


    def draw_unit_cell(self, strain):
        """Updates the 3D unit cell plot with atom displacements based on strain."""
        self.ax_3d.cla() # Clear previous plot

        # Get original positions from the stored copy
        original_atoms = self.original_atom_positions
        colors = self.params["colors"]
        shift_vector = self.params["shift_vector"]
        shifting_atom_types = self.params["shifting_atoms"]
        fixed_atom_types = self.params["fixed_atoms"]

        # Calculate shift magnitude based on longitudinal strain (S3)
        # The displacement delta_z is proportional to strain (delta_L / L)
        # delta_z = S3 * L_z (where L_z is unit cell height, assumed 1 here)
        # Scale this displacement for visibility
        shift_magnitude = strain * VIS_EXAGGERATION_MICRO

        plotted_labels = set() # To avoid duplicate legend entries

        # Draw atoms with calculated shifts
        for atom_type, positions in original_atoms.items():
            pos_array = np.array(positions) # Original positions
            color = colors.get(atom_type, 'black') # Default color if missing

            # Apply shift ONLY to designated 'shifting' atoms
            if atom_type in shifting_atom_types:
                # Calculate displacement vector (direction * magnitude)
                displacement = shift_vector * shift_magnitude
                # Add displacement to original positions
                current_pos_array = pos_array + displacement
            else: # Fixed atoms stay at original positions
                current_pos_array = pos_array

            # Ensure array is 2D for scatter plot
            if current_pos_array.ndim == 1:
                 current_pos_array = current_pos_array.reshape(1, -1)

            # Plotting
            if current_pos_array.shape[1] == 3:
                label = atom_type if atom_type not in plotted_labels else ""
                self.ax_3d.scatter(current_pos_array[:, 0], current_pos_array[:, 1], current_pos_array[:, 2],
                                   c=color, label=label, s=100, depthshade=True, alpha=0.8)
                plotted_labels.add(atom_type)

        # Draw unit cell outline (simple cube for now)
        corners = np.array([[0,0,0], [1,0,0], [1,1,0], [0,1,0], [0,0,1], [1,0,1], [1,1,1], [0,1,1]])
        edges = [[0,1], [1,2], [2,3], [3,0], [4,5], [5,6], [6,7], [7,4], [0,4], [1,5], [2,6], [3,7]]
        for edge in edges:
            points = corners[edge]
            self.ax_3d.plot(points[:,0], points[:,1], points[:,2], 'k--', linewidth=0.7, alpha=0.5)


        self.ax_3d.set_xlabel("X")
        self.ax_3d.set_ylabel("Y")
        self.ax_3d.set_zlabel("Z (Polar Axis)")
        self.ax_3d.set_title(f"Unit Cell ({self.params['structure']})", fontsize=9)

        # Set consistent limits to keep the view stable during shifts
        self.ax_3d.set_xlim(-0.2, 1.2)
        self.ax_3d.set_ylim(-0.2, 1.2)
        self.ax_3d.set_zlim(-0.2, 1.2) # Adjust if shifts go too far

        self.ax_3d.legend(fontsize=8)
        self.ax_3d.tick_params(axis='both', which='major', labelsize=8)
        self.ax_3d.view_init(elev=25, azim=40) # Adjust view angle

        self.unit_cell_canvas.draw_idle()


    def update_graphs(self):
        """Updates the 2D Matplotlib graphs."""
        if not self.stress_history or not self.voltage_history: return # Skip if no data

        try:
            # Stress vs Polarization plot
            if len(self.stress_history) > 1:
                sort_idx_sp = np.argsort(self.stress_history)
                sorted_stress = np.array(self.stress_history)[sort_idx_sp]
                sorted_polarization = np.array(self.polarization_history)[sort_idx_sp]
                self.line_sp.set_data(sorted_stress, sorted_polarization)
            else:
                 self.line_sp.set_data([],[]) # Clear plot if only origin point
            self.ax_sp.relim()
            self.ax_sp.autoscale_view(tight=False)
            self.ax_sp.margins(0.1) # Add margin
            self.fig_sp.canvas.draw_idle()

            # Strain vs Voltage plot
            if len(self.voltage_history) > 1:
                sort_idx_sv = np.argsort(self.voltage_history)
                sorted_voltage = np.array(self.voltage_history)[sort_idx_sv]
                sorted_strain = np.array(self.strain_history)[sort_idx_sv]
                self.line_sv.set_data(sorted_voltage, sorted_strain)
            else:
                 self.line_sv.set_data([],[]) # Clear plot if only origin point
            self.ax_sv.relim()
            self.ax_sv.autoscale_view(tight=False)
            self.ax_sv.margins(0.1)
            self.ax_sv.yaxis.set_major_formatter(plt.FormatStrFormatter('%.2e')) # Scientific notation for strain
            self.fig_sv.canvas.draw_idle()
        except Exception as e:
             print(f"Error updating graphs: {e}")


# --- Main Execution ---
if __name__ == "__main__":
    root = tk.Tk()
    app = PiezoSimulatorApp(root)
    root.mainloop()
```
