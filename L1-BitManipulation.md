
**Bit Manipulation – Lecture 1 Notes**

*(Covers conversions, complements, storage, and bitwise operators)*

---

### 1. Number Systems and Conversions

#### Decimal → Binary

**Method:**

Divide repeatedly by 2, store remainders, reverse the result.

| Step | n  | n // 2 | Remainder |
| ---- | -- | ------ | --------- |
| 1    | 13 | 6      | 1         |
| 2    | 6  | 3      | 0         |
| 3    | 3  | 1      | 1         |
| 4    | 1  | 0      | 1         |

**Binary:** 1101

**Code:**

```python
def toBinary(n):
    res = ""
    while n > 0:
        res += str(n % 2)
        n //= 2
    return res[::-1]
```

**Complexity:**

Time → O(log₂N)

Space → O(log₂N)

---

#### Binary → Decimal

Start from rightmost bit. Multiply by powers of 2.

| Bit | Power | Contribution |
| --- | ----- | ------------ |
| 1   | 2³   | 8            |
| 1   | 2²   | 4            |
| 0   | 2¹   | 0            |
| 1   | 2⁰   | 1            |

Sum = 13

**Code:**

```python
def toDecimal(b):
    num, p = 0, 1
    for bit in b[::-1]:
        if bit == '1':
            num += p
        p *= 2
    return num
```

Time → O(n)

Space → O(1)

---

### 2. How Computers Store Integers

| Type      | Bits | Range             |
| --------- | ---- | ----------------- |
| int       | 32   | -2³¹ → 2³¹-1 |
| long long | 64   | -2⁶³ → 2⁶³-1 |

* Stored in binary (base 2).
* Example: 13 = 00000000...00001101 (32 bits).
* On print, converted back to decimal.
* Computer never stores “13”, only binary.

---

### 3. Complements

| Type   | Meaning               | Example (13 → 1101) |
| ------ | --------------------- | -------------------- |
| One’s | Flip bits             | 0010                 |
| Two’s | One’s complement + 1 | 0011                 |

**Used for:** Representing negative numbers.

---

### 4. Bitwise Operators

#### AND (`&`)

| A | B | A & B |
| - | - | ----- |
| 0 | 0 | 0     |
| 0 | 1 | 0     |
| 1 | 0 | 0     |
| 1 | 1 | 1     |

**Example:**

13 & 7 → 1101 & 0111 = 0101 = 5

---

#### OR (`|`)

| A | B | A OR B |
| - | - | ------ |
| 0 | 0 | 0      |
| 0 | 1 | 1      |
| 1 | 0 | 1      |
| 1 | 1 | 1      |

**Example:**

13 | 7 → 1101 | 0111 = 1111 = 15

---

#### XOR (`^`)

| A | B | A ^ B |
| - | - | ----- |
| 0 | 0 | 0     |
| 0 | 1 | 1     |
| 1 | 0 | 1     |
| 1 | 1 | 0     |

**Rule:** 1 if odd number of 1s else 0

**Example:** 13 ^ 7 = 1101 ^ 0111 = 1010 = 10

**Gotcha:**

XOR with itself → 0

XOR with 0 → number unchanged

---

### 5. Shift Operators

#### Right Shift (`>>`)

Moves bits right by  *k* .

Each shift divides by 2.

Formula: `x >> k = x // (2^k)`

| Expression | Result | Explanation |
| ---------- | ------ | ----------- |
| 13 >> 1    | 6      | 13 / 2      |
| 13 >> 2    | 3      | 13 / 4      |
| 13 >> 4    | 0      | 13 / 16     |

**Gotcha:**

Right shifting a negative number depends on implementation (arithmetic vs logical shift).

---

#### Left Shift (`<<`)

Moves bits left by  *k* .

Each shift multiplies by 2.

Formula: `x << k = x * (2^k)`

| Expression | Result | Explanation |
| ---------- | ------ | ----------- |
| 13 << 1    | 26     | 13 * 2      |
| 13 << 2    | 52     | 13 * 4      |

**Gotcha:**

Left shift can cause  **overflow** .

If shifting beyond 31 bits on 32-bit int, result is undefined.

---

### 6. NOT Operator (`~`)

Flips all bits.

Formula: `~x = -(x + 1)`

| Input | Binary     | Output     | Decimal |
| ----- | ---------- | ---------- | ------- |
| 5     | 000...0101 | 111...1010 | -6      |
| -6    | 111...1010 | 000...0101 | 5       |

**Steps:**

1. Flip bits.
2. If sign bit becomes 1 → take two’s complement to get magnitude.

---

### 7. Storing Signed Numbers

| Bit          | Role                    |
| ------------ | ----------------------- |
| MSB (31st)   | Sign bit (0 = +, 1 = -) |
| Remaining 31 | Value (binary)          |

**Range:**

| Constant | Value                  |
| -------- | ---------------------- |
| INT_MAX  | 2³¹ - 1 = 2147483647 |
| INT_MIN  | -2³¹ = -2147483648   |

**Gotcha:**

`abs(INT_MIN)` overflows (can’t represent +2147483648).

---

### 8. Key Formulas & Properties

| Operation   | Meaning          | Formula / Result           |
| ----------- | ---------------- | -------------------------- |
| AND         | Both bits 1      | `a & b`                  |
| OR          | Either bit 1     | `a                         |
| XOR         | Odd number of 1s | `a ^ b`                  |
| NOT         | Flip all bits    | `~a = -(a + 1)`          |
| Left Shift  | Multiply         | `a << k = a * 2^k`       |
| Right Shift | Divide           | `a >> k = a // 2^k`      |
| XOR swap    | Swap a,b         | `a ^= b; b ^= a; a ^= b` |

---

### 9. Practical Gotchas

| Scenario              | Gotcha                                         |
| --------------------- | ---------------------------------------------- |
| `~0`                | = -1                                           |
| `x & (x-1)`         | Clears rightmost set bit                       |
| `x & (-x)`          | Isolates rightmost set bit                     |
| `1 << n`            | Represents 2ⁿ                                 |
| `x >> k`on negative | May depend on compiler (arithmetic vs logical) |
| Left shift overflow   | Undefined behavior                             |
| Use unsigned types    | To avoid sign-related shifts                   |

---

### 10. Practice Ideas

| Task                  | Hint                                 |
| --------------------- | ------------------------------------ |
| Count set bits        | Use `n & (n-1)`repeatedly          |
| Check even/odd        | `n & 1`                            |
| Convert between bases | Implement `toBinary`,`toDecimal` |
| Reverse bits          | Bit manipulation loop                |
| Use XOR               | Find unique element in array         |
