# 3212. Count Submatrices With Equal Frequency of X and Y

## 📌 Problem Summary
Given a 2D grid containing characters `'X'`, `'Y'`, or others, count the number of submatrices (from `(0,0)` to `(i,j)`) where:

- Number of `'X'` = Number of `'Y'`
- Count of `'X'` > 0 (to avoid empty matches)

---

## 💡 Approach

We use **2D Prefix Sum** to efficiently count occurrences of `'X'` and `'Y'` up to each cell.

### Key Idea:
- Maintain two prefix sum matrices:
  - `SumX[i][j]` → count of `'X'` from `(0,0)` to `(i,j)`
  - `SumY[i][j]` → count of `'Y'` from `(0,0)` to `(i,j)`

- For every cell `(i, j)`:
  - Compute prefix sums
  - Check:
    ```
    SumX[i][j] == SumY[i][j] && SumX[i][j] > 0
    ```
  - If true → increment count

---

## ⚙️ Code (Java)

```java
class Solution { 
    public int numberOfSubmatrices(char[][] grid) {
        int m = grid.length;
        int n = grid[0].length;

        int[][] SumX = new int[m][n];
        int[][] SumY = new int[m][n];

        int count = 0;

        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){

                // Initialize current cell
                SumX[i][j] = grid[i][j] == 'X' ? 1 : 0;
                SumY[i][j] = grid[i][j] == 'Y' ? 1 : 0;

                // Add top
                if(i > 0){
                    SumX[i][j] += SumX[i-1][j];
                    SumY[i][j] += SumY[i-1][j];
                }

                // Add left
                if(j > 0){
                    SumX[i][j] += SumX[i][j-1];
                    SumY[i][j] += SumY[i][j-1];
                }

                // Remove overlap
                if(i > 0 && j > 0){
                    SumX[i][j] -= SumX[i-1][j-1];
                    SumY[i][j] -= SumY[i-1][j-1];
                }

                // Check condition
                if(SumX[i][j] == SumY[i][j] && SumX[i][j] > 0){
                    count++;
                }
            }
        }

        return count;
    }
}
```
⏱️ Complexity

Time Complexity: O(m × n)

Space Complexity: O(m × n)

🧠 Key Learnings

2D Prefix Sum helps reduce repeated computation

Always subtract overlap (i-1, j-1)

Useful for submatrix-based problems
