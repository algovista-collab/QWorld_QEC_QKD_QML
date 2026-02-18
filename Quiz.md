<img width="771" height="340" alt="image" src="https://github.com/user-attachments/assets/965994db-2d58-483d-bc84-446310f792ac" />

<img width="246" height="252" alt="image" src="https://github.com/user-attachments/assets/c3b50864-e3bd-4993-8a0c-f55680a9e3ff" />

## Hamming Distance Calculation

**Strings:**
- $s = 010111101$
- $t = 110111010$

**Calculation:**
Using the formula $d_H(\mathbf{s}, \mathbf{t}) = \sum_{i} \mathbf{s}_i + \mathbf{t}_i$ (where addition is XOR):
1. Position 1: $0 \oplus 1 = 1$
2. Position 7: $1 \oplus 0 = 1$
3. Position 8: $0 \oplus 1 = 1$
4. Position 9: $1 \oplus 0 = 1$

**Total Distance:** 4

<img width="381" height="235" alt="image" src="https://github.com/user-attachments/assets/2a11cf6e-9e68-4c76-858d-4fd35d12af36" />

n is the number of columns, n = 5 and k is the number of rows, k = 3

## Hamming Code Encoding (m = 1011)

| Bit Type | D1 | D2 | D3 | D4 | P1 | P2 | P3 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Value** | 1 | 0 | 1 | 1 | 0 | 0 | **0** |

**Final Codeword:** `1011000`

### Result
The last bit of the output is **0**.

### For the Hamming code, what is the error corresponding to the syndrome (0,1,1)? 

The syndrome 011 indicates 3 in binary hence it is [0 0 1 0 0 0 0]

## Binary Code Vector Analysis [n, k, d]

To find the number of vectors that exist in the total space but outside the code, we subtract the number of valid codewords from the total number of possible vectors in the n-bit space.

| Attribute | Formula |
| :--- | :--- |
| Total vectors in $n$-bit space | $2^n$ |
| Valid codewords in $k$-dimension | $2^k$ |
| **Vectors outside the code** | **$2^n - 2^k$** |

### Final Answer
The number of vectors existing outside the code is **$2^n - 2^k$**.

## Parity-Check Matrix H Calculation

Given the generator matrix $G = [I_k | P]$:

$$G = \begin{pmatrix} 1 & 0 & 0 & 1 & 1 \\ 0 & 1 & 0 & 1 & 0 \\ 0 & 0 & 1 & 1 & 1 \end{pmatrix}$$

The parity-check matrix $H$ is constructed as $[P^T | I_{n-k}]$:

| Matrix | Column 1 | Column 2 | Column 3 | Column 4 | Column 5 |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Row 1** | 1 | 1 | 1 | 1 | 0 |
| **Row 2** | 1 | 0 | 1 | 0 | 1 |

### Final Result
The parity-check matrix **H** is:
**[[1, 1, 1, 1, 0], [1, 0, 1, 0, 1]]**
