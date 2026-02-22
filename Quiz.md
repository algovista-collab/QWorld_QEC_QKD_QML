<img width="771" height="340" alt="image" src="https://github.com/user-attachments/assets/965994db-2d58-483d-bc84-446310f792ac" />

<img width="246" height="252" alt="image" src="https://github.com/user-attachments/assets/c3b50864-e3bd-4993-8a0c-f55680a9e3ff" />

## Hamming Distance Calculation

**Strings:**
- $s = 010111101$
- $t = 110111010$

**Calculation:**
Using the formula $d_H(\mathbf{s}, \mathbf{t}) = \sum_{i} \mathbf{s}_i + \mathbf{t}_i$ (where addition is XOR):
1. Position 1: $0 \oplus 1 = 1$
2. Position 7: $1 \oplus 0 = 1$
3. Position 8: $0 \oplus 1 = 1$
4. Position 9: $1 \oplus 0 = 1$

**Total Distance:** 4

<img width="381" height="235" alt="image" src="https://github.com/user-attachments/assets/2a11cf6e-9e68-4c76-858d-4fd35d12af36" />

n is the number of columns, n = 5 and k is the number of rows, k = 3

## Hamming Code Encoding (m = 1011)

| Bit Type | D1 | D2 | D3 | D4 | P1 | P2 | P3 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Value** | 1 | 0 | 1 | 1 | 0 | 0 | **0** |

**Final Codeword:** `1011000`

### Result
The last bit of the output is **0**.

### For the Hamming code, what is the error corresponding to the syndrome (0,1,1)? 

The syndrome 011 indicates 3 in binary hence it is [0 0 1 0 0 0 0]

## Binary Code Vector Analysis [n, k, d]

To find the number of vectors that exist in the total space but outside the code, we subtract the number of valid codewords from the total number of possible vectors in the n-bit space.

| Attribute | Formula |
| :--- | :--- |
| Total vectors in $n$-bit space | $2^n$ |
| Valid codewords in $k$-dimension | $2^k$ |
| **Vectors outside the code** | **$2^n - 2^k$** |

### Final Answer
The number of vectors existing outside the code is **$2^n - 2^k$**.

## Parity-Check Matrix H Calculation

Given the generator matrix $G = [I_k | P]$:

$$G = \begin{pmatrix} 1 & 0 & 0 & 1 & 1 \\ 0 & 1 & 0 & 1 & 0 \\ 0 & 0 & 1 & 1 & 1 \end{pmatrix}$$

The parity-check matrix $H$ is constructed as $[P^T | I_{n-k}]$:

| Matrix | Column 1 | Column 2 | Column 3 | Column 4 | Column 5 |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Row 1** | 1 | 1 | 1 | 1 | 0 |
| **Row 2** | 1 | 0 | 1 | 0 | 1 |

### Final Result
The parity-check matrix **H** is:
**[[1, 1, 1, 1, 0], [1, 0, 1, 0, 1]]**

<img width="777" height="220" alt="image" src="https://github.com/user-attachments/assets/8eb17ec5-edaf-4340-b76b-5695bbc06e92" />

## 1. The Code Space Definitions
The 3-qubit bit-flip repetition code encodes a single logical qubit into three physical qubits using the following logical basis states:
* $|0\rangle_L = |000\rangle$
* $|1\rangle_L = |111\rangle$

## 2. Requirements for a Logical Hadamard ($\bar{H}$)
A valid logical Hadamard must perform the following transformation on the code space:
* $\bar{H}|0\rangle_L = \frac{1}{\sqrt{2}}(|0\rangle_L + |1\rangle_L) = \frac{|000\rangle + |111\rangle}{\sqrt{2}}$
* $\bar{H}|1\rangle_L = \frac{1}{\sqrt{2}}(|0\rangle_L - |1\rangle_L) = \frac{|000\rangle - |111\rangle}{\sqrt{2}}$

## 3. Applying the Transversal Operator $H^{\otimes 3}$
If we apply $H$ to each physical qubit individually, we get:
$$(H \otimes H \otimes H)|000\rangle = (H|0\rangle) \otimes (H|0\rangle) \otimes (H|0\rangle)$$
$$= \left(\frac{|0\rangle + |1\rangle}{\sqrt{2}}\right) \otimes \left(\frac{|0\rangle + |1\rangle}{\sqrt{2}}\right) \otimes \left(\frac{|0\rangle + |1\rangle}{\sqrt{2}}\right)$$

### Expanding the Product:
When you expand the tensor product above, you get a superposition of **all 8 possible basis states**:
$$\frac{1}{\sqrt{8}} (|000\rangle + |001\rangle + |010\rangle + |011\rangle + |100\rangle + |101\rangle + |110\rangle + |111\rangle)$$

## 4. Conclusion
The resulting state is **not** the logical state $\frac{|000\rangle + |111\rangle}{\sqrt{2}}$. 

* **Leakage:** The operator $H^{\otimes 3}$ maps a state within the code space to a state largely **outside** the code space (e.g., $|001\rangle$ is not a valid logical state).
* **Logical Error:** Because it doesn't preserve the code space or perform the correct logical rotation, it cannot be the logical $\bar{H}$.

> **Note:** For the bit-flip repetition code, the transversal $X$ operator ($X \otimes X \otimes X$) **is** a valid logical $\bar{X}$, because it maps $|000\rangle \leftrightarrow |111\rangle$.

<img width="787" height="248" alt="image" src="https://github.com/user-attachments/assets/4017b7b4-27d8-41a5-b0e7-058a475114dc" />

## 1. What is an Erasure Error?
An **erasure error** is a specific type of error where:
* A qubit is lost or becomes completely decohered.
* The location of the error (which qubit was lost) is **known** to the observer.
* This is easier to correct than a standard bit-flip error because you don't have to "guess" where the error occurred.

## 2. Why the Repetition Code Works
The 3-qubit bit-flip code uses the logical states $|0\rangle_L = |000\rangle$ and $|1\rangle_L = |111\rangle$. 

If Bob knows that **Qubit 1** was erased, he only needs to look at the remaining two qubits (Qubit 2 and Qubit 3):
* If the remaining qubits are $|00\rangle$, the original state was $|0\rangle_L$.
* If the remaining qubits are $|11\rangle$, the original state was $|1\rangle_L$.

## 3. Comparison of Error Capacities
Quantum codes generally have a higher capacity for correcting erasures than for correcting "blind" errors:

| Error Type | Capacity for $n$-qubit code | For 3-qubit Repetition Code |
| :--- | :--- | :--- |
| **Bit-Flips ($X$)** | Corrects up to $t = \lfloor \frac{d-1}{2} \rfloor$ | 1 Error |
| **Erasures** | Corrects up to $e = d - 1$ | 2 Erasures* |

*\*Note: While the 3-qubit code can technically correct up to 2 erasures (since a single remaining qubit $|0\rangle$ or $|1\rangle$ still identifies the logical state), the statement asks about a **single** erasure, which it handles easily.*

---

## 4. Conclusion
Because Bob knows the location of the lost qubit, he can ignore the "garbage" data from that position and reconstruct the logical information from the redundant information stored in the surviving qubits.

<img width="762" height="316" alt="image" src="https://github.com/user-attachments/assets/c06dccce-da09-4e8f-ab28-3b2c46c493d3" />

## 1. The Parity Check Operators
In a bit-flip repetition code, we don't measure the qubits directly (which would destroy the superposition). Instead, we measure the **parity** between adjacent qubits using these stabilizer operators:
* **$S_1 = Z_0 Z_1$**: Compares qubit 0 and qubit 1.
* **$S_2 = Z_1 Z_2$**: Compares qubit 1 and qubit 2.

## 2. Calculating the Syndrome for $X_0$
Even with the inverted encoding ($|0\rangle_L = |111\rangle$), the logic for identifying the error remains the same.

### Step-by-Step Transformation:
1. **Initial State**: Assume the logical state is $|111\rangle$.
2. **Error $X_0$ Occurs**: The operator $X$ flips the first qubit, resulting in the state $|011\rangle$.
3. **Apply $S_1 (Z_0 Z_1)$**: 
   - Qubit 0 is `0` and Qubit 1 is `1`. 
   - Since they are **different**, the parity is odd.
   - **Result: 1**
4. **Apply $S_2 (Z_1 Z_2)$**: 
   - Qubit 1 is `1` and Qubit 2 is `1`. 
   - Since they are the **same**, the parity is even.
   - **Result: 0**

**Final Syndrome ($S_1 S_2$): 10**

---

## 3. Syndrome Lookup Table
This table shows how Bob identifies which qubit flipped based on the measured syndrome:

| Error Type | Syndrome ($S_1 S_2$) | Meaning |
| :--- | :--- | :--- |
| **No Error** | **00** | All qubits match. |
| **$X_0$** | **10** | Qubits 0 and 1 differ; 1 and 2 match. |
| **$X_1$** | **11** | Qubits 0 and 1 differ; 1 and 2 differ. |
| **$X_2$** | **01** | Qubits 0 and 1 match; 1 and 2 differ. |



## 4. Conclusion
Even though Alice and Bob swapped the definitions of $|0\rangle_L$ and $|1\rangle_L$, the **relationships** between the physical qubits remain the same. An error on the first qubit will always trigger the first parity check ($S_1$) while leaving the second one ($S_2$) satisfied.

<img width="753" height="327" alt="image" src="https://github.com/user-attachments/assets/9ba736d7-3fd0-413d-942e-fc78ecb2626b" />

When a rotation error $R_z^{(0)}(\pi/4)$ occurs on the first qubit of a phase-flip repetition code, the resulting state becomes a superposition of the "no error" state and a "phase-flip" error state.

### Why there are two syndromes
A rotation $R_z(\theta)$ can be expressed as a linear combination of the Identity operator ($I$) and the Pauli-$Z$ operator:
$$R_z(\theta) = \cos(\theta/2)I - i\sin(\theta/2)Z$$

1.  **Projective Measurement**: When Bob measures the syndrome, the quantum state collapses into one of the eigenstates of the stabilizer operators.
2.  **Possible Outcomes**:
    * **Syndrome 00**: Corresponds to the $I$ component (no error detected).
    * **Syndrome 10**: Corresponds to the $Z_0$ component (a phase-flip on the first qubit detected).

Because the error is a coherent rotation rather than a discrete flip, the syndrome measurement itself "discretizes" the error into one of these two possibilities.

<img width="441" height="193" alt="image" src="https://github.com/user-attachments/assets/b3abc35e-8f43-45d7-9945-848a90c7fcda" />

# Statement Analysis: Shor Code Phase Equivalence

**Statement:** For the Shor code, $Z_0 |\bar{\psi}\rangle = Z_3 |\bar{\psi}\rangle$

**Answer: False**

---

## Why?

In the 9-qubit Shor code, the logical qubits are encoded using a concatenation of phase-flip and bit-flip codes. The basis states are:

$$|\bar{0}\rangle = \frac{1}{2\sqrt{2}} \left( |000\rangle + |111\rangle \right) \otimes \left( |000\rangle + |111\rangle \right) \otimes \left( |000\rangle + |111\rangle \right)$$

$$|\bar{1}\rangle = \frac{1}{2\sqrt{2}} \left( |000\rangle - |111\rangle \right) \otimes \left( |000\rangle - |111\rangle \right) \otimes \left( |000\rangle - |111\rangle \right)$$

The 9 qubits are indexed 0–8 and grouped into three blocks:
* **Block 1:** Qubits $\{0, 1, 2\}$
* **Block 2:** Qubits $\{3, 4, 5\}$
* **Block 3:** Qubits $\{6, 7, 8\}$

---

## Effect of Phase Operators

### What does $Z_0$ do?
* It applies a phase flip to qubit 0, which is part of the **first block**.
* In the Shor code, a $Z$ error on *any* single qubit in a block (e.g., $Z_0, Z_1, \text{ or } Z_2$) has the same effect: it flips the sign of that specific block's GHZ state.

$$Z_0 |\psi\rangle_L = \frac{1}{\sqrt{2}} (|000\rangle - |111\rangle) \otimes \frac{1}{\sqrt{2}} (|000\rangle + |111\rangle) \otimes \frac{1}{\sqrt{2}} (|000\rangle + |111\rangle)$$

### What does $Z_3$ do?
* It applies a phase flip to qubit 3, which is part of the **second block**.
* This affects the relative phase of the second GHZ triplet only.

$$Z_3 |\psi\rangle_L = \frac{1}{\sqrt{2}} (|000\rangle + |111\rangle) \otimes \frac{1}{\sqrt{2}} (|000\rangle - |111\rangle) \otimes \frac{1}{\sqrt{2}} (|000\rangle + |111\rangle)$$

---

## Conclusion
Because these operators act on **different blocks**, they result in different physical states. Specifically, $Z_0$ and $Z_3$ create different error syndromes that the code uses to identify which block needs correction.

<img width="710" height="277" alt="image" src="https://github.com/user-attachments/assets/a02d4d43-4f6d-4595-9edc-8fdc9fa37efc" />

# Analysis of Shor Code Errors

To determine which of these errors the Shor code is either immune to or can correct, we look at the structure of the **9-qubit Shor code**. The code is built by nesting a 3-qubit phase-flip code inside a 3-qubit bit-flip code, effectively creating three blocks of three qubits.

## Shor Code Structure
The 9 qubits are organized into **three distinct blocks**:
* **Block 1:** Qubits $\{1, 2, 3\}$
* **Block 2:** Qubits $\{4, 5, 6\}$
* **Block 3:** Qubits $\{7, 8, 9\}$



---

## Evaluating the Two-Qubit Errors

The Shor code is designed to correct any single-qubit error. However, it can also handle specific multi-qubit errors if they occur in different "protection zones" or cancel each other out.

| Error | Type/Location | Result | Why? |
| :--- | :--- | :--- | :--- |
| **$X_1 X_7$** | Bit-flip in Block 1 & Block 3 | **Correctable** | The bit-flip protection works per block. Since these are in separate blocks, the code fixes both. |
| **$Z_7 Z_8$** | Phase-flip in Block 3 (twice) | **Immune** | In the Shor code, $Z_7$ and $Z_8$ have the same effect on the logical state. Applying both ($Z \cdot Z = I$) leaves the state unaffected. |
| **$Z_1 X_6$** | Phase-flip in Block 1 & Bit-flip in Block 2 | **Correctable** | These are independent errors in different blocks. The hierarchy allows for simultaneous correction. |

---

### Summary
All three options provided in the image are errors that the Shor code can either **correct** (return to the original state via syndrome measurement) or is **immune** to (the logical state is never changed by the error).

> **Note:** The Shor code fails if two *bit-flips* ($X$) occur in the same block, or if *phase-flips* ($Z$) occur in two different blocks. None of the options provided meet those failure criteria.

<img width="277" height="182" alt="image" src="https://github.com/user-attachments/assets/c2865ae0-c110-4e89-be42-be976c115860" />

# Evaluating the Operator $HXZH$

To simplify the expression $HXZH$, we utilize the property that the Hadamard gate swaps the $X$ and $Z$ bases.

## 1. Relevant Identities
The Hadamard gate ($H$) has the following effects on Pauli operators:
* $HXH = Z$
* $HZH = X$
* $H^2 = I$ (where $I$ is the identity matrix)

---

## 2. Step-by-Step Derivation
We can solve this by inserting the identity $I = HH$ between the $X$ and $Z$ operators:

$$H X (Z) H$$

Since $H^2 = I$, we can rewrite this as:
$$H X (H H) Z H$$

Now, group the terms based on known identities:
$$(HXH) (HZH)$$

Using our identities:
* $(HXH) = Z$
* $(HZH) = X$

Substituting these back in gives us:
$$Z \cdot X = ZX$$

---

## 3. Final Result
The operator $HXZH$ simplifies to:
**$ZX$** (which is also equal to $iY$)

### **Q: How many simultaneous stabilizer states does the operator $Y \otimes Y \otimes Y$ have?**

**A: 4**

#### **Explanation**
* **Hilbert Space Dimension:** For a system with $n$ qubits, the total number of possible states (the dimension of the Hilbert space) is $2^n$. For 3 qubits ($Y \otimes Y \otimes Y$), the total dimension is $2^3 = 8$.
* **Eigenvalue Split:** Any Pauli operator (other than the identity) acting on $n$ qubits splits the Hilbert space exactly in half. One half belongs to the $+1$ eigenvalue, and the other half belongs to the $-1$ eigenvalue.
* **Stabilizer Definition:** A "stabilizer state" is a state that is an eigenstate of the operator with an eigenvalue of $+1$.
* **Calculation:** Total states divided by 2 gives the number of $+1$ eigenstates:
    $$\frac{2^3}{2} = \frac{8}{2} = 4$$

<img width="383" height="330" alt="image" src="https://github.com/user-attachments/assets/8caecd41-72dc-4f04-b995-e2ba902d0404" />

## 1. Requirement Checklist
To be valid generators for a stabilizer group $\mathcal{S}$, the operators must satisfy:
* **Elements of the Pauli Group:** They must be tensor products of $I, X, Y, Z$.
* **Pairwise Commutation:** $[g_i, g_j] = 0$ for all $i, j$.
* **Consistency:** The set must not generate $-I$ (the "all-negative" identity).
* **Independence:** No generator can be expressed as a product of the others.

---

## 2. Pairwise Commutation Check
We define the generators as:
* $g_1 = X \otimes Z \otimes Z$
* $g_2 = X \otimes I \otimes I$
* $g_3 = I \otimes X \otimes X$

### $g_1$ and $g_2$
* **Pos 1:** $X$ vs $X$ (Commute)
* **Pos 2:** $Z$ vs $I$ (Commute)
* **Pos 3:** $Z$ vs $I$ (Commute)
* **Result:** **Commute**

### $g_2$ and $g_3$
* **Pos 1:** $X$ vs $I$ (Commute)
* **Pos 2:** $I$ vs $X$ (Commute)
* **Pos 3:** $I$ vs $X$ (Commute)
* **Result:** **Commute**

### $g_1$ and $g_3$
* **Pos 1:** $X$ vs $I$ (Commute)
* **Pos 2:** $Z$ vs $X$ (Anticommute: $ZX = -XZ$)
* **Pos 3:** $Z$ vs $X$ (Anticommute: $ZX = -XZ$)
* **Math:** $(-1)^2 = +1$. Since there are an **even number** of anticommuting positions, the operators commute overall.
* **Result:** **Commute**

---

## 3. Independence and Consistency
* **Independence:** There is no combination of products (e.g., $g_1 g_2$) that results in $g_3$. For instance, $g_1 g_2 = I \otimes Z \otimes Z$, which is distinct from $g_3$.
* **Consistency:** Since all generators have a $+1$ phase and pairwise commute, their product will never yield $-I$.

### Conclusion: **True**
