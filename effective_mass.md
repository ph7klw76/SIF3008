```python
import numpy as np
import matplotlib.pyplot as plt

# ========================================================================
# PHYSICAL CONSTANTS AND PARAMETERS (in natural units where ħ = 1, m0 = 1)
# ========================================================================
ħ = 1.0         # Reduced Planck's constant (dimensionless units)
m0 = 1.0        # Free electron mass (reference mass)
t = 1.0         # Hopping parameter in tight-binding model (sets energy scale)
a = 1.0         # Lattice constant (distance between atoms)

# ========================================================================
# WAVE VECTOR GENERATION (First Brillouin Zone)
# ========================================================================
# The Brillouin zone extends from -π/a to π/a in 1D systems
k = np.linspace(-np.pi/a, np.pi/a, 1000)  # Wavevectors in reciprocal space

# ========================================================================
# ENERGY DISPERSION RELATIONS
# ========================================================================

# Free electron (parabolic) dispersion: E(k) = ħ²k²/(2m0)
# This represents electrons in empty space with no crystal potential
E_parabolic = (ħ**2 * k**2) / (2 * m0)

# Tight-binding dispersion: E(k) = -2t cos(ka)
# Represents electrons in a periodic crystal potential (1D chain)
E_cosine = -2 * t * np.cos(k * a)

# ========================================================================
# EFFECTIVE MASS CALCULATIONS
# ========================================================================
# Effective mass tensor component in 1D: m* = ħ²/(d²E/dk²)
# This determines how electrons respond to external forces in the crystal

# ---------------------------
# Parabolic band calculation
# ---------------------------
# For E(k) = ħ²k²/(2m0):
# d²E/dk² = ħ²/m0 → Constant curvature → Constant effective mass
d2E_dk2_para = (ħ**2 / m0) * np.ones_like(k)
m_star_para = ħ**2 / d2E_dk2_para  # Simplifies to m0 (free electron mass)

# ---------------------------
# Cosine band calculation
# ---------------------------
# For E(k) = -2t cos(ka):
# First derivative: dE/dk = 2ta sin(ka)
# Second derivative: d²E/dk² = 2ta² cos(ka) → k-dependent curvature
d2E_dk2_cos = 2 * t * (a**2) * np.cos(k * a)

# Calculate effective mass with safety for division by zero
m_star_cos = np.divide(ħ**2, d2E_dk2_cos,
                      out=np.full_like(d2E_dk2_cos, np.nan),
                      where=np.abs(d2E_dk2_cos) > 1e-6)

# ========================================================================
# PLOTTING
# ========================================================================
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(10, 12))

# Band structure plot
ax1.plot(k, E_parabolic, label='Parabolic Band (Free Electron)')
ax1.plot(k, E_cosine, label='Cosine Band (Tight-Binding)')
ax1.set_xlabel('Wavevector k [1/a]', fontsize=12)
ax1.set_ylabel('Energy E(k) [t]', fontsize=12)
ax1.set_title('Band Structure Comparison', fontsize=14)
ax1.legend()
ax1.grid(True)

# Effective mass plot
ax2.plot(k, m_star_para, label='Parabolic Band', color='C0')
ax2.plot(k, m_star_cos, label='Cosine Band', color='C1')
ax2.set_xlabel('Wavevector k [1/a]', fontsize=12)
ax2.set_ylabel('Effective Mass m* [m$_0$]', fontsize=12)
ax2.set_title('Effective Mass Variation in Brillouin Zone', fontsize=14)
ax2.legend()
ax2.grid(True)

# Special handling for divergent masses
ax2.set_yscale('symlog', linthresh=0.1, linscale=0.5)
ax2.set_ylim(-10, 10)  # Focus on physically meaningful range
ax2.axhline(1, color='gray', linestyle='--', alpha=0.5)  # Free mass reference
ax2.axhline(-1, color='gray', linestyle='--', alpha=0.5) # Negative mass reference

plt.tight_layout()
plt.show()
```

# Sign Change and Divergence of Effective Mass in the Tight‑Binding Model: Physics and Practical Implications

In crystalline solids, electrons behave very differently from free particles: their “inertial” response to external forces is captured not by the bare electron mass but by an effective mass $m^*$, which encodes the curvature of the energy‑momentum (dispersion) relation in a periodic lattice. Within the one‑dimensional nearest‑neighbor tight‑binding approximation, this effective mass undergoes a dramatic transition across the Brillouin zone—switching sign and even diverging to infinity at specific crystal momenta. These mathematical singularities have profound physical consequences, from the emergence of immobile carriers in flat‑band regions to the onset of hole‑like conduction and heavy‑fermion behavior in real materials.

## 1. Tight‑Binding Dispersion in One Dimension

In the simplest tight‑binding picture, electrons hop between adjacent sites on a one‑dimensional lattice of spacing $a$. The resulting dispersion relation is:

$$
E(k) = -2t \cos(ka),
$$

where $t$ is the nearest‑neighbor hopping amplitude and $k$ is the crystal momentum confined to the first Brillouin zone ($-\pi/a \leq k \leq \pi/a$).

## 2. Effective Mass: Definition and Derivation

The effective mass quantifies how an electron accelerates under an applied force and is defined semiclassically by the inverse curvature of the band:

$$
m^*(k) = \hbar^2 \left( \frac{d^2E}{dk^2} \right)^{-1}.
$$

Substituting the tight‑binding dispersion yields:

$$
\frac{d^2E}{dk^2} = 2t\,a^2 \cos(ka), \quad \Rightarrow \quad m^*(k) = \frac{\hbar^2}{2t\,a^2 \cos(ka)}.
$$

## 3. Sign Change and Divergence at Brillouin‑Zone Boundaries

Because $\cos(ka)$ changes sign within the first Brillouin zone, $m^*(k)$ evolves as follows:

| $k$               | $\cos(ka)$ | $m^*$                                      | Physical interpretation                          |
|------------------|------------|--------------------------------------------|--------------------------------------------------|
| $k = 0$          | $+1$       | $\frac{\hbar^2}{2ta^2}$ (finite, positive) | Free‑electron‑like, light carriers               |
| $k = \pm \frac{\pi}{2a}$ | $0$         | $\pm\infty$                                | Flat‑band (“immobile”) carriers                  |
| $k = \pm \frac{\pi}{a}$   | $-1$        | $-\frac{\hbar^2}{2ta^2}$ (finite, negative) | Hole‑like conduction                             |

At $k = \pm \frac{\pi}{2a}$, the band curvature vanishes $\left( \frac{d^2E}{dk^2} = 0 \right)$, causing an infinite discontinuity in $m^*$ and a sudden sign reversal.

## 4. Physical Interpretation

### 4.1 Infinite Effective Mass (Flat‑Band Regions)

Flat dispersion means zero group velocity $\left( \frac{dE}{dk} = 0 \right)$, so carriers do not respond to external fields (i.e., behave as infinitely “heavy”).

Real materials never exhibit perfect flat bands; however, near‑flat regions give rise to **van Hove singularities** in the density of states, enhancing correlation effects (e.g., magnetism, superconductivity) in systems such as twisted bilayer graphene.

### 4.2 Negative Effective Mass → Hole Behavior

In regions where $m^* < 0$, electrons accelerate opposite to an applied force, effectively behaving like positively charged “holes.”

This underpins valence‑band conduction in semiconductors and the formalism of hole carriers in transport.

### 4.3 Positive Effective Mass → Electron‑Like Behavior

Regions with $m^* > 0$ describe electrons that accelerate with the applied force, akin to free electrons but with modified inertia determined by band curvature.

## 5. Practical Implications

| Phenomenon                  | Role of Divergent/Sign‑Changing $m^*$                                       | Reference               |
|----------------------------|-------------------------------------------------------------------------------|-------------------------|
| Semiconductor hole conduction | Negative $m^*$ describes hole transport; essential for p‑type devices         |                         |
| Flat‑band correlated phases | Near‑infinite $m^*$ enhances density of states → strong electronic correlations |                         |
| Heavy fermions              | Effective masses up to ~10³ $m_e$ arise from extremely flat f‑electron bands   |                         |
| Bloch oscillations          | Infinite mass points mark turning points of oscillatory motion under constant field |                         |

## 6. Summary

In the one‑dimensional tight‑binding model, the effective mass $m^*(k)$ is a direct measure of band curvature. As the crystal momentum traverses the Brillouin zone, $m^*$ transitions from positive (electron‑like) through an infinite divergence (flat‑band) to negative (hole‑like). Although these infinities are mathematical idealizations of an ideal lattice, their fingerprints appear in real materials via van Hove singularities, heavy‑fermion physics, and the behavior of holes in semiconductors.

Understanding these sign changes and divergences is fundamental for interpreting transport, optical, and correlated phenomena in modern condensed matter physics.
