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

# The Pauli Group $\mathcal{P}_1$

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
