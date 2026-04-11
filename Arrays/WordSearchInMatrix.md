# Word Search (Backtracking)

## 📌 Problem Statement
Given a 2D board of characters and a string `word`, return `true` if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where:
- Adjacent cells are **horizontally or vertically neighboring**
- The same cell **cannot be used more than once**



## 🚀 Approach (Backtracking + DFS)

This solution uses **Depth-First Search (DFS)** with **Backtracking**.

### Key Idea:
- Start from every cell in the board
- Try to match the word character by character
- Explore all 4 possible directions:
  - Up, Down, Left, Right
- Mark visited cells temporarily to avoid reuse
- Backtrack after exploring each path


## 🧠 Algorithm Steps

1. Loop through each cell in the board
2. If the current cell matches the first character:
   - Start DFS (`find` function)
3. In DFS:
   - Check boundaries
   - Check if character matches
   - If last character is reached → return `true`
   - Mark cell as visited (`'#'`)
   - Explore all 4 directions
   - Restore the original value (backtrack)
4. If any path returns `true`, return `true`
5. Otherwise, return `false`



## 💻 Code Implementation

```java
class Solution {

    public boolean find(char[][] board, String word, int i, int j, int k) {
        if (i >= board.length || j >= board[0].length || i < 0 || j < 0)
            return false;

        char ch = board[i][j];
        if (ch != word.charAt(k))
            return false;

        if (k == word.length() - 1)
            return true;

        char temp = ch;
        board[i][j] = '#';

        boolean found = find(board, word, i + 1, j, k + 1) ||
                        find(board, word, i - 1, j, k + 1) ||
                        find(board, word, i, j + 1, k + 1) ||
                        find(board, word, i, j - 1, k + 1);

        board[i][j] = temp; // backtrack

        return found;
    }

    public boolean exist(char[][] board, String word) {
        int n = board.length;
        int m = board[0].length;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (find(board, word, i, j, 0)) {
                    return true;
                }
            }
        }
        return false;
    }
}
```
⏱️ Time Complexity-
O(n × m × 4^L)

n × m → starting points

4^L → exploring 4 directions for each character of length L

💾 Space Complexity -
O(L) → recursion stack (depth of the word)
