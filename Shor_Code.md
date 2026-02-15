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

$$Z_i \ket{\bar{\bar{0}}} = \frac{(\ket{000} - \ket{111})(\ket{000} + \ket{111})(\ket{000} + \ket{111})}{2\sqrt{2}} \tag{11}$$

$$Z_i \ket{\bar{\bar{1}}} = \frac{(\ket{000} + \ket{111})(\ket{000} - \ket{111})(\ket{000} - \ket{111})}{2\sqrt{2}} \tag{12}$$

---

### Outer Code Level Perspective
A clearer way to visualize this is at the **outer code level**, where the logical encoding is represented using the sign basis states $\ket{+}$ and $\ket{-}$:

$$\ket{\bar{\bar{0}}} = \ket{\bar{+}} \ket{\bar{+}} \ket{\bar{+}} \tag{13}$$
$$\ket{\bar{\bar{1}}} = \ket{\bar{-}} \ket{\bar{-}} \ket{\bar{-}} \tag{14}$$

When a $Z_i$ error occurs for $i = 0, 1, 2$, the effect on these outer states is:

$$Z_i \ket{\bar{\bar{0}}} = \ket{\bar{-}} \ket{\bar{+}} \ket{\bar{+}} \tag{15}$$
$$Z_i \ket{\bar{\bar{1}}} = \ket{\bar{+}} \ket{\bar{-}} \ket{\bar{-}} \tag{16}$$

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
