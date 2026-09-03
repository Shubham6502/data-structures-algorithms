# Pacific Atlantic Water Flow — DFS

## Problem

Given a matrix `heights`, water can flow from one cell to another if the next cell has a height **less than or equal to** the current cell.

There are two oceans:

* **Pacific Ocean** → touches the **top** and **left** edges.
* **Atlantic Ocean** → touches the **bottom** and **right** edges.

We need to return all cells from which water can flow to **both** oceans.

---

## Approach

Instead of starting DFS from every cell and checking whether it can reach both oceans, we reverse the direction.

We start DFS **from the oceans and move inward**.

Normally water flows:

```text
Higher → Lower
```

But during our DFS, we move from the ocean toward cells whose height is:

```text
Current cell height >= Previous cell height
```

This is why we use:

```java
prev > heights[row][col]
```

If the previous cell is higher than the current cell, we cannot move there.

---

## Example

Consider:

```text
1  2  2  3  5
3  2  3  4  4
2  4  5  3  1
6  7  1  4  5
5  1  1  2  4
```

We maintain two boolean matrices:

```java
boolean[][] pac = new boolean[m][n];
boolean[][] atl = new boolean[m][n];
```

### `pac`

```java
pac[i][j] == true
```

means:

> Cell `(i, j)` can reach the Pacific Ocean.

### `atl`

```java
atl[i][j] == true
```

means:

> Cell `(i, j)` can reach the Atlantic Ocean.

At the end:

```java
if(pac[i][j] && atl[i][j])
```

means that the cell can reach **both oceans**.

---

# DFS Function

```java
public void dfs(
    int[][] heights,
    boolean[][] vis,
    int prev,
    int row,
    int col,
    int m,
    int n
)
```

### Parameters

| Parameter | Meaning                     |
| --------- | --------------------------- |
| `heights` | Original height matrix      |
| `vis`     | Visited/reachable matrix    |
| `prev`    | Height of the previous cell |
| `row`     | Current row                 |
| `col`     | Current column              |
| `m`       | Number of rows              |
| `n`       | Number of columns           |

---

## DFS Base Condition

```java
if(
    row < 0 ||
    col < 0 ||
    row >= m ||
    col >= n ||
    vis[row][col] ||
    prev > heights[row][col]
)
    return;
```

There are five conditions where DFS stops.

### 1. Row is outside the matrix

```java
row < 0
```

### 2. Column is outside the matrix

```java
col < 0
```

### 3. Row exceeds the matrix

```java
row >= m
```

### 4. Column exceeds the matrix

```java
col >= n
```

### 5. Cell was already visited

```java
vis[row][col]
```

This prevents infinite recursion and repeated work.

### 6. Current cell is too low

```java
prev > heights[row][col]
```

For example:

```text
Previous = 5
Current  = 3
```

We cannot move:

```text
5 → 3
```

because when reversing the water-flow direction, we only move to cells that are **equal or higher**.

Valid:

```text
5 → 5
5 → 6
5 → 7
```

Invalid:

```text
5 → 4
5 → 3
```

---

# Mark Current Cell

```java
vis[row][col] = true;
```

This means:

> The current cell can reach this particular ocean.

For example, when using:

```java
dfs(heights, pac, ...)
```

then:

```java
pac[row][col] = true;
```

When using:

```java
dfs(heights, atl, ...)
```

then:

```java
atl[row][col] = true;
```

---

# Move in Four Directions

From the current cell, we check:

### Right

```java
dfs(
    heights,
    vis,
    heights[row][col],
    row,
    col + 1,
    m,
    n
);
```

### Left

```java
dfs(
    heights,
    vis,
    heights[row][col],
    row,
    col - 1,
    m,
    n
);
```

### Down

```java
dfs(
    heights,
    vis,
    heights[row][col],
    row + 1,
    col,
    m,
    n
);
```

### Up

```java
dfs(
    heights,
    vis,
    heights[row][col],
    row - 1,
    col,
    m,
    n
);
```

Notice that we pass:

```java
heights[row][col]
```

as the new `prev`.

For example:

```text
Current cell = 4
Next cell    = 6
```

The recursive call becomes:

```text
prev = 4
current = 6
```

Since:

```text
4 <= 6
```

the movement is allowed.

---

# Complete DFS

```java
public void dfs(
    int[][] heights,
    boolean[][] vis,
    int prev,
    int row,
    int col,
    int m,
    int n
) {

    if(
        row < 0 ||
        col < 0 ||
        row >= m ||
        col >= n ||
        vis[row][col] ||
        prev > heights[row][col]
    )
        return;

    vis[row][col] = true;

    // Right
    dfs(
        heights,
        vis,
        heights[row][col],
        row,
        col + 1,
        m,
        n
    );

    // Left
    dfs(
        heights,
        vis,
        heights[row][col],
        row,
        col - 1,
        m,
        n
    );

    // Down
    dfs(
        heights,
        vis,
        heights[row][col],
        row + 1,
        col,
        m,
        n
    );

    // Up
    dfs(
        heights,
        vis,
        heights[row][col],
        row - 1,
        col,
        m,
        n
    );
}
```

---

# Main Function

```java
public List<List<Integer>> pacificAtlantic(int[][] heights) {
```

First, get the dimensions:

```java
int m = heights.length;
int n = heights[0].length;
```

Where:

```text
m = number of rows
n = number of columns
```

---

# Create Two Visited Arrays

```java
boolean[][] pac = new boolean[m][n];
boolean[][] atl = new boolean[m][n];
```

We need two separate arrays because a cell can be reachable from:

```text
Pacific only
Atlantic only
Both
Neither
```

---

# Start DFS from Left and Right Borders

```java
for(int row = 0; row < m; row++) {

    dfs(
        heights,
        pac,
        heights[row][0],
        row,
        0,
        m,
        n
    );

    dfs(
        heights,
        atl,
        heights[row][n - 1],
        row,
        n - 1,
        m,
        n
    );
}
```

## Pacific

The Pacific Ocean touches the **left border**.

Therefore:

```java
dfs(
    heights,
    pac,
    heights[row][0],
    row,
    0,
    m,
    n
);
```

starts DFS from every cell in the first column.

---

## Atlantic

The Atlantic Ocean touches the **right border**.

Therefore:

```java
dfs(
    heights,
    atl,
    heights[row][n - 1],
    row,
    n - 1,
    m,
    n
);
```

starts DFS from every cell in the last column.

---

# Start DFS from Top and Bottom Borders

```java
for(int col = 0; col < n; col++) {

    dfs(
        heights,
        pac,
        heights[0][col],
        0,
        col,
        m,
        n
    );

    dfs(
        heights,
        atl,
        heights[m - 1][col],
        m - 1,
        col,
        m,
        n
    );
}
```

## Pacific

The Pacific Ocean also touches the **top border**.

```java
dfs(
    heights,
    pac,
    heights[0][col],
    0,
    col,
    m,
    n
);
```

## Atlantic

The Atlantic Ocean also touches the **bottom border**.

```java
dfs(
    heights,
    atl,
    heights[m - 1][col],
    m - 1,
    col,
    m,
    n
);
```

---

# Why Pass `heights[row][0]`?

This is important.

Consider:

```java
dfs(
    heights,
    pac,
    heights[row][0],
    row,
    0,
    m,
    n
);
```

The arguments are:

```text
heights      → matrix
pac          → Pacific visited array
heights[row][0] → starting cell's height
row          → starting row
0            → starting column
m            → rows
n            → columns
```

So:

```java
heights[row][0]
```

is **not the visited array**.

It is the initial value of:

```java
prev
```

For example, if:

```text
heights[row][0] = 3
```

then the first DFS call starts with:

```text
prev = 3
```

The next cells must have height:

```text
>= 3
```

---

# Why Use `heights[row][col]` in Recursive Calls?

Suppose:

```text
Current cell = 5
Next cell = 7
```

We call:

```java
dfs(
    heights,
    vis,
    heights[row][col],
    nextRow,
    nextCol,
    m,
    n
);
```

Therefore:

```text
prev = 5
current = 7
```

Check:

```java
prev > heights[nextRow][nextCol]
```

becomes:

```text
5 > 7
false
```

So DFS continues.

---

# Find Cells Reachable From Both Oceans

After completing all DFS calls:

```java
List<List<Integer>> res = new ArrayList<>();
```

Loop through every cell:

```java
for(int i = 0; i < m; i++) {

    for(int j = 0; j < n; j++) {

        if(pac[i][j] && atl[i][j]) {
            res.add(Arrays.asList(i, j));
        }
    }
}
```

The important condition is:

```java
pac[i][j] && atl[i][j]
```

This means:

```text
Pacific = true
AND
Atlantic = true
```

Therefore the cell can reach **both oceans**.

---

# Complete Code

```java
class Solution {

    public void dfs(
        int[][] heights,
        boolean[][] vis,
        int prev,
        int row,
        int col,
        int m,
        int n
    ) {

        if(
            row < 0 ||
            col < 0 ||
            row >= m ||
            col >= n ||
            vis[row][col] ||
            prev > heights[row][col]
        )
            return;

        vis[row][col] = true;

        // Right
        dfs(
            heights,
            vis,
            heights[row][col],
            row,
            col + 1,
            m,
            n
        );

        // Left
        dfs(
            heights,
            vis,
            heights[row][col],
            row,
            col - 1,
            m,
            n
        );

        // Down
        dfs(
            heights,
            vis,
            heights[row][col],
            row + 1,
            col,
            m,
            n
        );

        // Up
        dfs(
            heights,
            vis,
            heights[row][col],
            row - 1,
            col,
            m,
            n
        );
    }

    public List<List<Integer>> pacificAtlantic(int[][] heights) {

        int m = heights.length;
        int n = heights[0].length;

        boolean[][] pac = new boolean[m][n];
        boolean[][] atl = new boolean[m][n];

        // Left and Right borders
        for(int row = 0; row < m; row++) {

            // Pacific - Left
            dfs(
                heights,
                pac,
                heights[row][0],
                row,
                0,
                m,
                n
            );

            // Atlantic - Right
            dfs(
                heights,
                atl,
                heights[row][n - 1],
                row,
                n - 1,
                m,
                n
            );
        }

        // Top and Bottom borders
        for(int col = 0; col < n; col++) {

            // Pacific - Top
            dfs(
                heights,
                pac,
                heights[0][col],
                0,
                col,
                m,
                n
            );

            // Atlantic - Bottom
            dfs(
                heights,
                atl,
                heights[m - 1][col],
                m - 1,
                col,
                m,
                n
            );
        }

        // Find cells that can reach both oceans
        List<List<Integer>> res = new ArrayList<>();

        for(int i = 0; i < m; i++) {

            for(int j = 0; j < n; j++) {

                if(pac[i][j] && atl[i][j]) {
                    res.add(Arrays.asList(i, j));
                }
            }
        }

        return res;
    }
}
```

---

# Key Concept to Remember

The most important idea is:

```text
Normal water flow:

Higher
  ↓
Lower
```

But our DFS goes in the **reverse direction**:

```text
Ocean
  ↑
Lower
  ↑
Higher
```

Therefore our DFS condition is:

```java
prev <= currentHeight
```

or equivalently:

```java
prev > currentHeight
```

means **stop**.

---

# Complexity

Let:

```text
m = number of rows
n = number of columns
```

We perform DFS for Pacific and Atlantic.

Each cell is visited at most once for each ocean.

Therefore:

```text
Time:  O(m × n)

Space: O(m × n)
```

The space is used by:

```java
boolean[][] pac
boolean[][] atl
```

and the recursive DFS call stack.

---
