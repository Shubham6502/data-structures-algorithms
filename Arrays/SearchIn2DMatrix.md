# 🔍 Search in 2D Matrix

## 📌 Problem Statement

Given a **2D matrix** where:

* Each row is sorted in ascending order
* Each column is sorted in ascending order

Write a function to determine if a given `target` exists in the matrix.

---

## 💡 Approach (Optimal Solution)

We use a **Top-Right Corner Strategy**:

1. Start from the **top-right element**
2. Compare it with the target:

   * If equal → ✅ Found
   * If greater → move **left**
   * If smaller → move **down**
3. Repeat until found or out of bounds

---

## ⚡ Time & Space Complexity

| Complexity | Value        |
| ---------- | ------------ |
| Time       | **O(n + m)** |
| Space      | **O(1)**     |

---

## 🧠 Intuition

* Moving **left** reduces value
* Moving **down** increases value
* This helps eliminate one row or column at each step

---

## 🧾 Java Implementation

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int n = matrix.length;
        int m = matrix[0].length;

        int i = 0;
        int j = m - 1;

        while (i < n && j >= 0) {
            if (matrix[i][j] == target) {
                return true;
            } else if (matrix[i][j] > target) {
                j--; // move left
            } else {
                i++; // move down
            }
        }
        return false;
    }
}
```

---

## 📊 Example

### Input:

```
matrix = [
  [1, 4, 7],
  [2, 5, 8],
  [3, 6, 9]
]
target = 5
```

### Output:

```
true
```

---

## 🚀 Key Takeaways

* Efficient alternative to brute force
* Uses matrix properties for optimization
* Common interview question

---

## 🏷️ Tags

`Array` `Matrix` `Binary Search (variant)` `Greedy`
