<img width="687" height="442" alt="image" src="https://github.com/user-attachments/assets/1dfa2b20-ba0b-4b75-874b-dde9443a6f85" />

# Error Syndrome Calculation

To find the error syndrome $s$, we look at how an error vector interacts with the parity-check matrix $H$.

## 1. Define the Error Vector
The problem states that the **zeroth bit** is flipped. In coding theory, bits are indexed starting from $0$. For a $5$-bit code, a flip at the zeroth position corresponds to the error vector:

$$e = (1, 0, 0, 0, 0)$$

## 2. Calculate the Syndrome
The error syndrome $s$ is calculated by multiplying the parity-check matrix $H$ by the transpose of the error vector $e^T$:

$$s = H e^T$$

When you multiply a matrix by a vector that has a $1$ in the $i$-th position and $0$s elsewhere, the result is simply the **$i$-th column of the matrix**.

### Matrix Column Extraction
Looking at the matrix $H$:

$$H = \begin{pmatrix} \mathbf{1} & 1 & 1 & 0 & 0 \\ \mathbf{0} & 1 & 0 & 1 & 0 \\ \mathbf{1} & 0 & 0 & 0 & 1 \end{pmatrix}$$

The **zeroth column** (the first column) is:

$$\begin{pmatrix} 1 \\ 0 \\ 1 \end{pmatrix}$$

---

## Conclusion
The syndrome $s$ corresponds directly to the first column of the parity-check matrix.

**The correct answer is:**
$$(1, 0, 1)$$
