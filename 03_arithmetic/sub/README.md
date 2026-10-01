# Subtraction (`sub` / `sbb`) Flags Analysis


* **ZF (Zero Flag):** Set to `1` if the result of the subtraction is exactly zero.
* **CF (Carry Flag):** Acts as a *borrow* flag in subtraction. Set to `1` if subtracting a larger unsigned integer from a smaller one requires a borrow.
* **SF (Sign Flag):** Set to `1` if the result is negative (the Most Significant Bit is `1`).
* **OF (Overflow Flag):** Set to `1` if signed overflow occurs (e.g., subtracting a negative number from a positive number produces a result too large for the destination).

### 1. `sub1.asm`
**Operation:** 8-bit subtraction: `50 - 80`[cite: 8].
**Result:** `-30` (represented as `0xE2` or `11100010` in binary two's complement).

* **ZF is Cleared (0):** The result is -30, not zero.
* **CF is Set (1):** Because we are subtracting a larger unsigned number (80) from a smaller one (50), the CPU must "borrow" from a non-existent higher bit to perform the operation.
* **SF is Set (1):** The result is negative, meaning its MSB is `1`.
* **OF is Cleared (0):** No signed overflow occurred. The result (-30) fits perfectly within the bounds of a signed 8-bit register (-128 to 127).

### 2. `sub2.asm`
**Operation:** 16-bit subtraction: `1000 - 2000`[cite: 9].
**Result:** `-1000` (represented as `0xFC18` in binary two's complement).

* **ZF is Cleared (0):** The mathematical result is -1000, not zero.
* **CF is Set (1):** Subtracting 2000 from 1000 requires a borrow because the subtrahend is larger than the minuend.
* **SF is Set (1):** The MSB of `0xFC18` is `1`, indicating a negative result.
* **OF is Cleared (0):** The signed result (-1000) fits well within the 16-bit signed integer limits (-32,768 to 32,767).

### 3. `sub3.asm`
**Operation:** Demonstrates subtraction with borrow (`sbb`)[cite: 10]. It first subtracts `1` from `0` (`sub ax, [num2]`), then subtracts `0` and the Carry Flag from the result (`sbb ax, 0`)[cite: 10].
**Result:** The initial `sub` yields `-1` (`0xFFFF`). The `sbb` operation evaluates to `AX - 0 - CF`, which is `0xFFFF - 0 - 1 = 0xFFFE` (-2).

**Flags Status after the final `sbb ax, 0` instruction:**
* **ZF is Cleared (0):** The final result is `0xFFFE` (-2), not zero.
* **CF is Cleared (0):** The `sbb` instruction subtracted `1` (the carry flag) from `65535` (`0xFFFF` unsigned). Because the first operand was large enough, no *additional* borrow was needed for this specific step.
* **SF is Set (1):** The MSB of the result `0xFFFE` is `1`, indicating a negative value.
* **OF is Cleared (0):** Subtracting 1 from -1 results in -2, which fits safely in a 16-bit signed register.