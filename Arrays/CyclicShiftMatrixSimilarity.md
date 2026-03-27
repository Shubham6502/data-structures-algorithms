 *Matrix Similarity After Cyclic Shifts (Leetcode POTD)*
 
🔹 Problem Idea

We are given a matrix mat and an integer k.

We need to check whether the matrix remains unchanged after applying cyclic shifts:

Even rows (0, 2, 4...) → left shift by k
Odd rows (1, 3, 5...) → right shift by k
🔹 Cyclic Shift Concept
Left shift:
[1, 2, 3, 4] → [2, 3, 4, 1]
Right shift:
[1, 2, 3, 4] → [4, 1, 2, 3]
🔹 Key Observation

Instead of actually shifting the rows, we can use index mapping:

Left shift index:
(j + k) % n
Right shift index:
(j - k + n) % n
🔹 Algorithm
Loop through each row i
Loop through each column j
If row is even → check left shift condition
If row is odd → check right shift condition
If any mismatch → return false
Otherwise → return true
🔹 Java Code
public boolean areSimilar(int[][] mat, int k) {
    int m = mat.length;
    int n = mat[0].length;

    ```
    class Solution {
    public boolean areSimilar(int[][] mat, int k) {
        int m=mat.length;
        int n=mat[0].length;
        k=k%n;
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(i%2==0){
                    if(mat[i][(j+k)%n]!=mat[i][j]){
                        return false;
                    }
                }
                if(i%2==1){
                     if(mat[i][(j-k+n)%n]!=mat[i][j]){
                        return false;
                    }
                }
            }
        }
        return true;

    }
}
    ```
🔹 Time & Space Complexity
Time Complexity: O(m × n)
Space Complexity: O(1) (no extra space used)
