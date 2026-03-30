<img width="428" height="562" alt="image" src="https://github.com/user-attachments/assets/80a9b11e-005c-44c3-9ffc-277834f82356" />

## Finding the Generator Matrix

**Setup:** H is a 3×5 parity-check matrix, so this is an [n,k] = [5,2] code (k = 5−3 = 2).

### Step 1: Row-reduce H over GF(2)

$$H = \begin{pmatrix}1&1&1&0&0\\0&1&0&1&0\\1&0&0&0&1\end{pmatrix}$$

R₃ → R₃ + R₁, then R₃ → R₃ + R₂:

$$\begin{pmatrix}1&1&1&0&0\\0&1&0&1&0\\0&0&1&1&1\end{pmatrix}$$

Back-substitute to get RREF:

$$\begin{pmatrix}1&0&0&0&1\\0&1&0&1&0\\0&0&1&1&1\end{pmatrix}$$

### Step 2: Identify free variables

Pivot columns: {1, 2, 3} → Free variables: **x₄, x₅**

From RREF:
- x₁ = x₅
- x₂ = x₄
- x₃ = x₄ + x₅

### Step 3: Build the generator matrix

| x₄ | x₅ | Codeword         |
|----|----|------------------|
| 1  | 0  | (0, 1, 1, 1, 0)  |
| 0  | 1  | (1, 0, 1, 0, 1)  |

$$\boxed{G = \begin{pmatrix}1&0&1&0&1\\0&1&1&1&0\end{pmatrix}}$$

This is the **last option**. You can verify: every row of G satisfies HGᵀ = 0 over GF(2). ✓

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

<img width="890" height="226" alt="image" src="https://github.com/user-attachments/assets/093b8af3-77d2-481a-bf8a-921032295c28" />

# Quantum Repetition Code: Logical $\bar{Y}$ Operator

To determine if $A = X \otimes Y \otimes X$ is the logical $\bar{Y}$ for a 3-qubit bit-flip repetition code ($n=3$), we evaluate its action on the defined code space.

## 1. Define the Code Space
The bit-flip repetition code protects against $X$ errors by encoding one logical qubit into three physical qubits. The logical basis states are:
* $|\bar{0}\rangle = |000\rangle$
* $|\bar{1}\rangle = |111\rangle$

## 2. Theoretical Action of Pauli $Y$
A standard Pauli $Y$ operator acts on a single qubit basis as follows:
* $Y|0\rangle = i|1\rangle$
* $Y|1\rangle = -i|0\rangle$

For $A$ to be a valid **logical** $\bar{Y}$, it must satisfy these same relations relative to the logical basis:
1. $A|\bar{0}\rangle = i|\bar{1}\rangle$
2. $A|\bar{1}\rangle = -i|\bar{0}\rangle$

## 3. Testing the Operator $A = X \otimes Y \otimes X$

### Action on $|\bar{0}\rangle$:
$$
\begin{aligned}
A|\bar{0}\rangle &= (X \otimes Y \otimes X)|000\rangle \\
&= X|0\rangle \otimes Y|0\rangle \otimes X|0\rangle \\
&= |1\rangle \otimes i|1\rangle \otimes |1\rangle \\
&= i|111\rangle = i|\bar{1}\rangle
\end{aligned}
$$

### Action on $|\bar{1}\rangle$:
$$
\begin{aligned}
A|\bar{1}\rangle &= (X \otimes Y \otimes X)|111\rangle \\
&= X|1\rangle \otimes Y|1\rangle \otimes X|1\rangle \\
&= |0\rangle \otimes (-i|0\rangle) \otimes |0\rangle \\
&= -i|000\rangle = -i|\bar{0}\rangle
\end{aligned}
$$

---

## Conclusion
The operator $A = X \otimes Y \otimes X$ maps the code space to itself and mimics the exact algebraic behavior of a $Y$ gate on the logical qubits.

**The statement is: True**

<img width="872" height="318" alt="image" src="https://github.com/user-attachments/assets/066902a4-9000-4295-9899-2613c95235e4" />

# Shor Code: Error Syndromes and Degeneracy

In the 9-qubit Shor code, multiple physical errors can result in the same syndrome. This property is known as **degeneracy**. To understand why $Z_3$ and $Z_5$ yield the same syndrome, we look at the structure of the code.

## 1. The Structure of the Shor Code
The Shor code encodes one logical qubit into nine physical qubits arranged in three blocks:
* **Block 0:** Qubits $\{0, 1, 2\}$
* **Block 1:** Qubits $\{3, 4, 5\}$
* **Block 2:** Qubits $\{6, 7, 8\}$



## 2. Phase-Flip ($Z$) Protection
The Shor code protects against phase-flip errors by comparing the collective phase of the blocks. The stabilizers responsible for detecting $Z$ errors are:
* $S_1 = X_0 X_1 X_2 X_3 X_4 X_5 I_6 I_7 I_8$ (Compares Block 0 and Block 1)
* $S_2 = I_0 I_1 I_2 X_3 X_4 X_5 X_6 X_7 X_8$ (Compares Block 1 and Block 2)

### How $Z$ Errors Interact with Stabilizers
A $Z$ error on any qubit $i$ is detected if it **anti-commutes** with a stabilizer. Since $Z$ and $X$ anti-commute ($ZX = -XZ$):
* A $Z$ error on **any** qubit in Block 1 ($\{3, 4, 5\}$) will anti-commute with both $S_1$ and $S_2$ because both stabilizers contain $X$ operators on those specific qubit positions.
* Therefore, $Z_3$, $Z_4$, and $Z_5$ all produce the exact same syndrome: they trigger both phase-check stabilizers.

## 3. Evaluating the Options
* **$Z_3$ and $Z_7$:** $Z_3$ is in Block 1 (triggers $S_1, S_2$), while $Z_7$ is in Block 2 (triggers only $S_2$). Their syndromes are different.
* **$X_0$ and $X_1 X_2$:** These are bit-flip errors. They trigger the $ZZ$ stabilizers within Block 0. $X_0$ triggers $Z_0 Z_1$, while $X_1 X_2$ triggers $Z_0 Z_1$ and $Z_1 Z_2$ (the $X_1$ flips the syndrome of both, but $X_2$ flips it back for the second). They do not match.
* **$Z_3$ and $Z_5$:** Both are in Block 1. Both anti-commute with the same $X$-based stabilizers. **They yield the same syndrome.**
* **$X_0$ and $X_0 X_1 X_2$:** $X_0$ is a detectable error. $X_0 X_1 X_2$ is the **logical $\bar{X}$** operator for the first block; it commutes with all stabilizers and yields a "null" syndrome (all zeros).

---

## Conclusion
Because the Shor code treats each block of three qubits as a single logical unit for phase protection, a $Z$ error on any qubit within the same block is indistinguishable from a $Z$ error on another qubit in that same block.

**The correct answer is:**
**$Z_3$ and $Z_5$**

<img width="636" height="298" alt="image" src="https://github.com/user-attachments/assets/940cd155-a369-4e0f-9b05-71dbd46a8289" />

<img width="497" height="328" alt="image" src="https://github.com/user-attachments/assets/808721d0-e83a-4a38-8b5f-e677e8bd6feb" />

<img width="713" height="345" alt="image" src="https://github.com/user-attachments/assets/3e54ada6-6231-475e-848c-c4b78fe29bdd" />

# Stabilizer Group: Finding the Number of Generators

To determine the number of generators for the given stabilizer group, we apply the fundamental properties of stabilizer groups in quantum error correction.

## 1. Analyze the Group Size
First, let's list the elements provided in the set:
$$\mathcal{S} = \{ +III, +XZX, +YXX, +XXY, -ZYI, -IYZ, +ZIZ, -YZY \}$$

By counting the unique operators in this set, we find that the total number of elements $|\mathcal{S}|$ is:
**$|\mathcal{S}| = 8$**

## 2. Group Theory for Stabilizers
A stabilizer group acting on $n$ qubits is an abelian subgroup of the Pauli group. The relationship between the number of elements ($N$) and the number of independent generators ($k$) is given by:

$$|\mathcal{S}| = 2^k$$

### Solving for $k$:
Substituting our group size:
$$8 = 2^k$$
$$2^3 = 2^k$$
$$k = 3$$

## 3. Verification of Independence
To verify, we can select three elements and check if they can generate the remaining elements through multiplication (recalling that $XY=iZ$, $YX=-iZ$, and $X^2=I$):

* **Let $g_1 = XZX$**
* **Let $g_2 = YXX$**
* **Let $g_3 = XXY$**

**Example Multiplication:**
$g_1 \cdot g_2 = (XZX)(YXX) = (XY)(ZX)(XX) = (iZ)(iY)(I) = -ZYI$
*(This matches the 5th element in the original set.)*

Since 3 independent operators are required to produce a group of 8 elements, the set of generators has size 3.

---

## Conclusion
The number of generators is the $\log_2$ of the group's total size.

**The correct answer is:**
**3**

<img width="888" height="253" alt="image" src="https://github.com/user-attachments/assets/10193e54-84d1-4b44-9177-dea4a4282390" />

# Quantum Stabilizer Code Analysis: [[4, 2]] Code

To determine the parameters of a stabilizer code, we analyze the relationship between the stabilizer group size, the physical qubits, and the resulting logical space.

## 1. Number of Generators ($m$)
The provided stabilizer group is:
$$\mathcal{S} = \{ +IIII, +ZXXZ, +XZZX, +YYYY \}$$

The size of the group $|\mathcal{S}|$ is 4. The number of independent generators $m$ is calculated using:
$$|\mathcal{S}| = 2^m$$
$$4 = 2^m \implies m = 2$$

Two independent generators that could represent this group are $g_1 = ZXXZ$ and $g_2 = XZZX$. 
*(Note: $g_1 \cdot g_2 = (ZXXZ)(XZZX) = (ZX)(XZ)(ZX)(XZ) = (-iY)(iY)(-iY)(iY) = YYYY$)*.

## 2. Number of Logical Qubits ($k$)
The number of logical qubits $k$ is the dimension of the protected subspace, defined as:
$$k = n - m$$

Where:
* **$n = 4$**: The number of physical qubits (seen from the length of the Pauli strings).
* **$m = 2$**: The number of generators found above.

$$k = 4 - 2 = 2$$

---

## Conclusion
This stabilizer group defines a **$[[4, 2]]$** quantum code, meaning it encodes **2 logical qubits** into **4 physical qubits** using **2 generators**.

**Result:**
**$m = 2$**
**$k = 2$**

<img width="880" height="352" alt="image" src="https://github.com/user-attachments/assets/8e836cce-c170-4237-b866-e40226819eb2" />

# Quantum Error Syndrome Calculation

In a stabilizer code, the syndrome $(s_1, s_2, \dots, s_m)$ tells us which stabilizers were "tripped" by an error.

## 1. Parameters
* **Generators:** $g_1 = ZXX$, $g_2 = YYI$
* **Error:** $E = IXI$

## 2. Syndrome Logic
The $i$-th syndrome bit is defined as:
* $s_i = 0$ if $[g_i, E] = 0$ (Commute)
* $s_i = 1$ if $\{g_i, E\} = 0$ (Anti-commute)

## 3. Step-by-Step Check

### Generator 1 ($ZXX$):
Comparing $ZXX$ with $IXI$:
* $Z$ vs $I \rightarrow$ Commute
* $X$ vs $X \rightarrow$ Commute
* $X$ vs $I \rightarrow$ Commute
**Total:** Commute $\implies s_1 = 0$

### Generator 2 ($YYI$):
Comparing $YYI$ with $IXI$:
* $Y$ vs $I \rightarrow$ Commute
* $Y$ vs $X \rightarrow$ **Anti-commute**
* $I$ vs $I \rightarrow$ Commute
**Total:** Anti-commute $\implies s_2 = 1$

---

## Conclusion
The syndrome is the vector of these results.

**Result:**
**(0, 1)**

<img width="712" height="247" alt="image" src="https://github.com/user-attachments/assets/138bc12c-9c05-4ae1-b9df-044c2a44c25e" />

# Pauli Operator Commutation: Symplectic Inner Product

To check if two Pauli operators commute using binary string representation $(x|z)$, we calculate the symplectic inner product.

## 1. Operator Data
* **$P_1$**: $x_1 = (1,0,1,1)$, $z_1 = (0,0,0,1)$
* **$P_2$**: $x_2 = (0,1,1,0)$, $z_2 = (0,1,1,0)$

## 2. Formula
Operators commute if:
$$(x_1 \cdot z_2) \oplus (x_2 \cdot z_1) = 0$$
Operators anti-commute if the result is $1$.

## 3. Calculation
1.  **$x_1 \cdot z_2$**: $(1\cdot0) + (0\cdot1) + (1\cdot1) + (1\cdot0) = 1$
2.  **$x_2 \cdot z_1$**: $(0\cdot0) + (1\cdot0) + (1\cdot0) + (0\cdot1) = 0$
3.  **Result**: $1 \oplus 0 = 1$

---

## Conclusion
Because the result is $1$, the two operators **anti-commute**.

**Result:**
**Anti-Commute**




$$(1, 0, 1)$
