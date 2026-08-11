# Word Search

## Problem

Given a 2D board and a word, check whether the word exists in the board.

You can move **up, down, left, or right**, and a cell cannot be used more than once.

## Approach

Use **DFS + Backtracking**:

- Start DFS from every cell.
- If the character does not match, return `false`.
- Mark the current cell as visited.
- Explore all 4 directions.
- Restore the cell after recursion.

### Code

```java
class Solution {

    public boolean find(char[][] board, String word, int i, int j, int idx) {

        if(i < 0 || i >= board.length ||
           j < 0 || j >= board[0].length) {
            return false;
        }

        if(board[i][j] != word.charAt(idx)) {
            return false;
        }

        if(idx == word.length() - 1) {
            return true;
        }

        char temp = board[i][j];
        board[i][j] = '#';

        boolean found =
            find(board, word, i + 1, j, idx + 1) ||
            find(board, word, i - 1, j, idx + 1) ||
            find(board, word, i, j + 1, idx + 1) ||
            find(board, word, i, j - 1, idx + 1);

        board[i][j] = temp; // Backtrack

        return found;
    }

    public boolean exist(char[][] board, String word) {

        for(int i = 0; i < board.length; i++) {
            for(int j = 0; j < board[0].length; j++) {

                if(find(board, word, i, j, 0)) {
                    return true;
                }
            }
        }

        return false;
    }
}
```

### Key Idea  
Choose → Mark Visited → Explore → Backtrack  

### Complexity  
Time: O(m × n × 4^L)  
Space: O(L)  

m × n = board size, L = word length.  
