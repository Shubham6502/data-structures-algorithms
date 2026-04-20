# Search in 2D Matrix (Efficient Approach)

## 📌 Problem Statement

Given a **sorted 2D matrix**, determine if a target value exists in the matrix.

### Matrix Properties:

* Each row is sorted in ascending order
* Each column is sorted in ascending order

---

## 🧾 Example

**Input:**

```id="in1"
matrix = [
 [1, 4, 7, 11],
 [2, 5, 8, 12],
 [3, 6, 9, 16],
 [10,13,14,17]
]
target = 5
```

**Output:**

```id="out1"
true
```

---

## 🚀 Approach (Top-Right Search)

We start from the **top-right corner** of the matrix.

### Why Top-Right?

* Moving **left** → values decrease
* Moving **down** → values increase

This helps eliminate rows or columns efficiently.

---

## 🔄 Steps

1. Start at `(i = 0, j = last column)`
2. Compare `matrix[i][j]` with target:

   * If equal → return `true`
   * If current value > target → move **left** (`j--`)
   * If current value < target → move **down** (`i++`)
3. Repeat until out of bounds

---

## 💻 Code (Java)

```java id="code1"
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int i = 0;
        int j = matrix[0].length - 1;

        while (i < matrix.length && j >= 0) {
            if (target == matrix[i][j]) {
                return true;
            } else if (matrix[i][j] > target) {
                j--;  // Move left
            } else {
                i++;  // Move down
            }
        }
        return false;
    }
}
```

---

## ⏱️ Complexity Analysis

* **Time Complexity:** `O(m + n)`
  (At most one pass across rows and columns)

* **Space Complexity:** `O(1)`
  (No extra space used)

---

