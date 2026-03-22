# 🔄 Matrix Rotation Problem (LeetCode - Find Rotation 1886)

## 📌 Problem Statement
Given two `n x n` matrices `mat` and `target`, return `true` if `mat` can be rotated (in 90° increments) to match `target`, otherwise return `false`.

---

## 🚀 Approach

We need to check all possible rotations:
- 0° (original matrix)
- 90°
- 180°
- 270°

### Steps:
1. Compare `mat` with `target`
2. If equal → return `true`
3. Otherwise, rotate matrix by 90°
4. Repeat this process 4 times

---

## 🔁 Rotation Logic (90° Clockwise)

For rotating matrix:

rotation[j][n - 1 - i] = mat[i][j];


---

## ✅ Java Solution

```java
class Solution {
    public boolean findRotation(int[][] mat, int[][] target) {
        int n = mat.length;

        for (int k = 0; k < 4; k++) {

            // Step 1: Check if current matrix matches target
            if (isEqual(mat, target)) return true;

            // Step 2: Rotate matrix 90° clockwise
            int[][] rotation = new int[n][n];
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    rotation[j][n - 1 - i] = mat[i][j];
                }
            }

            // Step 3: Update matrix for next rotation
            mat = rotation;
        }

        return false;
    }

    // Helper function to compare two matrices
    private boolean isEqual(int[][] mat, int[][] target) {
        int n = mat.length;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (mat[i][j] != target[i][j]) return false;
            }
        }
        return true;
    }
}
```
⏱ Time Complexity
O(n²) per rotation
Total = O(4 × n²) = O(n²)
