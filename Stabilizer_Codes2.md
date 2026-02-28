# Vector form of Pauli operators and the generator matrix

It is getting quite tedious to write down multi-qubit Pauli operators like $X \otimes X \otimes I$. Here we are going to discuss a compact form for Pauli operators, that will make it very easy to do computations.

## Vector form of Pauli operators

We are going to map operators in the Pauli group $\mathcal{P}_n$ to binary vectors of length $2n$.

The elements of $\mathcal{P}_1$ are:

| Operator | Vector $(a|b)$ | Equation |
| :--- | :--- | :--- |
| $I$ | $(0 \mid 0)$ | (1) |
| $X$ | $(1 \mid 0)$ | (2) |
| $Y$ | $(1 \mid 1)$ | (3) |
| $Z$ | $(0 \mid 1)$ | (4) |

---

For larger $n$ we have a similar formula of operators getting mapped to the vector $v = (a|b)$ where:

* **$a$** is the "$X$ part", and is of length $n$.
* **$b$** is the "$Z$ part", and is also of length $n$.

Let $P = \bigotimes_i P_i$, where $P_i \in \{I_i, X_i, Y_i, Z_i\}$. Then, the mapping for each index $i$ is:

$$
\begin{aligned}
a_i = 0, b_i = 0 & \quad \text{if } P_i = I_i, & (5) \\
a_i = 1, b_i = 0 & \quad \text{if } P_i = X_i, & (6) \\
a_i = 1, b_i = 1 & \quad \text{if } P_i = Y_i, & (7) \\
a_i = 0, b_i = 1 & \quad \text{if } P_i = Z_i. & (8)
\end{aligned}
$$
