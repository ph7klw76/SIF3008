

![image](https://github.com/user-attachments/assets/a09c2851-40be-4edb-8e82-ec19616ffb86)


```python
import sys
import math
import random
# No 'time' import needed if relying on QTimer

# --- Constants ---
EPSILON_0 = 8.854187817e-12

# --- Simulation Logic Classes (Completely unchanged - No Qt dependency) ---

# <--- PASTE the full PiezoelectricSensor class definition here --->
# (This class is identical to the one in the PyQt6 version)
class PiezoelectricSensor:
    """
    Simulates a pressure sensor based on a piezoelectric film.
    (Keep the docstring and code exactly as before)
    """
    def __init__(self, material_name="PVDF", g33=0.2, thickness_um=28, area_mm2=10):
        if g33 <= 0 or thickness_um <= 0 or area_mm2 <= 0:
            raise ValueError("Sensor parameters (g33, thickness, area) must be positive.")
        self.material_name = material_name
        self.g33 = g33 # Vm/N
        self.thickness = thickness_um * 1e-6 # Convert um to m
        self.area = area_mm2 * 1e-6 # Convert mm^2 to m^2
        self.thickness_um = thickness_um # Store for display
        self.area_mm2 = area_mm2 # Store for display
        self.d33_derived = None
        self.epsilon_r_assumed = None
        # No print statements in __init__ for GUI version

    def _estimate_d33(self, epsilon_r=12.0):
        """ Estimate d33 based on g33 and an assumed relative permittivity. """
        self.epsilon_r_assumed = epsilon_r
        self.d33_derived = self.g33 * EPSILON_0 * epsilon_r # C/N or pC/N
        # No print statements needed here for GUI logic

    def calculate_voltage(self, pressure_pa):
        """
        Calculates the output voltage for a given applied pressure.
        Returns voltage or None if pressure is non-positive.
        """
        if pressure_pa <= 0:
            return None # Indicate invalid input for GUI handling
        voltage = self.g33 * self.thickness * pressure_pa
        return voltage

    def get_configuration_details(self):
        """ Returns a dictionary of configuration parameters for display. """
        details = {
            "Material": self.material_name,
            "g33 (Vm/N)": f"{self.g33:.3f}",
            "Thickness (µm)": f"{self.thickness_um:.1f}",
            "Area (mm²)": f"{self.area_mm2:.1f}",
            "Principle": "V = g33 * thickness * Pressure"
        }
        if self.d33_derived:
            details["Assumed ε_r"] = f"{self.epsilon_r_assumed:.1f}"
            details["Derived d33 (pC/N)"] = f"{self.d33_derived * 1e12:.2f}"
        return details

# <--- PASTE the full DielectricActuator class definition here --->
# (This class is identical to the one in the PyQt6 version)
class DielectricActuator:
    """
    Simulates a cantilever beam deflected by a dielectric actuator mechanism.
    (Keep the docstring and code exactly as before, removing prints from __init__)
    """
    def __init__(self, length_um=200, width_um=20, thickness_um=2, gap_um=1,
                 youngs_modulus_gpa=160, dielectric_constant=3.9):
        if length_um <= 0 or width_um <= 0 or thickness_um <= 0 or gap_um <= 0 or \
           youngs_modulus_gpa <= 0 or dielectric_constant <= 0:
            raise ValueError("Actuator parameters must be positive.")

        # Store original units for display
        self.length_um=length_um
        self.width_um=width_um
        self.thickness_um=thickness_um
        self.gap_um=gap_um
        self.youngs_modulus_gpa=youngs_modulus_gpa

        self.length = length_um * 1e-6 # m
        self.width = width_um * 1e-6 # m
        self.thickness = thickness_um * 1e-6 # m
        self.initial_gap = gap_um * 1e-6 # m
        self.youngs_modulus = youngs_modulus_gpa * 1e9 # Pa
        self.epsilon_r = dielectric_constant
        self.area = self.length * self.width # Approximation

        self.moment_of_inertia = (self.width * self.thickness**3) / 12 # m^4
        self.beam_stiffness = 0 # Initialize
        self.pull_in_voltage = float('inf')
        self.k_factor = 0 # Initialize

        if self.moment_of_inertia > 0 and self.length > 0 and self.youngs_modulus > 0:
             # Add safeguard for length being zero
             if self.length > 1e-12: # Avoid division by very small number too
                 self.beam_stiffness = (3 * self.youngs_modulus * self.moment_of_inertia) / self.length**3 # N/m
             else:
                  self.beam_stiffness = 0 # Assign zero stiffness if length is effectively zero
        else:
             self.beam_stiffness = 0 # Avoid division by zero or invalid calc


        if self.beam_stiffness > 0 and self.initial_gap > 0 and EPSILON_0 > 0 and self.epsilon_r > 0 and self.area > 0:
            pull_in_denominator = (27 * EPSILON_0 * self.epsilon_r * self.area)
            if pull_in_denominator > 0:
                pull_in_term = (8 * self.beam_stiffness * self.initial_gap**3) / pull_in_denominator
                if pull_in_term >= 0:
                    self.pull_in_voltage = math.sqrt(pull_in_term)

        # Calculate k_factor based on pull-in voltage if available
        if self.pull_in_voltage > 0 and self.pull_in_voltage != float('inf'):
            test_voltage = self.pull_in_voltage / 2.0
            target_deflection = self.initial_gap / 4.0 # Target deflection at V_pi/2
            if test_voltage > 1e-9: # Avoid division by zero
                 self.k_factor = target_deflection / (test_voltage**2) # m/V²
            else:
                 self.k_factor = (self.initial_gap / 10.0) / (max(1.0, self.initial_gap * 1e6)**2) # Default based on gap
        else:
             # Assign a default k_factor if pull-in couldn't be calculated, make somewhat realistic
             self.k_factor = (self.initial_gap / 10.0) / (max(1.0, self.initial_gap * 1e6)**2) # Deflect gap/10 at ~1V or scaled by gap

    def calculate_deflection(self, voltage):
        """
        Calculates deflection. Returns deflection (m), 'pull-in', or 0.
        """
        voltage = abs(voltage) # Use absolute voltage for deflection calculation
        if voltage < 1e-9: # Treat near-zero voltage as zero
             return 0.0

        if self.pull_in_voltage != float('inf') and voltage >= self.pull_in_voltage:
            return 'pull-in' # Special string code for pull-in

        # Ensure k_factor is non-negative
        if self.k_factor < 0:
              # print("Warning: k_factor is negative. Resetting deflection to 0.") # Keep prints out of core logic
              return 0.0

        deflection = self.k_factor * voltage**2

        # Physical limit check (less critical now pull-in handled first)
        # Theoretical pull-in happens at gap/3 deflection
        pull_in_deflection_limit = self.initial_gap / 3.0

        if self.pull_in_voltage != float('inf') and deflection >= pull_in_deflection_limit:
             return 'pull-in' # Treat as unstable pull-in

        # Final safeguard: Deflection cannot physically exceed the gap
        if deflection >= self.initial_gap:
             return 'pull-in'

        return deflection


    def get_configuration_details(self):
        """ Returns a dictionary of configuration parameters for display. """
        details = {
            "Length (µm)": f"{self.length_um}",
            "Width (µm)": f"{self.width_um}",
            "Thickness (µm)": f"{self.thickness_um}",
            "Initial Gap (µm)": f"{self.gap_um:.1f}",
            "Young's Mod. (GPa)": f"{self.youngs_modulus_gpa}",
            "Dielectric Const. (ε_r)": f"{self.epsilon_r:.1f}",
            "Beam Stiffness (N/m)": f"{self.beam_stiffness:.3f}",
            "Est. Pull-In Voltage (V)": f"{self.pull_in_voltage:.2f}" if self.pull_in_voltage != float('inf') else "N/A",
            "k_factor (m/V²)": f"{self.k_factor:.3e}",
            "Principle": "Deflection δ ≈ k * V² (until pull-in)"
        }
        return details

# <--- PASTE the full FerroelectricMemoryCell class definition here --->
# (This class is identical to the one in the PyQt6 version)
class FerroelectricMemoryCell:
    """
    Simulates a basic Ferroelectric RAM (FeRAM) memory cell.
    (Keep the docstring and code exactly as before, removing prints from __init__)
    """
    def __init__(self, material_name="PZT", coercive_field_MV_m=5, thickness_nm=100, remnant_polarization_uC_cm2=20):
        if coercive_field_MV_m <= 0 or thickness_nm <= 0 or remnant_polarization_uC_cm2 <= 0:
            raise ValueError("Memory cell parameters must be positive.")

        self.material_name = material_name
        self.coercive_field_MV_m = coercive_field_MV_m # Store for display
        self.thickness_nm = thickness_nm # Store for display
        self.remnant_polarization_uC_cm2 = remnant_polarization_uC_cm2 # Store for display

        self.coercive_field = coercive_field_MV_m * 1e6 # Convert MV/m to V/m
        self.thickness = thickness_nm * 1e-9 # Convert nm to m
        self.remnant_polarization = remnant_polarization_uC_cm2 * 1e-6 * 1e4 # Convert µC/cm² to C/m²
        self.coercive_voltage = self.coercive_field * self.thickness # V

        # Initialize state randomly or to a default (e.g., 0)
        self._state = random.choice([0, 1]) # 0 represents -Pr, 1 represents +Pr

    def write(self, value, applied_voltage):
        """
        Attempts to write a binary value (0 or 1) to the memory cell.
        Returns (bool: success, bool: state_changed, str: message)
        """
        required_polarity_ok = (value == 1 and applied_voltage > 0) or \
                               (value == 0 and applied_voltage < 0)
        state_changed = False

        if value not in [0, 1]:
             return False, state_changed, f"Invalid desired state: {value}. Must be 0 or 1."

        if not required_polarity_ok:
            message = "Write Failed: Incorrect voltage polarity for desired state."
            if value == 1: message += " (Need V > 0 for state 1)"
            else: message += " (Need V < 0 for state 0)"
            return False, state_changed, message

        if abs(applied_voltage) > self.coercive_voltage:
            if self._state != value:
                old_state_str = f"{self._state} ({'+Pr' if self._state == 1 else '-Pr'})"
                self._state = value
                new_state_str = f"{self._state} ({'+Pr' if self._state == 1 else '-Pr'})"
                state_changed = True
                # Simulate delay effect handled by GUI timer now
                return True, state_changed, f"State switched from {old_state_str} to {new_state_str}."
            else:
                return True, state_changed, f"Write successful: Voltage sufficient, state already {self._state}."
        else:
            return False, state_changed, f"Write Failed: |V| ({abs(applied_voltage):.3f}V) <= Vc ({self.coercive_voltage:.3f}V). State remains {self._state}."

    def read(self):
        """ Reads the current state of the memory cell. """
        return self._state

    def get_state_description(self):
        """ Returns a string describing the current state. """
        return f"{self._state} ({'+Pr' if self._state == 1 else '-Pr'})"


    def get_configuration_details(self):
        """ Returns a dictionary of configuration parameters for display. """
        details = {
            "Material": self.material_name,
            "Coercive Field (MV/m)": f"{self.coercive_field_MV_m:.1f}",
            "Thickness (nm)": f"{self.thickness_nm:.0f}",
            "Remnant Pol. (µC/cm²)": f"{self.remnant_polarization_uC_cm2:.1f}",
            "Coercive Voltage (Vc) (V)": f"{self.coercive_voltage:.3f}",
            "Principle": "Apply |V| > Vc with correct polarity to flip state"
        }
        return details


# --- PyQt5 GUI Application ---
# Changed imports from PyQt6 to PyQt5
from PyQt5.QtWidgets import (
    QApplication, QMainWindow, QWidget, QVBoxLayout, QHBoxLayout,
    QLabel, QPushButton, QDoubleSpinBox, QTabWidget, QFormLayout,
    QGroupBox, QTextEdit, QComboBox, QMessageBox, QSizePolicy, QFrame
)
from PyQt5.QtCore import (
    Qt, QTimer, QRectF, QPointF, pyqtSignal, QPropertyAnimation, QObject,
    pyqtProperty, QAbstractAnimation # QPropertyAnimation needs QObject, pyqtProperty
)
from PyQt5.QtGui import (
    QPainter, QColor, QPen, QBrush, QFont, QPainterPath, QPolygonF
)


# --- Visualization Widgets (Using PyQt5 imports now) ---

class SensorVisualWidget(QWidget):
    """Widget to visualize the piezoelectric sensor."""
    def __init__(self, parent=None):
        super().__init__(parent)
        self.setMinimumSize(200, 150)
        self._pressure = 0.0
        self._voltage = 0.0
        self._max_voltage_display = 1.0 # Adjust based on expected range, maybe dynamically
        self._max_pressure_display = 10000.0 # 10kPa

    def set_pressure_voltage(self, pressure, voltage):
        self._pressure = pressure if pressure is not None else 0.0
        self._voltage = voltage if voltage is not None else 0.0

        # Auto-adjust max voltage display slightly above current max
        if self._voltage > self._max_voltage_display * 0.95:
            self._max_voltage_display = self._voltage * 1.2
        elif self._voltage < 1e-6 and self._max_voltage_display > 1.0: # Reset if voltage drops low
             self._max_voltage_display = 1.0

        # Similar auto-adjust for pressure (optional)
        if self._pressure > self._max_pressure_display * 0.95:
             self._max_pressure_display = self._pressure * 1.2
        elif self._pressure < 1 and self._max_pressure_display > 1000:
             self._max_pressure_display = 1000.0


        self.update() # Trigger repaint

    def paintEvent(self, event):
        painter = QPainter(self)
        painter.setRenderHint(QPainter.Antialiasing) # Use non-namespaced enum for PyQt5

        width = self.width()
        height = self.height()
        padding = 15

        # --- Draw Sensor Representation ---
        sensor_rect = QRectF(padding * 2, height * 0.3, width * 0.4, height * 0.4)
        painter.setBrush(QColor(200, 200, 220)) # Light purple/gray
        painter.setPen(QPen(Qt.black, 1)) # Use Qt.black instead of Qt.GlobalColor.black
        painter.drawRect(sensor_rect.toRect()) # Often need toRect() for int versions in PyQt5 painter funcs
        painter.drawText(sensor_rect.toRect(), Qt.AlignCenter, "Piezo Film") # Use Qt.AlignCenter

        # --- Draw Pressure Arrows ---
        pressure_ratio = min(self._pressure / self._max_pressure_display, 1.0) if self._max_pressure_display > 0 else 0
        num_arrows = max(1, int(pressure_ratio * 5))
        arrow_len = 15 + 10 * pressure_ratio

        painter.setPen(QPen(Qt.red, 2)) # Use Qt.red
        for i in range(num_arrows):
            x_start = sensor_rect.left() + sensor_rect.width() * (i + 1) / (num_arrows + 1)
            y_start = sensor_rect.top() - 5
            y_end = y_start - arrow_len
            # Draw arrow line
            painter.drawLine(QPointF(x_start, y_start), QPointF(x_start, y_end))
            # Draw arrowhead
            arrow_head = QPolygonF([
                QPointF(x_start, y_end),
                QPointF(x_start - 4, y_end + 8),
                QPointF(x_start + 4, y_end + 8),
            ])
            painter.setBrush(Qt.red) # Use Qt.red
            painter.drawPolygon(arrow_head)

        painter.drawText(QPointF(sensor_rect.center().x(), sensor_rect.top() - arrow_len - 10),
                         f"P ≈ {self._pressure:.1f} Pa")


        # --- Draw Voltage Meter ---
        meter_x = width * 0.7
        meter_width = width * 0.15
        meter_height = height * 0.7
        meter_y = (height - meter_height) / 2

        meter_rect = QRectF(meter_x, meter_y, meter_width, meter_height)
        painter.setBrush(Qt.white) # Use Qt.white
        painter.setPen(QPen(Qt.black, 1))
        painter.drawRect(meter_rect.toRect())

        # Draw voltage bar
        voltage_ratio = min(abs(self._voltage) / self._max_voltage_display, 1.0) if self._max_voltage_display > 0 else 0
        bar_height = meter_height * voltage_ratio
        bar_y = meter_y + meter_height - bar_height
        bar_rect = QRectF(meter_x, bar_y, meter_width, bar_height)
        painter.setBrush(QColor(100, 200, 100) if self._voltage >= 0 else QColor(200, 100, 100)) # Green for pos, Red for neg (unlikely here)
        painter.drawRect(bar_rect.toRect())

        # Draw scale ticks (optional)
        num_ticks = 5
        painter.setPen(QPen(Qt.black, 1)) # Ensure pen is black for ticks
        for i in range(num_ticks + 1):
             tick_y = meter_y + meter_height * (1 - i / num_ticks)
             painter.drawLine(QPointF(meter_x - 3, tick_y), QPointF(meter_x, tick_y))
             v_label = self._max_voltage_display * (i/num_ticks)
             painter.drawText(QPointF(meter_x - padding*1.5 - 5, tick_y + 4), f"{v_label:.2f}") # Adjust text position slightly

        painter.drawText(QPointF(meter_x, meter_y + meter_height + padding), "Voltage (V)")


# Helper class for animating deflection smoothly
class ActuatorAnimator(QObject):
    def __init__(self, visual_widget):
        super().__init__()
        self._widget = visual_widget
        # Property name must be bytes in PyQt5
        self._animation = QPropertyAnimation(self, b"current_deflection_ratio")
        self._animation.setDuration(300) # ms duration for animation

    # Define a property that the animation system can work with
    # Type needs to be specified for pyqtProperty in PyQt5
    @pyqtProperty(float)
    def current_deflection_ratio(self):
        return self._widget._current_deflection_ratio

    @current_deflection_ratio.setter
    def current_deflection_ratio(self, ratio):
        self._widget.set_current_deflection_ratio(ratio) # Update widget's internal state and trigger repaint

    def animate_to(self, target_ratio):
        # Check state before stopping animation in PyQt5
        if self._animation.state() == QAbstractAnimation.Running:
            self._animation.stop()
        self._animation.setStartValue(self.current_deflection_ratio)
        self._animation.setEndValue(target_ratio)
        self._animation.start()


class ActuatorVisualWidget(QWidget):
    """Widget to visualize the dielectric actuator cantilever."""
    def __init__(self, parent=None):
        super().__init__(parent)
        self.setMinimumSize(300, 200)
        self._target_deflection_m = 0.0 # Target based on calculation
        self._current_deflection_ratio = 0.0 # Current animated state (0 to 1 of target)
        self._initial_gap_m = 1e-6
        self._length_m = 200e-6
        self._thickness_m = 2e-6
        self._is_pull_in = False
        self._voltage = 0.0

        self._animator = ActuatorAnimator(self) # Create animator instance

    def set_geometry(self, initial_gap_m, length_m, thickness_m):
        self._initial_gap_m = max(initial_gap_m, 1e-9) # Avoid zero gap
        self._length_m = max(length_m, 1e-9)
        self._thickness_m = max(thickness_m, 1e-9)

    def set_deflection(self, deflection_m, voltage):
        """Sets the target deflection and starts animation."""
        self._is_pull_in = False
        self._voltage = voltage
        # Target deflection capped by gap for realistic drawing
        self._target_deflection_m = min(deflection_m, self._initial_gap_m * 0.99) # Don't draw fully touching

        # Calculate target ratio for animator (0 to 1 of the gap)
        target_ratio = self._target_deflection_m / self._initial_gap_m if self._initial_gap_m > 0 else 0
        # Ensure target_ratio is float
        self._animator.animate_to(float(target_ratio))


    def set_current_deflection_ratio(self, ratio):
         """Called by the animator during animation."""
         self._current_deflection_ratio = ratio
         self.update() # Trigger repaint for each animation frame


    def set_pull_in(self, voltage):
        """Sets the state to pull-in and starts animation."""
        self._is_pull_in = True
        self._voltage = voltage
        self._target_deflection_m = self._initial_gap_m # Target is full gap for pull-in
        self._animator.animate_to(1.0) # Animate towards full gap closure


    def paintEvent(self, event):
        painter = QPainter(self)
        painter.setRenderHint(QPainter.Antialiasing)
        painter.fillRect(self.rect(), Qt.white) # White background

        width = self.width()
        height = self.height()
        padding = 20
        drawing_area_height = height - 2 * padding
        drawing_area_width = width - 2 * padding

        # --- Scaling ---
        vis_length = drawing_area_width * 0.8
        vis_gap = drawing_area_height * 0.3
        vis_thickness = max(3, vis_gap * (self._thickness_m / self._initial_gap_m) if self._initial_gap_m > 0 else 3)

        # --- Coordinates ---
        fixed_support_width = drawing_area_width * 0.1
        anchor_x = padding + fixed_support_width
        anchor_y = padding + drawing_area_height * 0.3
        base_electrode_y = anchor_y + vis_gap + vis_thickness
        base_electrode_height = drawing_area_height * 0.15

        # --- Draw Fixed Support ---
        painter.setBrush(Qt.darkGray)
        painter.setPen(Qt.black)
        painter.drawRect(int(padding), int(padding), int(fixed_support_width), int(drawing_area_height * 0.6))

        # --- Draw Base Electrode ---
        painter.setBrush(Qt.lightGray)
        painter.drawRect(int(anchor_x), int(base_electrode_y), int(vis_length), int(base_electrode_height))
        painter.drawText(QPointF(anchor_x + 5, base_electrode_y + base_electrode_height / 2 + 5), "Base Electrode")


        # --- Draw Cantilever ---
        # beam_pen = QPen(QColor(0, 0, 150), vis_thickness) # Blue, thickness scaled
        # beam_pen.setCapStyle(Qt.FlatCap) # Use Qt.FlatCap
        # Need to create pen first, then set properties
        beam_pen = QPen(QColor(0, 0, 150))
        beam_pen.setWidthF(vis_thickness) # Use setWidthF for float thickness
        beam_pen.setCapStyle(Qt.FlatCap)

        current_vis_deflection = self._current_deflection_ratio * vis_gap

        if self._is_pull_in:
             beam_pen.setColor(Qt.red)
             end_y = anchor_y + vis_gap + vis_thickness - vis_thickness/2
             path = QPainterPath()
             path.moveTo(anchor_x, anchor_y)
             control_y_pullin = anchor_y + vis_gap * 0.6
             path.quadTo(anchor_x + vis_length * 0.5, control_y_pullin, anchor_x + vis_length, end_y)
             painter.setPen(beam_pen)
             painter.strokePath(path, beam_pen) # strokePath takes path and pen

             painter.setPen(Qt.black) # Reset pen for text
             painter.drawText(QPointF(padding, height - padding + 10), f"V = {self._voltage:.2f} V -> PULL-IN!")

        else:
            end_x = anchor_x + vis_length
            end_y = anchor_y + current_vis_deflection
            path = QPainterPath()
            path.moveTo(anchor_x, anchor_y)
            control_x = anchor_x + vis_length * 0.5
            control_y = anchor_y + current_vis_deflection * 0.5
            path.quadTo(control_x, control_y, end_x, end_y)

            painter.setPen(beam_pen)
            painter.strokePath(path, beam_pen)

            deflection_um = (self._current_deflection_ratio * self._target_deflection_m) * 1e6
            painter.setPen(Qt.black) # Reset pen for text
            painter.drawText(QPointF(padding, height - padding + 10),
                             f"V = {self._voltage:.2f} V, Deflection ≈ {deflection_um:.3f} µm")

        # Draw Gap Line (optional)
        gap_pen = QPen(Qt.darkCyan, 1, Qt.DashLine) # Use Qt.DashLine directly
        painter.setPen(gap_pen)
        painter.drawLine(int(anchor_x), int(anchor_y + vis_thickness/2) , int(anchor_x), int(base_electrode_y))
        painter.setPen(Qt.black) # Reset pen for text
        painter.drawText(QPointF(anchor_x+5, anchor_y + vis_gap/2 ), f"Gap ≈ {self._initial_gap_m*1e6:.1f} µm")



class MemoryVisualWidget(QWidget):
    """Widget to visualize the ferroelectric memory cell state."""
    def __init__(self, parent=None):
        super().__init__(parent)
        self.setMinimumSize(150, 200)
        self._state = 0 # 0 (-Pr) or 1 (+Pr)
        self._is_writing = False
        self._write_success = None # True, False, or None

        self._animation_timer = QTimer(self)
        self._animation_timer.setSingleShot(True)
        self._animation_timer.timeout.connect(self._end_write_animation)
        self._flash_color = None

    def set_state(self, state, writing=False, success=None):
        self._state = state
        self._is_writing = writing
        self._write_success = success
        self._flash_color = None # Reset flash

        if self._is_writing:
            if self._write_success == False:
                 self._flash_color = QColor(255, 100, 100, 150) # Reddish flash for fail
            elif self._write_success == True:
                 self._flash_color = QColor(100, 255, 100, 150) # Greenish flash for success
            self._animation_timer.start(300) # ms duration for the writing animation/flash
        else:
            # Make sure timer stops if setting state directly while it was running
            if self._animation_timer.isActive():
                self._animation_timer.stop()


        self.update()

    def _end_write_animation(self):
        """Called by timer to finish the write visual state."""
        self._is_writing = False
        self._flash_color = None
        self.update()

    def paintEvent(self, event):
        painter = QPainter(self)
        painter.setRenderHint(QPainter.Antialiasing)
        painter.fillRect(self.rect(), Qt.white) # Background

        width = self.width()
        height = self.height()
        padding = 15

        # --- Draw Capacitor Structure ---
        electrode_height = height * 0.15
        dielectric_height = height * 0.4
        structure_width = width * 0.6
        x_center = width / 2
        structure_x = x_center - structure_width / 2
        top_electrode_y = padding
        dielectric_y = top_electrode_y + electrode_height
        bottom_electrode_y = dielectric_y + dielectric_height

        painter.setBrush(Qt.lightGray)
        painter.setPen(Qt.black)
        painter.drawRect(int(structure_x), int(top_electrode_y), int(structure_width), int(electrode_height))
        painter.drawRect(int(structure_x), int(bottom_electrode_y), int(structure_width), int(electrode_height))
        painter.drawText(QPointF(structure_x + structure_width + 5, top_electrode_y + electrode_height/2), "+V")
        painter.drawText(QPointF(structure_x + structure_width + 5, bottom_electrode_y + electrode_height/2), "-V")


        painter.setBrush(QColor(220, 220, 180)) # Creamy yellow
        painter.drawRect(int(structure_x), int(dielectric_y), int(structure_width), int(dielectric_height))

        # --- Draw Polarization Arrows ---
        arrow_width = structure_width * 0.15
        arrow_height = dielectric_height * 0.5
        arrow_head_height = arrow_height * 0.3
        num_arrows = 3
        spacing = structure_width / (num_arrows + 1)

        current_opacity = painter.opacity() # Store current opacity
        if self._is_writing:
             painter.setOpacity(0.4 * current_opacity) # Fade during writing (relative)
             pen_color = Qt.darkGray
             brush_color = Qt.gray
        else:
             # painter.setOpacity(1.0) # Already handled by restoring later
             if self._state == 1: # +Pr (Up)
                 pen_color = QColor(0, 0, 200) # Blue
                 brush_color = QColor(100, 100, 255)
             else: # -Pr (Down)
                 pen_color = QColor(200, 0, 0) # Red
                 brush_color = QColor(255, 100, 100)

        arrow_pen = QPen(pen_color, 2)
        painter.setPen(arrow_pen)
        painter.setBrush(brush_color)

        for i in range(num_arrows):
             current_arrow_x = structure_x + spacing * (i + 1) - arrow_width / 2

             if self._state == 1: # Pointing Up
                 arrow_base_y = dielectric_y + dielectric_height * 0.8
                 arrow_tip_y = arrow_base_y - arrow_height
                 painter.drawRect(int(current_arrow_x), int(arrow_tip_y + arrow_head_height), int(arrow_width), int(arrow_height - arrow_head_height))
                 arrow_head = QPolygonF([
                     QPointF(current_arrow_x - arrow_width * 0.3, arrow_tip_y + arrow_head_height),
                     QPointF(current_arrow_x + arrow_width * 1.3, arrow_tip_y + arrow_head_height),
                     QPointF(current_arrow_x + arrow_width / 2, arrow_tip_y)
                 ])
                 painter.drawPolygon(arrow_head)
             else: # Pointing Down
                 arrow_base_y = dielectric_y + dielectric_height * 0.2
                 arrow_tip_y = arrow_base_y + arrow_height
                 painter.drawRect(int(current_arrow_x), int(arrow_base_y), int(arrow_width), int(arrow_height - arrow_head_height))
                 arrow_head = QPolygonF([
                      QPointF(current_arrow_x - arrow_width * 0.3, arrow_tip_y - arrow_head_height),
                      QPointF(current_arrow_x + arrow_width * 1.3, arrow_tip_y - arrow_head_height),
                      QPointF(current_arrow_x + arrow_width / 2, arrow_tip_y)
                  ])
                 painter.drawPolygon(arrow_head)

        painter.setOpacity(current_opacity) # Restore opacity

        # --- Draw Flash Effect ---
        if self._flash_color:
             painter.setBrush(self._flash_color)
             painter.setPen(Qt.NoPen) # Use Qt.NoPen
             painter.drawRect(self.rect()) # Draw translucent overlay


        # --- Draw State Label ---
        state_text = "State: 1 (+Pr)" if self._state == 1 else "State: 0 (-Pr)"
        if self._is_writing:
            state_text = "Writing..."
        painter.setPen(Qt.black)
        # Use painter.boundingRect for text positioning if needed, simpler drawText here
        painter.drawText(QRectF(0, bottom_electrode_y + electrode_height, width, padding*2).toRect(), # Use toRect()
                        Qt.AlignCenter, state_text) # Use Qt.AlignCenter


# --- Main Application Window (Using PyQt5 imports) ---
class MaterialSandboxGUI(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Sensor-Actuator-Memory Triad Sandbox (PyQt5 GUI + Visualization)") # Updated Title
        self.setGeometry(100, 100, 850, 650)

        # Initialize simulation modules (error handling included - no changes here)
        try:
            self.sensor = PiezoelectricSensor(material_name="PVDF", g33=0.2, thickness_um=28, area_mm2=10)
            self.sensor._estimate_d33(epsilon_r=12)

            self.actuator = DielectricActuator(length_um=200, width_um=20, thickness_um=2, gap_um=1,
                                               youngs_modulus_gpa=160, dielectric_constant=3.9)

            self.memory = FerroelectricMemoryCell(material_name="PZT", coercive_field_MV_m=5, thickness_nm=100, remnant_polarization_uC_cm2=20)
        except ValueError as e:
            QMessageBox.critical(self, "Initialization Error", f"Failed to initialize simulation modules:\n{e}\n\nPlease check the default parameters in the code.")
            sys.exit(1)
        except Exception as e:
             QMessageBox.critical(self, "Initialization Error", f"An unexpected error occurred during initialization:\n{e}")
             sys.exit(1)


        self.central_widget = QWidget()
        self.main_layout = QVBoxLayout(self.central_widget)
        self.setCentralWidget(self.central_widget)

        self.tab_widget = QTabWidget()
        self.main_layout.addWidget(self.tab_widget)

        # Create tabs (use PyQt5 widgets)
        self._create_sensor_tab()
        self._create_actuator_tab()
        self._create_memory_tab()

        # Set initial state for visualizations
        self._read_memory_state() # Update memory visual initially
        self.actuator_visual_widget.set_geometry(
            self.actuator.initial_gap, self.actuator.length, self.actuator.thickness)
        self.actuator_visual_widget.set_deflection(0.0, 0.0) # Start at zero deflection


        # Access status bar using self.statusBar()
        self.statusBar().showMessage("Ready")

    def _create_config_display(self, config_dict):
        # (This helper function remains the same - uses basic widgets)
        group_box = QGroupBox("Configuration")
        layout = QFormLayout()
        bold_font = QFont()
        bold_font.setBold(True)

        for key, value in config_dict.items():
            label_widget = QLabel(key + ":")
            value_widget = QLabel(str(value))
            value_widget.setWordWrap(True)
            if "principle" in key.lower():
                label_widget.setFont(bold_font)
                value_widget.setFont(bold_font)
            layout.addRow(label_widget, value_widget)

        group_box.setLayout(layout)
        # Use PyQt5 SizePolicy values if needed, but Preferred usually works
        group_box.setSizePolicy(QSizePolicy.Preferred, QSizePolicy.Preferred)
        return group_box

    # --- Sensor Tab ---
    def _create_sensor_tab(self):
        tab = QWidget()
        main_hbox = QHBoxLayout(tab)

        left_vbox = QVBoxLayout()
        # Use Qt.AlignTop directly in PyQt5
        left_vbox.setAlignment(Qt.AlignTop)

        input_group = QGroupBox("Simulation Input")
        input_layout = QFormLayout()
        self.pressure_input = QDoubleSpinBox()
        self.pressure_input.setRange(0, 1e7)
        self.pressure_input.setDecimals(2)
        self.pressure_input.setValue(1000.0)
        self.pressure_input.setSuffix(" Pa")
        input_layout.addRow("Applied Pressure:", self.pressure_input)
        input_group.setLayout(input_layout)
        left_vbox.addWidget(input_group)

        calc_button = QPushButton("Calculate Voltage")
        calc_button.clicked.connect(self._calculate_sensor_voltage)
        left_vbox.addWidget(calc_button)

        results_group = QGroupBox("Results")
        results_layout = QFormLayout()
        self.sensor_voltage_output_v = QLabel("-- V")
        self.sensor_voltage_output_mv = QLabel("-- mV")
        results_layout.addRow("Generated Voltage:", self.sensor_voltage_output_v)
        results_layout.addRow("Generated Voltage (mV):", self.sensor_voltage_output_mv)
        results_group.setLayout(results_layout)
        left_vbox.addWidget(results_group)

        config_details = self.sensor.get_configuration_details()
        left_vbox.addWidget(self._create_config_display(config_details))

        left_widget = QWidget()
        left_widget.setLayout(left_vbox)
        main_hbox.addWidget(left_widget, 1)

        right_vbox = QVBoxLayout()
        right_vbox.setAlignment(Qt.AlignTop)

        visual_group = QGroupBox("Visualization")
        visual_layout = QVBoxLayout()
        self.sensor_visual_widget = SensorVisualWidget()
        visual_layout.addWidget(self.sensor_visual_widget)
        visual_group.setLayout(visual_layout)
        right_vbox.addWidget(visual_group)

        explanation = QTextEdit()
        explanation.setReadOnly(True)
        explanation.setHtml(
            "<h3>Piezoelectric Sensor</h3>"
            "<b>Learning Impact:</b> This simulation shows how mechanical pressure applied "
            "to a piezoelectric material (like PVDF or PZT) generates an electrical voltage. "
            # ... rest of explanation ...
             "<br><br><b>Use Case:</b> Pressure sensors, force sensors, microphones, energy harvesting."
        )
        # Use PyQt5 SizePolicy values
        explanation.setSizePolicy(QSizePolicy.Expanding, QSizePolicy.Expanding)
        right_vbox.addWidget(explanation)

        right_widget = QWidget()
        right_widget.setLayout(right_vbox)
        main_hbox.addWidget(right_widget, 2)

        self.tab_widget.addTab(tab, "1. Piezoelectric Sensor")

    def _calculate_sensor_voltage(self):
        # (Logic remains the same, uses GUI elements updated below)
        try:
            pressure = self.pressure_input.value()
            voltage = self.sensor.calculate_voltage(pressure)

            if voltage is None:
                 self.sensor_voltage_output_v.setText("N/A (Pressure must be > 0)")
                 self.sensor_voltage_output_mv.setText("N/A")
                 self.statusBar().showMessage("Calculation error: Pressure must be positive.", 3000)
            else:
                 self.sensor_voltage_output_v.setText(f"{voltage:.6f} V")
                 self.sensor_voltage_output_mv.setText(f"{voltage * 1e3:.3f} mV")
                 self.statusBar().showMessage(f"Voltage calculated for {pressure:.2f} Pa.", 3000)

            self.sensor_visual_widget.set_pressure_voltage(pressure, voltage)

        except Exception as e:
            QMessageBox.warning(self, "Calculation Error", f"An error occurred: {e}")
            self.sensor_voltage_output_v.setText("Error")
            self.sensor_voltage_output_mv.setText("Error")
            self.sensor_visual_widget.set_pressure_voltage(0, 0)
            self.statusBar().showMessage("Calculation failed.", 3000)


    # --- Actuator Tab ---
    def _create_actuator_tab(self):
        tab = QWidget()
        main_hbox = QHBoxLayout(tab)

        left_vbox = QVBoxLayout()
        left_vbox.setAlignment(Qt.AlignTop)

        input_group = QGroupBox("Simulation Input")
        input_layout = QFormLayout()
        self.actuator_voltage_input = QDoubleSpinBox()
        v_pi = self.actuator.pull_in_voltage
        max_volt = (v_pi * 1.2) if v_pi != float('inf') and v_pi > 0 else 100.0
        max_volt = max(1.0, max_volt)
        self.actuator_voltage_input.setRange(0, max_volt)
        self.actuator_voltage_input.setDecimals(3)
        default_volt = min(max(1.0, v_pi/4 if v_pi != float('inf') and v_pi > 0 else 5.0), max_volt * 0.8)
        self.actuator_voltage_input.setValue(default_volt)
        self.actuator_voltage_input.setSuffix(" V")
        input_layout.addRow("Applied Voltage:", self.actuator_voltage_input)
        input_group.setLayout(input_layout)
        left_vbox.addWidget(input_group)

        calc_button = QPushButton("Calculate Deflection")
        calc_button.clicked.connect(self._calculate_actuator_deflection)
        left_vbox.addWidget(calc_button)

        results_group = QGroupBox("Results")
        results_layout = QFormLayout()
        self.actuator_deflection_output = QLabel("-- µm")
        self.actuator_status_output = QLabel("Status: --")
        results_layout.addRow("Tip Deflection:", self.actuator_deflection_output)
        results_layout.addRow("", self.actuator_status_output)
        results_group.setLayout(results_layout)
        left_vbox.addWidget(results_group)

        config_details = self.actuator.get_configuration_details()
        left_vbox.addWidget(self._create_config_display(config_details))

        left_widget = QWidget()
        left_widget.setLayout(left_vbox)
        main_hbox.addWidget(left_widget, 1)

        right_vbox = QVBoxLayout()
        right_vbox.setAlignment(Qt.AlignTop)

        visual_group = QGroupBox("Visualization")
        visual_layout = QVBoxLayout()
        self.actuator_visual_widget = ActuatorVisualWidget()
        visual_layout.addWidget(self.actuator_visual_widget)
        visual_group.setLayout(visual_layout)
        right_vbox.addWidget(visual_group)

        explanation = QTextEdit()
        explanation.setReadOnly(True)
        explanation.setHtml(
            "<h3>Dielectric Actuator (MEMS Cantilever)</h3>"
            "<b>Learning Impact:</b> This simulation demonstrates how applying a voltage across a dielectric "
            # ... rest of explanation ...
            "<br><br><b>Use Case:</b> Micro-mirrors (DLP projectors), RF switches, micro-relays, AFM probes."
        )
        explanation.setSizePolicy(QSizePolicy.Expanding, QSizePolicy.Expanding)
        right_vbox.addWidget(explanation)

        right_widget = QWidget()
        right_widget.setLayout(right_vbox)
        main_hbox.addWidget(right_widget, 2)

        self.tab_widget.addTab(tab, "2. Dielectric Actuator")


    def _calculate_actuator_deflection(self):
        # (Logic remains the same)
        try:
            voltage = self.actuator_voltage_input.value()
            result = self.actuator.calculate_deflection(voltage)

            if isinstance(result, str) and result == 'pull-in':
                self.actuator_deflection_output.setText("PULL-IN!")
                v_pi_text = f"{self.actuator.pull_in_voltage:.2f}V" if self.actuator.pull_in_voltage != float('inf') else "N/A"
                self.actuator_status_output.setText(f"Status: Voltage ({voltage:.2f}V) >= V_pi ({v_pi_text})")
                self.statusBar().showMessage("Pull-in occurred!", 3000)
                self.actuator_visual_widget.set_pull_in(voltage)
            elif isinstance(result, (int, float)):
                deflection_um = result * 1e6
                self.actuator_deflection_output.setText(f"{deflection_um:.4f} µm")
                percent_gap = (result / self.actuator.initial_gap) * 100 if self.actuator.initial_gap > 0 else 0
                self.actuator_status_output.setText(f"Status: OK ({percent_gap:.1f}% of gap)")
                self.statusBar().showMessage(f"Deflection calculated for {voltage:.2f} V.", 3000)
                self.actuator_visual_widget.set_deflection(result, voltage)
            else:
                 raise TypeError(f"Unexpected return type from calculate_deflection: {type(result)}")

        except Exception as e:
            QMessageBox.warning(self, "Calculation Error", f"An error occurred: {e}")
            self.actuator_deflection_output.setText("Error")
            self.actuator_status_output.setText("Status: Error")
            self.actuator_visual_widget.set_deflection(0.0, 0.0)
            self.statusBar().showMessage("Calculation failed.", 3000)


    # --- Memory Tab ---
    def _create_memory_tab(self):
        tab = QWidget()
        main_hbox = QHBoxLayout(tab)

        left_vbox = QVBoxLayout()
        left_vbox.setAlignment(Qt.AlignTop)

        state_group = QGroupBox("Current Memory State")
        state_layout = QFormLayout()
        self.memory_state_label = QLabel("--")
        state_font = QFont()
        state_font.setPointSize(12)
        state_font.setBold(True)
        self.memory_state_label.setFont(state_font)
        # Use Qt.AlignCenter directly
        self.memory_state_label.setAlignment(Qt.AlignCenter)
        state_layout.addRow(self.memory_state_label)
        state_group.setLayout(state_layout)
        left_vbox.addWidget(state_group)

        control_group = QGroupBox("Memory Operations")
        control_layout = QVBoxLayout()

        read_button = QPushButton("Read State (Refresh Display)")
        read_button.clicked.connect(self._read_memory_state)
        control_layout.addWidget(read_button)

        write_sub_group = QGroupBox("Write Operation")
        write_layout = QFormLayout()
        self.memory_write_value_combo = QComboBox()
        self.memory_write_value_combo.addItems(["0 (-Pr)", "1 (+Pr)"])
        self.memory_write_voltage_input = QDoubleSpinBox()
        vc = self.memory.coercive_voltage
        vc = max(vc, 0.1)
        self.memory_write_voltage_input.setRange(-vc * 1.5, vc * 1.5)
        self.memory_write_voltage_input.setDecimals(3)
        self.memory_write_voltage_input.setValue(vc * 1.1 if vc > 0 else 1.0)
        self.memory_write_voltage_input.setSuffix(" V")
        self.memory_write_voltage_input.setToolTip(f"Coercive Voltage (Vc) = {self.memory.coercive_voltage:.3f} V. Use positive for State 1, negative for State 0.")
        write_layout.addRow("Desired State:", self.memory_write_value_combo)
        write_layout.addRow("Applied Voltage:", self.memory_write_voltage_input)
        write_sub_group.setLayout(write_layout)
        control_layout.addWidget(write_sub_group)

        write_button = QPushButton("Attempt Write Operation")
        write_button.clicked.connect(self._write_memory_state)
        control_layout.addWidget(write_button)

        control_group.setLayout(control_layout)
        left_vbox.addWidget(control_group)

        self.memory_write_status_label = QLabel("Write Status: --")
        self.memory_write_status_label.setWordWrap(True)
        left_vbox.addWidget(self.memory_write_status_label)

        config_details = self.memory.get_configuration_details()
        left_vbox.addWidget(self._create_config_display(config_details))

        left_widget = QWidget()
        left_widget.setLayout(left_vbox)
        main_hbox.addWidget(left_widget, 1)

        right_vbox = QVBoxLayout()
        right_vbox.setAlignment(Qt.AlignTop)

        visual_group = QGroupBox("Visualization")
        visual_layout = QVBoxLayout()
        self.memory_visual_widget = MemoryVisualWidget()
        visual_layout.addWidget(self.memory_visual_widget)
        visual_group.setLayout(visual_layout)
        right_vbox.addWidget(visual_group)

        explanation = QTextEdit()
        explanation.setReadOnly(True)
        explanation.setHtml(
             "<h3>Ferroelectric Memory (FeRAM) Cell</h3>"
             "<b>Learning Impact:</b> This simulation models how ferroelectric materials store information. "
            # ... rest of explanation ...
             "<br><br><b>Use Case:</b> Non-volatile RAM (FeRAM) offering fast write speeds and high endurance, smart cards, RFID tags."
         )
        explanation.setSizePolicy(QSizePolicy.Expanding, QSizePolicy.Expanding)
        right_vbox.addWidget(explanation)

        right_widget = QWidget()
        right_widget.setLayout(right_vbox)
        main_hbox.addWidget(right_widget, 1)

        self.tab_widget.addTab(tab, "3. Ferroelectric Memory")


    def _read_memory_state(self):
        # (Logic remains the same)
        current_state = self.memory.read()
        current_state_desc = self.memory.get_state_description()
        self.memory_state_label.setText(current_state_desc)
        self.memory_visual_widget.set_state(current_state, writing=False)
        self.memory_write_status_label.setText("Read Status: State display refreshed.")
        self.statusBar().showMessage(f"Memory state read: {current_state_desc}", 3000)
        return current_state


    def _write_memory_state(self):
        # (Logic remains the same)
        try:
            desired_state_text = self.memory_write_value_combo.currentText()
            desired_state = 0 if "0" in desired_state_text else 1
            applied_voltage = self.memory_write_voltage_input.value()

            success, state_changed, message = self.memory.write(desired_state, applied_voltage)

            self.memory_write_status_label.setText(f"Write Status: {message}")

            current_state = self.memory.read()
            current_state_desc = self.memory.get_state_description()
            self.memory_state_label.setText(current_state_desc)

            self.memory_visual_widget.set_state(current_state, writing=state_changed, success=success)

            if success:
                 self.statusBar().showMessage("Write operation attempted (see status).", 3000)
            else:
                 self.statusBar().showMessage("Write operation failed (see status).", 3000)

        except Exception as e:
            QMessageBox.warning(self, "Write Error", f"An error occurred during write: {e}")
            self.memory_write_status_label.setText("Write Status: Error occurred.")
            self._read_memory_state() # Attempt to sync display after error
            self.statusBar().showMessage("Write operation failed.", 3000)


# --- Main Execution (Using PyQt5) ---
if __name__ == "__main__":
    # QApplication instance needed before creating widgets
    app = QApplication(sys.argv)

    # Optional: Apply a style available in PyQt5
    # app.setStyle('Fusion') # Fusion is generally available

    window = MaterialSandboxGUI()
    window.show()
    sys.exit(app.exec_()) # Use exec_() in PyQt5
```
