# Set Matrix Zeroes

## 📌 Problem Statement
Given an `m x n` integer matrix, if an element is `0`, set its entire row and column to `0`.  
You must modify the matrix **in-place**.



## 🚀 Approach (Using HashSet)

This solution uses two `HashSet`s to track which rows and columns need to be set to zero.

### Key Idea:
- Traverse the matrix and store:
  - Row indices in `itrack` where `0` is found
  - Column indices in `jtrack` where `0` is found
- In the second pass:
  - Set all elements of tracked rows to `0`
  - Set all elements of tracked columns to `0`


## 🧠 Algorithm Steps

1. Initialize two sets:
   - `itrack` → stores row indices
   - `jtrack` → stores column indices

2. Traverse the matrix:
   - If `matrix[i][j] == 0`
     - Add `i` to `itrack`
     - Add `j` to `jtrack`

3. Set rows to zero:
   - For each row index in `itrack`, set all columns to `0`

4. Set columns to zero:
   - For each column index in `jtrack`, set all rows to `0`



## 💻 Code Implementation

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        Set<Integer> itrack = new HashSet<>();
        Set<Integer> jtrack = new HashSet<>();

        int n = matrix.length, m = matrix[0].length;

        // Step 1: Track rows and columns with zero
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j] == 0) {
                    itrack.add(i);
                    jtrack.add(j);
                }
            }
        }

        // Step 2: Set entire rows to zero
        for (int i : itrack) {
            for (int j = 0; j < m; j++) {
                matrix[i][j] = 0;
            }
        }

        // Step 3: Set entire columns to zero
        for (int i = 0; i < n; i++) {
            for (int j : jtrack) {
                matrix[i][j] = 0;
            }
        }
    }
}
```
⏱️ Time Complexity

O(n × m) → Traverse matrix twice

💾 Space Complexity

O(n + m) → For storing row and column indices
