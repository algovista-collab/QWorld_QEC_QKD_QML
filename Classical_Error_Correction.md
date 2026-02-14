# Error-Correction by Repetition: A Summary

The **Repetition Code** is one of the most fundamental methods in classical error-correction, designed to improve data reliability over noisy channels.

---

## 1. The Problem: Transmission Without Correction
When a message is sent bit-by-bit without any protection:
* **Noise Model:** Each bit has a probability $p$ of being flipped.
* **Reliability:** The receiver (Bob) has no way to verify if a bit was distorted during transmission.
* **Inference:** Bob can only assume the received bit is correct, with a $1-p$ chance of being right.

## 2. The Solution: Encoding and Decoding
To improve the odds of success, Alice and Bob use a **Code**—a mathematical process that adds redundancy.

* **Encoding:** Alice transforms her original message ($\bar{m}$) into a longer bit-string called a **codeword** ($c$).
* **Transmission:** The codeword is sent through the noisy channel, resulting in a corrupted codeword ($c'$).
* **Decoding:** Bob applies an algorithm to $c'$ to recover the original message.



---

## 3. The Repetition Process (Example: $n=3$)
Alice repeats each bit of her message three times to create blocks:
* If the message bit is **0**, she sends the block **000**.
* If the message bit is **1**, she sends the block **111**.

> **Example:** > **Original Message:** `1 0 1`  
> **Encoded Codeword:** `111 000 111`

## 4. Error Correction: Majority Voting
Bob uses a **Majority Voting Algorithm** to decode the received blocks. He identifies which bit value (0 or 1) appears most frequently in each 3-bit block.

| Received Block | Majority Vote | Status |
| :--- | :---: | :--- |
| `111` | **1** | Correct |
| `101` | **1** | **Corrected** (1 bit flipped) |
| `011` | **1** | **Corrected** (1 bit flipped) |
| `001` | **0** | **Failure** (2 bits flipped) |

---

## Key Takeaway
The repetition code is effective only if the number of errors in a block is **less than half** the block size. 
* In a 3-bit code, **1 error** is corrected.
* In a 3-bit code, **2 or more errors** lead to an incorrect decoding.

# Probability Analysis of the Repetition Code ($n=3$)

To evaluate the effectiveness of the repetition code, we use the **Binomial Distribution**. We assume each bit has an independent probability $p$ of being flipped (an error) and a probability $1-p$ of being transmitted correctly.

---

## 1. Probability of Errors in a 3-Bit Block
The probability of exactly $k$ errors occurring in a block of $n=3$ bits is calculated using the formula:  
$P(k) = \binom{n}{k} p^k (1-p)^{n-k}$

| Number of Errors ($k$) | Calculation | Probability Expression |
| :--- | :--- | :--- |
| **Zero Errors** | $\binom{3}{0} p^0 (1-p)^3$ | $(1-p)^3$ |
| **One Error** | $\binom{3}{1} p^1 (1-p)^2$ | $3p(1-p)^2$ |
| **Two Errors** | $\binom{3}{2} p^2 (1-p)^1$ | $3p^2(1-p)$ |
| **Three Errors** | $\binom{3}{3} p^3 (1-p)^0$ | $p^3$ |

---

## 2. When is Error Correction Beneficial?

The repetition code is beneficial only when the probability of the decoded bit being correct ($P_{code}$) is higher than the probability of a single unprotected bit being correct ($P_{naive}$).

* **Decoded Correctness ($P_{code}$):** Bob is correct if there are **0 or 1** errors.  
  $P_{code} = (1-p)^3 + 3p(1-p)^2$
* **Naive Correctness ($P_{naive}$):** Bob is correct if the single bit isn't flipped.  
  $P_{naive} = 1-p$

### The Threshold: $p = 0.5$
Mathematical analysis shows that the repetition code helps when **$p < 0.5$**.

1. **If $p < 0.5$:** The physical error rate is low. It is statistically likely that the "majority" of the three bits will remain unchanged. The code effectively "filters" out single-bit flips.
2. **If $p > 0.5$:** The channel is so noisy that it flips bits more often than not. In this case, the majority of the bits in a block are likely to be errors. Using a repetition code here actually **increases** the chance of Bob receiving the wrong message.
3. **If $p = 0.5$:** The channel is pure noise (like a fair coin flip). No amount of repetition can recover the information because the input and output are statistically independent.



---

## 3. Why the Code Can Be "Worse"
It seems counterintuitive, but if $p > 0.5$, Alice is essentially sending her message through a "Liar Channel." Because the majority voting algorithm *trusts* the most frequent bit, it will consistently choose the error over the truth. 

In a high-noise environment ($p=0.8$), sending more bits simply gives the channel more opportunities to "outvote" the correct bit, compounding the original error.

## Logical Operations on Repetition Codes

To perform logic on encoded messages without decoding, apply the operation **bitwise** to the blocks.

### NOT Operation
Apply NOT to every bit in the $n$-bit block:
- `000` becomes `111`
- `111` becomes `000`
- `010` (corrupted 0) becomes `101` (corrupted 1)

### OR Operation
Compare two blocks bit-by-bit using the standard OR truth table:
- `000` OR `111` = `111`
- `100` (corrupted 0) OR `000` = `100` (still decodes to 0)

# Linear Block Codes: From Repetition to Hamming

This document outlines the vector space foundations of error-correction and the transition from simple repetition to efficient Hamming codes.

---

## 1. Mathematical Definitions

An error-correcting code involves mapping data between distinct vector spaces over the binary field $\mathbb{F}_2$.

| Space | Symbol | Dimension | Description |
| :--- | :---: | :---: | :--- |
| **Message Space** | $\mathbb{F}_2^k$ | $k$ | The space of original information bits (length $k$). |
| **Codespace** | $\mathbb{F}_2^n$ | $n$ | The space of all possible bit strings of length $n$. |
| **The Code ($C$)** | $C \subseteq \mathbb{F}_2^n$ | $k$ | The $k$-dimensional subspace containing valid codewords. |

**Key Concept:** A code is an $[n, k, d]$ system where:
- $n$ = Codeword length
- $k$ = Message length
- $d$ = Minimum distance (the minimum number of bit flips to turn one valid codeword into another).

**Repetition Code Label:** For a 3-bit repetition code, the label is $[3, 1, 3]$.

---

## 2. The Encoding Process & Generator Matrix

To move a message $\bar{m}$ into the code $C$, we use a **Generator Matrix ($G$)**. The first four bits have to be copied as is, so the first four columns of G must be the identity matrix. The next three columns must be the various ways of summing up the data bits.

* **Definition:**

$G$ is a $k \times n$ matrix whose rows form a basis for $C$.

* **Procedure:** A message vector $m$ is encoded as $c = mG$.

### Example: Repetition Code ($k=1, n=3$)
The generator matrix is:

$$G = \begin{pmatrix} 1 & 1 & 1 \end{pmatrix}$$

- Message `[0]` becomes `[0,0,0]`
- Message `[1]` becomes `[1,1,1]`

### Standard Form
Any generator matrix can be reduced to **Standard Form**: 
$$G = [I_k \mid P]$$
Where $I_k$ is the $k \times k$ identity matrix (keeping message bits visible) and $P$ is the $k \times (n-k)$ parity-check matrix.

---

## 3. The Hamming(7,4) Code

The Hamming code is a powerful $[7, 4, 3]$ code. It encodes 4 message bits into 7-bit codewords using **overlapping parity**.



### The Codeword Structure
A codeword $x$ consists of message bits $(x_1, x_2, x_3, x_4)$ and parity bits $(x_5, x_6, x_7)$:
1. **$x_5$** = Parity of $\{x_1, x_2, x_3\}$
2. **$x_6$** = Parity of $\{x_2, x_3, x_4\}$
3. **$x_7$** = Parity of $\{x_1, x_3, x_4\}$

### Error Correction Protocol
Bob (the receiver) re-calculates the parities upon arrival:
- **Zero Parity Changes:** No single-bit error occurred.
- **One Parity Change:** A parity bit ($x_5, x_6,$ or $x_7$) was flipped.
- **Multiple Parity Changes:** A message bit ($x_1, x_2, x_3,$ or $x_4$) was flipped.

Because each message bit belongs to a unique combination of parity sets, Bob can "triangulate" exactly which bit is wrong and flip it back.

---

## Summary Table: Efficiency Comparison

| Code Type | $n$ | $k$ | Redundancy Bits | Error Correction |
| :--- | :---: | :---: | :---: | :--- |
| **Repetition** | 3 | 1 | 2 | Corrects 1 bit |
| **Hamming** | 7 | 4 | 3 | Corrects 1 bit |

*Note: To protect 4 bits with a repetition code, you would need 12 bits total. Hamming does the same job with only 7.*

# Systematic Error Correction: Parity-Check Matrices and Syndromes

To identify and correct errors without guessing, linear codes use the properties of linear algebra to "extract" noise from a received message.

---

## 1. Information Allocation
In any $[n, k]$ code, the $n$ bits of a codeword are divided by purpose:
* **$k$ bits**: Carry the actual message information.
* **$n - k$ bits**: Provide redundancy used to identify errors.

Because there are $n - k$ bits of redundancy, the code can distinguish between $2^{n-k}$ different error scenarios (including the "no error" scenario).



---

## 2. The Parity-Check Matrix ($H$)
The matrix $H$ acts as the mathematical "gatekeeper" of the code. It defines the consistency checks that every valid codeword must satisfy.

* **Definition:** A vector $c$ is a valid codeword if and only if:
  $$H \cdot c^T = 0$$
* **Dimensions:** $H$ is an $(n-k) \times n$ matrix.
* **Uniqueness:** $H$ is not unique; row operations can create different versions of the same parity-check matrix for the same code.

---

## 3. Error Syndromes
When Alice sends codeword $c$ and Bob receives $c'$, the received signal is $c' = c + e$ (where $e$ is the error vector). Bob calculates the **Syndrome ($s$)** to isolate the error:

$$s = H \cdot c'$$

### How the Math Works:
Since $c' = c + e$, we can expand the equation:
1. $s = H(c + e)$
2. $s = Hc + He$
3. Since $Hc = 0$ (by definition of a codeword), then **$s = He$**.

**The Result:** The syndrome depends *only* on the error vector $e$, not the original message $m$. This allows Bob to identify the noise regardless of what Alice actually said.

---

## 4. Correcting the Error
Each correctable error pattern corresponds to a distinct, non-zero syndrome. 

### Example: Hamming (7,4) Code
* **Redundancy:** $n - k = 3$ bits.
* **Syndromes:** $2^3 = 8$ possible syndromes.
* **Correction Capacity:** * 1 syndrome for "No Error" $(0,0,0)$.
    * 7 syndromes to uniquely identify which of the 7 bits was flipped.



### The Recovery Process:
1. Bob receives $c'$.
2. Bob computes $s = Hc'$.
3. Bob looks up $s$ in a table to find the corresponding error vector $e$.
4. Bob recovers the message: $c = c' - e$.

### Formula for the Parity-Check Matrix ($H$)

For a linear code in **standard form**, the matrices $G$ and $H$ are related as follows:

1. **Generator Matrix ($G$):** $[I_k \mid P]$
2. **Parity-Check Matrix ($H$):** $[P^T \mid I_{n-k}]$

**Orthogonality Property:**
The core requirement for these matrices is:
$$G \cdot H^T = 0 \pmod 2$$
This ensures that every valid codeword $c$ satisfies the parity equations: $H \cdot c^T = 0$.

# Distance of a Code

A crucial quantity of interest when discussing codes is their **distance**, as it determines the number of errors that can be corrected. When an error occurs on a codeword $\mathbf{c}$, it creates a corrupted codeword $\mathbf{\tilde{c}}$ that is a certain "distance" away from the original.

---

## Hamming Distance

The **Hamming distance** is the metric used to build the notion of code distance.

> **Definition:** The Hamming distance between two bit strings $\mathbf{s}, \mathbf{t}$ is the number of places where they have different symbols. This is denoted as $d_H(\mathbf{s}, \mathbf{t})$.

### Mathematical Calculation
The Hamming distance determines the number of substitutions or errors required to transform a length-$n$ vector $\mathbf{s}$ into another length-$n$ vector $\mathbf{t}$. It is calculated as:

$$d_H(\mathbf{s}, \mathbf{t}) = \sum_{i} \mathbf{s}_i + \mathbf{t}_i$$

*Note: The addition inside the sum is in the binary field (XOR), but the final summation is regular integer addition.*

### Example
The distance between **1011** and **0111** is **two**.
* Position 1: $1 \neq 0$ (Different)
* Position 2: $0 \neq 1$ (Different)
* Position 3: $1 = 1$ (Same)
* Position 4: $1 = 1$ (Same)
* **Total Distance:** 2 (You must flip the first two entries to go from the first string to the second).

---

## Significance in Error Correction
* **Correction Threshold:** If the distance is small, it is possible to correct the error.
* **Correction Limit:** If the distance of the error is larger than a specific threshold value (the "distance of the code"), the error cannot be corrected.
