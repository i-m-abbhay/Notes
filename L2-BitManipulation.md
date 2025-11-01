
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

**Concept:**

XOR of two same numbers → 0

XOR of number with 0 → same number

**Time Complexity:** O(1)

---

## 2. Check if the *i-th* Bit is Set or Not

### Using Left Shift

```cpp
if (n & (1 << i))
    cout << "Set";
else
    cout << "Not Set";
```

### Using Right Shift

```cpp
if ((n >> i) & 1)
    cout << "Set";
else
    cout << "Not Set";
```

**Idea:**

* `(1 << i)` creates a mask with 1 at the i-th position.
* AND (`&`) checks if that bit is 1.

**Time Complexity:** O(1)

---

## 3. Set the *i-th* Bit

Make the bit 1 irrespective of its current state.

```cpp
n = n | (1 << i);
```

**Idea:**

`OR` (`|`) with 1 → always results in 1.

---

## 4. Clear the *i-th* Bit

Make the bit 0.

```cpp
n = n & ~(1 << i);
```

**Idea:**

`~(1 << i)` creates a mask with 0 at i-th bit and 1 everywhere else.

AND makes that bit 0 while keeping others unchanged.

---

## 5. Toggle the *i-th* Bit

Flip 0 → 1 or 1 → 0.

```cpp
n = n ^ (1 << i);
```

**Idea:**

XOR with 1 toggles a bit.

XOR with 0 leaves it unchanged.

---

## 6. Remove the Last Set Bit

```cpp
n = n & (n - 1);
```

**Idea:**

`n - 1` flips the rightmost set bit to 0 and all bits to the right become 1.

ANDing with `n` clears that rightmost 1.

**Example:**

```
n  = 40 (101000)
n-1= 39 (100111)
n & (n-1) = 100000  // removes last set bit
```

---

## 7. Check if Number is Power of Two

```cpp
if (n && !(n & (n - 1)))
    cout << "Power of Two";
else
    cout << "Not Power of Two";
```

**Idea:**

Power of two numbers have exactly one set bit.

After `n & (n - 1)`, result becomes 0 only for such numbers.

**Edge Case:** Ensure `n != 0`.

---

## 8. Count the Number of Set Bits

### (a) Using Bitwise Loop

```cpp
int count = 0;
while (n > 0) {
    count += (n & 1);
    n >>= 1;
}
```

### (b) Using Brian Kernighan’s Algorithm

```cpp
int count = 0;
while (n) {
    n = n & (n - 1); // removes rightmost set bit
    count++;
}
```

**Time Complexity:** O(number of set bits)

Efficient for sparse bits.

### (c) STL Shortcut

```cpp
__builtin_popcount(n); // in GCC/Clang
```

---

## 9. Key Bitwise Operators Recap

| Operator    | Symbol | What it does              | Arithmetic intuition*                    |
| ----------- | ------ | ------------------------- | ---------------------------------------- |
| AND         | `&`  | 1 only if both bits are 1 | Masks bits on                            |
| OR          | `\|`  | 1 if either bit is 1      | Sets bits                                |
| XOR         | `^`  | 1 if bits differ          | Toggles bits                             |
| NOT         | `~`  | Inverts all bits          | Bitwise complement                       |
| Left Shift  | `<<` | Shifts bits left by*k*  | Multiplies by `2^k`(if no overflow)    |
| Right Shift | `>>` | Shifts bits right by*k* | Divides by `2^k`(logical for unsigned) |

---

## Conceptual Summary

| Task                     | Correct expression                                                    | Notes                                            |
| ------------------------ | --------------------------------------------------------------------- | ------------------------------------------------ |
| Swap `a`/`b`(XOR)    | `a ^= b; b ^= a; a ^= b;`                                           | Works when `a`and `b`are distinct variables. |
| Check i-th bit           | `(n & (1u << i)) != 0` **or** `((n >> i) & 1u)`             | Use `1u`to avoid sign/overflow issues.         |
| Set i-th bit             | `n \|= (1u << i)`                                                    | Forces bit to 1.                                 |
| Clear i-th bit           | `n &= ~(1u << i)`                                                   | Forces bit to 0.                                 |
| Toggle i-th bit          | `n ^= (1u << i)`                                                    | Flips bit.                                       |
| Remove rightmost set bit | `n &= (n - 1)`                                                      | Clears lowest 1-bit.                             |
| Is power of two          | `n != 0 && (n & (n - 1)) == 0`                                      | Works for unsigned `n`.                        |
| Count set bits           | `__builtin_popcount(n)`(32-bit),`__builtin_popcountll(n)`(64-bit) | Or loop with `n &= (n - 1)`until `n == 0`.   |

---

## Practice Plan

1. Write and dry-run XOR swap for multiple pairs.
2. Implement functions:
   * `checkIthBit(n, i)`
   * `setIthBit(n, i)`
   * `clearIthBit(n, i)`
   * `toggleIthBit(n, i)`
3. Re-implement power-of-two and count-set-bits functions from scratch.
