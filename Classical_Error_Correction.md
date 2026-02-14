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
