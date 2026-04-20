# Spiral Matrix Traversal (Java)

## 📌 Problem Statement

Given a 2D matrix, return all elements of the matrix in **spiral order**.

### Example

**Input:**

```
matrix = [
 [1, 2, 3],
 [4, 5, 6],
 [7, 8, 9]
]
```

**Output:**

```
[1, 2, 3, 6, 9, 8, 7, 4, 5]
```

---

## 🚀 Approach

We use **four pointers** to track the current boundaries of the matrix:

* `rowStart` → top boundary
* `rowEnd` → bottom boundary
* `colStart` → left boundary
* `colEnd` → right boundary

### Traversal Steps:

1. Traverse **left → right** (top row)
2. Traverse **top → bottom** (right column)
3. Traverse **right → left** (bottom row) *(if rows remain)*
4. Traverse **bottom → top** (left column) *(if columns remain)*

After completing one layer, shrink boundaries inward.

---

## 🧠 Key Conditions

* `if (rowStart == rowEnd)` → avoid duplicate row traversal
* `if (colStart == colEnd)` → avoid duplicate column traversal

---

## 💻 Code (Java)

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        int rowStart = 0, rowEnd = matrix.length - 1;
        int colStart = 0, colEnd = matrix[0].length - 1;
        
        ArrayList<Integer> result = new ArrayList<>();

        while (rowStart <= rowEnd && colStart <= colEnd) {
            
            // Traverse top row
            for (int i = colStart; i <= colEnd; i++) {
                result.add(matrix[rowStart][i]);
            }

            // Traverse right column
            for (int i = rowStart + 1; i <= rowEnd; i++) {
                result.add(matrix[i][colEnd]);
            }

            // Traverse bottom row
            for (int i = colEnd - 1; i >= colStart; i--) {
                if (rowStart == rowEnd) break;
                result.add(matrix[rowEnd][i]);
            }

            // Traverse left column
            for (int i = rowEnd - 1; i >= rowStart + 1; i--) {
                if (colStart == colEnd) break;
                result.add(matrix[i][colStart]);
            }

            // Move boundaries inward
            rowStart++;
            rowEnd--;
            colStart++;
            colEnd--;
        }

        return result;
    }
}
```

---

## ⏱️ Complexity Analysis

* **Time Complexity:** `O(m × n)`
  (Each element is visited once)

* **Space Complexity:** `O(1)`
  (Ignoring output list)

---

