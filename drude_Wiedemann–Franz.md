# Drude Model and its connection to the Wiedemann–Franz law

## Below is a step-by-step, detailed, and quantitative derivation of thermal conductivity in the Drude Model and its connection to the Wiedemann–Franz law. We will:

- Start from kinetic theory considerations of how electrons carry heat.
- Arrive at an expression for thermal conductivity $\kappa$.
- Show how the ratio $\kappa/\sigma$ (where $\sigma$ is the electrical conductivity) becomes proportional to temperature, reproducing (at least in form) the Wiedemann–Franz law.

## 1. Kinetic Theory in the Drude Model

In Drude’s picture, conduction electrons are treated as classical, random-moving particles—analogous to molecules in an ideal gas. We assume:

- Average electron speed $\langle v \rangle$ comes from a thermal (Maxwell–Boltzmann) distribution.
- Collisions happen on average every $\tau$ seconds (the mean free time).
- Mean free path $\ell$ of an electron is $\ell = \langle v \rangle \tau$.
- Classical equipartition for each electron, giving an average internal energy $(3/2) k_B T$.
- Electrons are the main carriers of heat (as well as charge) in metals under Drude’s assumptions.

---

## 2. Step-by-Step Derivation of Thermal Conductivity

### 2.1. Heat Flux From Kinetic Theory

In classical kinetic theory (for a gas or free electrons), the heat flux $q$ under a small temperature gradient $\nabla T$ is often written (heuristically) as:

$$
q = -\kappa \nabla T.
$$

We want to derive $\kappa$. A standard result from gas-like considerations is:

$$
q = -\frac{1}{3} n \langle E \rangle \langle v \rangle \ell \nabla T \quad \text{(for one species of carriers)},
$$

where:

- $n$ is the number density of carriers (electrons),
- $\langle E \rangle$ is the average energy per electron (or sometimes the relevant energy per degree of freedom that changes with $T$),
- $\langle v \rangle$ is the average speed,
- $\ell$ is the mean free path,
- and the factor $\frac{1}{3}$ arises from detailed kinetic-theory arguments about velocity components and scattering angles.

#### 2.1.1. Identifying $\langle E \rangle$

Classically (Drude model assumption), each electron has an average kinetic energy:

$$
\langle E \rangle = \frac{3}{2} k_B T.
$$

However, what actually matters for conduction of heat is how sensitive the electron energy is to temperature changes, which ties into the specific heat. In Drude’s purely classical approach, you can treat $\langle E \rangle \propto T$.

#### 2.1.2. Mean Free Path and Speeds

- **Mean free path:**  
  $$\ell = \langle v \rangle \tau.$$
  
- **Average speed (for a Maxwell–Boltzmann distribution):**  
  $$\langle v \rangle \approx \sqrt{\frac{8 k_B T}{\pi m}},$$  
  or one may use $\sqrt{3 k_B T / m}$. (Different numeric factors appear depending on whether you use the RMS speed or average speed, but the order-of-magnitude outcome is similar.)

Putting these pieces into the formula for the heat flux:

$$
q = \left| q \right| = \frac{1}{3} n \langle E \rangle \langle v \rangle \ell |\nabla T|.
$$

Hence the thermal conductivity $\kappa$ is identified via:

$$
\kappa = \frac{1}{3} n \langle E \rangle \langle v \rangle \ell.
$$

---

### 2.2. Explicit Expression for $\kappa$

Plugging in the details:

$$
\ell = \langle v \rangle \tau.
$$

$$
\langle E \rangle = \frac{3}{2} k_B T.
$$

$\langle v \rangle$ is taken as a typical thermal speed scale. Thus,

$$
\kappa = \frac{1}{3} n \left( \frac{3}{2} k_B T \right) \langle v \rangle \left( \langle v \rangle \tau \right).
$$

Simplify:

$$
\kappa = \frac{1}{3} n \frac{3}{2} k_B T \langle v \rangle^2 \tau = \frac{1}{2} n k_B T \langle v \rangle^2 \tau.
$$

If we assume a classical Maxwell–Boltzmann distribution, then:

$$
\langle v^2 \rangle = \frac{3 k_B T}{m}.
$$

Hence,

$$
\langle v \rangle^2 \sim \langle v^2 \rangle = \frac{3 k_B T}{m}.
$$

Therefore,

$$
\kappa \approx \frac{1}{2} n k_B T \times \frac{3 k_B T}{m} \times \tau = \frac{3}{2} \frac{n k_B^2 T^2 \tau}{m}.
$$

This is a typical Drude-level expression for the thermal conductivity of electrons in a metal if we treat them classically.

---

## 3. Wiedemann–Franz Law in the Drude Model

The Wiedemann–Franz law states that the ratio $\kappa/\sigma$ is (approximately) proportional to the absolute temperature $T$. Experimentally, for many metals,

$$
\frac{\kappa}{\sigma} \approx L T,
$$

where $L$ is the Lorenz number, empirically found to be:

$$
L \approx 2.44 \times 10^{-8} \, \text{W} \cdot \Omega / \text{K}^2.
$$

### 3.1. Electrical Conductivity $\sigma$ in the Drude Model

Recall that from Drude’s derivation of electric conduction:

$$
\sigma = \frac{n e^2 \tau}{m}.
$$

### 3.2. Forming the Ratio $\kappa/\sigma$

Using the (approximate) Drude expressions:

$$
\kappa \approx \frac{3}{2} \frac{n k_B^2 T^2 \tau}{m},
$$

$$
\sigma = \frac{n e^2 \tau}{m}.
$$

Divide $\kappa$ by $\sigma$:

$$
\frac{\kappa}{\sigma} = \frac{\frac{3}{2} n k_B^2 T^2 \tau / m}{n e^2 \tau / m} = \frac{3}{2} \frac{k_B^2}{e^2} T.
$$

Hence,

$$
\frac{\kappa}{\sigma} = \left( \frac{3}{2} \frac{k_B^2}{e^2} \right) T.
$$

This shows:

$$
\kappa = \left( \frac{3}{2} \frac{k_B^2}{e^2} \right) \sigma T,
$$

which is precisely of the form:

$$
\frac{\kappa}{\sigma} = L_{\text{Drude}} T \quad \text{with} \quad L_{\text{Drude}} = \frac{3}{2} \frac{k_B^2}{e^2}.
$$

---

### 3.2.1. Comparison to Experimental Lorenz Number

The Drude-model result:

$$
L_{\text{Drude}} = 1.5 \frac{k_B^2}{e^2}.
$$

Experimentally (and via Sommerfeld’s quantum refinement), one obtains:

$$
L_{\text{exp}} \approx \frac{\pi^2}{3} \frac{k_B^2}{e^2} \approx 3.2899 \frac{k_B^2}{e^2} \approx 2.44 \times 10^{-8} \, \text{W} \cdot \Omega / \text{K}^2.
$$

Hence, while Drude’s ratio is not numerically correct ($\frac{3}{2} \neq \frac{\pi^2}{3}$), the important qualitative feature—that $\kappa/\sigma \propto T$—is captured. The Drude model “reproduces” the Wiedemann–Franz law in structure but not in precise magnitude. The better (larger) factor $\frac{\pi^2}{3}$ emerges only when electron quantum statistics (Fermi-Dirac distribution) are properly taken into account (Sommerfeld model).

# 4. Why Drude Still Captures the Essence

Despite the numerical discrepancy, this step-by-step shows that:

- Heat transport in the Drude model is derived via kinetic theory (like a gas).
- Electrical conduction is accounted for by the relaxation-time approach.
- Both conduction processes boil down to the same carriers (electrons), leading to a strong correlation between electrical and thermal conductivity.

This correlation is the crux of the Wiedemann–Franz law:

$$
\frac{\kappa}{\sigma} \sim T.
$$

Drude’s approach, while purely classical and incomplete, was historically the first to provide a microscopic rationale for why such a law should exist—“the same electrons do both jobs”—and to show that the proportionality to $T$ can emerge simply.

---

# 5. Summary of Key Formulas

### Drude Thermal Conductivity (classical approximation):

$$
\kappa_{\text{Drude}} \approx \frac{3}{2} \frac{n k_B^2 T^2}{m} \tau.
$$

### Drude Electrical Conductivity:

$$
\sigma_{\text{Drude}} = \frac{n e^2 \tau}{m}.
$$

### Wiedemann–Franz-Type Ratio:

$$
\frac{\kappa_{\text{Drude}}}{\sigma_{\text{Drude}}} = \left( \frac{3}{2} \frac{k_B^2}{e^2} \right) T \equiv L_{\text{Drude}} T.
$$

While the exact Lorenz number,

$$
L_{\text{Drude}} = \frac{3}{2} \frac{k_B^2}{e^2}
$$

differs from the experimentally observed:

$$
L \approx \frac{\pi^2}{3} \frac{k_B^2}{e^2},
$$

the Drude model’s qualitative success remains crucial. Later, the Sommerfeld Model—incorporating Fermi–Dirac statistics—corrects this factor and agrees far better with experimental data for metallic conduction at typical temperatures.

---

## Concluding Remarks

This quantitative derivation shows how Drude’s kinetic theory for electrons naturally leads to thermal conductivity proportional to $T$, in line with the Wiedemann–Franz law. Although the numeric coefficient is off, it was Drude’s first demonstration that the same electrons carrying electricity are also primarily responsible for carrying heat, thereby unveiling the fundamental carrier unification behind conduction phenomena in metals. This insight paved the way for the Sommerfeld (Free Electron) Model, which fine-tuned the thermal transport predictions to match experiments by incorporating quantum statistics.

