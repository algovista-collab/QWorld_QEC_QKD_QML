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
