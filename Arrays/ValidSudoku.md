# Valid Sudoku - HashSet Approach (Java)

## Problem Statement

Determine if a `9 x 9` Sudoku board is valid.

A valid Sudoku board must satisfy:
- Each row contains digits `1-9` without repetition.
- Each column contains digits `1-9` without repetition.
- Each `3 x 3` sub-box contains digits `1-9` without repetition.
- Empty cells are represented by `'.'`.

---

## Approach

We use three arrays of `HashSet<Character>`:

- `row[9]` → Stores digits present in each row.
- `col[9]` → Stores digits present in each column.
- `boxes[9]` → Stores digits present in each 3×3 box.

For every cell:

1. Ignore the cell if it contains `'.'`.
2. Check whether the digit already exists in its row.
3. Check whether the digit already exists in its column.
4. Compute the corresponding 3×3 box index and check for duplicates.
5. If a duplicate is found, return `false`.
6. Otherwise, insert the digit into the respective row, column, and box HashSets.

If all cells are processed without finding duplicates, return `true`.

---

## Box Index Formula

```java
int idx = (i / 3) * 3 + (j / 3);
```

### Box Mapping

```
0 1 2
3 4 5
6 7 8
```

Example:

| Cell (i, j) | Box Index |
|--------------|-----------|
| (0,0) | 0 |
| (1,2) | 0 |
| (2,1) | 0 |
| (4,4) | 4 |
| (8,8) | 8 |

---

## Java Solution

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {

        HashSet<Character>[] row = new HashSet[9];
        HashSet<Character>[] col = new HashSet[9];
        HashSet<Character>[] boxes = new HashSet[9];

        for (int i = 0; i < 9; i++) {
            row[i] = new HashSet<>();
            col[i] = new HashSet<>();
            boxes[i] = new HashSet<>();
        }

        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {

                char ch = board[i][j];

                if (ch == '.')
                    continue;

                // Check Row
                if (row[i].contains(ch))
                    return false;
                row[i].add(ch);

                // Check Column
                if (col[j].contains(ch))
                    return false;
                col[j].add(ch);

                // Check 3x3 Box
                int idx = (i / 3) * 3 + (j / 3);

                if (boxes[idx].contains(ch))
                    return false;
                boxes[idx].add(ch);
            }
        }

        return true;
    }
}
```



## Time Complexity

- Traversing the board: **O(9 × 9) = O(81)**
- HashSet operations (`contains()` and `add()`) are **O(1)** on average.

**Overall Time Complexity:** `O(81) = O(1)`

---

## Space Complexity

Three arrays of 9 HashSets are used.

Maximum stored elements:

- Rows → 81
- Columns → 81
- Boxes → 81

**Overall Space Complexity:** `O(81) = O(1)`

---

