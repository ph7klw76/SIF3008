# Sommerfeld Specific Heat Capacity

# 1.1. Drude’s Classical Paradox

### **Classical Assumption:**  
Every conduction electron has energy **$\sim \frac{3}{2} k_B T$** at temperature **$T$**.

### **Resulting Specific Heat:**  
Per electron, that yields a large contribution to **$c_v$**. Scaled by the electron density **$n$**, it would exceed **measured values** by over an **order of magnitude** at low **$T$**.

### **Experiment:**  
Observed **electron contribution to heat capacity** is **$\propto T$** and is **numerically much smaller** than the **Drude guess**.

---

# 1.2. Pauli Exclusion and the Fermi Sea

Arnold **Sommerfeld** realized electrons in a metal **fill quantum states** up to the **Fermi energy $E_F$**, with the **Fermi–Dirac distribution**:

$$
f(\varepsilon) = \frac{1}{\exp\left(\frac{\varepsilon - \mu}{k_B T} \right) + 1}
$$

where **$\mu(T) \approx E_F$** at low **$T$**. The **Pauli principle** “blocks” excitations of electrons well below **$E_F$**: only those **within $\sim k_B T$ of $E_F$** can be **thermally excited**. Hence, **most electrons remain inert at low $T$**.

---

# 2. Total Electronic Energy at Finite Temperature

## **2.1. General Expression**
Let **$E(T)$** be the **total internal energy** of the electron gas. In continuous form:

$$
E(T) = 2 \int_0^\infty \varepsilon \, g(\varepsilon) \, f(\varepsilon) \, d\varepsilon.
$$

Here:

- **Factor 2** accounts for **spin-$\frac{1}{2}$** (two electrons per quantum state).
- **$g(\varepsilon)$** is the **density of states (DOS)** for electrons (**in 3D free-electron model, $g(\varepsilon) \propto \varepsilon$**).
- **$f(\varepsilon)$** is the **Fermi–Dirac occupation for energy $\varepsilon$**.

---

## **2.1.1. Density of States (Free Electron Model)**

In a **3D free electron model**, the **density of states** is:

$$
g(\varepsilon) = \frac{V}{2\pi^2} \left( \frac{2m}{\hbar^2} \right)^{3/2} \varepsilon^{1/2},
$$

where **$V$** is the **volume** and **$m$** is the **electron mass**.

Although we will need **$g(\varepsilon)$** at **$\varepsilon = E_F$**, the **key steps** do not require the **full explicit form** until the **end**.

---

## **2.2. Zero-Temperature Baseline, $E(0)$**
At **$T = 0$**, the **Fermi–Dirac distribution** becomes a **step function**:

$$
f(\varepsilon) =
\begin{cases}
1, & \varepsilon < E_F \\
0, & \varepsilon > E_F
\end{cases}
$$

So,

$$
E(0) = 2 \int_0^{E_F} \varepsilon \, g(\varepsilon) \, d\varepsilon.
$$

This integral is **constant at $T = 0$**, giving the **ground-state energy of the electron gas** (the **“Fermi sea”** is completely **filled up to $E_F$**).

---

## **2.3. Finite $T$: Small Deviations Near $E_F$**
For **$T > 0$**, the **step at $E_F$** **“smears out”** over an energy range **$\sim k_B T$**.

Effectively:

- **Electrons with $\varepsilon \ll E_F - k_B T$ remain blocked from excitation (still fully occupied).**
- **Electrons with $\varepsilon \gg E_F + k_B T$ remain mostly unoccupied.**
- **Electrons within** **$\pm k_B T$** **of $E_F$ see partial occupation changes.**

Hence, the **main change to energy $E(T)$ vs. $E(0)$** arises **from states near $\varepsilon \approx E_F$**.

---

# 3. Sommerfeld Expansion: Mathematical Tool

Sommerfeld used a **powerful approximation** for integrals of the form:

$$
\int_0^\infty \varepsilon^n g(\varepsilon) f(\varepsilon) d\varepsilon
$$

when **$T$** is **small compared to the Fermi temperature $T_F = E_F / k_B$**.

### **3.1. Formal Statement (Simplified)**  
For a **smooth function** $\phi(\varepsilon)$:

$$
\int_0^\infty \phi(\varepsilon) f(\varepsilon) d\varepsilon
\approx
\int_0^{E_F} \phi(\varepsilon) d\varepsilon +
\frac{\pi^2}{6} \left[ \phi' (E_F) \right] (k_B T)^2 + \dots
$$

where the **derivative $\phi'(E_F)$** is **evaluated at $\varepsilon = E_F$**.

### **3.2. Applying to $E(T)$**
Set:

$$
\phi(\varepsilon) = 2 \varepsilon g(\varepsilon).
$$

Thus,

$$
E(T) \approx E(0) + \frac{\pi^2}{6} \left[ 2 \varepsilon g(\varepsilon) \right]' \Big|_{\varepsilon = E_F} (k_B T)^2 + \dots
$$

The first term is **$E(0)$**. The correction is:

$$
\Delta E(T) = \frac{\pi^2}{6} \frac{d}{d\varepsilon} [2\varepsilon g(\varepsilon)] \Big|_{\varepsilon = E_F} (k_B T)^2.
$$

---

# 4. Computing $c_v, \text{electronic} = \left(\frac{\partial E}{\partial T} \right)_V$

## **4.1. Differentiate $\Delta E(T)$**
Since:

$$
E(T) = E(0) + \Delta E(T),
$$

and:

$$
\Delta E(T) \propto (k_B T)^2,
$$

we differentiate:

$$
c_v, \text{electronic} = \frac{\partial E}{\partial T} \approx \frac{\partial \Delta E(T)}{\partial T}.
$$

If:

$$
\Delta E(T) = A (k_B T)^2,
$$

then:

$$
c_v, \text{electronic} = 2 A (k_B^2 T).
$$

## **4.2. Final Form: Linear in $T$**
Hence, at **low $T$**, the **electronic contribution**:

$$
c_v, \text{electronic} \approx \gamma T,
$$

where **$\gamma$** is a constant that depends on **$g(E_F)$** and **fundamental constants**:

$$
c_v, \text{electronic} = \frac{\pi^2}{3} k_B^2 g(E_F) T.
$$

---

# 5. Physical Picture: Only Electrons Near $E_F$ Contribute

## **5.1. Pauli Blocking**
- At **$T = 0$**, all states up to **$E_F$** are **filled**.
- At **finite, small $T$**, **only electrons within $\sim k_B T$ of $E_F$** can be **excited**.
- This fraction is **$\sim (k_B T / E_F)$**, which is typically **$\ll 1$**.

## **5.2. Linear $T$-Dependence**
- The fraction of **electrons that can be thermally excited** grows **linearly with $T$**.
- This makes the **energy change $\sim T^2$** and the **specific heat $c_v = \frac{\partial E}{\partial T} \sim T$**.

---

# 6. Final Formula and Experimental Match
Sommerfeld arrives at:

$$
c_v, \text{electronic} (T) = \gamma T,
$$

with:

$$
\gamma = \frac{\pi^2}{3} k_B^2 g(E_F).
$$

This **matches experiments** for **metals at low $T$**, proving Sommerfeld’s **free electron model** successfully **explains heat capacity** better than **Drude’s classical approach**.
