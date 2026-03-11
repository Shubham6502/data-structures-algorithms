# Number Complement (Bitwise Complement)-Leetcode(1009)

## Problem

Given an integer `n`, return the **bitwise complement** of its binary representation.

The complement of a number is obtained by **flipping all bits** in its binary form:

- `1 → 0`
- `0 → 1`

---

## Example

Input:

```
n = 5
```

Binary representation of `5`:

```
101
```

Flip all bits:

```
010
```

Convert back to decimal:

```
2
```

Output:

```
2
```

---

# Approach

Instead of manually flipping bits, we create a **mask of all 1s** with the same number of bits as `n`.

Steps:

1. Find the **highest set bit** of `n`.
2. Create a **mask of all 1s** up to that bit.
3. XOR the mask with the number to flip the bits.

Formula:

```
complement = mask ^ n
```

Where:

```
mask = (Integer.highestOneBit(n) << 1) - 1
```

Explanation:

- `Integer.highestOneBit(n)` → largest power of 2 in `n`
- `<< 1` → shift left to create the next power
- `- 1` → create a mask of all 1s
- `^` → XOR flips the bits

---

# Example Walkthrough

For:

```
n = 5
```

Binary:

```
101
```

### Step 1 — Highest One Bit

```
Integer.highestOneBit(5) = 4
```

Binary:

```
100
```

### Step 2 — Shift Left

```
100 << 1 = 1000
```

Decimal:

```
8
```

### Step 3 — Subtract 1

```
8 - 1 = 7
```

Binary mask:

```
111
```

### Step 4 — XOR with number

```
111
101
----
010
```

Result:

```
2
```

---

# Java Implementation

```java
class Solution {
    
    public int bitwiseComplement(int n) {
        if(n == 0) return 1;
        return ((Integer.highestOneBit(n) << 1) - 1) ^ n;
    }
}
```

---

# Complexity Analysis

| Type | Complexity |
|------|------------|
| Time Complexity | **O(1)** |
| Space Complexity | **O(1)** |

---

# Key Bit Manipulation Concepts

This problem uses:

- **Bitwise XOR (`^`)**
- **Left Shift (`<<`)**
- **Mask creation**
- **Highest set bit**

These tricks are commonly used in **coding interviews and competitive programming**.

---

# Interview Tip

To find the complement:

1. Create a mask of `111...` up to the number of bits.
2. XOR with the number.

```
Complement = Mask ^ Number
```

This flips all bits efficiently.
