# The Binary Representation of Pauli Operators

In quantum error correction, we often bypass long strings of matrices like $X \otimes Z \otimes Y$ by mapping them to strings of $0$s and $1$s. This **symplectic representation** makes it much easier to track operator commutation and stabilizer transformations.

## 1. The Core Idea: Mapping to Vectors
For a single qubit, we represent the four basic Pauli operators as pairs of bits $(a|b)$.
* The first bit ($a$) tracks the **$X$ component**.
* The second bit ($b$) tracks the **$Z$ component**.

Since $Y = iXZ$ (ignoring the phase $i$), it is treated as having both an $X$ and a $Z$ component.

| Pauli Operator | Vector $(a|b)$ | Why? |
| :--- | :--- | :--- |
| $I$ (Identity) | $(0|0)$ | No $X$, no $Z$. |
| $X$ | $(1|0)$ | Has $X$, no $Z$. |
| $Z$ | $(0|1)$ | No $X$, has $Z$. |
| $Y$ | $(1|1)$ | Has both $X$ and $Z$. |

---

## 2. Scaling to Multi-Qubit Systems ($n$ Qubits)
When you have $n$ qubits, the vector becomes length $2n$. It is split into two halves:
1.  **The $a$ vector** (first $n$ bits): Tracks the $X$ operators for every qubit.
2.  **The $b$ vector** (last $n$ bits): Tracks the $Z$ operators for every qubit.

### Example: $P = X \otimes I \otimes Z$
For a 3-qubit system ($n=3$):
* **Qubit 1 is $X$**: $a_1=1, b_1=0$
* **Qubit 2 is $I$**: $a_2=0, b_2=0$
* **Qubit 3 is $Z$**: $a_3=0, b_3=1$

The resulting vector $v = (a|b)$ is:
$$v = (1, 0, 0 \ | \ 0, 0, 1)$$

---

## 3. Why is this useful?
This representation turns quantum mechanics problems into **linear algebra over the finite field $\mathbb{F}_2$** (binary arithmetic).

* **Multiplication:** Multiplying two Pauli operators corresponds to adding their binary vectors (modulo 2).
* **Commutation:** You can determine if two operators commute by calculating the **symplectic inner product**. 
    * If the result is $0$, they commute.
    * If the result is $1$, they anti-commute.
* This is the foundation for the **Stabilizer Formalism**, which is how we design and simulate almost all modern quantum error-correcting codes, like the Surface Code.
* In this formulation, we ignore the phase of the operators, i.e. if P = v then so are -P = v and +iP = v and -iP = v

Following the stabilizer formalism, we represent $n$-qubit Pauli operators as binary vectors of length $2n$ in the form $(a|b)$, where $a$ tracks $X$ components and $b$ tracks $Z$ components.

## 1. $X_0 Z_0 X_1 Z_1$
This operator acts on **2 qubits**. We first simplify the components for each qubit:
* **Qubit 0:** $X Z \rightarrow Y$ (contains both $X$ and $Z$ components)
* **Qubit 1:** $X Z \rightarrow Y$ (contains both $X$ and $Z$ components)

| Qubit | Operator | $a$ ($X$-bit) | $b$ ($Z$-bit) |
| :--- | :--- | :--- | :--- |
| 0 | $Y$ | 1 | 1 |
| 1 | $Y$ | 1 | 1 |

**Vector Form:** $$(1, 1 \ | \ 1, 1)$$

---

## 2. $Y_0 Y_1 Y_2$
This operator acts on **3 qubits**. Since $Y$ contains both an $X$ and a $Z$ component:
* **Qubit 0:** $Y \rightarrow a_0=1, b_0=1$
* **Qubit 1:** $Y \rightarrow a_1=1, b_1=1$
* **Qubit 2:** $Y \rightarrow a_2=1, b_2=1$

| Qubit | Operator | $a$ ($X$-bit) | $b$ ($Z$-bit) |
| :--- | :--- | :--- | :--- |
| 0 | $Y$ | 1 | 1 |
| 1 | $Y$ | 1 | 1 |
| 2 | $Y$ | 1 | 1 |

**Vector Form:** $$(1, 1, 1 \ | \ 1, 1, 1)$$

---

## 3. $Z_0 Z_1 X_2$
This operator acts on **3 qubits**:
* **Qubit 0:** $Z \rightarrow$ No $X$, has $Z$ ($a_0=0, b_0=1$)
* **Qubit 1:** $Z \rightarrow$ No $X$, has $Z$ ($a_1=0, b_1=1$)
* **Qubit 2:** $X \rightarrow$ Has $X$, no $Z$ ($a_2=1, b_2=0$)

| Qubit | Operator | $a$ ($X$-bit) | $b$ ($Z$-bit) |
| :--- | :--- | :--- | :--- |
| 0 | $Z$ | 0 | 1 |
| 1 | $Z$ | 0 | 1 |
| 2 | $X$ | 1 | 0 |

**Vector Form:** $$(0, 0, 1 \ | \ 1, 1, 0)$$

---

## 2. Operations

### Multiplication (Vector Addition)
Multiplying two operators corresponds to the **bitwise XOR** (addition modulo 2) of their binary vectors.

**Example:** $X \times X = I$
- $(1 | 0) + (1 | 0) = (0 | 0)$

### Commutation (Symplectic Inner Product)
To determine if two operators $P_1 = (a | b)$ and $P_2 = (c | d)$ commute, use the symplectic inner product:

$$P_1 \cdot P_2 = (a \cdot d \oplus b \cdot c) \pmod 2$$

- If result = **0**: Operators **Commute**
- If result = **1**: Operators **Anti-commute**

---

## 3. Calculation Example

**Operators:**
- $P_1 = (10 | 01) \rightarrow (X \otimes Z)$
- $P_2 = (01 | 00) \rightarrow (I \otimes X)$

**Inner Product Calculation:**
$$(10 \cdot 00) \oplus (01 \cdot 01) = 0 \oplus 1 = 1$$

**Result:** The operators **anti-commute**.

### Symplectic Inner Product Calculation

**Operators:**
- $P_1 = (101 | 110)$ where $a=(1,0,1), b=(1,1,0)$
- $P_2 = (011 | 111)$ where $c=(0,1,1), d=(1,1,1)$

**Formula:**
$$P_1 \cdot P_2 = (a \cdot d \oplus b \cdot c) \pmod 2$$

**Step-by-Step:**
1. **$a \cdot d$**: $(1 \cdot 1) + (0 \cdot 1) + (1 \cdot 1) = 2 \equiv 0$
2. **$b \cdot c$**: $(1 \cdot 0) + (1 \cdot 1) + (0 \cdot 1) = 1 \equiv 1$
3. **Total**: $0 \oplus 1 = 1$

**Result:** **1** (Operators Anti-commute)
