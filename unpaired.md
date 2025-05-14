# Why Unpaired Electrons Are Not Enough

*A quantum‑mechanical deep dive into paramagnets, ferromagnets, and the birth of permanent magnets*

---

## 1. Local Moments: Every Unpaired Electron Is a Tiny Magnet

In an isolated atom, an unpaired spin $S$ contributes a magnetic moment

$$
\mu = -g \mu_B S,
$$

where $g \approx 2$ and $\mu_B$ is the Bohr magneton. Both paramagnets and ferromagnets possess such local moments, so the story must hinge on how those moments interact.  


---

## 2. Exchange Coupling: Turning Singles into a Choir

The quantum‐mechanical exchange interaction favours particular alignments of neighbouring spins. In the local‑moment (Heisenberg) picture we write

$$
H_{\text{ex}} = -\sum_{\langle i,j \rangle} J_{ij} \, \mathbf{S}_i \cdot \mathbf{S}_j.
$$

- **Sign of $J_{ij}$**:  
  - If $J_{ij} > 0$, the lowest energy is achieved when spins are **parallel** (*ferromagnetic*).  
  - If $J_{ij} < 0$, they prefer **antiparallel** order (*anti-/ferrimagnetic*).  

- **Magnitude of $J_{ij}$**:  
  The Bethe–Slater curve shows how $J$ changes with the ratio of inter‑atomic spacing to the radial size of the 3d shell, predicting that only Fe, Co and Ni sit in the “strong positive” window.  


When moments are itinerant (spread over a band), we instead use the **Stoner model**. Parallel alignment wins if

$$
I \, D(E_F) > 1,
$$

where $I$ is the exchange integral and $D(E_F)$ is the density of states per spin at the Fermi level.  


**Why Al is only paramagnetic**:  
Aluminium has unpaired 3s/3p electrons, but its $D(E_F)$ is tiny, so the Stoner product is far below unity. Its volume susceptibility is a meek

$$
+2.2 \times 10^{-5}.
$$  


---

## 3. Thermal Tug‑of‑War and the Curie Temperature

If the net exchange energy per spin exceeds thermal agitation, cooperative order emerges below the Curie point $T_C$. Mean‑field theory gives:

$$
k_B T_C = \frac{2}{3} z J S(S+1)
$$

for localized spins (where $z =$ coordination number). Above $T_C$, the material reverts to a paramagnetic Curie‑Weiss susceptibility:

$$
\chi = \frac{C}{T - \theta} \;\longrightarrow\; \frac{C}{T} \quad (\theta \approx 0 \text{ for simple paramagnets}).
$$

Because $J$ is too small in Al, Li, Mg, etc., $T_C<0$ (no ordering at any temperature), whereas for Fe it is $1043\,\text{K}$.

---

## 4. Domains, Anisotropy and the Making of a Permanent Magnet

Even a perfect ferromagnet would still flip its total moment easily if it had no energetic *easy axis*. Two additional ingredients harden the magnet:

1. **Magnetocrystalline anisotropy** adds an energy term:

$$
\mathcal{H}_{\text{ani}} = K_1 \sin^2\theta + \dots
$$

that locks the magnetization to certain lattice directions.

2. **Defects & grain boundaries** pin domain walls, raising the field required to move them (coercivity $H_c$).

   Materials with

$$
H_c \gtrsim 10^5 \;\text{A·m}^{-1}
$$

   are called magnetically **hard** and retain a large **remanent flux** after the external field is removed.  
   *Source: Wikipedia*

**Example**:  
Alloys such as Nd‑Fe‑B combine a very high anisotropy constant with strong exchange, so their magnetic domains stay frozen—hence refrigerator-door magnets.  

Ordinary iron is ferromagnetic but “soft”: its $H_c$ is only a few hundred A·m $^{-1}$ —perfect for transformer cores but useless for sticking notes.

---

## 5. Why a Paramagnet Immediately “Forgets”

Because $J \approx 0$, the free energy has a single minimum at $M = 0$. Once the applied field $H$ is switched off, each local moment executes random thermal rotations; the net magnetization decays to zero within nanoseconds.  

There are no domains to pin, no anisotropy energy to overcome—nothing to remember the field’s direction.

---

## 6. Key Takeaways

| **Ingredient**                    | **Paramagnet** | **Ferromagnet** | **Hard (permanent) magnet** |
|----------------------------------|----------------|------------------|-----------------------------|
| Unpaired electrons               | ✔              | ✔                | ✔                           |
| Strong, positive exchange        | ✖              | ✔                | ✔                           |
| Long‑range order below $T_C$     | ✖              | ✔                | ✔                           |
| Magnetocrystalline anisotropy    | low            | medium           | high                        |
| Domain‑wall pinning              | negligible     | moderate         | strong                      |
| Remanent magnetization           | 0              | small            | large                       |

---

**Conclusion**  
Unpaired electrons light the spark, but only cooperative exchange, crystallographic anisotropy, and defect‑assisted pinning keep the fire burning after the external field is gone.
