# Reverse Submatrix in a Grid

## 📌 Problem Statement
Given a 2D matrix `grid`, and three integers:
- `x` → starting row
- `y` → starting column
- `k` → size of submatrix

Reverse the rows of the `k × k` submatrix starting at position `(x, y)`.

---


```
class Solution {
    public int[][] reverseSubmatrix(int[][] grid, int x, int y, int k) {

        // Reverse rows inside k x k submatrix
        for (int i = 0; i < k / 2; i++) {
            for (int j = 0; j < k; j++) {

                int temp = grid[x + i][y + j];
                grid[x + i][y + j] = grid[x + k - 1 - i][y + j];
                grid[x + k - 1 - i][y + j] = temp;
            }
        }

        return grid;
    }
}
```
```
🧠 Example
Input:
1 2 3
4 5 6
7 8 9
Output after reversing rows:
7 8 9
4 5 6
1 2 34
```
🎯 Key Points
Loop only within the submatrix (k)
Use k / 2 for row reversal
Swap using:
x + i
x + k - 1 - i
⏱️ Time Complexity
O(k²) → Only submatrix is processed
📦 Space Complexity
O(1) → In-place swapping
