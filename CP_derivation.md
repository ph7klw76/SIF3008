
![image](https://github.com/user-attachments/assets/00289f68-f717-4c45-8299-6e7e05ff1e33)

to
![image](https://github.com/user-attachments/assets/a87383cb-4ff9-407f-ba4e-5ba84d5de324)

Dirac‐comb Potential

# 1. Set up the model

We consider a one‐dimensional, infinitely periodic potential consisting of delta spikes at  
$x = n a$ for all integers $n$. The potential can be written

$$
V(x) = P \sum_{n=-\infty}^{+\infty} \delta(x - n a),
$$

where:

- $a$ is the period (the separation between successive spikes).
- $P$ is a real constant with dimensions of energy × length, which measures the strength of each delta barrier.
- $\delta(\cdot)$ is the Dirac delta function.

We want to solve the time‐independent Schrödinger equation

$$
-\frac{\hbar^2}{2m} \frac{d^2 \psi(x)}{dx^2} + V(x) \psi(x) = E \psi(x),
$$

subject to the requirement that $\psi(x)$ be a Bloch wave (i.e., it satisfies Bloch’s theorem for a crystal‐like periodic potential).

Bloch’s theorem tells us that for a periodic potential of period $a$, the wavefunction $\psi$ can always be taken to satisfy

$$
\psi(x + a) = e^{i k a} \psi(x), \quad \psi'(x + a) = e^{i k a} \psi'(x),
$$

for some crystal-momentum $k$. This is the essence of Bloch periodicity.

Our goal is to find the allowed values of the energy $E$ (and thus $\alpha = \frac{2mE}{\hbar}$) that satisfy these boundary/bulk conditions in the presence of the delta‐function barriers.

# 2. Solve Schrödinger’s equation in one unit cell (free region)

Between any two neighboring delta spikes—say between $x = 0$ and $x = a$—the potential $V(x) = 0$. Hence, in that interval the Schrödinger equation reduces to the free‐particle form

$$
-\frac{\hbar^2}{2m} \frac{d^2 \psi}{dx^2} = E\psi \quad \Rightarrow \quad \frac{d^2 \psi}{dx^2} = - \alpha^2 \psi,
$$

where we define

$$
\alpha = \frac{\sqrt{2mE}}{\hbar}.
$$

The general solution in the region $0 < x < a$ is then

$$
\psi(x) = A e^{i \alpha x} + B e^{-i \alpha x},
$$

for constants $A$ and $B$.

Because the potential is the same in every period (i.e., between $x = n a$ and $(n+1)a$), in each such region the solution will have exactly the same functional form, differing only by phase or amplitude factors that enforce continuity and Bloch’s theorem.

# 3. Match boundary conditions at a delta spike

A delta function at $x = 0$ imposes two conditions on the wavefunction $\psi$:

### 3.1. Continuity of $\psi$ at $x = 0$

The wavefunction itself does not jump at a delta spike:

$$
\psi(0^+) = \psi(0^-).
$$

### 3.2. Discontinuity of the derivative at $x = 0$
Integrating the Schrödinger equation across an infinitesimal interval around the spike shows that

$$
\psi'(0^+) - \psi'(0^-) = \frac{2mP}{\hbar^2} \psi(0)
$$

This jump in the derivative is proportional to the local value of $\psi$, with proportionality constant $\frac{2mP}{\hbar^2}$.

Since our representative unit cell can be taken from $x = 0$ to $x = a$, we apply these matching rules at $x = 0$. Likewise, from Bloch’s theorem, the wavefunction at $x = a$ is related to that at $x = 0$.

## 3.1. Write the wavefunction just to the right and left of $x = 0$

For $0 < x < a$, we already stated:

![image](https://github.com/user-attachments/assets/fa022357-ef82-4a94-8c55-9a81618e58c8)


For $-a < x < 0$, by Bloch’s theorem, the wavefunction can be expressed similarly, but it will be related to $\psi$ in $(0, a)$ by the phase factor $e^{-i k a}$ when you shift by one period. A convenient choice is

![image](https://github.com/user-attachments/assets/94318f1e-a3da-4400-a6d8-234bf9b9843d)

although one can also parametrize with other constants and then match them.  
Either way, continuity and derivative‐jump conditions at $x = 0$ will yield relations among $A$ and $B$.

## 3.2. Continuity at $x = 0$

Set $\psi(0^-) = \psi(0^+)$. From the above forms:

- Just to the right of $0$:
  
$$
\psi(0^+) = A + B.
$$
  
- Just to the left of $0$:
  
$$
\psi(0^-) = e^{-i k a} (A + B).
$$

Continuity gives

$$
A + B = e^{-i k a} (A + B).
$$

Either $A + B = 0$ or the factor enforces $e^{-i k a} = 1$. However, we actually must retain a more general condition that will ultimately get combined with the derivative jump and with Bloch’s condition at $x = a$. In the most standard derivations, one proceeds by imposing Bloch’s theorem at $x = a$ instead.

To keep things streamlined in the textbook approach, one typically uses:

### Bloch’s theorem at $x = a$:

$$
\psi(a) = e^{i k a} \psi(0), \quad \psi'(a) = e^{i k a} \psi'(0).
$$

We write $\psi(a)$ and $\psi'(a)$ in terms of $A, B$ and then match them to $e^{i k a} \psi(0)$ and $e^{i k a} \psi'(0)$. Meanwhile, we impose the derivative jump at $x = 0$. Let us do precisely that.

# 4. Apply Bloch’s theorem at $x = a$

From the same wavefunction

![image](https://github.com/user-attachments/assets/cba952ff-0822-4c3a-bd08-96573ead67df)


At $x = 0$:

$$
\psi(0) = A + B, \quad \psi'(0) = i\alpha A - i\alpha B.
$$

At $x = a$:

$$
\psi(a) = A e^{i\alpha a} + B e^{-i\alpha a},
$$

$$
\psi'(a) = i\alpha A e^{i\alpha a} - i\alpha B e^{-i\alpha a}.
$$

Bloch’s theorem says

$$
\psi(a) = e^{i k a} \psi(0), \quad \psi'(a) = e^{i k a} \psi'(0).
$$

Hence we get the system:

$$
\begin{cases}
A e^{i\alpha a} + B e^{-i\alpha a} = e^{i k a} (A + B), \\
i\alpha [A e^{i\alpha a} - B e^{-i\alpha a}] = e^{i k a} [i\alpha (A - B)].
\end{cases}
$$



# A.1  To solve

We label them for convenience:

$$
Ae^{i\alpha a} + Be^{-i\alpha a} = e^{i k a} (A+B). \quad (1)
$$

$$
i\alpha [Ae^{i\alpha a} - Be^{-i\alpha a}] = i\alpha e^{i k a} (A - B). \quad (2)
$$

Observe that each side of (2) has an overall factor of $i\alpha$.

## A.2. Divide equation (2) by $i\alpha$

Dropping the common factor $i\alpha$ simply yields

$$
Ae^{i\alpha a} - Be^{-i\alpha a} = e^{i k a} (A - B). \quad (2')
$$

That is all that “divide the second equation by $i\alpha$” means: we now have two structurally similar equations, (1) and (2'):

**(1):**

$$
Ae^{i\alpha a} + Be^{-i\alpha a} = e^{i k a} (A+B).
$$

**(2'):**

$$
Ae^{i\alpha a} - Be^{-i\alpha a} = e^{i k a} (A-B).
$$

## A.3. Form linear combinations to isolate sums and differences

With (1) and (2') in hand, a standard next step is to add and subtract these equations (or otherwise combine them) to isolate expressions like $Ae^{i\alpha a} \pm Be^{-i\alpha a}$. Let us do that explicitly:

### A.3.1. Add (1) and (2')

$$
[Ae^{i\alpha a} + Be^{-i\alpha a}] + [Ae^{i\alpha a} - Be^{-i\alpha a}] = [e^{i k a} (A+B)] + [e^{i k a} (A-B)].
$$

**Left-hand side:**

$$
(Ae^{i\alpha a} + Be^{-i\alpha a}) + (Ae^{i\alpha a} - Be^{-i\alpha a}) = 2Ae^{i\alpha a}.
$$

(Notice how $+Be^{-i\alpha a}$ and $-Be^{-i\alpha a}$ cancel out.)

**Right-hand side:**

$$
e^{i k a} (A+B) + e^{i k a} (A-B) = e^{i k a} [(A+B) + (A-B)] = 2Ae^{i k a}.
$$

Hence the addition gives

$$
2Ae^{i\alpha a} = 2Ae^{i k a}.
$$

Provided $A \neq 0$, we can divide by $2A$ to get

$$
e^{i\alpha a} = e^{i k a}.
$$

This implies

$$
\alpha a = k a + 2\pi n,
$$

which (if taken literally for all $A \neq 0$) would suggest $\alpha = k$. But in the full Kronig–Penney problem, one cannot simply conclude $\alpha = k$; the $\delta$-function potential at $x = 0$ modifies the continuity conditions in a way that forces additional terms. So in the presence of the potential strength $P$, the “jump” condition at $x = 0$ will alter this naive conclusion.

Nonetheless, you can see how adding (1) and (2') by itself would force $\alpha = k$ if no other boundary condition intervened. In a free-particle crystal (no barriers), that is exactly what happens.

### A. 3.2. Subtract (2') from (1)

Now do

$$
(1) - (2') : [Ae^{i\alpha a} + Be^{-i\alpha a}] - [Ae^{i\alpha a} - Be^{-i\alpha a}] = e^{i k a} (A+B) - e^{i k a} (A-B).
$$

**Left-hand side:**

$$
[Ae^{i\alpha a} + Be^{-i\alpha a}] - [Ae^{i\alpha a} - Be^{-i\alpha a}] = 2Be^{-i\alpha a}.
$$

(Here $Ae^{i\alpha a}$ cancels out.)

**Right-hand side:**

$$
e^{i k a} (A+B) - e^{i k a} (A-B) = e^{i k a} [(A+B) - (A-B)] = 2Be^{i k a}.
$$

Hence the subtraction yields

$$
2Be^{-i\alpha a} = 2Be^{i k a}.
$$

If $B \neq 0$, we get

$$
e^{-i\alpha a} = e^{i k a} \Rightarrow -\alpha a = k a + 2\pi n.
$$

Again, taken at face value (in the absence of the delta-function jump), this would give $\alpha = -k$, which combined with the previous conclusion $\alpha = +k$ forces either $A = 0$ or $B = 0$ or $k = 0$, etc.


# 5. Enforce derivative discontinuity at 𝑥=0

For a 1D Schrödinger equation with a delta‐function barrier of strength $P$ at $x = 0$, we know the wavefunction $\psi(x)$ must be continuous at $x = 0$, but its derivative jumps there by an amount proportional to $\psi(0)$. Concretely,

$$
\psi'(0^+) - \psi'(0^-) = \frac{2m}{\hbar^2} P \psi(0). \quad (*)
$$

Here:

- $\psi'(0^+)$ means the derivative just to the right of $x = 0$.
- $\psi'(0^-)$ means the derivative just to the left of $x = 0$.
- $\frac{2m}{\hbar^2} P$ is the coefficient that appears upon integrating the Schrödinger equation across the delta spike.

## 5.2. Wavefunction forms on either side of $x = 0$

Consider the interval $(-a,0)$ to the left of the spike and $(0,a)$ to the right of the spike. In each region (where the potential is zero), the wavefunction is a superposition of plane waves $\exp(\pm i\alpha x)$.

### Right side $(0 < x < a)$:

$$
\psi(0<x<a) = A e^{i\alpha x} + B e^{-i\alpha x}.
$$

From this, evaluate at $x = 0$:

$$
\psi(0^+) = A + B, \quad \psi'(0^+) = \frac{d}{dx}[\psi(x)]_{x=0^+} = i\alpha A - i\alpha B.
$$

Thus,

$$
\psi'(0^+) = i\alpha (A - B).
$$

### Left side $(-a < x < 0)$:

Because of Bloch’s theorem (periodicity in a crystal), $\psi(x)$ on the left side is the same shape but acquires a phase factor $e^{-i k a}$ when going one lattice period to the left. A common way to write this is

$$
\psi(-a<x<0) = e^{-i k a} [A e^{i\alpha x} + B e^{-i\alpha x}].
$$

Evaluate at $x = 0$:

$$
\psi(0^-) = e^{-i k a} [A+B],
$$

and

$$
\psi'(0^-) = \frac{d}{dx} \left[ e^{-i k a} (A e^{i\alpha x} + B e^{-i\alpha x}) \right]_{x=0^-}.
$$

The factor $e^{-i k a}$ is just a constant, so

$$
\psi'(0^-) = e^{-i k a} [i\alpha A - i\alpha B]_{x=0} = e^{-i k a} (i\alpha A - i\alpha B).
$$

Hence,

$$
\psi'(0^-) = i\alpha (A - B) e^{-i k a}.
$$

## 5.3. Compute $\psi'(0^+) - \psi'(0^-)$

From the two expressions just found:

$$
\psi'(0^+) = i\alpha (A - B),
$$

$$
\psi'(0^-) = i\alpha (A - B) e^{-i k a}.
$$

Therefore,

$$
\psi'(0^+) - \psi'(0^-) = i\alpha (A - B) - i\alpha (A - B) e^{-i k a}.
$$

Factor out $i\alpha (A - B)$:

$$
\psi'(0^+) - \psi'(0^-) = i\alpha (A - B) [1 - e^{-i k a}].
$$

In many textbook derivations, this expression may be rearranged or left in that factored form.

## 5.4. Match to $\frac{2m}{\hbar^2} P \psi(0)$

According to the delta‐function boundary condition $(*)$, we must have:

$$
\psi'(0^+) - \psi'(0^-) = \frac{2m}{\hbar^2} P \psi(0).
$$

We already know $\psi(0^+) = A + B$. Continuity of $\psi$ at $x = 0$ means $\psi(0^-) = \psi(0^+)$, so $\psi(0) = A + B$ on both sides. Hence the right side is

$$
\frac{2m}{\hbar^2} P (A + B).
$$

Thus the jump condition becomes

$$
i\alpha (A - B) [1 - e^{-i k a}] = \frac{2m}{\hbar^2} P (A + B). \quad (**)
$$

This is the explicit relationship that ties together:

- $A$ and $B$ (the coefficients in the wavefunction),
- $\alpha = \frac{\sqrt{2mE}}{\hbar}$ (related to the energy $E$),
- $k$ (the Bloch wavevector),
- and $P$ (the delta‐function strength).

## 5.5 Combine with Bloch’s conditions at $x = a$

One must also impose the Bloch boundary conditions at $x = a$:

$$
\psi(a) = e^{i k a} \psi(0), \quad \psi'(a) = e^{i k a} \psi'(0).
$$

Those lead to the pair of equations (often labeled (1) and (2) in the standard derivation) involving:

$$
A e^{i\alpha a} + B e^{-i\alpha a} = e^{i k a} (A+B),
$$

$$
i\alpha [A e^{i\alpha a} - B e^{-i\alpha a}] = i\alpha e^{i k a} (A - B).
$$

Meanwhile, continuity at $x = 0$ told us $\psi(0^-) = \psi(0^+)$.

Putting all these conditions together (the pair at $x = a$, continuity at $0$, and derivative jump at $0$) yields a system of equations for $A$ and $B$. For a nontrivial solution ($A, B \neq 0$), you get a constraint that can be rewritten as

$$
\cos(k a) = \cos(\alpha a) + \frac{P}{\alpha a} \sin(\alpha a).
$$

This is precisely the Kronig–Penney dispersion relation for a chain of delta spikes of strength $P$ spaced by distance $a$.

## 6. Conclusion

### Why the extra term?

The factor $\frac{P}{\alpha a} \sin(\alpha a)$ on the right side of the final equation is directly traceable to the jump condition $(**)$ that we derived, i.e.,

$$
\psi'(0^+) - \psi'(0^-) = \frac{2m}{\hbar^2} P \psi(0).
$$

That is what allows a nonzero $P$ to shift the band structure relative to the simple “$\alpha = k$” result one would get for a free electron in a perfect crystal with no barriers.

### Physical interpretation

- The wavefunction is continuous (even at the delta spike).
- The derivative has a finite jump $\propto \psi(0)$.
- Because the wavefunction is forced to obey Bloch’s theorem over each period of length $a$, that jump modifies the condition linking $k$, $\alpha$, and $P$.
- Solutions for $\alpha$ (hence the allowed energies $E = \frac{\hbar^2 \alpha^2}{2m}$) exist only when $\cos(k a)$ fits within the range implied by the right‐hand side $\cos(\alpha a) + \frac{P}{\alpha a} \sin(\alpha a)$. This leads to allowed energy bands and forbidden gaps in one dimension.



