# Pauli Group

To understand **stabilizer codes**—the simplest class of quantum error-correcting codes—we must first establish the algebraic theory of the Pauli group.

## The Pauli Matrices

The fundamental building blocks are the three Pauli matrices along with the identity matrix: $\{I, X, Y, Z\}$. 

### Key Property
Multiplying any two Pauli matrices together results in another matrix from the same set, up to a phase factor of $\pm 1$ or $\pm i$. 

**Standard relations:**
* $XY = iZ$
* $YZ = iX$
* $ZX = iY$ (Note: The image contains a typo stating $ZX=iX$; standard cyclic permutation dictates $ZX=iY$)

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
