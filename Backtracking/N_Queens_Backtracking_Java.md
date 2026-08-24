# N-Queens --- Java Backtracking Solution

## Problem

Given an integer `n`, place `n` queens on an `n × n` chessboard so that
no two queens attack each other.

A queen can attack another queen if they are in the same:

-   Row
-   Column
-   Main diagonal (`\`)
-   Anti-diagonal (`/`)

The solution uses **Backtracking**.

------------------------------------------------------------------------

## Complete Java Code

``` java
class Solution {

    public boolean isSafe(char [][]matrix,int row,int col){

        for(int i=col;i>=0;i--){
            if(matrix[row][i]=='Q') return false;
        }

        for(int i=row;i>=0;i--){
            if(matrix[i][col]=='Q') return false;
        }

        for(int i=row-1,j=col-1;i>=0 && j>=0;i--,j--){
            if(matrix[i][j]=='Q') return false;
        }

        for(int i=row-1,j=col+1;i>=0 && j<matrix.length;i--,j++){
            if(matrix[i][j]=='Q') return false;
        }

        return true;
    }

    public void solve(char [][]matrix,List<List<String>>res,int row,int n){

        if(row==n){

            List<String>list=new ArrayList<>();

            for(char[] c:matrix){
                list.add(new String(c));
            }

            res.add(list);
            return;
        }

        for(int col=0;col<n;col++){

            if(isSafe(matrix,row,col)){

                matrix[row][col]='Q';

                solve(matrix,res,row+1,n);

                matrix[row][col]='.';
            }
        }
    }

    public List<List<String>> solveNQueens(int n) {

        char matrix[][]=new char[n][n];

        List<List<String>>res=new ArrayList<>();

        for(int i=0;i<n;i++){
            Arrays.fill(matrix[i],'.');
        }

        solve(matrix,res,0,n);

        return res;
    }
}
```

------------------------------------------------------------------------

# 1. Imports

For a standalone Java program, you need:

``` java
import java.util.*;
```

On LeetCode, imports are usually already available, so the `Solution`
class can be submitted directly.

------------------------------------------------------------------------

# 2. Class Declaration

``` java
class Solution {
```

This is the class required by LeetCode.

------------------------------------------------------------------------

# 3. `isSafe()` Method

``` java
public boolean isSafe(char [][]matrix,int row,int col)
```

### Purpose

Checks whether a queen can safely be placed at:

``` text
(row, col)
```

It checks four directions:

1.  Left in the same row
2.  Up in the same column
3.  Upper-left diagonal
4.  Upper-right diagonal

We do **not** need to check:

-   Right side of the row
-   Below the current row

because the algorithm places queens **row by row from top to bottom**.

Therefore, those positions have not been processed yet.

------------------------------------------------------------------------

# 4. Check Left Side of Current Row

``` java
for(int i=col;i>=0;i--){
    if(matrix[row][i]=='Q') return false;
}
```

### Meaning

Start from the current column and move toward the left.

``` text
(row, col)
(row, col-1)
(row, col-2)
...
(row, 0)
```

If a queen is found:

``` java
if(matrix[row][i]=='Q')
```

the position is unsafe.

``` java
return false;
```

### Example

``` text
. . Q .
. . . .
. . . .
. . . .
```

Trying to place another queen in the same row:

``` text
. . Q Q
```

is invalid.

------------------------------------------------------------------------

# 5. Check Upper Side of Current Column

``` java
for(int i=row;i>=0;i--){
    if(matrix[i][col]=='Q') return false;
}
```

### Meaning

Move upward in the same column.

``` text
(row, col)
(row-1, col)
(row-2, col)
...
(0, col)
```

If a queen exists there, the new queen would attack it vertically.

------------------------------------------------------------------------

# 6. Check Upper-Left Diagonal

``` java
for(int i=row-1,j=col-1;
    i>=0 && j>=0;
    i--,j--){

    if(matrix[i][j]=='Q')
        return false;
}
```

The upper-left diagonal moves:

``` text
row - 1
col - 1
```

Example:

``` text
Q . . .
. Q . .
. . ? .
. . . .
```

The `?` position is attacked by the queens on its upper-left diagonal.

### Why start from `row-1`?

``` java
row-1
```

We don't need to check the current cell because it has not received the
new queen yet.

------------------------------------------------------------------------

# 7. Check Upper-Right Diagonal

``` java
for(int i=row-1,j=col+1;
    i>=0 && j<matrix.length;
    i--,j++){

    if(matrix[i][j]=='Q')
        return false;
}
```

The movement is:

``` text
row - 1
col + 1
```

So we move diagonally upward and toward the right.

Example:

``` text
. . . Q
. . ? .
. . . .
. . . .
```

The `?` position is attacked by the queen above-right.

------------------------------------------------------------------------

# 8. Return `true`

``` java
return true;
```

If none of the four checks found a queen, the position is safe.

------------------------------------------------------------------------

# 9. `solve()` Method

``` java
public void solve(
    char [][]matrix,
    List<List<String>>res,
    int row,
    int n
)
```

This is the main **backtracking function**.

### Parameters

  Parameter   Meaning
  ----------- ----------------------------
  `matrix`    Chessboard
  `res`       Stores all valid solutions
  `row`       Current row
  `n`         Board size

------------------------------------------------------------------------

# 10. Base Case

``` java
if(row==n){
```

When:

``` text
row == n
```

it means queens have been successfully placed in all `n` rows.

For example, for `n = 4`:

``` text
row = 0 → first row
row = 1 → second row
row = 2 → third row
row = 3 → fourth row
row = 4 → finished
```

------------------------------------------------------------------------

# 11. Create a Solution List

``` java
List<String>list=new ArrayList<>();
```

Each chessboard solution is represented as:

``` text
[
    ".Q..",
    "...Q",
    "Q...",
    "..Q."
]
```

So we create a `List<String>`.

------------------------------------------------------------------------

# 12. Convert `char[]` to `String`

``` java
for(char[] c:matrix){
    list.add(new String(c));
}
```

The board is:

``` java
char[][]
```

but the expected LeetCode output is:

``` java
List<List<String>>
```

Therefore:

``` java
new String(c)
```

converts each row from:

``` text
['.', 'Q', '.', '.']
```

to:

``` text
".Q.."
```

------------------------------------------------------------------------

# 13. Add Solution to Result

``` java
res.add(list);
```

The completed board is added to the final result.

Then:

``` java
return;
```

stops this recursive call.

------------------------------------------------------------------------

# 14. Try Every Column

``` java
for(int col=0;col<n;col++){
```

For the current row, we try placing the queen in every column.

For `n = 4`:

``` text
col = 0
col = 1
col = 2
col = 3
```

------------------------------------------------------------------------

# 15. Check Whether Position Is Safe

``` java
if(isSafe(matrix,row,col)){
```

Only place a queen if the position does not conflict with previously
placed queens.

------------------------------------------------------------------------

# 16. Place the Queen

``` java
matrix[row][col]='Q';
```

Example:

``` text
. . . .
. Q . .
. . . .
. . . .
```

The queen is temporarily placed.

This is the **choice** step of backtracking.

------------------------------------------------------------------------

# 17. Recursive Call

``` java
solve(matrix,res,row+1,n);
```

Move to the next row.

For example:

``` text
solve(row 0)
    ↓
solve(row 1)
    ↓
solve(row 2)
    ↓
solve(row 3)
    ↓
solve(row 4)
```

When `row == n`, a complete solution has been found.

------------------------------------------------------------------------

# 18. Backtracking

``` java
matrix[row][col]='.';
```

This is the most important line in the algorithm.

After exploring all possibilities with the current queen position,
remove the queen.

This allows the algorithm to try another column.

### Example

First:

``` text
Q . . .
. . . .
. . . .
. . . .
```

After exploring this choice:

``` text
. . . .
. . . .
. . . .
. . . .
```

Then the algorithm tries the next position.

------------------------------------------------------------------------

# 19. Why Backtracking Is Needed

The algorithm follows:

``` text
Choose
  ↓
Explore
  ↓
If successful → save solution
  ↓
If unsuccessful → undo
  ↓
Try another choice
```

In code:

``` java
if(isSafe(...)) {

    // Choose
    matrix[row][col] = 'Q';

    // Explore
    solve(...);

    // Undo
    matrix[row][col] = '.';
}
```

This is the standard backtracking pattern.

------------------------------------------------------------------------

# 20. `solveNQueens()` Method

``` java
public List<List<String>> solveNQueens(int n)
```

This is the method called by LeetCode.

It:

1.  Creates the board
2.  Fills it with `.`
3.  Starts backtracking
4.  Returns all solutions

------------------------------------------------------------------------

# 21. Create the Chessboard

``` java
char matrix[][]=new char[n][n];
```

For:

``` text
n = 4
```

this creates:

``` text
. . . .
. . . .
. . . .
. . . .
```

The actual `char` array initially contains null characters, so we fill
it with `.` next.

------------------------------------------------------------------------

# 22. Create Result List

``` java
List<List<String>>res=new ArrayList<>();
```

This stores every valid board.

The structure is:

``` text
List
 ├── Solution 1
 │    ├── Row 1
 │    ├── Row 2
 │    ├── Row 3
 │    └── Row 4
 │
 ├── Solution 2
 │    ├── Row 1
 │    ├── Row 2
 │    ├── Row 3
 │    └── Row 4
 │
 └── ...
```

------------------------------------------------------------------------

# 23. Fill Board With Dots

``` java
for(int i=0;i<n;i++){
    Arrays.fill(matrix[i],'.');
}
```

Every cell becomes:

``` text
.
```

For `n = 4`:

``` text
. . . .
. . . .
. . . .
. . . .
```

`Arrays.fill()` fills the entire row with `.`.

------------------------------------------------------------------------

# 24. Start Backtracking

``` java
solve(matrix,res,0,n);
```

We start from:

``` text
row = 0
```

because rows are zero-indexed.

------------------------------------------------------------------------

# 25. Return Result

``` java
return res;
```

After all possible arrangements have been explored, return the list of
valid solutions.

------------------------------------------------------------------------

# Complete Flow

The entire algorithm works like this:

``` text
solveNQueens(n)
       |
       ↓
Create n × n board
       |
       ↓
Fill board with '.'
       |
       ↓
solve(row = 0)
       |
       ↓
Try every column
       |
       ↓
Is position safe?
     /       \
   No         Yes
   |           |
Skip      Place 'Q'
               |
               ↓
         solve(row + 1)
               |
               ↓
       All rows completed?
          /          \
        No            Yes
        |              |
     Continue       Save board
                       |
                       ↓
                  Backtrack
                       |
                       ↓
                 Remove 'Q'
                       |
                       ↓
                Try next column
```

------------------------------------------------------------------------

# Example: N = 4

One valid solution is:

``` text
.Q..
...Q
Q...
..Q.
```

Another valid solution is:

``` text
..Q.
Q...
...Q
.Q..
```

The algorithm finds all valid configurations.

------------------------------------------------------------------------

# Why We Only Check Previous Rows

A common question is:

> Why don't we check the entire board?

Because the algorithm always places queens from:

``` text
row 0 → row 1 → row 2 → ... → row n-1
```

When we are currently placing a queen in row `row`, there cannot be any
queens below it yet.

Therefore, we only need to check:

``` text
← Left
↑ Up
↖ Upper-left
↗ Upper-right
```

This is enough.

------------------------------------------------------------------------

# Time Complexity

The worst-case time complexity is approximately:

``` text
O(N!)
```

The algorithm tries many permutations of queen placements.

The `isSafe()` method itself takes:

``` text
O(N)
```

because it may scan a row, column, and diagonals.

So the practical complexity is often described as approximately:

``` text
O(N × N!)
```

depending on how the recursive search is analyzed.

------------------------------------------------------------------------

# Space Complexity

The board requires:

``` text
O(N²)
```

space.

The recursion depth is:

``` text
O(N)
```

and the result list requires additional space proportional to the number
of solutions.

Ignoring the output:

``` text
O(N²)
```

auxiliary board space.

------------------------------------------------------------------------

# Important Java Syntax Used

## 1. Two-Dimensional Character Array

``` java
char[][] matrix = new char[n][n];
```

------------------------------------------------------------------------

## 2. Generic List

``` java
List<List<String>> res = new ArrayList<>();
```

Meaning:

``` text
List
  → List<String>
      → String
```

------------------------------------------------------------------------

## 3. Enhanced For Loop

``` java
for(char[] c : matrix)
```

This means:

``` text
for every row c inside matrix
```

------------------------------------------------------------------------

## 4. Standard For Loop

``` java
for(int i = 0; i < n; i++)
```

Structure:

``` java
for(initialization; condition; update)
```

------------------------------------------------------------------------

## 5. Multiple Variables in a For Loop

``` java
for(int i=row-1, j=col-1;
    i>=0 && j>=0;
    i--, j--)
```

Here two variables are updated together:

``` java
i--
j--
```

This is useful for diagonal traversal.

------------------------------------------------------------------------

## 6. Character Comparison

``` java
matrix[row][i] == 'Q'
```

`'Q'` is a `char`.

For an empty cell:

``` java
'.'
```

------------------------------------------------------------------------

## 7. Character Assignment

``` java
matrix[row][col] = 'Q';
```

Place queen.

``` java
matrix[row][col] = '.';
```

Remove queen during backtracking.

------------------------------------------------------------------------

## 8. Convert Character Array to String

``` java
new String(c)
```

Example:

``` java
char[] c = {'.', 'Q', '.', '.'};

String s = new String(c);
```

Result:

``` text
.Q..
```

------------------------------------------------------------------------

## 9. `Arrays.fill()`

``` java
Arrays.fill(matrix[i], '.');
```

Fills the entire array with the specified value.

------------------------------------------------------------------------

# Key Backtracking Template

You can remember N-Queens using this pattern:

``` java
for(each possible choice){

    if(choice is valid){

        make choice;

        backtrack(next state);

        undo choice;
    }
}
```

For N-Queens:

``` java
for(int col=0; col<n; col++){

    if(isSafe(matrix,row,col)){

        matrix[row][col] = 'Q';

        solve(matrix,res,row+1,n);

        matrix[row][col] = '.';
    }
}
```

The three most important lines are:

``` java
matrix[row][col] = 'Q';   // Choose

solve(matrix,res,row+1,n); // Explore

matrix[row][col] = '.';   // Undo
```

This is the core idea of **Backtracking**.
