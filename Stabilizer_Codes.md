# Pauli Group

To understand **stabilizer codes**—the simplest class of quantum error-correcting codes—we must first establish the algebraic theory of the Pauli group.

## The Pauli Matrices

The fundamental building blocks are the three Pauli matrices along with the identity matrix: $\{I, X, Y, Z\}$. 

### Key Property
Multiplying any two Pauli matrices together results in another matrix from the same set, up to a phase factor of $\pm 1$ or $\pm i$. 

**Standard relations:**
* $XY = iZ$
* $YZ = iX$
* $ZX = iY$

---

## Task 1: Multiplication Table

The following table demonstrates the results of multiplying the Pauli operators (where the row is the first operator and the column is the second).

| $\times$ | $I$ | $X$ | $Y$ | $Z$ |
| :--- | :--- | :--- | :--- | :--- |
| **$I$** | $II = I$ | $IX = X$ | $IY = Y$ | $IZ = Z$ |
| **$X$** | $XI = X$ | $XX = I$ | $XY = iZ$ | $XZ = -iY$ |
| **$Y$** | $YI = Y$ | $YX = -iZ$ | $YY = I$ | $YZ = iX$ |
| **$Z$** | $ZI = Z$ | $ZX = iY$ | $ZY = -iX$ | $ZZ = I$ |

> **Note on Non-Commutativity:** > Unlike classical variables, these operators do not always commute. For example, $XY = iZ$ while $YX = -iZ$. This means $XY = -YX$, a property known as **anti-commutation**.

---

### Important identities for calculation:
1.  **Squares:** $X^2 = Y^2 = Z^2 = I$
2.  **Anti-commutation:** $\{A, B\} = AB + BA = 0$ for any two distinct Pauli matrices $\{X, Y, Z\}$.
3.  **Cyclic Order:** Follow the order $X \rightarrow Y \rightarrow Z \rightarrow X$. Moving forward gives $+i$, moving backward (anti-cyclic) gives $-i$.

## 1. The Pauli Group $\mathcal{P}_1$

The single-qubit Pauli group, denoted as $\mathcal{P}_1$, consists of 16 matrices formed by the four Pauli matrices $\{I, X, Y, Z\}$ and four possible phase factors $\{\pm 1, \pm i\}$.

$$\mathcal{P}_1 = \{ \pm I, \pm iI, \pm X, \pm iX, \pm Y, \pm iY, \pm Z, \pm iZ \}$$

## Group Axioms
The set $\mathcal{P}_1$ forms a mathematical **group** because it satisfies the following four axioms under the operation of matrix multiplication:

* **Closure:** Multiplying any two elements in $\mathcal{P}_1$ results in another element already within the set $\mathcal{P}_1$.
* **Associativity:** For any elements $P, Q, R \in \mathcal{P}_1$, the order of grouping doesn't matter: $(PQ)R = P(QR)$. This is a fundamental property of matrix multiplication.
* **Identity Element:** There exists an identity element $I$ such that for any $P \in \mathcal{P}_1$, $PI = IP = P$.
* **Inverses:** For every element in $\mathcal{P}_1$, there is an inverse element within the group that, when multiplied, results in the identity $I$.

---

## Task: Determining Inverses in $\mathcal{P}_1$

To find the inverse $P^{-1}$ of an element $P$, we look for the matrix that satisfies $P \cdot P^{-1} = I$. 


### Inverse Table for $\mathcal{P}_1$

| Element ($P$) | Inverse ($P^{-1}$) | Mathematical Proof |
| :--- | :--- | :--- |
| $\pm I$ | $\pm I$ | $(I)(I) = I$ and $(-I)(-I) = I$ |
| $\pm iI$ | $\mp iI$ | $(iI)(-iI) = -i^2 I = -(-1)I = I$ |
| $\pm X$ | $\pm X$ | $X$ is its own inverse ($X^2 = I$) |
| $\pm iX$ | $\mp iX$ | $(iX)(-iX) = -i^2 X^2 = -(-1)I = I$ |
| $\pm Y$ | $\pm Y$ | $Y$ is its own inverse ($Y^2 = I$) |
| $\pm iY$ | $\mp iY$ | $(iY)(-iY) = -i^2 Y^2 = -(-1)I = I$ |
| $\pm Z$ | $\pm Z$ | $Z$ is its own inverse ($Z^2 = I$) |
| $\pm iZ$ | $\mp iZ$ | $(iZ)(-iZ) = -i^2 Z^2 = -(-1)I = I$ |

### Key Takeaway
* **Hermitian elements** ($\pm I, \pm X, \pm Y, \pm Z$) are their own inverses.
* **Anti-Hermitian elements** (those with a factor of $\pm i$) require a sign flip for their inverse (e.g., the inverse of $iX$ is $-iX$).

---

## 2. The Two-Qubit Pauli Group ($\mathcal{P}_2$)

When considering two qubits, we use the **tensor product** ($\otimes$) of two Pauli matrices. In general for n Pauli matrices, $P = P^{(0)} \otimes P^{(1)} \otimes P^{(2)} ... \otimes P^{(n)}$

### Multiplication Rule
Given $P = P^{(0)} \otimes P^{(1)}$ and $Q = Q^{(0)} \otimes Q^{(1)}$, the product is calculated entry-wise:
$$PQ = (P^{(0)} \otimes P^{(1)})(Q^{(0)} \otimes Q^{(1)}) = P^{(0)}Q^{(0)} \otimes P^{(1)}Q^{(1)}$$

**Example Calculation:**
$$(X \otimes Z)(Y \otimes Z) = (XY \otimes ZZ) = (iZ) \otimes I = i(Z \otimes I)$$

### Size of the Group
The total number of elements in $\mathcal{P}_2$ is **64**.
* There are 4 choices for the first matrix $P^{(0)}$.
* There are 4 choices for the second matrix $P^{(1)}$.
* There are 4 possible phase factors ($\pm 1, \pm i$).
* **Total:** $4^2 \times 4 = 4^{2+1} = 64$ elements. In generak, $4^{n+1}$ elements in ($\mathcal{P}_n$)

### Group Axioms for $\mathcal{P}_2$
* **Closure:** Multiplying any of the 64 elements results in another element of the set.
* **Associativity:** Multiplication remains associative.
* **Identity:** The identity element is $I \otimes I$.
* **Inverses:** The inverse of $P^{(0)} \otimes P^{(1)}$ is $P^{(0)^{-1}} \otimes P^{(1)^{-1}}$.

---

## Task: Determine the inverse of $i(X \otimes Y)$

To find the inverse of $i(X \otimes Y)$, we need an element that results in the identity $I \otimes I$.

1.  **Phase:** The inverse of $i$ is $-i$.
2.  **Matrices:** Both $X$ and $Y$ are their own inverses ($X^2=I, Y^2=I$).
3.  **Result:** The inverse is **$-i(X \otimes Y)$**.

**Verification:**
$$[i(X \otimes Y)] \cdot [-i(X \otimes Y)] = -(i^2)(X^2 \otimes Y^2) = -(-1)(I \otimes I) = I \otimes I$$

## 3. Subgroups of the Pauli Group
A **subgroup** is a subset of a group that itself satisfies all four group axioms.

### Finite Subgroup Theorem
For finite groups like $\mathcal{P}_n$, a non-empty subset $\mathcal{H}$ is a subgroup if and only if it is **closed under multiplication** and if it holds so will the other three axioms.
* If $A, B \in \mathcal{H}$ implies $AB \in \mathcal{H}$, then $\mathcal{H}$ is a subgroup.

### Worked Examples
| Subset | Subgroup? | Reasoning |
| :--- | :--- | :--- |
| $\{I, X\} \subset \mathcal{P}_1$ | **Yes** | Closed: $X \cdot X = I$. |
| $\{I, iX\} \subset \mathcal{P}_1$ | **No** | Not closed: $(iX)(iX) = -I$, which is not in the set. |
| $G = \{I I I, X I I, I Z I, X Z I\}$ | **Yes** | All elements commute and products stay within the set. |

---

## Summary of $\mathcal{H} \subset \mathcal{P}_3$
A common subgroup structure in 3-qubit systems involves fixing one position:
$$\mathcal{H} = \{Q \in \mathcal{P}_3 \text{ s.t. } Q = P^{(0)} \otimes I \otimes P^{(2)}\}$$
* **Identity:** $I \otimes I \otimes I$ is included.
* **Inverses:** If $Q$ is in the set, $Q^{-1}$ is also of the form $P^{(0)^{-1}} \otimes I \otimes P^{(2)^{-1}}$.

# Stabilizer States

The eigenvalues and eigenvectors of Pauli matrices are:

$$
\begin{aligned}
X|+\rangle &= |+\rangle, & X|-\rangle &= -|-\rangle, \\
Y|+i\rangle &= |+i\rangle, & Y|-i\rangle &= -|-i\rangle, \\
Z|0\rangle &= |0\rangle, & Z|1\rangle &= -|1\rangle.
\end{aligned}
$$

## Stabilizer states

For the states on the left, acting on the state by the corresponding operator leaves the state unchanged (even accounting for a phase).

> Given $P \in \mathcal{P}_n$, a state $|\psi\rangle$ such that $P|\psi\rangle = |\psi\rangle$, is called a **stabilizer state** of $P$.

**Example:** Consider the Pauli operator $X \otimes Z$. From the equations above, it is quite easy to see that

$$(X \otimes Z) |+\rangle \otimes |0\rangle = |+\rangle \otimes |0\rangle.$$

The tensor product of the $+1$ eigenvectors of the individual operators gives a $+1$ eigenvector of the tensor-product operator. 

$$(X \otimes Z) |-\rangle \otimes |1\rangle = (-1)^2 |-\rangle \otimes |1\rangle = |-\rangle \otimes |1\rangle.$$

Any linear combination of these 2 states is also a +1 eigenstate of $$(X \otimes Z)$$

<img width="790" height="75" alt="image" src="https://github.com/user-attachments/assets/f437fe4e-807d-4559-a52d-aa2e473fe67c" />

<img width="1058" height="303" alt="image" src="https://github.com/user-attachments/assets/1e1e2d15-d694-415e-a767-a3ac4d59ca0d" />

# Task 1: Stabilizer States Basis

To find the basis of the stabilizer states for an operator $S$, we identify the states $|\psi\rangle$ such that $S|\psi\rangle = +1|\psi\rangle$. 

---

## 1. $X \otimes X \otimes X$
The product of three $X$ eigenvalues must be $+1$. This occurs if there are 0 or 2 negative eigenvalues.

* **0 negatives:** $|+\rangle|+\rangle|+\rangle$
* **2 negatives:** * $|+\rangle|-\rangle|-\rangle$
  * $|-\rangle|+\rangle|-\rangle$
  * $|-\rangle|-\rangle|+\rangle$

**Basis:** $\{|+++\rangle, |+--\rangle, |-+-\rangle, |--+\rangle\}$

---

## 2. $Z \otimes Z \otimes Z$
The product of three $Z$ eigenvalues must be $+1$. This occurs if there are 0 or 2 negative eigenvalues ($|1\rangle$ states).

* **0 negatives:** $|0\rangle|0\rangle|0\rangle$
* **2 negatives:** * $|0\rangle|1\rangle|1\rangle$
  * $|1\rangle|0\rangle|1\rangle$
  * $|1\rangle|1\rangle|0\rangle$

**Basis:** $\{|000\rangle, |011\rangle, |101\rangle, |110\rangle\}$

---

## 3. $Z \otimes I \otimes X$
The identity ($I$) always contributes $+1$. We only need the product of $Z$ (qubit 1) and $X$ (qubit 3) to be $+1$.
* **(+1, +1):** $|0\rangle|0\rangle|+\rangle$ and $|0\rangle|1\rangle|+\rangle$
* **(-1, -1):** $|1\rangle|0\rangle|-\rangle$ and $|1\rangle|1\rangle|-\rangle$

**Basis:** $\{|00+\rangle, |01+\rangle, |10-\rangle, |11-\rangle\}$

---

## 4. $Z \otimes X \otimes X \otimes Z$
The product of all four eigenvalues must be $+1$. This happens with 0, 2, or 4 negative eigenvalues.

**Basis:** $\{|0++0\rangle, |0+-1\rangle, |0-+1\rangle, |0--0\rangle, |1++1\rangle, |1+-0\rangle, |1-+0\rangle, |1--1\rangle\}$

# Stabilizer States of Multiple Operators

It is possible to construct quantum states that are **simultaneously stabilized** by multiple Pauli operators. This means the state $|\psi\rangle$ satisfies $P_i|\psi\rangle = +1|\psi\rangle$ for all operators $P_i$ in a given set.

## Example: Simultaneous Stabilizers
Consider the operators $P_1 = Z \otimes Z \otimes I$ and $P_2 = Z \otimes I \otimes Z$ in the Pauli group $\mathcal{P}_3$:

* **$Z \otimes Z \otimes I$** stabilizes: $\{|000\rangle, |001\rangle, |110\rangle, |111\rangle\}$.
* **$Z \otimes I \otimes Z$** stabilizes: $\{|000\rangle, |010\rangle, |101\rangle, |111\rangle\}$.

The **intersection** of these sets is $\{|000\rangle, |111\rangle\}$. Any linear combination of these two states is stabilized by both $P_1$ and $P_2$. These states also form the basis for the quantum repetition code for bit-flip errors.

---

## Task 2: Simultaneous Stabilizers for $X$ Operators
**Goal:** Determine the states simultaneously stabilized by $P_1 = X \otimes X \otimes I$ and $P_2 = X \otimes I \otimes X$.

Using the property that $X|+\rangle = |+\rangle$ and $X|-\rangle = -|-\rangle$:
* **States stabilized by $P_1$ ($X_0 X_1$):** $\{|++0\rangle, |++1\rangle, |--0\rangle, |--1\rangle\}$ (or any middle state in the $X$ basis: $\{|+++\rangle, |++-\rangle, |--+\rangle, |--- \rangle\}$).
* **States stabilized by $P_2$ ($X_0 X_2$):** $\{| +0+\rangle, | +1+\rangle, | -0-\rangle, | -1-\rangle\}$ (or in $X$ basis: $\{|+++\rangle, |+-+\rangle, |-++\rangle, |--- \rangle\}$).

**Simultaneous Stabilizer Basis:** $$\{|+++\rangle, |--- \rangle\}$$

> **Note:** These are the basis states for the phase-flip repetition code.

---

# Task 3: Commutation and Simultaneous Stabilizers

To determine if two Pauli operators $P_1$ and $P_2$ share simultaneously stabilized states ($+1$ eigenstates), we check if they **commute**. $P_1$ and $P_2$ are Pauli operators of $P_3$. $P_1 = Z_0 Z_1$ means $P_1 = Z \otimes Z \otimes I$ and $P_2 = X_0 X_1$ means $P_2 = X \otimes X \otimes I$   

## The General Rule
* If $[P_1, P_2] = 0$ (**Commute**): They share a simultaneous eigenbasis.
* If $\{P_1, P_2\} = 0$ (**Anti-commute**): They cannot share a simultaneous $+1$ eigenstate.

To check commutation for tensor products, count the number of qubit positions where the individual Pauli operators anti-commute ($X$ vs $Z$). 
* **Even number** of anti-commutations $\rightarrow$ Operators **Commute**.
* **Odd number** of anti-commutations $\rightarrow$ Operators **Anti-commute**.

---

## 1. Do $P_1 = Z_0 Z_1$ and $P_2 = X_0 X_1$ have simultaneous states?

**Analysis:**
* **Qubit 0:** $Z$ and $X$ anti-commute (1).
* **Qubit 1:** $Z$ and $X$ anti-commute (2).

Since there are **two** (an even number) anti-commuting pairs, the overall operators commute:
$$(Z_0 Z_1)(X_0 X_1) = (-1)(-1)(X_0 X_1)(Z_0 Z_1) = (X_0 X_1)(Z_0 Z_1)$$

1. **Check $Z_0 Z_1$**: 
   - $Z_0 Z_1 |00\rangle = (+1)(+1)|00\rangle = |00\rangle$
   - $Z_0 Z_1 |11\rangle = (-1)(-1)|11\rangle = |11\rangle$
   - Total: $Z_0 Z_1 |\phi^+\rangle = +1|\phi^+\rangle$

2. **Check $X_0 X_1$**:
   - $X_0 X_1 |00\rangle = |11\rangle$
   - $X_0 X_1 |11\rangle = |00\rangle$
   - Total: $X_0 X_1 |\phi^+\rangle = \frac{1}{\sqrt{2}}(|11\rangle + |00\rangle) = +1|\phi^+\rangle$

**Result:** **Yes.** They share simultaneous stabilized states, such as the Bell state $|\phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}}$.

---

## 2. What about $P_1 = Z_0 Z_1$ and $P_2 = X_0 X_2$?

**Analysis:**
* **Qubit 0:** $Z$ and $X$ anti-commute (1).
* **Qubit 1:** $Z$ (from $P_1$) and $I$ (from $P_2$) commute.
* **Qubit 2:** $I$ (from $P_1$) and $X$ (from $P_2$) commute.

Since there is only **one** (an odd number) anti-commuting pair at index 0, the operators anti-commute:
$$(Z_0 Z_1 I_2)(X_0 I_1 X_2) = -(X_0 I_1 X_2)(Z_0 Z_1 I_2)$$

**Result:** **No.** They cannot have simultaneously stabilized states.

# Stabilizer Groups

Among the many subgroups of the Pauli group $\mathcal{P}_n$, there is a particular type of subgroup called the **stabilizer group** $S$. 

In addition to the standard group axioms, a stabilizer group must satisfy two specific properties:

### Core Properties
* **Commutativity**: All pairs of elements in $S$ commute. 
    * For every $P, Q \in S$, we have $PQ = QP$.
* **Consistency**: The negative identity $-I$ is **not** a part of $S$.
    * $-I = -I_0 \otimes I_1 \otimes \cdots \otimes I_{n-1} \notin S$.

---

### Example: $n=3$ Qubits
To build intuition, consider the following subset $S$ of the Pauli group $\mathcal{P}_3$:

#### Group Elements
The subset $S = \{P_0, P_1, P_2, P_3\}$ is defined by:

1.  **$P_0 = I \otimes I \otimes I$** (The identity)
2.  **$P_1 = Z \otimes Z \otimes I$**
3.  **$P_2 = Z \otimes I \otimes Z$**
4.  **$P_3 = I \otimes Z \otimes Z$**

#### Observations
* **Closure**: Notice that $P_1 \cdot P_2 = (Z \cdot Z) \otimes (Z \cdot I) \otimes (I \cdot Z) = I \otimes Z \otimes Z = P_3$.
* **Commutativity**: Since all elements are composed of $I$ and $Z$ operators (and $[Z, I] = 0$), all elements in this specific $S$ commute.
* **No $-I$**: All products of these operators result in coefficients of $+1$, never $-1$.

> **Note:** These properties are useful for defining a "protected" subspace of quantum states that are simultaneous $+1$ eigenstates of all operators in $S$.

### Task 1: Multiplication Table for Subgroup $S$

The following table demonstrates that the subset $S = \{I, P_1, P_2, P_3\}$ is closed under multiplication, confirming it is a subgroup of $\mathcal{P}_3$.

| $\times$ | $I$   | $P_1$ | $P_2$ | $P_3$ |
| :---:    | :---: | :---: | :---: | :---: |
| **$I$** | $I$   | $P_1$ | $P_2$ | $P_3$ |
| **$P_1$**| $P_1$ | $I$   | $P_3$ | $P_2$ |
| **$P_2$**| $P_2$ | $P_3$ | $I$   | $P_1$ |
| **$P_3$**| $P_3$ | $P_2$ | $P_1$ | $I$   |

**Verification of Closure:**
* $P_1 P_2 = (Z \otimes Z \otimes I)(Z \otimes I \otimes Z) = I \otimes Z \otimes Z = P_3$
* $P_1 P_3 = (Z \otimes Z \otimes I)(I \otimes Z \otimes Z) = Z \otimes I \otimes Z = P_2$
* $P_2 P_3 = (Z \otimes I \otimes Z)(I \otimes Z \otimes Z) = Z \otimes Z \otimes I = P_1$

# Generators of a Group

Generators are a subset of elements used to construct every other element within the group. They are not unique.

### Example: $P_1$ and $P_2$ as Generators
In the subgroup $S$ defined above, every element can be expressed as a product of $P_1$ and $P_2$:
* **Identity**: $I = P_1^2 = P_2^2$
* **$P_1$**: $Z \otimes Z \otimes I = P_1$
* **$P_2$**: $I \otimes Z \otimes Z = P_2$
* **$P_3$**: $Z \otimes I \otimes Z = P_1 P_2$

### Example: $P_1$ and $P_3$ as Generators
We can also generate the group using $\{P_1, P_3\}$:
* **Identity**: $I = P_1^2 = P_3^2$
* **$P_1$**: $P_1$
* **$P_2$**: $P_1 P_3$
* **$P_3$**: $P_3$

### Advantages of Generators
Using generators reduces the storage space required to specify a group because of two properties:
1.  **Commutativity**: All elements in a stabilizer group commute.
2.  **Pauli Property**: For all Pauli operators, $P^2$ is either $I$ or $-I$.

For a stabilizer group $S$ with generators $\{g_i\}_{i=0}^{m-1}$, any element $h \in S$ can be written as a product of these generators.

> **Lemma**: If a stabilizer group $S$ has $m$ independent generators, it contains $2^m$ elements.

---

### States Stabilized by Stabilizer Groups
A stabilizer group defines a set of operators, and we seek the quantum states simultaneously stabilized (having a $+1$ eigenvalue) by all of them.

**Example Analysis for $S = \{I, Z \otimes Z \otimes I, Z \otimes I \otimes Z, I \otimes Z \otimes Z\}$:**
* **$I$**: Stabilizes all states.
* **$Z \otimes Z \otimes I$** and **$I \otimes Z \otimes Z$**: Jointly stabilize $\{|000\rangle, |111\rangle\}$.
* **$Z \otimes I \otimes Z$**: Has stabilizer states $\{|000\rangle, |010\rangle, |101\rangle, |111\rangle\}$.
* **Common Intersection**: The common state stabilized by all elements is $\{|000\rangle, |111\rangle\}$. The group S stabilizes these states and any of their linear combinations.

#### Generators are Sufficient
We only need to determine states stabilized by the **generators** to find the states stabilized by the entire group:
* If $P_1$ and $P_2$ stabilize $|\psi\rangle$, then any product of them (any $h \in S$) also stabilizes $|\psi\rangle$.
* If a generator does not stabilize a state, that state cannot be in the final set.
* **Lemma**: The basis of states stabilized by $S$ is the intersection of states stabilized by its generators.

Stabilizer codes are defined with respect to a stabilizer group $S$. We label the stabilizer code based on $S$ as $C(S)$. Then, the ingredients for $C(S)$ are

* **Encoding:** The simultaneous stabilizer states of the stabilizer group $S$ define the basis elements of the codespace. An encoding circuit can be systematically constructed using the generators of $S$;
* **Syndrome measurements:** All generators of $S$ are measured one by one. The results of the measurement indicate which error has occurred.
* **Decoding:** Decoding is also based on $S$.
* **Logical gates:** These can also be constructed from $S$.

# Phase-Flip Repetition Code as a Stabilizer Code

To show that the 3-qubit phase-flip repetition code is a stabilizer code, we define its code space and the corresponding stabilizer group $S$ that fixes every state within that space.

## 1. The Code Space
The phase-flip code protects against $Z$ errors by encoding logical states into the Hadamard (sign) basis. Let:
- $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$
- $|-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$

The logical codewords are:
- $|0_L\rangle = |+++\rangle$
- $|1_L\rangle = |---\rangle$

Any state in the code space is a linear combination $|\psi_L\rangle = \alpha|+++\rangle + \beta|---\rangle$.

## 2. Defining the Stabilizer Group $S$
A stabilizer $M$ must satisfy $M|\psi_L\rangle = |\psi_L\rangle$. For the phase-flip code, the stabilizers check for consistency in the $X$ basis. The generators of the stabilizer group $S$ are:

1. $M_1 = X \otimes X \otimes I$ (or $X_1 X_2$)
2. $M_2 = I \otimes X \otimes X$ (or $X_2 X_3$)

## 3. Verification of Stability
We verify that these generators stabilize the basis states:

### For $|0_L\rangle = |+++\rangle$:
- $M_1 |+++\rangle = (X|+\rangle)(X|+\rangle)|+\rangle = |+++\rangle$ (Since $X|+\rangle = |+\rangle$)
- $M_2 |+++\rangle = |+\rangle(X|+\rangle)(X|+\rangle) = |+++\rangle$

### For $|1_L\rangle = |---\rangle$:
- $M_1 |---\rangle = (X|-\rangle)(X|-\rangle)|-\rangle = (-|-\rangle)(-\|-\rangle)|-\rangle = |---\rangle$ (Since $X|-\rangle = -|-\rangle$, and $(-1)^2 = 1$)
- $M_2 |---\rangle = |-\rangle(X|-\rangle)(X|-\rangle) = |---\rangle$

Because $M_1$ and $M_2$ stabilize both basis states, they stabilize the entire code space.

## 4. Error Detection (Syndrome Measurements)
Phase-flip errors ($Z$) anti-commute with $X$ operators ($ZX = -XZ$). We detect errors by measuring the eigenvalues of the stabilizers:

| Error Type | $M_1$ ($X_1X_2$) | $M_2$ ($X_2X_3$) | Interpretation |
| :--- | :---: | :---: | :--- |
| **No Error** | $+1$ | $+1$ | No error detected |
| **$Z_1$** (on Qubit 1) | $-1$ | $+1$ | $Z_1$ flips the sign of $M_1$ |
| **$Z_2$** (on Qubit 2) | $-1$ | $-1$ | $Z_2$ flips the sign of both $M_1$ and $M_2$ |
| **$Z_3$** (on Qubit 3) | $+1$ | $-1$ | $Z_3$ flips the sign of $M_2$ |

---

## Summary
The phase-flip repetition code is a stabilizer code where the code space is the simultaneous $+1$ eigenspace of the group $S = \langle X_1X_2, X_2X_3 \rangle$. This matches the "ingredients" of a stabilizer code:
- **Encoding:** Defined by the $+1$ eigenspace of $S$.
- **Syndromes:** Measured via the generators of $S$.

# Theorem: Error Detection in Stabilizer Codes

**Theorem:** Let $S$ be a stabilizer group, with generators $\{g_i\}$, and let $C(S)$ be the associated stabilizer code. Then $C(S)$ detects an error $E \in \mathcal{P}_n$ if it anti-commutes with at least one generator $g_k$ of $S$.

---

## Overview

To detect errors, we measure all the generators of the stabilizer group. Let's concentrate on the measurement of just one generator $g_k$. There are three possibilities:

1.  **No error has occurred** and the data qubits are in state $|\bar{\psi}\rangle$.
2.  **Error $E$ has occurred**, and the data qubits are in state $E|\bar{\psi}\rangle$. The two sub-possibilities are:
    * **(a)** $E$ commutes with $g_k$.
    * **(b)** $E$ anti-commutes with $g_k$.

## Syndrome Measurements: Mathematical Framework

If we measure $g_k$ on some state $|\phi\rangle$, then at the end of the measurement circuit, the qubits will be in state:

$$|\Phi\rangle = (|\phi\rangle + g_k|\phi\rangle) |0\rangle + (|\phi\rangle - g_k|\phi\rangle) |1\rangle$$

Where $|\phi\rangle$ is either the uncorrupted state $|\bar{\psi}\rangle$ or the corrupted state $E|\bar{\psi}\rangle$.

---

## Analysis of Possibilities

### Possibility 1: No error
If there is no error, then $|\phi\rangle = |\bar{\psi}\rangle$. Since $g_k$ is a generator of the stabilizer for this state, $g_k|\bar{\psi}\rangle = |\bar{\psi}\rangle$. 

Substituting this into Equation above:
* The $|1\rangle$ component vanishes: $(|\bar{\psi}\rangle - |\bar{\psi}\rangle) = 0$.
* The state becomes $|\Phi\rangle = |\bar{\psi}\rangle |0\rangle$.

**Result:** Measuring the ancilla will yield **0**.

### Possibility 2 (a): $E$ commutes with $g_k$
Now, $|\phi\rangle = E|\bar{\psi}\rangle$. If $E$ and $g_k$ commute ($g_k E = E g_k$):

$$g_k |\phi\rangle = g_k (E |\bar{\psi}\rangle) = E g_k |\bar{\psi}\rangle = E |\bar{\psi}\rangle = |\phi\rangle$$

**Result:** Since $g_k |\phi\rangle = |\phi\rangle$, we again find that $|\Phi\rangle = |\phi\rangle |0\rangle$. The ancilla will measure **0**, and the error is **not detected** by this generator.

## Possibility 2 (b) $E$ anti-commutes with $g_k$

Now, $|\phi\rangle = E |\bar{\psi}\rangle$. First note that

$$g |\phi\rangle = g(E |\bar{\psi}\rangle) = -Eg |\bar{\psi}\rangle = -(E |\bar{\psi}\rangle) = - |\phi\rangle.$$

Hence, $|\Phi\rangle = |\phi\rangle |1\rangle$. This time, the ancilla will measure 1.

What we have discovered is that if and only if $E$ and $g_k$ anti-commute does the ancilla trigger. Therefore, it is quite easy to see that if at least one of $E$ and $\{g_i\}$ anti-commute, the code will detect the error.

### A fourth possibility

Note, that any code is also immune to errors that don't change the state. For instance, for the Shor code, we saw that errors like $Z_0 Z_1$ leave $|\bar{\psi}\rangle$ unchanged.

---

## Task 2

Determine the commutation relations of single-qubit $X$ errors with the generators of the repetition code for bit-flips. Put 0 in the table if they commute, and 1 if they anti-commute.

| Error \ Generator | $Z \otimes Z \otimes I$ | $I \otimes Z \otimes Z$ |
| :--- | :---: | :---: |
| $I \otimes I \otimes I$ | 0 | 0 |
| $X \otimes I \otimes I$ | 1 | 0 |
| $I \otimes X \otimes I$ | 1 | 1 |
| $I \otimes I \otimes X$ | 0 | 1 |

## Task 3

Determine the commutation relations of single-qubit $Z$ errors with the generators of the repetition code for phase-flips. Put 0 in the table if they commute, and 1 if they anti-commute.

| Error \ Generator | $X \otimes X \otimes I$ | $I \otimes X \otimes X$ |
| :--- | :---: | :---: |
| $I \otimes I \otimes I$ | 0 | 0 |
| $Z \otimes I \otimes I$ | 1 | 0 |
| $I \otimes Z \otimes I$ | 1 | 1 |
| $I \otimes I \otimes Z$ | 0 | 1 |

<img width="1122" height="648" alt="image" src="https://github.com/user-attachments/assets/4a2e4bf9-cc76-4b1c-a433-5b34c218171d" />

<img width="1150" height="592" alt="image" src="https://github.com/user-attachments/assets/ef300794-2803-4c8d-a3e3-962b4dd267bd" />

