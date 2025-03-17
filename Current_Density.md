# Derivation and Meaning of the Current Density Expression

The given expression describes the **current density** $J$ in a system of electrons, particularly in a **semiconductor or metal under an applied electric field**. Let’s break it down systematically.

---

## Step 1: Definition of Current Density

Current density $J$ is the **electric current per unit area**, given by:

$$
J = -e \sum_{\text{electrons}} v
$$

Since electrons are distributed in **momentum space** according to the distribution function $f(k)$, we replace the summation with an **integral over all wavevectors** $k$:

$$
J = -e \int v(k) f(k) g(k) \, d^3 k
$$

where:

- **$v(k)$** is the **velocity** of an electron with wavevector $k$.
- **$f(k)$** is the **distribution function**, which describes the **number of electrons at each $k$**.
- **$g(k)$** is the **density of states** in $k$-space, which tells us **how many states exist** in an infinitesimal volume $d^3 k$.

---

## Step 2: Density of States in k-Space

From quantum mechanics, the **number of available electron states per unit volume in momentum space** is:

$$
g(k) = \frac{2}{(2\pi)^3}
$$

- The factor **2** accounts for the **two possible spin states** (spin-up and spin-down) of an electron.
- The factor **$(2\pi)^3$** comes from the **quantization of allowed wavevectors** in a finite-sized system.

### **Substituting this into the integral:**

$$
J = -e \int v(k) f(k) \frac{2}{(2\pi)^3} d^3 k
$$

which simplifies to:

$$
J = -\frac{2e}{(2\pi)^3} \int v(k) f(k) \, d^3 k
$$

This is the **given formula**.

---

## Step 3: Electron Velocity in Terms of $k$

From **semiclassical transport theory**, the **velocity** of an electron in a crystal is given by:

$$
v(k) = \frac{1}{\hbar} \nabla_k E(k)
$$

For a **free electron** (neglecting band structure effects), the **energy dispersion** is:

$$
E(k) = \frac{\hbar^2 k^2}{2m}
$$

Taking the gradient:

$$
\nabla_k E(k) = \frac{\hbar^2}{m} k
$$

So the velocity simplifies to:

$$
v(k) = \frac{\hbar k}{m}
$$

This is the **second equation given in the problem statement**.

---

## Step 4: Meaning of $f(k)$ (The Distribution Function)

The **distribution function** $f(k)$ describes the **probability of an electron occupying a state at wavevector $k$**.

In **equilibrium**, it follows the **Fermi-Dirac distribution**:

$$
f_0(k) = \frac{1}{e^{(E - \mu) / k_B T} + 1}
$$

When an **electric field is applied**, the **distribution deviates slightly from equilibrium**:

$$
f(k) = f_0(k) + \delta f(k)
$$

where **$\delta f(k)$** is a **small perturbation** due to the applied field.

This deviation **$\delta f(k)$** is responsible for **electrical conductivity**, as it leads to a **net flow of charge carriers**.

---

## Step 5: Physical Interpretation of the Expression

The expression:

$$
J = -\frac{2e}{(2\pi)^3} \int v(k) f(k) \, d^3 k
$$

means that the **total current density** is obtained by **integrating over all possible electron states**, weighting each state by:

- **The electron charge** $-e$.
- **The electron velocity** $v(k)$.
- **The occupation probability** $f(k)$.
- **A factor of 2** to account for the **two possible spin states** of each electron.

---

# **Final Summary**

### **Equation Breakdown**

| Term | Meaning |
|------|---------|
| $-2e$ | Charge per electron, including spin factor |
| $\int d^3 k$ | Summation over all possible momentum states |
| $(2\pi)^3$ | Normalization due to quantum mechanical density of states |
| $v(k)$ | Electron velocity as a function of wavevector |
| $f(k)$ | Occupation probability of each state |

### **Key Insights**
- The **total current** depends on the **shape of the Fermi surface** and **how electrons are distributed**.
- If **$f(k)$ is symmetric** in **$k$-space**, **no net current flows**.
- When an **electric field is applied**, **$f(k)$ shifts slightly**, leading to a **net current**.
- The **integral** is used to derive **important transport properties** like **conductivity and the Drude model**.
