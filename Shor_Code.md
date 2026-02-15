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
