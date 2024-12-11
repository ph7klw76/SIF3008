# Why Magnets Lose Their Magnetization Over Time, Even in Ideal Conditions, and the Effect of Mechanical Impacts

Permanent magnets are ubiquitous, used in everything from computer hard drives to electric motors. A “perfect” permanent magnet, isolated from external magnetic fields, might seem stable indefinitely. However, fundamental principles of physics dictate that, given enough time, even a magnet protected from external perturbations (except a constant-temperature reservoir) will gradually lose some of its magnetization. Moreover, a physical shock—such as dropping a magnet—can accelerate this loss. In this article, we delve into the microscopic origin of this phenomenon, employing solid-state physics, thermodynamics, and micromagnetic theory.

---

## The Origin of Permanent Magnetization

### Spontaneous Magnetization
In ferromagnetic materials below their Curie temperature $T_C$, atomic spins (associated with unpaired electron angular momenta) spontaneously align due to the quantum-mechanical exchange interaction. This alignment can persist in the absence of an external magnetic field, producing a finite magnetization $M_s$.

### Magnetic Anisotropy and Domains
Real ferromagnets rarely exist as uniformly magnetized single domains. Instead, they break into multiple magnetic domains. The formation of domains reduces the macroscopic demagnetizing field and thus lowers the total free energy of the system. Magnetic anisotropy energies—arising from spin-orbit coupling and crystallographic symmetry—define “easy” directions for magnetization. A uniaxial anisotropy may be described by an energy density:

$$
E_K(\theta) = K_u \sin^2 \theta,
$$

where:
- $K_u > 0$ is the anisotropy constant,
- $\theta$ is the angle between the magnetization vector and the easy axis.

High $K_u$ ensures large energy barriers separating distinct magnetization states.

---

## Thermal Fluctuations and Metastability

Even if a magnet is in free space, isolated from external fields, it remains in contact with a thermal bath at temperature $T$. This means the system is subject to continuous thermal fluctuations. The magnetization state is generally a metastable configuration with a large, but finite, energy barrier $\Delta E$ separating it from other states of lower net magnetization.

From the standpoint of statistical mechanics, the probability per unit time $\Gamma$ of a thermally activated transition over an energy barrier $\Delta E$ is often approximated by a Néel-Arrhenius law:

$$
\Gamma = f_0 e^{-\Delta E / (k_B T)},
$$

where:
- $k_B$ is Boltzmann’s constant,
- $f_0$ is an attempt frequency on the order of the spin precessional or magnon frequencies (~$10^9–10^{12} \, \text{s}^{-1}$).

While $\Delta E / (k_B T)$ is typically very large for a good permanent magnet at room temperature, $\Gamma$ is not strictly zero. Over astronomical timescales, even these minuscule probabilities accumulate.

**Result:**
If we consider a collection of magnetic moments or domains, each separated by barriers $\Delta E$, the macroscopic magnetization $M(t)$ can slowly decay:

$$
M(t) \approx M_0 e^{-t / \tau},
$$

with $\tau \approx 1 / \Gamma$. For a high-quality permanent magnet, $\tau$ can exceed the age of the universe at room temperature, making the loss negligible on human timescales. However, the principle remains: the magnet is only metastable, not eternally stable.

---

## Micromagnetic Theory and Domain Wall Motion

### LLG Equation and Stochastic Fields
The Landau-Lifshitz-Gilbert (LLG) equation describes the time evolution of the magnetization $M(r, t)$ at the micromagnetic scale:

$$
\frac{dM}{dt} = -\gamma M \times H_\text{eff} + \frac{\alpha}{M_s} (M \times \frac{dM}{dt}),
$$

where:
- $\gamma$ is the gyromagnetic ratio,
- $\alpha$ is the Gilbert damping constant,
- $H_\text{eff}$ is the effective field incorporating exchange, anisotropy, demagnetizing, and thermal fluctuations.

Thermal fluctuations enter as a random, time-dependent effective field $H_\text{th}(t)$ with:

$$
\langle H_\text{th,i}(t) H_\text{th,j}(t') \rangle = \frac{2 \alpha k_B T}{\mu_0 M_s \gamma V} \delta_{ij} \delta(t - t'),
$$

for a coherent volume $V$. Although $H_\text{th}$ is typically very small, given sufficient time it can assist domain walls to surmount pinning barriers or nucleate reversed domains.

### Domain Nucleation and Growth
Large magnets comprise many domains separated by domain walls. These domain walls can be pinned by defects, grain boundaries, and inclusions. Thermal fluctuations facilitate rare events where walls jump from one pinning site to another. Over immense timescales, this leads to gradual morphological changes in the domain structure, reducing net magnetization.

---

## Mechanical Disturbances: The Effect of Dropping a Magnet

When you drop a magnet onto a hard surface, mechanical energy is abruptly introduced. This mechanical shock can affect magnetization in several ways:

### Microstructural Changes
A strong impact can generate stress, cause minor lattice defects, or even microcracks. Such defects alter local anisotropy and create new, lower-energy domain configurations. Magnetoelastic coupling relates stress $\sigma$ to changes in anisotropy energy:

$$
E_\sigma = -\frac{3}{2} \lambda_s \sigma \cos^2 \theta,
$$

where $\lambda_s$ is the magnetostriction constant. Stress reduces effective anisotropy barriers for certain domain configurations, making it easier for walls to move or for domains to flip.

### Domain Wall Unpinning via Impact
Domain walls pinned at shallow potential minima can be dislodged by the mechanical impulse. Once a wall is free, it may move to a new equilibrium position corresponding to a lower net magnetization state. The result is an instantaneous, partial demagnetization.

### Thermally Assisted Aftershock
Even if the immediate domain rearrangement is modest, the new defect landscape created by the shock can lower energy barriers and facilitate faster thermally assisted demagnetization over subsequent time.

### Quantitative Impact Model
Suppose the impact effectively reduces the average barrier $\Delta E$ to $\Delta E'$. The new transition rate:

$$
\Gamma' = f_0 e^{-\Delta E' / (k_B T)},
$$

is higher than before. Thus, after the shock, the magnet’s magnetization decreases more rapidly than it would have without the mechanical perturbation.

---

## Fundamental Principles Underlying Magnetization Decay

1. **Thermodynamics and Metastability**: The existence of large but finite barriers and thermal fluctuations ensures that the perfectly aligned magnetic state is a metastable equilibrium, not an absolute ground state.
2. **Quantum and Statistical Mechanics**: Spin states and domain configurations are discrete and separated by energy barriers. The Boltzmann factor $e^{-\Delta E / (k_B T)}$ quantifies the probability of surmounting these barriers over time.
3. **Coupling Between Mechanics and Magnetics (Magnetoelastic Coupling)**: Mechanical stress affects magnetic anisotropy, enabling mechanical energy to influence the magnetic energy landscape and domain wall stability.

---

## Conclusion

In principle, no permanent magnet remains perfectly magnetized indefinitely. Although for practical high-anisotropy magnets, the spontaneous loss of magnetization at room temperature can be extraordinarily slow, it is never zero. Over extremely long timescales, thermally activated processes will gradually erode even a well-protected magnet’s magnetization.

Moreover, mechanical impacts provide a shortcut by creating defects or dislodging domain walls, effectively lowering energy barriers and allowing more rapid demagnetization events to occur. This explains why dropping a magnet can reduce its magnetization more noticeably and rapidly than mere thermal aging would.

In short, the persistent magnetization of a permanent magnet is the result of large, but finite, energy barriers maintained by anisotropy and pinning. Thermal agitation ensures these barriers are not insurmountable, and mechanical shocks can assist the magnetization in moving toward configurations of lower net magnetization. This interplay of thermodynamics, micromagnetics, and materials science reveals that permanent magnets, while extraordinarily stable, are not eternally immutable.
