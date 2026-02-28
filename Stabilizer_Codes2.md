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

<img width="735" height="250" alt="image" src="https://github.com/user-attachments/assets/49935764-f9c1-48f4-ac2d-7bd59060ed97" />

### Comparison of Key Stabilizer Codes

| Code | Type | $n$ | $k$ | $d$ | Capability |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **[[4, 2, 2]]** | Detection | 4 | 2 | 2 | Detects 1 error; cannot correct. |
| **[[5, 1, 3]]** | Perfect | 5 | 1 | 3 | Smallest code to correct 1 error. |
| **[[7, 1, 3]]** | Steane | 7 | 1 | 3 | CSS code; allows transversal gates. |
| **[[8, 3, 3]]** | High-Rate | 8 | 3 | 3 | Encodes 3 logical qubits; corrects 1 error. |

---

### Detailed Code Profiles

#### 1. [[4, 2, 2]] — Error-Detecting Code
- **Focus:** Hardware efficiency.
- **Limitation:** Since $d=2$, it can tell you *that* an error happened, but not *where*. It is primarily used for post-selection (discarding faulty runs).

#### 2. [[5, 1, 3]] — The "Perfect" Code
- **Focus:** Minimum overhead.
- **Logic:** Uses the smallest possible number of qubits to satisfy the Quantum Hamming Bound. It is a non-CSS code, meaning stabilizers mix $X$ and $Z$ on the same qubits.



#### 3. [[7, 1, 3]] — The Steane Code
- **Focus:** Fault tolerance.
- **Logic:** Derived from the classical Hamming [7,4,3] code. Because it is a CSS code, $X$ and $Z$ errors are handled by separate, independent stabilizer checks.



#### 4. [[8, 3, 3]] — Color/Stabilizer Code
- **Focus:** Density.
- **Logic:** Often used in topological contexts or as a high-rate stabilizer code. It offers a better ratio of logical-to-physical qubits ($3/8$) while maintaining a distance of 3.

# Encoding Circuits for Stabilizer Codes

This guide outlines the systematic three-part process developed by Daniel Gottesman to transform a stabilizer generator matrix into a physical quantum encoding circuit.

---

## 1. Bringing the Generator Matrix to Standard Form

The generator matrix $G$ of a stabilizer code (e.g., the $[[7,1,3]]$ Steane code) consists of $n-k$ rows and $2n$ columns. We use Gaussian elimination over $GF(2)$ to simplify these generators without changing the code they represent.

### Permitted Operations:
* **Row Addition:** Replacing a generator $M_i$ with $M_i \cdot M_j$ (corresponds to binary row addition).
* **Row Permutation:** Reindexing the generators.
* **Column Permutation:** Reindexing physical qubits (must be done simultaneously on both $X$ and $Z$ sides of the matrix).

### The Goal:
The matrix is reduced to **Reduced Row Echelon Form (RREF)** to produce the **Standard Form**:
$$S = \begin{bmatrix} I & A & | & B & C_1 \\ 0 & 0 & | & D & I \end{bmatrix}$$
This structure simplifies the generators to contain as few Pauli operators as possible.

---

## 2. Constructing Logical Operators

Logical operators ($\bar{X}$ and $\bar{Z}$) allow us to manipulate encoded data. They must commute with all stabilizers but anti-commute with each other.

For a code encoding $k$ logical qubits, we derive $k$ pairs of operators using the blocks identified in the Standard Form:

* **Logical $\bar{X}$:** Constructed as the rows of the matrix $[0, E^T, I, | , E^T D^T + C_2^T, 0, 0]$.
* **Logical $\bar{Z}$:** Constructed as the rows of the matrix $[0, 0, 0, | , A^T, I, 0]$.

> **Note:** Logical operators are not unique; multiple representations can act identically on the logical basis states.

---

## 3. The Encoding Algorithm (Circuit Synthesis)

Once the matrix and logical operators are defined, we follow a systematic gate sequence to map $n$ physical qubits from $|0\rangle^{\otimes n}$ to the logical zero state $|0\rangle_L$.

### The Algorithm Steps:
1. **Initialize:** Start with $n$ qubits in the $|0\rangle$ state.
2. **Apply Logical $\bar{X}$ constraints:** For $i$ in range $k$, apply $CX$ gates between logical and ancilla qubits based on the $\bar{X}$ matrix.
3. **Apply Stabilizer constraints:**
   - Apply **Hadamard (H)** gates to create necessary superpositions.
   - Apply **Controlled-NOT (CX)** gates where $X$ components exist in the standard generators.
   - Apply **Controlled-Z (CZ)** gates where $Z$ components exist.
   - If both $X$ and $Z$ exist for a qubit, apply both $CX$ and $CZ$.

### Example: Steane Code [[7,1,3]] Logical Zero State
Simulating the resulting circuit produces the following superposition of 8 basis states, each with amplitude $\approx 0.354$:

| Basis State | Amplitude |
| :--- | :--- |
| `0000000` | 0.354 |
| `1111000` | 0.354 |
| `1100110` | 0.354 |
| `0011110` | 0.354 |
| `1010101` | 0.354 |
| `0101101` | 0.354 |
| `0110011` | 0.354 |
| `1001011` | 0.354 |



---

## Summary of State Preparation
To create the **Logical One State** $|1\rangle_L$, you can:
1. Feed the $|1\rangle$ state into the encoding circuit.
2. Apply the derived **Logical $\bar{X}$** operator to the already prepared $|0\rangle_L$ state.

# Decoding Circuits for Stabilizer Codes

While error correction often happens "in-place" during quantum computation (meaning we don't always need to decode to the computational basis), decoding is essential for **quantum communication protocols** where the logical state must be retrieved as a physical qubit.

---

## 1. Simple Decoding: Circuit Reversal
The most straightforward way to decode is to simply run the encoding circuit in reverse.
* **Logic**: If the encoding circuit $U$ maps $|0\dots0\psi\rangle \rightarrow |\psi\rangle_L$, then the adjoint circuit $U^\dagger$ maps $|\psi\rangle_L \rightarrow |0\dots0\psi\rangle$.
* **Verification**: In the provided `stac` example, a random state is encoded into the Steane code and successfully retrieved by appending the `.reverse()` circuit.

---

## 2. Gottesman's Decoding Algorithm
This method uses the **Standard Form** of the stabilizer generator matrix and extracts the logical information onto a separate **ancilla qubit**.

### The Procedure:
1. **Standard Form**: Compute the RREF of the generator matrix $G$.
2. **Logical Operators**: Determine the Logical $\bar{X}$ and Logical $\bar{Z}$ operations.
3. **Synthesis**:
    - For each bit in the Logical $Z$ matrix, apply a `CX` from the physical qubit $j$ to the ancilla.
    - For each bit in the Logical $X$ matrix, apply `CX` or `CZ` (or both) from the ancilla to the physical qubit.



---

## 3. Simulation Results & Observations
* **Logical Zero ($|0\rangle_L$)**: After decoding, the decoding ancilla (the last qubit) returns to state `0`.
* **Logical One ($|1\rangle_L$)**: After decoding, the decoding ancilla flips to state `1`.
* **No-Cloning Theorem**: Upon decoding, the original physical qubits are left in a state outside the logical code space. This confirms that information was **transferred** to the ancilla, not copied.

---

# Task 1 Solution: Decoding the [[5,1,3]] Code

To decode the $[[5,1,3]]$ "Perfect Code" using the `stac` library, follow these steps:

### Step 1: Generate and Standardize
```python
import stac
# Initialize the 5-qubit code
cd_513 = stac.CommonCodes.generate_code('[[5,1,3]]')
cd_513.construct_standard_form()
cd_513.construct_logical_operators()
```
