# 📘 Minimum Absolute Difference in K×K Subgrid

## 🔹 Problem Statement

Given a 2D grid of integers and an integer `k`,  
for every **k × k subgrid**, compute the **minimum absolute difference between any two distinct elements**.

Return a 2D array `ans` where:
- `ans[i][j]` represents the result for subgrid starting at `(i, j)`.

---

## 🧠 Approach

### Step 1: Iterate over all valid subgrids
- Only go till:
  - `i <= m - k`
  - `j <= n - k`

### Step 2: Collect elements of subgrid
- Use a **TreeSet** to:
  - Automatically sort elements
  - Remove duplicates

### Step 3: Compute minimum difference
- Traverse sorted values
- Find minimum difference between consecutive elements

---

## ✅ Java Code

```java
import java.util.*;

class Solution {
    public int[][] minAbsDiff(int[][] grid, int k) {

        int m = grid.length;
        int n = grid[0].length;

        int[][] ans = new int[m - k + 1][n - k + 1];

        for (int i = 0; i <= m - k; i++) {
            for (int j = 0; j <= n - k; j++) {

                TreeSet<Integer> set = new TreeSet<>();

                // collect k x k subgrid elements
                for (int r = i; r < i + k; r++) {
                    for (int c = j; c < j + k; c++) {
                        set.add(grid[r][c]);
                    }
                }

                // if only one unique element
                if (set.size() <= 1) {
                    ans[i][j] = 0;
                    continue;
                }

                int minDiff = Integer.MAX_VALUE;

                Integer prev = null;

                for (int val : set) {
                    if (prev != null) {
                        minDiff = Math.min(minDiff, val - prev);
                    }
                    prev = val;
                }

                ans[i][j] = minDiff;
            }
        }
```
⏱️ Time Complexity

Each subgrid → k × k elements

TreeSet operations → log(k²)

Overall:
O(m * n * k² log(k²))
        return ans;
    }
}
