# The Drude Model & the Free Electron Model:
## History, Motivation, Equations, and Limitations

### 1. Historical Backdrop and Motivation

#### 1.1. Pre-1900 Understanding of Metal Conductivity

By the late 19th century, scientists knew metals were excellent conductors of electricity and heat. Empirical observations included:

**Ohm’s Law**  
($V = IR$): Demonstrating that the current $I$ through a metal is proportional to the applied voltage $V$. In differential form:

$$
J = \sigma E,
$$

where $J$ is current density, $E$ the electric field, and $\sigma$ the conductivity.

**Wiedemann–Franz Law:**  
A striking relation between the electrical conductivity $\sigma$ and thermal conductivity $\kappa$ of metals:

$$
\frac{\kappa}{\sigma} \approx L T,
$$

where $L$ is the Lorenz number and $T$ is temperature. This implied a common carrier for both heat and electric current—strongly hinting that electrons bear both.

**Discovery of the Electron (1897):**  
J.J. Thomson had established that negatively charged electrons exist; metals appeared to have these electrons available in large supply.

Despite these clues, no microscopic theory explained why electrons in metals moved so freely or how collisions and temperature affected conduction. Physicists yearned for a model that could tie these empirical laws to fundamental principles of mechanics and electromagnetism.

#### 1.2. Paul Drude’s Inspiration and the Kinetic Theory of Gases

Motivated by the kinetic theory of gases, Paul Drude in 1900 proposed that electrons in a metal behave similarly to molecules in an ideal gas—except now, they move through a background of fixed, positively charged ions (the metal lattice). Key motivations for Drude were:

- **Analogy to Gas Molecules:** Treat electrons as classical particles that collide with “obstacles” (ions).
- **Simplification:** Assume these collisions occur randomly, with some average time between collisions.
- **Electrons as Free Charges:** Just like free molecules in a box, conduction electrons in metals roam freely except when they collide.

Although classical at heart, this was a major milestone. It marked the first time a metal’s measurable properties (conductivity, heat conduction, Hall effect) were linked to a microscopic picture of electrons scattering from a lattice.

#### 1.3. Toward Quantum Mechanics: The Birth of the Free Electron (Sommerfeld) Model

Over the next two decades, more experiments revealed inconsistencies in the Drude model—especially regarding specific heat and the temperature-dependence of properties. Once quantum mechanics emerged, Arnold Sommerfeld (1927) proposed a refined approach that respected the Pauli exclusion principle and Fermi-Dirac statistics. This new **“Free Electron Model”** kept the idea of nearly free electrons in a metal but acknowledged that electrons occupy quantized energy states and only a thin shell near the Fermi surface can respond to thermal or electric stimuli at low temperatures.

---

## 2. The Drude Model: Assumptions and Detailed Equations

### 2.1. Core Assumptions

- **Classical Electrons:**  
  Electrons are treated as classical point particles of mass $m$ and charge $-e$ ($e > 0$).

- **Free Except for Collisions:**  
  Electrons accelerate under electric fields but occasionally collide with lattice ions. A single parameter, $\tau$ (mean free time), encapsulates the frequency of collisions.

- **No Electron-Electron Interactions:**  
  Collisions with ions are paramount; electron-electron interactions are neglected or assumed to average out.

- **Random Velocity Reset:**  
  Immediately after a collision, an electron’s velocity is assumed uncorrelated with its velocity before collision (memory loss).

### 2.2. Equation of Motion

If an electric field $E$ is applied, the electron feels a force $-eE$. (If there is also a magnetic field $B$, the Lorentz force $-e v \times B$ can be added, but let us first consider only $E$.)

Drude introduced a damping term $\frac{m}{\tau} v$ to simulate the momentum loss due to collisions. Thus:

$$
m \frac{d v}{d t} = -e E - \frac{m}{\tau} v.
$$

In steady state ($\frac{d v}{d t} = 0$), we have

$$
0 = -e E - \frac{m}{\tau} v \Rightarrow v_{\text{drift}} = -\frac{e \tau}{m} E.
$$

### 2.3. Conductivity and Drift Velocity

The current density $J$ is given by the charge density times the drift velocity:

$$
J = -e n v_{\text{drift}} = -e n \left( -\frac{e \tau}{m} E \right) = \frac{n e^2 \tau}{m} E.
$$

Hence, the electrical conductivity $\sigma$ is:

$$
\sigma = \frac{n e^2 \tau}{m}.
$$

This formula alone was revolutionary: it tied the observed conductivity $\sigma$ of a metal to measurable quantities like the electron density $n$, electron mass $m$, charge $e$, and collision time $\tau$. The resistivity $\rho$ is just:

$$
\rho = \frac{1}{\sigma}.
$$

![image](https://github.com/user-attachments/assets/a702d091-1742-4911-bb49-dedd6285887f)


### 2.4. Thermal Conductivity in Drude’s Picture

Wiedemann–Franz demanded a link between thermal and electric conduction. Drude borrowed from kinetic theory:

**Thermal Flux:**

$$
q \approx \frac{1}{3} n \langle v \ell \rangle c_v \Delta T,
$$

where $\ell \approx v_{\text{thermal}} \tau$ is the mean free path, $v_{\text{thermal}}$ a thermal speed scale, and $c_v$ the specific heat per electron.

**Wiedemann–Franz Law:**  
Drude’s approach qualitatively reproduced

$$
\frac{\kappa}{\sigma} \propto T,
$$

in line with experimental data—although the precise proportionality constant (the Lorenz number) wasn’t perfectly predicted.

### 2.5. Key Successes and Curiosities Sparked by Drude

- **Correct Order of Magnitude:**  
  Drude’s estimate for conductivity was in the right ballpark—first time a classical model gave a “semi-microscopic” basis for $\sigma$.

- **Qualitative Explanation:**  
  Showed how collisions degrade electron momentum, giving rise to resistance.

- **Triggered Many Questions:**  
  For instance, why do electrons not contribute the large classical specific heat predicted by equipartition? Why are Hall coefficients sometimes opposite sign or different magnitude than Drude’s forecast?

These discrepancies set the stage for the next leap, quantum mechanics.

### 2.6. Limitations

- **Classical Specific Heat Overshoot:**  
  Drude predicted an enormous electron contribution to specific heat, whereas experiments at low $T$ show it is much smaller.

- **Hall Effect Sign:**  
  Certain metals exhibit Hall coefficients with sign or magnitude that do not match the naive Drude predictions.

- **No Quantum Statistics:**  
  Electrons must obey Pauli’s exclusion principle, which modifies occupancy drastically, especially at low $T$.

Yet, Drude’s model remained a cornerstone: it was the first to arouse curiosity about how conduction electrons really behave and how classical assumptions might fail at the microscopic level.
# 3. The Free Electron Model (Sommerfeld Model)

## 3.1. Quantum Evolution

Arnold Sommerfeld, leveraging the newly emerging quantum framework (~1920s), tackled Drude’s failings by imposing:

**Fermi–Dirac Statistics:**  
Electrons fill quantum states up to a Fermi energy $E_F$ at $T = 0$.

**Electron Waves:**  
Electrons in a metal are free quantum particles in a box, subject (initially) to periodic boundary conditions (neglecting the periodic ionic potential as a first approximation).

This improved model answered Drude’s biggest shortcoming regarding specific heat and gave more accurate temperature dependences of metallic properties.

## 3.2. Quantized States in a 3D Box

Consider a cubic box of volume $V$. The allowed electron wavefunctions may be written:

$$
\psi_k (r) = \frac{1}{V} e^{i k \cdot r},
$$

with the energy dispersion relation:

$$
E(k) = \frac{\hbar^2 k^2}{2m}.
$$

Here, $k$ is the wave vector. To count how many electron states are available up to a certain $\vert k \vert$, we impose a boundary condition that quantizes $k$. The free electron model typically uses periodic (Born–von Kármán) boundary conditions or the “infinite well” approximation.

## 3.3. Fermi-Dirac Statistics and the Fermi Surface

Because electrons are fermions with spin $1/2$, each quantum state can be occupied by at most two electrons (spin up and down). At $T = 0$, all states below the Fermi energy $E_F$ are filled, and states above are empty. The Fermi wave vector $k_F$ defines a Fermi sphere in $k$-space:

$$
E_F = \frac{\hbar^2 k_F^2}{2m}.
$$

The electron density $n = N/V$ (number of electrons per volume) relates to $k_F$ via the total number of occupied states inside that sphere:

$$
N = 2 \times \frac{V}{(2\pi)^3} \frac{4}{3} \pi k_F^3 \Rightarrow k_F = \left( 3 \pi^2 n \right)^{1/3}.
$$

Hence,

$$
E_F = \frac{\hbar^2}{2m} \left( 3\pi^2 n \right)^{2/3}.
$$

At finite $T$, the Fermi–Dirac distribution is:

$$
f(E) = \frac{1}{\exp \left( \frac{E - \mu(T)}{k_B T} \right) + 1},
$$

where $\mu(T)$ is the chemical potential, which approaches $E_F$ at low $T$.

## 3.4. Conductivity Revisited

Sommerfeld’s free electron model retains a similar form for conductivity:

$$
\sigma \approx \frac{n e^2 \tau}{m^*},
$$

where $m^*$ may be the bare electron mass $m$ (in a perfectly free electron assumption) or an effective mass if we incorporate band effects. The crucial quantum refinement is that only electrons near the Fermi surface can scatter under small thermal excitations. This addresses Drude’s problem of “too many electrons” all contributing equally.

## 3.5. Electronic Specific Heat

A major triumph of the Sommerfeld approach is the correct form of the electronic contribution to the specific heat. Classically, Drude would assume each electron contributes $(3/2) k_B T$ to the energy, implying a large specific heat. Observed data, however, shows that the electron contribution $c_{v,\text{electronic}}$ grows linearly with $T$ at low temperatures—and is much smaller than the classical estimate.

Sommerfeld’s derivation shows that only electrons within $\sim k_B T$ of $E_F$ can be thermally excited (thanks to the Pauli exclusion principle “blocking” lower-energy states). Consequently,

$$
c_{v,\text{electronic}} = \gamma T,
$$

where $\gamma$ depends on the density of states at the Fermi energy. This matches experimental results with much better accuracy.

## 3.6. Limitations of the Free Electron Model

- **Ignores the Periodic Lattice Potential:**  
  Real solids have band structures with allowed and forbidden bands, especially crucial for semiconductors and insulators.

- **Neglects Electron-Electron Interactions:**  
  In many transition metals (with partially filled $d$-bands, for example), electron correlations can be substantial.

- **Overly Simplified Fermi Surface:**  
  Complex metals can have complicated Fermi surfaces that deviate strongly from a perfect sphere.

Nevertheless, the Free Electron Model drastically improved upon Drude’s classical framework and explained a wealth of phenomena—especially the low-temperature electronic properties that Drude could not handle.

---

# 4. Comparisons, Contrasts, and Continuing Curiosity

### Drude vs. Sommerfeld:

- **Common Ground:**  
  Both treat conduction electrons as (mostly) free, with collisions represented by a relaxation time $\tau$.

- **Key Difference:**  
  Sommerfeld enforces Fermi-Dirac statistics, giving a drastically improved quantitative and qualitative match to experiments.

### Success Stories:

- Drude’s “relaxation time” approach was crucial and is still used in modern transport theories as the “relaxation time approximation.”
- Sommerfeld’s quantum refinement overcame classical flaws, capturing the correct magnitude of low-temperature specific heat and the refined behavior of conduction electrons.

### Spark for Further Models:

- The partial success of the Free Electron Model led to the **Band Theory of Solids**, employing Bloch’s theorem to handle the periodic lattice potential.
- In real materials, phenomena like effective mass, band gaps, and electron correlation needed more advanced frameworks like the **Nearly-Free Electron Model**, **Tight-Binding Model**, and eventually sophisticated **Density Functional Theory (DFT)** to handle electron-electron interactions.

### Intellectual Legacy:

Both Drude and Sommerfeld started from fundamental questions: “Why do metals conduct so well, and how do collisions work microscopically?” Their models remain indispensable “toy theories” that help students and researchers appreciate how conduction emerges from microscopic electron dynamics.

---

# 5. Key Equations and Their Significance

### Drude Model

**Equation of Motion:**

$$
m \frac{d v}{d t} = -e E - \frac{m}{\tau} v.
$$

**Steady-State Drift Velocity:**

$$
v_{\text{drift}} = -\frac{e \tau}{m} E.
$$

**Conductivity:**

$$
\sigma = \frac{n e^2 \tau}{m}, \quad \text{thus} \quad \rho = \frac{m}{n e^2 \tau}.
$$

### Sommerfeld Free Electron Model

**Energy Dispersion:**

$$
E(k) = \frac{\hbar^2 k^2}{2m}.
$$

**Fermi Wave Vector:**

$$
k_F = \left( 3 \pi^2 n \right)^{1/3}, \quad E_F = \frac{\hbar^2 k_F^2}{2m}.
$$

**Fermi–Dirac Distribution:**

$$
f(E) = \frac{1}{\exp \left( \frac{E - \mu(T)}{k_B T} \right) + 1}.
$$

**Electronic Specific Heat (Low $T$):**

$$
c_{v,\text{electronic}} \approx \gamma T.
$$

**Thermal Conductivity (Drude-Sommerfeld Style Approximation):**

$$
\kappa \sim \left( \frac{1}{3} v_F \ell n \right) c_{v,\text{electronic}},
$$

where $v_F = \sqrt{2 E_F / m}$ is the Fermi velocity and $\ell$ is the mean free path. This leads to an approximate Wiedemann–Franz law.

These equations highlight the contrast between a purely classical approach and one incorporating quantum occupation of states. They also reveal how the key parameter $\tau$ (the relaxation time) remains central to both theories’ descriptions of transport.

# 6. Final Reflections and Ongoing Fascination

## 6.1. Why These Models Still Matter

**Pedagogical Gateway:**  
They show how to connect microscopic electron dynamics with macroscopic conductivity or heat transport.

**Historic Significance:**  
Drude’s model was a bold innovation for its time, and Sommerfeld’s refinement was a crucial step in applying quantum mechanics to many-electron systems.

**Foundation for Band Theory:**  
Once you understand how free electrons behave, the next step is factoring in the periodic ionic potential, leading to energy bands, band gaps, and explaining semiconductors vs. metals.

## 6.2. Remaining Curiosities and Research Pathways

**Strongly Correlated Systems:**  
Materials where electron-electron interactions dominate (like high-$T_c$ superconductors) require theories well beyond free electrons.

**Advanced Transport Phenomena:**  
Topics such as quantum Hall effect, topological insulators, and non-Fermi liquids illustrate that real materials often deviate profoundly from Drude-Sommerfeld assumptions.

Yet, despite these complexities, the Drude and Free Electron Models persist as conceptual cornerstones. They offer intuition on how conduction emerges from “free” carriers scattering off a lattice, help us reason about orders of magnitude in conductivity, and remind us of the power of simple but bold physical assumptions.

## Concluding Thoughts

From Drude’s classical, gas-like electron model to Sommerfeld’s quantum refinement, we see a progressive unveiling of how electrons in metals actually behave. These models solved major puzzles of the early 20th century, stimulated further curiosity, and set the groundwork for the band theory that dominates today’s understanding of electronic materials. Indeed, the saga of Drude to Sommerfeld exemplifies physics in action: start with a bold hypothesis, compare to experiments, refine with deeper principles—and keep going until the data and theory align.

In essence, these theories continue to fascinate because they straddle the realms of simple and surprisingly successful while leaving open the door to deeper quantum mysteries that still challenge researchers in modern condensed matter physics.


