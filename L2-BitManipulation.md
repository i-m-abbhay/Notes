# Bit Manipulation — Core Concepts & Problems

## 1. Swap Two Numbers (Without Third Variable)

**Normal Way:**

```cpp
int temp = a;
a = b;
b = temp;
```

**Using XOR (⨁):**

```cpp
a = a ^ b;
b = a ^ b;  // now b = original a
a = a ^ b;  // now a = original b
```

**Compact Form:**

```cpp
a ^= b; b ^= a; a ^= b;
```

**Concept:**

* XOR of two same numbers → 0
* XOR of number with 0 → same number

**Step-by-Step Example:**

```
a = 5 (101), b = 7 (111)

Step 1: a = a ^ b = 101 ^ 111 = 010 (2)
Step 2: b = a ^ b = 010 ^ 111 = 101 (5) ✓ original a
Step 3: a = a ^ b = 010 ^ 101 = 111 (7) ✓ original b
```

**⚠️ Important Warning:**

This fails if `a` and `b` are the same variable or point to the same memory location! Both will become 0.

```cpp
int x = 5;
x ^= x; x ^= x; x ^= x;  // x = 0 (wrong!)
```

**Time Complexity:** O(1)

**Note:** Rarely used in practice; modern compilers optimize temp variable swaps.

---

## 2. Check if the *i-th* Bit is Set or Not

### Using Left Shift (Recommended)

```cpp
// Assumes 0 <= i < 32 for 32-bit integers
if (n & (1u << i))
    cout << "Set";
else
    cout << "Not Set";
```

### Using Right Shift

```cpp
if ((n >> i) & 1u)
    cout << "Set";
else
    cout << "Not Set";
```

**Idea:**

* `(1u << i)` creates a mask with 1 at the i-th position (using unsigned to avoid sign issues).
* AND (`&`) checks if that bit is 1.

**Example:**

```
n = 13 (1101), check bit 2 (0-indexed from right)

1u << 2 = 0100
1101 & 0100 = 0100 ≠ 0 → Bit is SET
```

**Time Complexity:** O(1)

**Bounds Check:** Always validate `0 <= i < sizeof(n) * 8`

---

## 3. Set the *i-th* Bit

Make the bit 1 irrespective of its current state.

```cpp
n |= (1u << i);
```

**Idea:**

`OR` (`|`) with 1 → always results in 1.

**Example:**

```
n = 13 (1101), set bit 1

1u << 1 = 0010
1101 | 0010 = 1111 (15)
```

---

## 4. Clear the *i-th* Bit

Make the bit 0.

```cpp
n &= ~(1u << i);
```

**Idea:**

`~(1u << i)` creates a mask with 0 at i-th bit and 1 everywhere else.

AND makes that bit 0 while keeping others unchanged.

**Example:**

```
n = 13 (1101), clear bit 2

1u << 2 = 0100
~(0100) = 1011
1101 & 1011 = 1001 (9)
```

---

## 5. Toggle the *i-th* Bit

Flip 0 → 1 or 1 → 0.

```cpp
n ^= (1u << i);
```

**Idea:**

* XOR with 1 toggles a bit.
* XOR with 0 leaves it unchanged.

**Example:**

```
n = 13 (1101), toggle bit 1

1u << 1 = 0010
1101 ^ 0010 = 1111 (15)

Toggle again:
1111 ^ 0010 = 1101 (13) ← back to original
```

---

## 6. Remove the Rightmost Set Bit (LSB)

```cpp
n &= (n - 1);
```

**Idea:**

`n - 1` flips the rightmost set bit to 0 and all bits to the right become 1.

ANDing with `n` clears that rightmost 1.

**Visual Example:**

```
n  = 40 = 101000
n-1= 39 = 100111
n & (n-1) = 100000 (32)  // removes last set bit
```

**Step-by-Step:**

```
n = 40:
  Bit position:  5  4  3  2  1  0
  Binary:        1  0  1  0  0  0
  
n - 1 = 39:
  Binary:        1  0  0  1  1  1
  
n & (n-1):
  Result:        1  0  0  0  0  0  = 32
```

---

## 7. Get the Rightmost Set Bit (Isolate LSB)

Extract only the rightmost set bit, set all others to 0.

```cpp
int lsb = n & (-n);  // or n & (~(n - 1))
```

**Idea:**

* `-n` in two's complement = `~(n - 1)`
* This creates a mask with only the rightmost set bit preserved.

**Example:**

```
n  = 40 = 101000
-n = -40 (two's complement) = ...111011000 (in 32-bit)
n & (-n) = 001000 = 8  // isolated rightmost bit
```

**Use Case:** Useful in Fenwick Trees (Binary Indexed Trees).

---

## 8. Check if Number is Power of Two

```cpp
if (n > 0 && (n & (n - 1)) == 0)
    cout << "Power of Two";
else
    cout << "Not Power of Two";
```

**Idea:**

Power of two numbers have exactly one set bit.

After `n & (n - 1)`, result becomes 0 only for such numbers.

**Examples:**

```
n = 16 = 10000
n - 1 = 15 = 01111
n & (n-1) = 0 ✓ Power of 2

n = 18 = 10010
n - 1 = 17 = 10001
n & (n-1) = 10000 ≠ 0 ✗ Not power of 2
```

**Edge Case:** Ensure `n > 0` (0 is not considered a power of 2).

---

## 9. Check if Number is Power of Four

```cpp
bool isPowerOfFour(int n) {
    return n > 0 && 
           (n & (n - 1)) == 0 &&  // power of 2
           (n & 0xAAAAAAAA) == 0; // power of 4 check
}
```

**Idea:**

* Must be power of 2.
* AND with `0xAAAAAAAA` (binary: 10101010...) ensures the single set bit is at an even position (0, 2, 4, ...).

**Example:**

```
4^0 = 1  = 0001 (bit 0) ✓
4^1 = 4  = 0100 (bit 2) ✓
4^2 = 16 = 10000 (bit 4) ✓
2^1 = 2  = 0010 (bit 1) ✗ (power of 2, not power of 4)
```

---

## 10. Count the Number of Set Bits (Popcount)

### (a) Using Bitwise Loop

```cpp
int count = 0;
while (n > 0) {
    count += (n & 1u);
    n >>= 1;
}
return count;
```

**Time Complexity:** O(log n)

### (b) Using Brian Kernighan's Algorithm (Recommended)

```cpp
int count = 0;
while (n) {
    n &= (n - 1); // removes rightmost set bit
    count++;
}
return count;
```

**Time Complexity:** O(number of set bits)

Efficient for sparse bits (numbers with few 1s).

**Example:**

```
n = 40 = 101000

Iteration 1: n = 40 & 39 = 32 (100000), count = 1
Iteration 2: n = 32 & 31 = 0, count = 2
Done. Total set bits = 2
```

### (c) Built-in Functions

```cpp
// GCC/Clang (most common)
__builtin_popcount(n);        // 32-bit
__builtin_popcountll(n);      // 64-bit

// C++20 standard library (requires <bit>)
std::popcount(n);             // Works with any unsigned type
```

**Time Complexity:** O(1) typically (hardware-accelerated on modern CPUs)

**Note:** `std::popcount()` is preferred in C++20+ for portability.

---

## 11. Find Position of Rightmost Set Bit

Get the index (0-indexed from right) of the rightmost set bit.

```cpp
// Method 1: Using log
int pos = log2(n & (-n));

// Method 2: Built-in (GCC/Clang)
int pos = __builtin_ffs(n) - 1;  // ffs = find first set (1-indexed)

// Method 3: Loop
int pos = 0;
while ((n & 1) == 0) {
    n >>= 1;
    pos++;
}
```

**Example:**

```
n = 40 = 101000
Rightmost set bit is at position 3 (0-indexed from right).
```

---

## 12. Reverse Bits

Reverse the binary representation of a number.

```cpp
unsigned int reverseBits(unsigned int n) {
    unsigned int rev = 0;
    int bits = sizeof(n) * 8;
    
    for (int i = 0; i < bits; i++) {
        rev <<= 1;
        rev |= (n & 1);
        n >>= 1;
    }
    return rev;
}
```

**Example:**

```
n = 13 = 1101 (4-bit example)
Reversed = 1011 = 11
```

**Built-in (GCC/Clang):**

```cpp
__builtin_bitreverse32(n);  // 32-bit
```

---

## 13. Find Single Unique Number (XOR Pattern)

Given an array where every element appears twice except one, find the unique element.

```cpp
int singleNumber(vector<int>& nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;
    }
    return result;
}
```

**Idea:**

* XOR is commutative and associative.
* `a ^ a = 0`
* `0 ^ a = a`
* All pairs cancel out, leaving the unique number.

**Example:**

```
Array: [2, 3, 2, 4, 4]
Result = 2 ^ 3 ^ 2 ^ 4 ^ 4
       = (2 ^ 2) ^ (4 ^ 4) ^ 3
       = 0 ^ 0 ^ 3
       = 3 ✓
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

## 14. Key Bitwise Operators Recap

| Operator    | Symbol | What it does              | Arithmetic intuition*                    | Common use case                           |
| ----------- | ------ | ------------------------- | ---------------------------------------- | ----------------------------------------- |
| AND         | `&`    | 1 only if both bits are 1 | Masks bits on                            | Check if bit is set, clear bits           |
| OR          | `\|`   | 1 if either bit is 1      | Sets bits                                | Set bits, combine flags                   |
| XOR         | `^`    | 1 if bits differ          | Toggles bits                             | Swap, find unique, toggle bits            |
| NOT         | `~`    | Inverts all bits          | Bitwise complement                       | Create masks, flip all bits               |
| Left Shift  | `<<`   | Shifts bits left by *k*   | Multiplies by `2^k` (if no overflow)     | Multiply by 2, create bit masks           |
| Right Shift | `>>`   | Shifts bits right by *k*  | Divides by `2^k` (logical for unsigned)  | Divide by 2, extract bits                  |

---

## Conceptual Summary

| Task                     | Correct expression                                           | Notes                                                              |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------------ |
| Swap `a`/`b` (XOR)       | `a ^= b; b ^= a; a ^= b;`                                    | ⚠️ Works only when `a` and `b` are distinct variables.            |
| Check i-th bit           | `(n & (1u << i)) != 0` **or** `((n >> i) & 1u)`             | Use `1u` to avoid sign/overflow issues. Validate `0 <= i < 32`.   |
| Set i-th bit             | `n \|= (1u << i)`                                            | Forces bit to 1.                                                    |
| Clear i-th bit           | `n &= ~(1u << i)`                                            | Forces bit to 0.                                                    |
| Toggle i-th bit          | `n ^= (1u << i)`                                             | Flips bit.                                                          |
| Remove rightmost set bit | `n &= (n - 1)`                                               | Clears lowest 1-bit. Efficient for counting set bits.               |
| Get rightmost set bit    | `n & (-n)` **or** `n & (~(n - 1))`                          | Isolates LSB. Used in Fenwick Trees.                               |
| Is power of two          | `n > 0 && (n & (n - 1)) == 0`                                | Works for unsigned `n`. Must check `n > 0`.                        |
| Is power of four         | `n > 0 && (n & (n-1)) == 0 && (n & 0xAAAAAAAA) == 0`        | Must be power of 2 with set bit at even position.                  |
| Count set bits           | `__builtin_popcount(n)` (32-bit), `__builtin_popcountll(n)`  | Or loop with `n &= (n - 1)` until `n == 0`. C++20: `std::popcount`.|
| Find position of LSB     | `__builtin_ffs(n) - 1` **or** `log2(n & (-n))`              | 0-indexed from right.                                               |
| Reverse bits             | Loop or `__builtin_bitreverse32(n)`                          | Swap MSB with LSB, etc.                                             |
| Find unique number       | XOR all numbers                                              | Cancels pairs, leaves unique.                                       |

---

## Edge Cases and Gotchas

| Scenario                      | Gotcha                                                                 | Solution                                           |
| ----------------------------- | ---------------------------------------------------------------------- | -------------------------------------------------- |
| XOR swap with same variable   | Both become 0                                                          | Always check `a != b` before swap                  |
| Bit index out of bounds       | Undefined behavior                                                     | Validate `0 <= i < sizeof(n) * 8`                  |
| Signed vs unsigned shifts     | Right shift on negative numbers is implementation-defined              | Use `unsigned` types for bit manipulation          |
| `1 << 31` on signed int       | Overflow/undefined behavior                                            | Use `1u << 31` for unsigned operations            |
| Left shift overflow           | Shifting beyond bit width is undefined                                | Check bounds before shifting                       |
| `~0`                          | Results in -1 (all bits set)                                           | Use `~0u` for unsigned max                         |
| `x & (-x)` on zero            | Returns 0 (no set bits)                                                | Check `n != 0` before using                        |
| Power of 2 check for 0        | `0 & -1 = 0` (wrong result)                                            | Always check `n > 0` first                         |
| Right shift of negative number| Arithmetic vs logical shift (compiler-dependent)                       | Use unsigned types consistently                    |
| Bit position vs bit index     | Position 0 = LSB (rightmost), Position 31 = MSB (leftmost for 32-bit) | Be consistent with indexing convention             |

---

## Practice Problems

### LeetCode

| Problem                                  | Difficulty | Key Concepts                            |
| ---------------------------------------- | ---------- | --------------------------------------- |
| [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/) | Easy       | Count set bits                          |
| [Power of Two](https://leetcode.com/problems/power-of-two/)       | Easy       | `n & (n-1)` pattern                     |
| [Power of Four](https://leetcode.com/problems/power-of-four/)     | Easy       | Power of 2 + position check             |
| [Single Number](https://leetcode.com/problems/single-number/)     | Easy       | XOR pattern                             |
| [Reverse Bits](https://leetcode.com/problems/reverse-bits/)       | Easy       | Bit reversal                            |
| [Missing Number](https://leetcode.com/problems/missing-number/)  | Easy       | XOR with range                          |
| [Hamming Distance](https://leetcode.com/problems/hamming-distance/) | Easy   | XOR + count bits                        |
| [Single Number II](https://leetcode.com/problems/single-number-ii/) | Medium | Bit manipulation with counting          |
| [Counting Bits](https://leetcode.com/problems/counting-bits/)    | Easy       | Dynamic programming + bit manipulation  |

### HackerRank / Competitive Programming

* Bit manipulation problems in "Cracking the Coding Interview"
* Fenwick Tree / Binary Indexed Tree (uses `n & (-n)`)
* Segment Trees with bit operations

---

## Practice Plan

### Week 1: Basic Operations

1. ✅ Implement all single-bit operations:
   * `checkIthBit(n, i)`
   * `setIthBit(n, i)`
   * `clearIthBit(n, i)`
   * `toggleIthBit(n, i)`
2. ✅ Practice XOR swap (understand why it fails for same variable)
3. ✅ Implement power-of-two and power-of-four checks

### Week 2: Advanced Patterns

1. ✅ Implement Brian Kernighan's algorithm for counting set bits
2. ✅ Implement `getRightmostSetBit(n)`
3. ✅ Solve "Single Number" using XOR pattern
4. ✅ Practice reverse bits problem

### Week 3: Application Problems

1. ✅ Solve LeetCode problems (Number of 1 Bits, Power of Two, etc.)
2. ✅ Implement bit manipulation solutions for competitive programming
3. ✅ Study Fenwick Trees (uses LSB extraction)

### Tips

* Always use `1u` (unsigned) instead of `1` for bit masks
* Test edge cases: 0, negative numbers, maximum values
* Draw binary representations for complex operations
* Use built-in functions in competitive programming for speed
