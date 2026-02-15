# Quantum Repetition Code (Bit-Flip Code)

The **Quantum Repetition Code** is the quantum analogue of the classical repetition code. It is designed to protect a quantum state from **bit-flip errors ($X$ gates)** by encoding a single logical qubit into multiple physical qubits.

---

## 1. The Problem: Noisy Channels
Alice wants to send a single-qubit state $|\psi\rangle$ to Bob:

$$|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$$

In a noisy channel, there is a probability $p$ that a bit-flip error ($X$) occurs.
* **Without encoding:** Bob has a probability $p$ of receiving the wrong state $X|\psi\rangle$.
* **The Challenge:** In quantum mechanics, Bob cannot simply "measure" the qubit to see if it flipped without collapsing the superposition.

---

## 2. Encoding Process
To protect the state, Alice encodes the **logical qubit** into three **physical qubits** (data qubits).

### Basis State Transformation
The encoding follows these rules for the basis states:
* $|0\rangle \rightarrow |000\rangle$
* $|1\rangle \rightarrow |111\rangle$

### General State Encoding
The arbitrary state $|\psi\rangle$ is transformed using two ancilla qubits initially in the $|0\rangle$ state:

$$|\psi\rangle_L = \alpha|000\rangle + \beta|111\rangle$$



> **Terminology:**
> * **Unencoded logical qubit:** The original state $|\psi\rangle$.
> * **Data qubits:** The three physical qubits used for the code.
> * **Encoded logical qubit:** The resulting state $|\psi\rangle_L$.

---

## 3. The Encoding Circuit
The transformation is achieved using **CNOT gates**. The original qubit acts as the control, and the two ancilla qubits act as targets.

<img width="287" height="163" alt="image" src="https://github.com/user-attachments/assets/db996c0f-0521-41af-a34b-1ef2b0ba4324" />

---

## 4. Error Model: The Bit-Flip Channel
We assume the channel applies the $X$ operator to each qubit independently with probability $p$.

**Possible outcomes for 3 qubits:**
1. **No error:** $|000\rangle$ (Prob: $(1-p)^3$)
2. **Single bit-flip:** e.g., $|100\rangle, |010\rangle, |001\rangle$ (Prob: $\approx 3p$)
3. **Multiple bit-flips:** e.g., $|110\rangle$ (Prob: $\approx 3p^2$)

By using three qubits, Bob can perform **syndrome measurements** to detect which qubit flipped and correct it, provided that no more than one qubit was affected.

# Decoding the Quantum Repetition Code

The decoding process consists of three distinct phases: **Error Detection**, **Error Correction**, and **State Recovery**.

---

## 1. Detecting the Error (Syndrome Measurement)
In quantum mechanics, Bob cannot simply measure the qubits to see their values, as this would collapse the superposition ($\alpha|0\rangle + \beta|1\rangle$) and destroy the information Alice sent.

### The Strategy: Partial Measurement
Instead of measuring the state itself, Bob performs a **syndrome measurement**. This determines if the qubits are different from one another without revealing what their actual values are.

* **Comparison 1:** Compare the 1st and 2nd qubits.
* **Comparison 2:** Compare the 2nd and 3rd qubits.

### Syndrome Table
The ancilla qubits used for this are called **syndrome qubits**. Based on their measurement, Bob can identify the error:

| Syndrome ($s_1 s_2$) | Error Type | Action to Correct |
| :--- | :--- | :--- |
| `00` | No Error | Do nothing ($I$) |
| `10` | Error on Qubit 1 ($X_1$) | Apply $X_1$ to Qubit 1 |
| `11` | Error on Qubit 2 ($X_2$) | Apply $X_2$ to Qubit 2 |
| `01` | Error on Qubit 3 ($X_3$) | Apply $X_3$ to Qubit 3 |

<img width="498" height="278" alt="image" src="https://github.com/user-attachments/assets/904829e6-edfa-43b3-b3cc-42c2016d5df1" />

---

## 2. Correcting the Error
Once the error is identified via the syndrome, Bob applies the inverse operation. In the case of bit-flips ($X$ gates), the operators are **self-inverse**:
$$X \cdot X = I$$

If Bob detects an error $X_1$, he simply applies $X_1$ to the first qubit to return the state to $|\psi\rangle_L$.

---

## 3. Decoding (State Recovery)
After the error is corrected, Bob has the state $|\psi\rangle_L = \alpha|000\rangle + \beta|111\rangle$. To recover the original single-qubit state $|\psi\rangle$, he must undo the encoding.

Since the encoding circuit (CNOTs) is also **self-inverse**, Bob passes the three qubits through the same circuit used for encoding:
1.  Apply CNOT from $Q_1$ to $Q_2$.
2.  Apply CNOT from $Q_1$ to $Q_3$.

**Result:** The first qubit returns to the state $\alpha|0\rangle + \beta|1\rangle$, and the other two qubits return to $|00\rangle$.

---

> ### Key Takeaway
> The quantum repetition code works because we measure the **relationship** between qubits (parity) rather than the **state** of the qubits themselves. This allows us to fix bit-flips while preserving quantum coherence.

# Linear Algebraic Analysis of the Quantum Repetition Code

To understand why the code works, we can analyze the **vector spaces** and how errors shift the state between them.

---

## 1. Hilbert Spaces and Subspaces
* **Original State:** The unencoded logical qubit lives in a **2-dimensional Hilbert space**.
* **Data Qubits:** The three physical qubits occupy a larger **$2^3$-dimensional Hilbert space**.
* **Codespace:** The process of encoding maps the original 2D space into a specific **2-dimensional subspace** of the physical qubits.

---

## 2. Error-Induced Subspace Shifts
Bit-flip errors ($X$) do not destroy the state; they move the encoded state from the original **quantum code subspace** into a different 2-dimensional subspace.

### Subspace Mapping Table
The following table shows how specific errors transform the basis of the codespace:

<img width="282" height="326" alt="image" src="https://github.com/user-attachments/assets/3257965c-0ff2-4628-9c4d-e2c88b48be3c" />

---

## 3. Why This Enables Correction
1. **Detection:** Error detection is simply the process of determining **which subspace** the current state is in.
2. **Non-Destructive:** Because each error moves the state to a unique, orthogonal subspace, identifying the subspace does not distort or collapse the actual state information ($\alpha$ and $\beta$).
3. **Correction:** Once the subspace is identified, error correction moves the state back to the original **quantum code subspace**.

> This notion of "subspaces" is the formal foundation used to define more complex quantum error-correction codes.

## 2. Correcting Continuous $X$ Rotation Errors
The bit-flip code is more versatile than it looks—it can correct continuous rotation errors, not just discrete bit-flips.

### The Rotation Operator
Consider a rotation about the $X$ axis on the $i$-th qubit:
$$R_x^{(i)}(\theta) = \cos \theta I - i \sin \theta X^{(i)}$$

When the encoded state $|\bar{\psi}\rangle$ passes through a channel with this rotation, the corrupted state becomes:
$$|\tilde{\psi}\rangle = \cos \theta |\bar{\psi}\rangle - i \sin \theta X^{(i)} |\bar{\psi}\rangle$$

### Measurement and Collapse
When this state runs through the error-detection circuit with two ancillas, the state before measurement is:
$$\cos \theta |\bar{\psi}\rangle |00\rangle - i \sin \theta X^{(i)} |\bar{\psi}\rangle |s\rangle$$

When Bob measures the ancilla qubits, the superposition **collapses** into one of two outcomes:

1. **Result `00`**: (Prob: $|\cos \theta|^2$) The state collapses back to the original $|\bar{\psi}\rangle$.
2. **Result `s`**: (Prob: $|\sin \theta|^2$) The state collapses to $X^{(i)} |\bar{\psi}\rangle$.

In both cases, the subsequent error-correction procedure (doing nothing for `00` or applying $X^{(i)}$ for `s`) will fix the state perfectly.

> **Conclusion**: Because of the linearity of quantum mechanics and the collapse mechanism of measurement, we can correct any error that is a linear combination of errors the code is designed to handle.

# 3. Quantum Repetition Code for Phase-Flips
While the previous code corrected $X$ errors, the **Phase-Flip Code** is designed specifically for $Z$ errors.

### Setup
If a channel applies the $Z$ operator with probability $p$, the state is distorted as:
$$|\tilde{\psi}\rangle = Z|\psi\rangle = Z(\alpha|0\rangle + \beta|1\rangle) = \alpha|0\rangle - \beta|1\rangle \text{}$$

### Encoding
To protect against phase-flips, we use a repetition code in the **Hadamard basis** ($|+\rangle, |-\rangle$):

* $|0\rangle \rightarrow |\bar{0}\rangle = |+++\rangle$
* $|1\rangle \rightarrow |\bar{1}\rangle = |---\rangle$

**Encoded Logical Qubit:**
$$|\psi\rangle |00\rangle \rightarrow |\bar{\psi}\rangle = \alpha|+++\rangle + \beta|---\rangle \text{}$$
