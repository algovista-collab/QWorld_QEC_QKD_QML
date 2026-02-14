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
