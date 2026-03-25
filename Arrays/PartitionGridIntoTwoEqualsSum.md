# 🧩 Partition Grid Into Two Equal Sum Parts

## 📌 Problem Statement
Given a 2D grid of integers, determine whether it is possible to partition the grid into two parts such that:

- The partition is either:
  - **Horizontal** (cut between rows), or
  - **Vertical** (cut between columns)
- Both parts have **equal sum**
- Both parts must be **non-empty**

---

## 💡 Approach

### Step 1: Compute Row and Column Sums
- `row[i]` → sum of elements in row `i`
- `col[j]` → sum of elements in column `j`

---

### Step 2: Compute Total Sum
- `totalRow` = sum of all row sums
- `totalCol` = sum of all column sums

---

### Step 3: Try Horizontal Partition
- Iterate from `0 → m-2`
- Maintain prefix sum of rows
- Check:

- prefix == totalRow - prefix


---

### Step 4: Try Vertical Partition
- Iterate from `0 → n-2`
- Maintain prefix sum of columns
- Check:


prefix == totalCol - prefix


## ✅ Final Code

```java
class Solution {
  public boolean canPartitionGrid(int[][] grid) {
      int m = grid.length;
      int n = grid[0].length;

      long[] row = new long[m];
      long[] col = new long[n];

      // Step 1: Calculate row and column sums
      for (int i = 0; i < m; i++) {
          for (int j = 0; j < n; j++) {
              row[i] += grid[i][j];
              col[j] += grid[i][j];
          }
      }

      // Step 2: Total sums
      long totalRow = 0;
      for (long r : row) totalRow += r;

      long totalCol = 0;
      for (long c : col) totalCol += c;

      // Step 3: Horizontal cut
      long prefix = 0;
      for (int i = 0; i < m - 1; i++) {
          prefix += row[i];
          if (prefix == totalRow - prefix) return true;
      }

      // Step 4: Vertical cut
      prefix = 0;
      for (int j = 0; j < n - 1; j++) {
          prefix += col[j];
          if (prefix == totalCol - prefix) return true;
      }

      return false;
  }
}
```
⏱️ Complexity
Time Complexity: O(m × n)
Space Complexity: O(m + n)
  
