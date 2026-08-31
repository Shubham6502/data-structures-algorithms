# Max Area of Island – DFS (Java)

## Problem

You are given a 2D grid where:

* `1` → Land
* `0` → Water

An island is a group of connected land cells.

Cells are connected in **4 directions**:

* Up
* Down
* Left
* Right

The task is to find the **maximum area of an island**.

---

# Approach: DFS

We traverse every cell in the grid.

Whenever we find an **unvisited land cell (`1`)**, we start DFS.

The DFS:

1. Marks the current cell as visited.
2. Counts the current cell as `1`.
3. Visits all connected land cells.
4. Returns the total area of that island.

We compare the returned area with `maxArea`.

---

# Complete Code

```java
class Solution {
  
    public int dfs(int[][] grid, int i, int j, boolean[][] vis) {

        // Base conditions
        if (i < 0 || j < 0 ||
            i >= grid.length || j >= grid[0].length ||
            grid[i][j] == 0 ||
            vis[i][j]) {
            return 0;
        }

        // Mark current cell as visited
        vis[i][j] = true;

        // Current cell = 1
        // Add area from all 4 directions
        return 1
                + dfs(grid, i + 1, j, vis)
                + dfs(grid, i, j + 1, vis)
                + dfs(grid, i - 1, j, vis)
                + dfs(grid, i, j - 1, vis);
    }

    public int maxAreaOfIsland(int[][] grid) {

        // Stores maximum island area
        int maxArea = 0;

        // Visited array
        boolean[][] vis =
                new boolean[grid.length][grid[0].length];

        // Traverse the complete grid
        for (int i = 0; i < grid.length; i++) {

            for (int j = 0; j < grid[0].length; j++) {

                // Found an unvisited land cell
                if (grid[i][j] == 1 && !vis[i][j]) {

                    // Find area of current island
                    int area = dfs(grid, i, j, vis);

                    // Update maximum area
                    maxArea = Math.max(area, maxArea);
                }
            }
        }

        return maxArea;
    }
}
```

---

# DFS Function Syntax

```java
public int dfs(int[][] grid, int i, int j, boolean[][] vis)
```

## Parameters

| Parameter | Meaning                           |
| --------- | --------------------------------- |
| `grid`    | 2D grid containing land and water |
| `i`       | Current row                       |
| `j`       | Current column                    |
| `vis`     | Stores whether a cell is visited  |

The DFS function returns:

```java
int
```

because it calculates and returns the **area of an island**.

---

# Base Conditions

```java
if (i < 0 || j < 0 ||
    i >= grid.length || j >= grid[0].length ||
    grid[i][j] == 0 ||
    vis[i][j]) {
    return 0;
}
```

DFS returns `0` when:

## 1. Row goes outside the grid

```java
i < 0
```

or

```java
i >= grid.length
```

---

## 2. Column goes outside the grid

```java
j < 0
```

or

```java
j >= grid[0].length
```

---

## 3. Current cell is water

```java
grid[i][j] == 0
```

Water does not contribute to the island area.

So:

```java
return 0;
```

---

## 4. Current cell is already visited

```java
vis[i][j]
```

We do not want to count the same cell again.

So:

```java
return 0;
```

---

# Mark Cell as Visited

```java
vis[i][j] = true;
```

We mark the cell before exploring its neighbors.

This prevents DFS from visiting the same cell repeatedly.

---

# Calculate Island Area

```java
return 1
        + dfs(grid, i + 1, j, vis)
        + dfs(grid, i, j + 1, vis)
        + dfs(grid, i - 1, j, vis)
        + dfs(grid, i, j - 1, vis);
```

## Important Logic

The current land cell contributes:

```java
1
```

Then we add the area returned by all four directions.

```text
Current Cell
      +
Down Area
      +
Right Area
      +
Up Area
      +
Left Area
```

---

# DFS in Four Directions

## Down

```java
dfs(grid, i + 1, j, vis);
```

## Right

```java
dfs(grid, i, j + 1, vis);
```

## Up

```java
dfs(grid, i - 1, j, vis);
```

## Left

```java
dfs(grid, i, j - 1, vis);
```

---

# Direction Pattern

```text
          Up
        (i-1, j)
            |
Left ---------------- Right
(i, j-1)   CELL   (i, j+1)
            |
          Down
        (i+1, j)
```

---

# Dry Run

Consider this grid:

```text
1 1 0
1 1 0
0 0 1
```

There are two islands.

## Island 1

```text
1 1
1 1
```

Area:

```text
4
```

## Island 2

```text
1
```

Area:

```text
1
```

Therefore:

```text
maxArea = 4
```

---

# Main Logic

When we find:

```java
if (grid[i][j] == 1 && !vis[i][j])
```

we start DFS:

```java
int area = dfs(grid, i, j, vis);
```

The DFS returns the complete area of that island.

Then:

```java
maxArea = Math.max(area, maxArea);
```

updates the maximum area.

---

# Example

Suppose:

```text
Island 1 area = 3
Island 2 area = 7
Island 3 area = 2
```

Then:

```text
maxArea = 7
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

For the visited array:

```java
boolean[][] vis
```

The recursion stack can also take up to `O(m × n)` in the worst case.

---

# Important Interview Difference

## Number of Islands

DFS only visits all cells of an island.

```java
dfs(...);
count++;
```

DFS usually returns:

```java
void
```

---

## Max Area of Island

DFS must calculate the size of the island.

```java
int area = dfs(...);
```

DFS returns:

```java
int
```

The key logic is:

```java
return 1
        + dfs(...)
        + dfs(...)
        + dfs(...)
        + dfs(...);
```

---

# Key Pattern to Remember

```text
Traverse Grid
      ↓
Find unvisited land
      ↓
Run DFS
      ↓
Mark cells visited
      ↓
DFS returns island area
      ↓
Compare with maxArea
```

---

# Short Interview Explanation

> I traverse every cell in the grid. Whenever I find an unvisited land cell, I start DFS. The DFS marks the cell as visited and returns `1` plus the area of all connected land cells in four directions. Each DFS call gives me the total area of one island, and I use `Math.max()` to find the maximum island area.

---

# Most Important Syntax to Remember

```java
return 1
        + dfs(grid, i + 1, j, vis)
        + dfs(grid, i - 1, j, vis)
        + dfs(grid, i, j + 1, vis)
        + dfs(grid, i, j - 1, vis);
```

## Simple Meaning

```text
1 = Current land cell

DFS calls = Connected land cells

Total = Area of island
```
