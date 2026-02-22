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
<img width="1058" height="303" alt="image" src="https://github.com/user-attachments/assets/1e1e2d15-d694-415e-a767-a3ac4d59ca0d" />

<img width="790" height="75" alt="image" src="https://github.com/user-attachments/assets/f437fe4e-807d-4559-a52d-aa2e473fe67c" />
