# Physical System Setup

We consider a paramagnetic material composed of a large number of identical, non-interacting ions. Each ion has a total angular momentum quantum number $J$. This $J$ encapsulates both spin and orbital contributions, such that the total angular momentum operator $\hat{J}$ satisfies:

$$
\hat{J}^2 \lvert J, m \rangle = \hbar^2 J(J+1) \lvert J, m \rangle,
$$

$$
\hat{J}_z \lvert J, m \rangle = \hbar m \lvert J, m \rangle,
$$

where $m$ takes values $-J, -J+1, \dots, J-1, J$.

## Key Properties:

1. **Degeneracy**: The degeneracy of the spin states is $2J+1$.
2. **Magnetic Moment**: For each state labeled by $m$, the magnetic moment along the $z$-axis is given by:

   $$
   \mu_z = -g \mu_B \frac{\hat{J}_z}{\hbar} = g \mu_B m,
   $$

   where $\mu_B$ is the Bohr magneton, and $g$ is the Landé g-factor.

   - Higher $m$ values align better with the field.

## Magnetic Field Applied

A static uniform magnetic field $\mathbf{B} = B\hat{z}$ is applied. The potential energy of a magnetic moment in this field is:

$$
E_m = -\mu_z B = -(g \mu_B m) B.
$$

Thus, states with larger $m$ have lower energy (since $m > 0$), favoring alignment with the field direction.

---

# Statistical Mechanics Framework

At thermal equilibrium with temperature $T$, the population of states follows the Boltzmann distribution. The probability $p_m$ of finding the ion in the state $\lvert J, m \rangle$ is:

$$
p_m = \frac{e^{-E_m / (k_B T)}}{Z},
$$

where $k_B$ is Boltzmann’s constant, and $Z$ is the single-ion partition function:

$$
Z = \sum_{m=-J}^{J} e^{-E_m / (k_B T)}.
$$

Substituting $E_m = -g \mu_B m B$:

$$
Z = \sum_{m=-J}^{J} e^{g \mu_B m B / (k_B T)}.
$$

---

## Evaluating the Partition Function

Define the dimensionless quantity:

$$
x = e^{g \mu_B B / (k_B T)}.
$$

Then:

$$
Z = \sum_{m=-J}^{J} x^m = x^{-J} + x^{-J+1} + \dots + x^{J-1} + x^J.
$$

This is a geometric series with $(2J+1)$ terms. Using the formula for a finite geometric series:

$$
\sum_{m=0}^N r^m = \frac{r^{N+1} - 1}{r - 1},
$$

we can shift indices and write:

$$
Z = x^{-J} \frac{x^{2J+1} - 1}{x - 1}.
$$

---

# Simplified Partition Function

Using $x = e^{g \mu_B B / (k_B T)}$, the partition function can be written in hyperbolic sine form. Define $\beta = g \mu_B B / (2k_B T)$. Then:

$$
Z = \frac{\sinh\left((2J+1)\beta\right)}{\sinh(\beta)}.
$$

---

# Average Magnetic Moment

The expectation value of $\mu_z$ for a single ion is:

$$
\langle \mu_z \rangle = \frac{1}{Z} \sum_{m=-J}^J (g \mu_B m) e^{g \mu_B m B / (k_B T)}.
$$

This simplifies to:

$$
\langle \mu_z \rangle = k_B T \frac{\partial}{\partial B} \ln Z.
$$

Differentiating $Z$:

$$
Z = \frac{\sinh\left((2J+1)\beta\right)}{\sinh(\beta)}, \quad \beta = \frac{g \mu_B B}{2k_B T}.
$$

Take the derivative of $\ln Z$ with respect to $B$:

$$
\frac{\partial \ln Z}{\partial B} = (2J+1)\coth((2J+1)\beta) - \coth(\beta).
$$

So:

$$
\langle \mu_z \rangle = g \mu_B \frac{(2J+1)\coth((2J+1)\beta) - \coth(\beta)}{2}.
$$

---

# High-Temperature (Weak-Field) Limit

In the limit $g \mu_B B \ll k_B T$, $\beta \to 0$, and $\coth(y) \approx 1/y$. The leading term in the expansion gives:

$$
\langle \mu_z \rangle \approx \frac{g^2 \mu_B^2 J(J+1)}{3k_B T} B.
$$

This leads to **Curie’s law**:

$$
\chi = \frac{n \mu_0 g^2 \mu_B^2 J(J+1)}{3k_B T}.
$$

---

# Conclusion

The Curie constant is:

$$
C = \frac{\mu_0 \mu_B^2 n g^2 J(J+1)}{3k_B}.
$$

This completes the derivation, showing how Curie’s law emerges from the fundamental quantum properties of paramagnetic ions.
