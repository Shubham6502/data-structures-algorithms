# Number of Islands – DFS (Java)

## Problem

Given a 2D grid containing:

* `'1'` → Land
* `'0'` → Water

Find the total number of **islands**.

An island is formed by connecting adjacent land cells **horizontally or vertically**.

---

## Approach: DFS + Visited Array

We traverse every cell in the grid.

Whenever we find:

```java
grid[i][j] == '1' && !vis[i][j]
```

it means we found a **new unvisited island**.

Then we:

1. Start DFS from that cell.
2. Visit all connected land cells.
3. Mark them as visited.
4. Increase the island count by `1`.

---

# Complete Code

```java
class Solution {

    public void dfs(char[][] grid, boolean[][] vis, int i, int j) {

        // Base conditions

        // Out of grid boundary
        if (i < 0 || j < 0 ||
            i >= grid.length || j >= grid[0].length ||
            vis[i][j] ||
            grid[i][j] != '1') {
            return;
        }

        // Mark current land as visited
        vis[i][j] = true;

        // Visit all 4 directions

        // Down
        dfs(grid, vis, i + 1, j);

        // Up
        dfs(grid, vis, i - 1, j);

        // Right
        dfs(grid, vis, i, j + 1);

        // Left
        dfs(grid, vis, i, j - 1);
    }

    public int numIslands(char[][] grid) {

        // Stores total number of islands
        int count = 0;

        // Visited array
        boolean[][] vis = new boolean[grid.length][grid[0].length];

        // Traverse every cell
        for (int i = 0; i < grid.length; i++) {

            for (int j = 0; j < grid[0].length; j++) {

                // Found an unvisited land cell
                if (grid[i][j] == '1' && !vis[i][j]) {

                    // Visit the complete island
                    dfs(grid, vis, i, j);

                    // One complete island found
                    count++;
                }
            }
        }

        return count;
    }
}
```

---

# DFS Function Syntax

```java
public void dfs(char[][] grid, boolean[][] vis, int i, int j)
```

### Parameters

| Parameter | Meaning                               |
| --------- | ------------------------------------- |
| `grid`    | The 2D grid containing land and water |
| `vis`     | Stores whether a cell is visited      |
| `i`       | Current row                           |
| `j`       | Current column                        |

---

# Base Condition

```java
if (i < 0 || j < 0 ||
    i >= grid.length || j >= grid[0].length ||
    vis[i][j] ||
    grid[i][j] != '1') {
    return;
}
```

DFS stops when:

### 1. Row is outside the grid

```java
i < 0
```

or

```java
i >= grid.length
```

### 2. Column is outside the grid

```java
j < 0
```

or

```java
j >= grid[0].length
```

### 3. Cell is already visited

```java
vis[i][j]
```

### 4. Cell is water

```java
grid[i][j] != '1'
```

---

# Mark Cell as Visited

```java
vis[i][j] = true;
```

This is important because otherwise DFS can visit the same cell again and create an infinite loop.

---

# DFS in 4 Directions

## Down

```java
dfs(grid, vis, i + 1, j);
```

## Up

```java
dfs(grid, vis, i - 1, j);
```

## Right

```java
dfs(grid, vis, i, j + 1);
```

## Left

```java
dfs(grid, vis, i, j - 1);
```

### Direction Summary

```text
        Up
        i-1
         |
Left ---- Cell ---- Right
j-1               j+1
         |
        Down
        i+1
```

---

# Main Condition

```java
if (grid[i][j] == '1' && !vis[i][j])
```

Meaning:

```text
Current cell is land
        AND
Current cell is not visited
```

Then:

```java
dfs(grid, vis, i, j);
count++;
```

DFS visits the complete connected island.

After DFS finishes:

```java
count++;
```

because one complete island has been found.

---

# Dry Run

### Grid

```text
1 1 0 0
1 0 0 1
0 0 1 1
0 0 0 0
```

### Island 1

```text
1 1
1
```

DFS visits all connected `1`s.

```text
count = 1
```

### Island 2

```text
1
1 1
```

DFS visits all connected `1`s.

```text
count = 2
```

### Final Answer

```text
2
```

---

# Time Complexity

```text
O(m × n)
```

Where:

* `m` = number of rows
* `n` = number of columns

Each cell is visited at most once.

---

# Space Complexity

```text
O(m × n)
```

For:

```java
boolean[][] vis
```

The DFS recursion stack can also take up to:

```text
O(m × n)
```

in the worst case.

---

# Important Interview Point

## Why do we need `vis`?

Because without marking cells as visited, DFS can repeatedly visit the same connected cells.

Example:

```text
1 1
```

DFS can move:

```text
(0,0) → (0,1) → (0,0) → (0,1)
```

and continue forever.

So we use:

```java
vis[i][j] = true;
```

---

# Common Mistake

❌ Wrong:

```java
grid[i][j] != '0'
```

This would stop DFS when it reaches land.

✅ Correct:

```java
grid[i][j] != '1'
```

DFS should continue only when the current cell is land.

---

# Short Interview Explanation

> I traverse every cell in the grid. Whenever I find an unvisited land cell, I start DFS from it. The DFS visits and marks all connected land cells as visited in four directions. Since one DFS call covers one complete island, I increment the island count after each DFS call.

---

# Key Pattern to Remember

```text
Traverse Grid
      ↓
Find unvisited valid cell
      ↓
Run DFS/BFS
      ↓
Mark connected cells visited
      ↓
Increase answer
```

This pattern is commonly used in:

* Number of Islands
* Flood Fill
* Connected Components
* Surrounded Regions
* Rotting Oranges
* Matrix Traversal
* Graph Connected Components
