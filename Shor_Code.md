# The Shor Code

The **Shor Code** is a quantum error-correcting code designed to handle more than one type of error. This construction demonstrates a technique called **code concatenation**.

---

## 1. Code Concatenation
In code concatenation, we first encode the logical qubit using a particular code into $n$ data qubits. Then, we encode each of those $n$ data qubits individually using the same or another code.

### Phase-Flip Encoding (Layer 1)
To illustrate this, recall that in the phase-flip code, the logical basis states are encoded as:

$$|0\rangle \rightarrow |\bar{0}\rangle = |+ + +\rangle$$
$$|1\rangle \rightarrow |\bar{1}\rangle = |- - -\rangle$$

---

## 2. Bit-Flip Repetition (Layer 2)
Next, we take each of the three data qubits and encode them using the **bit-flip repetition code**. In this code, the plus and minus states encode as:

$$|+\rangle = \frac{|0\rangle + |1\rangle}{\sqrt{2}} \rightarrow |\bar{+}\rangle = \frac{|000\rangle + |111\rangle}{\sqrt{2}}$$

$$|-\rangle = \frac{|0\rangle - |1\rangle}{\sqrt{2}} \rightarrow |\bar{-}\rangle = \frac{|000\rangle - |111\rangle}{\sqrt{2}}$$

---

## 3. Final Concatenated Encoding
When we apply this bit-flip encoding to each of the three qubits in the phase-flip encoding, we obtain the 9-qubit Shor Code:



### Logical Zero ($|\bar{\bar{0}}\rangle$)
$$|\bar{\bar{0}}\rangle = \frac{(|000\rangle + |111\rangle)(|000\rangle + |111\rangle)(|000\rangle + |111\rangle)}{2\sqrt{2}}$$

### Logical One ($|\bar{\bar{1}}\rangle$)
$$|\bar{\bar{1}}\rangle = \frac{(|000\rangle - |111\rangle)(|000\rangle - |111\rangle)(|000\rangle - |111\rangle)}{2\sqrt{2}}$$

---
> **Note:** This nested structure allows the code to correct both bit-flip ($X$) and phase-flip ($Z$) errors simultaneously.

# $X$-errors (Bit-Flip Errors in Shor Code)

Suppose a single-qubit $X$ error occurs on the fourth qubit (index 3). The logical states $\ket{\bar{\bar{0}}}$ and $\ket{\bar{\bar{1}}}$ are transformed as follows:

$$X_4 \ket{\bar{\bar{0}}} = \frac{(\ket{000} + \ket{111})(\ket{010} + \ket{101})(\ket{000} + \ket{111})}{2\sqrt{2}}$$

$$X_4 \ket{\bar{\bar{1}}} = \frac{(\ket{000} - \ket{111})(\ket{010} - \ket{101})(\ket{000} - \ket{111})}{2\sqrt{2}}$$

---

## Syndrome Measurements
To detect the occurrence of a bit-flip error, we perform syndrome measurements in the style of a standard 3-qubit bit-flip code across three triplets:

* **Compare** the values of qubits 0, 1, and 2.
* **Compare** the values of qubits 3, 4, and 5.
* **Compare** the values of qubits 6, 7, and 8.

### Detection Example
In this specific case (an error on the 4th qubit):
1.  Comparing the **3rd qubit with the 4th** shows a discrepancy.
2.  Comparing the **4th qubit with the 5th** shows a discrepancy.
3.  This identifies the 4th qubit as the source of the error, which can then be corrected using an $X$ gate.

---

### Error Correction Logic
| Comparison | Result | Conclusion |
| :--- | :--- | :--- |
| $q_3 \oplus q_4$ | 1 | Qubits 3 and 4 differ |
| $q_4 \oplus q_5$ | 1 | Qubits 4 and 5 differ |
| **Identity** | **Error at $q_4$** | **Apply $X_4$ to fix** |

# $Z$-errors (Phase-Flip Errors in Shor Code)

Detecting a single-qubit $Z$ error is slightly more complex. If a $Z$ error occurs on any one of the first three qubits ($i = 0, 1, 2$), the sign (phase) in the first block will change.

### State Transformation (Inner Code Level)
For $i = 0, 1, 2$, the logical states transform as follows:

$$Z_i \ket{\bar{\bar{0}}} = \frac{(\ket{000} - \ket{111})(\ket{000} + \ket{111})(\ket{000} + \ket{111})}{2\sqrt{2}}$$

$$Z_i \ket{\bar{\bar{1}}} = \frac{(\ket{000} + \ket{111})(\ket{000} - \ket{111})(\ket{000} - \ket{111})}{2\sqrt{2}}$$

---

### Outer Code Level Perspective
A clearer way to visualize this is at the **outer code level**, where the logical encoding is represented using the sign basis states $\ket{+}$ and $\ket{-}$:

$$\ket{\bar{\bar{0}}} = \ket{\bar{+}} \ket{\bar{+}} \ket{\bar{+}}$$
$$\ket{\bar{\bar{1}}} = \ket{\bar{-}} \ket{\bar{-}} \ket{\bar{-}}$$

When a $Z_i$ error occurs for $i = 0, 1, 2$, the effect on these outer states is:

$$Z_i \ket{\bar{\bar{0}}} = \ket{\bar{-}} \ket{\bar{+}} \ket{\bar{+}}$$
$$Z_i \ket{\bar{\bar{1}}} = \ket{\bar{+}} \ket{\bar{-}} \ket{\bar{-}}$$

---

### Error Detection Strategy
At the outer level, the detection strategy becomes intuitive:
* **Procedure**: Apply the phase-flip code's error-detection procedure to the three encoded qubits (the blocks of three).
* **Logic**: By comparing the phases of the three blocks, we can identify which block contains the phase-flip error.

| Error Location | Effect on Blocks | Detection |
| :--- | :--- | :--- |
| Qubits 0, 1, or 2 | Sign flip in Block 1 | Block 1 differs from 2 & 3 |
| Qubits 3, 4, or 5 | Sign flip in Block 2 | Block 2 differs from 1 & 3 |
| Qubits 6, 7, or 8 | Sign flip in Block 3 | Block 3 differs from 1 & 2 |

# Shor Code ($Y$ Errors)

## $Y$-errors (Combined Bit-and-Phase-Flip)
The Shor code can correct errors beyond $X$ and $Z$, such as the $Y$ error ($Y = iZX$), which acts as a combined bit-flip and phase-flip.

### Example: $Y_4$ Error
Applying $Y_4$ (an error on the second block) results in corrupted basis states:

$$Y_4 \ket{\bar{\bar{0}}} = i\frac{(\ket{000} + \ket{111})(\ket{010} - \ket{101})(\ket{000} + \ket{111})}{2\sqrt{2}}$$

$$Y_4 \ket{\bar{\bar{1}}} = i\frac{(\ket{000} - \ket{111})(\ket{010} + \ket{101})(\ket{000} - \ket{111})}{2\sqrt{2}}$$

### Detection and Correction
$Y$ errors are handled in two stages:
1. **$X$ detection stage:** Identifies that the fourth qubit is flipped and fixes it.
2. **$Z$ detection stage:** Identifies that the middle block has experienced a phase error and fixes it.

---

## Conclusion: Correctable Error Set
The Shor code can correct any single-qubit error in the set:

$$\mathcal{E} = I \cup \{X_i\}_i \cup \{Y_i\}_i \cup \{Z_i\}_i, \quad i = 0, \dots, 8$$

# Multi-Qubit Error Correction in the Shor Code

The Shor code is a concatenated quantum code that can handle specific multi-qubit error patterns by utilizing its hierarchical structure.

## Correctable Two-Qubit Errors
The code can correct certain two-qubit errors provided they do not exceed the correction capacity of a single "stage" of the code:

* **Independent Bit-Flips ($X$):** The code can handle two $X$ errors if they occur in different triplets. Since each triplet (qubits 0-2, 3-5, and 6-8) is an independent bit-flip code, it can correct one $X$ error per block.
* **Combined $X$ and $Z$ Errors:** The code can correct a simultaneous $X$ error and $Z$ error on different qubits (or the same qubit, which constitutes a $Y$ error). This is possible because the $X$-syndrome and $Z$-syndrome measurements are independent.
* **Restriction on Phase-Flips ($Z$):** The code **cannot** correct two $Z$ errors if they occur in different blocks (e.g., qubit 1 and qubit 4). Because a $Z$ error on any qubit in a block flips the phase of that entire block, two such errors would appear as two phase-flipped logical blocks, which exceeds the outer code's capacity.

---

## Correctable Three-Qubit Errors
The Shor code can handle specific three-qubit errors if they are distributed to avoid overwhelming the component codes:

* **Distributed Bit-Flips:** The code can handle exactly **three $X$ errors** if, and only if, there is exactly one error in each of the three triplets (e.g., qubits 0, 3, and 6).
* **Mixed Error Patterns:** It can handle combinations such as two $X$ errors in different triplets and one $Z$ error anywhere. The inner code corrects the bit-flips, while the outer code corrects the phase-flip.

---

## Summary of Correctable Error Conditions

| Error Type | Capacity | Condition |
| :--- | :--- | :--- |
| **Bit-Flip ($X$)** | Up to 3 | Max one $X$ error per triplet block. |
| **Phase-Flip ($Z$)** | Exactly 1 | Max one $Z$ error across all nine qubits. |
| **General ($\mathcal{E}$)** | Variable | Any error in the set $I \cup \{X_i\}_i \cup \{Y_i\}_i \cup \{Z_i\}_i$. |

# Syndrome Measurements: An Alternate View

This notebook explores a technique for measuring qubits used in **repetition and Shor codes**. The objective is to perform a **partial measurement** to determine if an error has occurred without a destructive measurement of the entire state.

---

## 1. The Indirect Measurement Concept
To measure an operator $M$ (with eigenvalues $\pm1$) on an unknown state $|\psi\rangle$, an **ancilla qubit** is used to store the measurement result.

### The Circuit Logic
The process follows these steps as shown in the circuit diagram:
1. **Initialize** an ancilla qubit in the state $|0\rangle$.
2. **Superposition**: Apply a Hadamard ($H$) gate to the ancilla.
3. **Entanglement**: Apply a **Controlled-M** operation (ancilla as control, $|\psi\rangle$ as target).
4. **Interference**: Apply a second $H$ gate to the ancilla.
5. **Readout**: Measure the ancilla qubit.



---

## 2. Mathematical Evolution (Example: $M = Z$)
Using the state $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$ and measuring the $Z$ operator, the system evolves through the following stages:

1. **Initial State**: $|\psi\rangle|0\rangle$
2. **After first $H$**: $|\psi\rangle|0\rangle + |\psi\rangle|1\rangle$
3. **After $CZ_{10}$**: $|\psi\rangle|0\rangle + (Z|\psi\rangle)|1\rangle$
4. **After second $H$**: $(|\psi\rangle + Z|\psi\rangle)|0\rangle + (|\psi\rangle - Z|\psi\rangle)|1\rangle$
5. **Resulting State**: $\alpha|00\rangle + \beta|11\rangle$

---

## 3. Measurement Outcomes
When the ancilla is measured, the results correlate to the state of the target qubit (Qubit 0):

| Measurement Outcome | Probability | Post-measure State of Qubit 0 |
| :--- | :--- | :--- |
| **0** (Eigenvalue +1) | $|\alpha|^2$ | $|0\rangle$ |
| **1** (Eigenvalue -1) | $|\beta|^2$ | $|1\rangle$ |

### Key Inference
By measuring only the ancilla, we can infer the **syndrome** (whether an error occurred) while the circuit ensures the target qubit is collapsed onto the appropriate eigenstate of $M$.
