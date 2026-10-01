# Addition (`add`) Flags Analysis

This folder contains programs demonstrating the `add` instruction and its effect on the EFLAGS register. Below is the analysis of the flags after the addition operations in `add1.asm` and `add2.asm`.

### 1. `add1.asm`
**Operation:** Adds `120` (01111000b) and `10` (00001010b) in an 8-bit register (`al`)[cite: 2].
**Result:** `130` (10000010b).

* **ZF (Zero Flag) is Cleared (0):** The mathematical result is 130, not zero.
* **CF (Carry Flag) is Cleared (0):** The result (130) fits perfectly within the maximum unsigned 8-bit capacity of 255. No carry out of the most significant bit (MSB) occurred.
* **SF (Sign Flag) is Set (1):** In binary, 130 is `10000010`. The MSB (the leftmost bit) is `1`, which the processor interprets as a negative number in two's complement signed representation (-126).
* **OF (Overflow Flag) is Set (1):** Signed overflow occurred. We added two positive numbers (120 and 10), but the result exceeded the maximum 8-bit signed positive integer limit (+127), wrapping around into negative territory. 

### 2. `add2.asm`
**Operation:** Adds `32000` and `500` in a 16-bit register (`ax`)[cite: 3].
**Result:** `32500` (0111111011110100b in binary).

* **ZF (Zero Flag) is Cleared (0):** The mathematical result is 32500, not zero.
* **CF (Carry Flag) is Cleared (0):** The result (32500) is well below the maximum 16-bit unsigned capacity (65535), so no carry is generated out of the 16th bit.
* **SF (Sign Flag) is Cleared (0):** In binary, 32500 starts with a `0` (0111...). Because the MSB is `0`, the result is positive.
* **OF (Overflow Flag) is Cleared (0):** No signed overflow occurred. We added two positive numbers, and the result (32500) fits safely within the maximum 16-bit signed positive limit of +32767, so it remains positive.