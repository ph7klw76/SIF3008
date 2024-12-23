# 1. Setting Up the Problem

## 1.1 Supercurrent and the Superconducting Electron Density

In the superconducting state, a fraction $n_s$ of the electrons condense into a coherent “superfluid” that moves without dissipation. Let:

- $n_s = \text{density of superconducting (Cooper-paired) electrons}$,
- $e = \text{magnitude of the electron charge} \ (e > 0, \text{though the electron’s actual charge is } -e),$
- $m = \text{effective mass of the superconducting electrons},$
- $v_s = \text{velocity of the superconducting electrons},$
- $j_s = \text{supercurrent density}.$

By definition, the supercurrent density is:

$$
j_s = n_s e v_s.
$$

## 1.2 Perfect Conductivity Assumption

Superconductors have zero electrical resistance, implying there is no friction or scattering term acting on the superconducting electrons. Classically, the electrons thus obey a force equation akin to:

$$
m \frac{d v_s}{d t} = e E,
$$

where $E$ is the electric field. This implies that any nonzero electric field accelerates the superconducting electrons.

# 2. First London Equation

## 2.1 Derivation of the First London Equation

Starting from:

$$
m \frac{d v_s}{d t} = e E,
$$

we multiply both sides by $n_s e$:

$$
n_s e m \frac{d v_s}{d t} = n_s e \cdot e E.
$$

Recognizing that $n_s e v_s = j_s$, we can rewrite the equation as:

$$
\frac{d j_s}{d t} = n_s e m \frac{d v_s}{d t} = \frac{n_s e^2}{m} E.
$$

Hence, we arrive at the first London equation:

$$
\frac{\partial j_s}{\partial t} = \frac{n_s e^2}{m} E.
$$

### Interpretation

This equation states that the rate of change of the supercurrent density is proportional to the local electric field. In other words, the superconducting carriers accelerate in response to an electric field without any resistive dissipation.

# 2. Derivation of the Second London Equation

There are multiple ways to derive the second London equation. We will detail two common approaches:

- **Approach A**: Directly from the requirement of gauge-invariant momentum and the relationship between $v_s$ and the vector potential $A$.
- **Approach B**: Using Maxwell’s equations and the properties of the supercurrent in the static or quasi-static regime.

After presenting both, we will reconcile them to show they yield the same result.

## 2.1 Approach A: Gauge-Invariant Momentum Argument

### 2.1.1 Gauge-Invariant Momentum

In quantum mechanics (and in particular in superconductivity), the canonical momentum of a charged particle in an electromagnetic field is replaced by the gauge-invariant kinetic momentum:

$$
p = mv_s + eA,
$$

where $A$ is the magnetic vector potential, defined by $B = \nabla \times A$.

For superconducting electrons (Cooper pairs), the supercurrent density can be written as:

$$
j_s = n_s e v_s = \frac{n_s e}{m} (p - eA).
$$

But inside a superconductor, the pair wavefunction’s phase and the electromagnetic gauge are related such that the total momentum $p$ of the Cooper pairs is constant (or zero in many gauge choices). Thus for a uniform superconductor, we often set $p = 0$. This simplification yields:

$$
j_s = -\frac{n_s e^2}{m} A.
$$

### 2.1.2 Taking the Curl

If $j_s = -\frac{n_s e^2}{m} A$, we can immediately take the curl of both sides:

$$
\nabla \times j_s = -\frac{n_s e^2}{m} (\nabla \times A).
$$

Recall that $B = \nabla \times A$. Therefore:

$$
\nabla \times j_s = -\frac{n_s e^2}{m} B.
$$

This result is the **Second London Equation** in its standard form:

$$
\nabla \times j_s = -\frac{n_s e^2}{m} B.
$$

---

## 2.2 Approach B: Using Maxwell’s Equations

### 2.2.1 Combine Ampère’s Law and the First London Equation

We start with Ampère’s Law (neglecting the displacement current for simplicity in a static or low-frequency regime):

$$
\nabla \times B = \mu_0 j_s.
$$

Next, consider the first London equation:

$$
\frac{\partial j_s}{\partial t} = \frac{n_s e^2}{m} E. 
$$

### 2.2.2 Express $E$ in Terms of $B$

We can relate $E$ to $B$ via Faraday’s law:

$$
\nabla \times E = -\frac{\partial B}{\partial t}.
$$

While this does not immediately give $E$ in terms of $B$, we can glean certain conditions from static or near-static assumptions. For instance, in a steady-state or static magnetization scenario inside a superconductor, $E \approx 0$ and $\frac{\partial j_s}{\partial t} \approx 0$. Thus, from the first London equation, $E = 0$ implies no acceleration of the supercurrent in steady state.

### 2.2.3 Take the Curl of Ampère’s Law

Now, to find a direct link between $j_s$ and $B$, take the curl of both sides of Eq. (1):

$$
\nabla \times (\nabla \times B) = \mu_0 \nabla \times j_s.
$$

Using the vector identity:

$$
\nabla \times (\nabla \times B) = \nabla (\nabla \cdot B) - \nabla^2 B,
$$

and noting $\nabla \cdot B = 0$ (no magnetic monopoles), we get:

$$
-\nabla^2 B = \mu_0 \nabla \times j_s.
$$

### 2.2.4 Relate $\nabla \times j_s$ to $B$

We want an expression of the form $\nabla \times j_s \propto B$. Empirically or from the gauge-invariant argument (Approach A), one finds that in a superconductor:

$$
\nabla \times j_s = -\frac{n_s e^2}{m} B.
$$

Putting this directly into the right-hand side of Eq. (4) yields:

$$
-\nabla^2 B = -\mu_0 \frac{n_s e^2}{m} B.
$$

Or equivalently:

$$
\nabla^2 B = \frac{1}{\lambda_L^2} B,
$$

where the London penetration depth $\lambda_L$ is defined as:

$$
\lambda_L^2 = \frac{m}{\mu_0 n_s e^2}.
$$

Hence:

$$
\nabla^2 B = \frac{1}{\lambda_L^2} B.
$$

This partial differential equation implies that magnetic fields decay exponentially inside a superconductor, leading to the Meissner effect.

---

Thus, in summary, once one assumes or derives that $\nabla \times j_s$ is proportional to $-B$, the constant of proportionality can be identified as $\frac{n_s e^2}{m}$. That is precisely the statement of the **Second London Equation**:

$$
\nabla \times j_s = -\frac{n_s e^2}{m} B.
$$

# 3. London Penetration Depth and the Meissner Effect

## 3.1 Deriving the Penetration Depth

From Ampère’s law:

$$
\nabla \times B = \mu_0 j_s
$$

and the second London equation:

$$
\nabla \times j_s = -\frac{n_s e^2}{m} B,
$$

one obtains a differential equation for $B$. Taking the curl of Ampère’s law, we get:

$$
\nabla \times (\nabla \times B) = \mu_0 \nabla \times j_s.
$$

Using the vector identity:

$$
\nabla \times (\nabla \times B) = \nabla (\nabla \cdot B) - \nabla^2 B,
$$

and noting that $\nabla \cdot B = 0$ (since there are no magnetic monopoles), this becomes:

$$
-\nabla^2 B = \mu_0 \nabla \times j_s.
$$

Substitute the second London equation:

$$
\nabla \times j_s = -\frac{n_s e^2}{m} B,
$$

to get:

$$
-\nabla^2 B = -\mu_0 \frac{n_s e^2}{m} B.
$$

Hence, we arrive at the London equation for the magnetic field:

$$
\nabla^2 B = \frac{\mu_0 n_s e^2}{m} B.
$$

Define the London penetration depth $\lambda_L$ by:

$$
\lambda_L^2 = \frac{m}{\mu_0 n_s e^2}.
$$

Then the field equation becomes:

$$
\nabla^2 B = \frac{1}{\lambda_L^2} B.
$$

In one dimension (e.g., near a flat superconducting surface at $x = 0$), the solution is an exponential decay of the magnetic field:

$$
B(x) = B(0) e^{-x / \lambda_L}.
$$

This exponential screening underlies the Meissner effect, where the superconductor expels magnetic fields from its interior (or, more precisely, confines them to a thin boundary layer of thickness $\lambda_L$).

---

## 3.2 Physical Meaning

- **Perfect Diamagnetism**: Because $B$ decays exponentially in the superconductor, the superconductor behaves as if it “repels” magnetic fields.
- **Meissner Effect**: Even if a magnetic field is present when the material transitions into the superconducting state, the superconductor drives out (screens) that magnetic flux from its bulk.

Python program to solve the second London equation can be found at 
https://github.com/ph7klw76/SQA7018/blob/main/London_equation.md


