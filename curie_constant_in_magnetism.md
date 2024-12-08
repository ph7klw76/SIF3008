# Physical System Setup

We consider a paramagnetic material composed of a large number of identical, non-interacting ions. Each ion has a total angular momentum quantum number $J$. This $J$ encapsulates both spin and orbital contributions, such that the total angular momentum operator $\hat{J}$ satisfies:

$$
\hat{J}^2 \lvert J, m \rangle = \hbar^2 J(J + 1) \lvert J, m \rangle, \quad \hat{J}_z \lvert J, m \rangle = \hbar m \lvert J, m \rangle,
$$

where $m$ takes values $-J, -J + 1, \dots, J - 1, J$.

## Key Properties

1. **Degeneracy of Spin States**:  
   The degeneracy of the spin states is $2J + 1$.

2. **Magnetic Moment Along $z$-axis**:  
   For each state labeled by $m$, the magnetic moment along the $z$-axis is given by:

$$
   \mu_z = -g \mu_B \frac{\hat{J}_z}{\hbar} = g \mu_B m,
$$

   where $\mu_B$ is the Bohr magneton, and $g$ is the Landé g-factor. The sign is often absorbed or interpreted depending on orientation, but here we write:

$$
   \mu_z = g \mu_B m
$$

   understanding that higher $m$ aligns better with the field.

## Magnetic Field Applied

We apply a static uniform magnetic field $\mathbf{B} = B \hat{z}$. The potential energy of a magnetic moment in this field is:

$$
E_m = -\mu_z B = -(g \mu_B m) B.
$$

Thus, states with larger $m$ have lower energy (since $m$ can be positive), favoring alignment with the field direction.

# Statistical Mechanics Framework

At thermal equilibrium with temperature $T$, the population of states follows the Boltzmann distribution. The probability $p_m$ of finding the ion in the state $\lvert J, m \rangle$ is:

$$
p_m = \frac{e^{-E_m / (k_B T)}}{Z},
$$

where $k_B$ is Boltzmann’s constant, and $Z$ is the single-ion partition function:

$$
Z = \sum_{m = -J}^J e^{-E_m / (k_B T)}.
$$

Inserting $E_m = -g \mu_B m B$:

$$
Z = \sum_{m = -J}^J e^{\frac{g \mu_B m B}{k_B T}}.
$$

### Evaluating the Partition Function

Define a dimensionless quantity:

$$
x = e^{\frac{g \mu_B B}{k_B T}}.
$$

Then:

$$
Z = \sum_{m = -J}^J x^m = x^{-J} + x^{-J+1} + \dots + x^{J-1} + x^J.
$$

This is a geometric series with $(2J + 1)$ terms. The generic formula for a finite geometric series is:

$$
\sum_{m=0}^N r^m = \frac{r^{N+1} - 1}{r - 1}.
$$

For our series, shifting indices to start from $-J$:

$$
Z = x^{-J} \sum_{m=0}^{2J} x^m = x^{-J} \frac{x^{2J+1} - 1}{x - 1}.
$$

### Hyperbolic Sine Form

Using the identity of the hyperbolic sine function:

$$
\sinh(y) = \frac{e^y - e^{-y}}{2},
$$

and after algebraic manipulation, we find the partition function:

$$
Z = \frac{\sinh\left(\frac{(2J+1) g \mu_B B}{2 k_B T}\right)}{\sinh\left(\frac{g \mu_B B}{2 k_B T}\right)}.
$$

## Average Magnetic Moment

The magnetization $M$ of the material is related to the average magnetic moment per ion times the number density of ions. First, we find the average moment of a single ion. The expectation value of $\mu_z$ for one ion is:

$$
\langle \mu_z \rangle = \frac{1}{Z} \sum_{m=-J}^J (g \mu_B m) e^{\frac{g \mu_B m B}{k_B T}}.
$$

Recognizing this as the derivative of the partition function with respect to $B$:

$$
\langle \mu_z \rangle = k_B T \frac{\partial}{\partial B} \ln Z.
$$

Substituting $Z$:

$$
\langle \mu_z \rangle = g \mu_B \frac{(2J + 1) \coth\left(\frac{(2J+1) g \mu_B B}{2 k_B T}\right) - \coth\left(\frac{g \mu_B B}{2 k_B T}\right)}{2}.
$$

In the high-temperature (weak-field) limit:

$$
\langle \mu_z \rangle \approx \frac{g^2 \mu_B^2 J(J+1) B}{3 k_B T}.
$$

## Magnetic Susceptibility

The total magnetization $M$ is given by:

$$
M = n \langle \mu_z \rangle,
$$

where $n$ is the number density of spins. The magnetic susceptibility $\chi$ is defined as:

$$
M = \chi H.
$$

Using $B = \mu_0 H$, the susceptibility becomes:

$$
\chi = \frac{n \mu_0 g^2 \mu_B^2 J(J+1)}{3 k_B T}.
$$

This follows **Curie’s Law**:

$$
\chi = \frac{C}{T}, \quad \text{where } C = \frac{n \mu_0 g^2 \mu_B^2 J(J+1)}{3 k_B}.
$$

# Conclusion

Starting from the quantum states of a spin in a magnetic field, we used statistical mechanics to derive the thermal average magnetization and showed that in the high-temperature limit, the susceptibility follows Curie’s law. The Curie constant is:

$$
C = \frac{n \mu_0 g^2 \mu_B^2 J(J+1)}{3 k_B}.
$$
