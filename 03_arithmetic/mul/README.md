# Multiplication (`mul`) Flags Analysis

For multiplication instructions, the CPU handles flags differently than addition or subtraction:
* **CF (Carry Flag) and OF (Overflow Flag):** These flags are set to `1` if the upper half of the result register (e.g., `AH` for 8-bit, `DX` for 16-bit, `EDX` for 32-bit) contains significant data (is non-zero). If the entire result fits within the lower half of the register, CF and OF are cleared to `0`. 
* **ZF, SF, PF, AF:** The Intel x86 architecture defines these flags as **undefined** after a `mul` instruction. Their values cannot be reliably used.

### 1. `mul1.asm`
**Operation:** 8-bit unsigned multiplication multiplying `25` by `10`[cite: 6].
**Result:** `250` (stored in the 16-bit `AX` register)[cite: 6].

* **CF and OF are Cleared (0):** An 8-bit multiplication stores its result in `AX` (which consists of `AH` and `AL`). The result of 25 * 10 is 250. Because 250 fits entirely within the lower 8 bits (`AL` can hold up to 255), the upper half (`AH`) is `0`. Since the upper half is zero, the CPU clears both the Carry and Overflow flags.
* **ZF, SF:** These are left in an undefined state.

### 2. `mul2.asm`
**Operation:** 16-bit unsigned multiplication multiplying `3000` by `200`[cite: 7].
**Result:** `600,000` (stored across the `DX:AX` register pair)[cite: 7].

* **CF and OF are Set (1):** A 16-bit multiplication stores its result in `DX:AX`, where `DX` holds the upper 16 bits and `AX` holds the lower 16 bits[cite: 7]. The result of 3000 * 200 is 600,000, which in hexadecimal is `0x927C0`. This value exceeds the maximum 16-bit capacity (65,535). Therefore, the lower half (`AX`) holds `0x27C0`, and the upper half (`DX`) holds `0x0009`. Because the upper half (`DX`) is non-zero, the processor sets both the Carry and Overflow flags to `1` to indicate that the product spilled out of the primary lower register.
* **ZF, SF:** These are left in an undefined state.