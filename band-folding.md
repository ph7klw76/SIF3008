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
