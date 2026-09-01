# Rotten Oranges - Multi-Source BFS

## Problem

Given a grid where:

* `0` = Empty cell
* `1` = Fresh orange
* `2` = Rotten orange

Every minute, a rotten orange makes its adjacent fresh oranges rotten.

We need to return:

* Minimum time required to rot all oranges.
* `-1` if some fresh orange cannot become rotten.

---

# Approach

We use **Multi-Source BFS**.

## Why Multi-Source BFS?

There can be multiple rotten oranges.

All rotten oranges start spreading at the same time.

So first, we add **all rotten oranges** to the queue.

```text
All Rotten Oranges
        ↓
Add to Queue
        ↓
Start BFS
        ↓
Rot Adjacent Fresh Oranges
        ↓
Increase Time
```

---

# Complete Code

```java
import java.util.*;

class Solution {

    class Node {
        int i, j, time;

        Node(int i, int j, int time) {
            this.i = i;
            this.j = j;
            this.time = time;
        }
    }

    Queue<Node> q = new LinkedList<>();

    public int bfs(int[][] grid, boolean[][] vis, int n, int m) {

        int max = 0;

        while (!q.isEmpty()) {

            Node curr = q.poll();

            int i = curr.i;
            int j = curr.j;
            int time = curr.time;

            max = Math.max(time, max);

            // Down
            if (i + 1 < n &&
                grid[i + 1][j] == 1 &&
                !vis[i + 1][j]) {

                q.add(new Node(i + 1, j, time + 1));
                vis[i + 1][j] = true;
            }

            // Right
            if (j + 1 < m &&
                grid[i][j + 1] == 1 &&
                !vis[i][j + 1]) {

                q.add(new Node(i, j + 1, time + 1));
                vis[i][j + 1] = true;
            }

            // Up
            if (i - 1 >= 0 &&
                grid[i - 1][j] == 1 &&
                !vis[i - 1][j]) {

                q.add(new Node(i - 1, j, time + 1));
                vis[i - 1][j] = true;
            }

            // Left
            if (j - 1 >= 0 &&
                grid[i][j - 1] == 1 &&
                !vis[i][j - 1]) {

                q.add(new Node(i, j - 1, time + 1));
                vis[i][j - 1] = true;
            }
        }

        return max;
    }

    public int orangesRotting(int[][] grid) {

        int n = grid.length;
        int m = grid[0].length;

        boolean[][] vis = new boolean[n][m];

        // Add all rotten oranges to the queue
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {

                if (grid[i][j] == 2) {
                    q.add(new Node(i, j, 0));
                    vis[i][j] = true;
                }
            }
        }

        // Start Multi-Source BFS
        int time = bfs(grid, vis, n, m);

        // Check if any fresh orange remains
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {

                if (grid[i][j] == 1 && !vis[i][j]) {
                    return -1;
                }
            }
        }

        return time;
    }
}
```

---

# Important Syntax

## 1. 2D Array

### Syntax

```java
int[][] array = new int[rows][columns];
```

Example:

```java
int[][] grid;
```

Access an element:

```java
grid[i][j];
```

---

# 2. Boolean 2D Array

### Syntax

```java
boolean[][] visited = new boolean[rows][columns];
```

Example:

```java
boolean[][] vis = new boolean[n][m];
```

By default:

```text
false false false
false false false
false false false
```

Mark as visited:

```java
vis[i][j] = true;
```

Check if not visited:

```java
!vis[i][j]
```

---

# 3. Inner Class

### Syntax

```java
class ClassName {

    class InnerClass {

    }
}
```

Example:

```java
class Solution {

    class Node {

    }
}
```

---

# 4. Constructor

### Syntax

```java
ClassName(parameters) {
    // initialization
}
```

Example:

```java
Node(int i, int j, int time) {
    this.i = i;
    this.j = j;
    this.time = time;
}
```

Create object:

```java
Node node = new Node(i, j, time);
```

---

# 5. Queue

### Declaration

```java
Queue<DataType> q = new LinkedList<>();
```

Example:

```java
Queue<Node> q = new LinkedList<>();
```

---

# 6. Add to Queue

### Syntax

```java
q.add(element);
```

Example:

```java
q.add(new Node(i, j, 0));
```

---

# 7. Remove from Queue

### Syntax

```java
DataType variable = q.poll();
```

Example:

```java
Node curr = q.poll();
```

`poll()` removes and returns the first element.

---

# 8. Check if Queue is Empty

### Syntax

```java
q.isEmpty()
```

Example:

```java
while (!q.isEmpty()) {

}
```

Meaning:

```text
While queue contains elements:
    Process elements
```

---

# 9. Access Object Variables

Suppose:

```java
Node curr = q.poll();
```

Access variables:

```java
int i = curr.i;
int j = curr.j;
int time = curr.time;
```

---

# 10. Math.max()

### Syntax

```java
Math.max(a, b);
```

Example:

```java
max = Math.max(time, max);
```

It stores the maximum value.

---

# BFS Logic

## Step 1: Add All Rotten Oranges

```java
if (grid[i][j] == 2) {
    q.add(new Node(i, j, 0));
    vis[i][j] = true;
}
```

All rotten oranges are added first.

This makes the solution **Multi-Source BFS**.

---

# Step 2: Process Queue

```java
while (!q.isEmpty()) {
    Node curr = q.poll();
}
```

Take one orange from the queue.

---

# Step 3: Get Current Position

```java
int i = curr.i;
int j = curr.j;
int time = curr.time;
```

---

# Step 4: Check Four Directions

```text
        Up
        ↑
Left ← Cell → Right
        ↓
       Down
```

---

## Down

```java
i + 1
```

Condition:

```java
if (i + 1 < n &&
    grid[i + 1][j] == 1 &&
    !vis[i + 1][j]) {

    q.add(new Node(i + 1, j, time + 1));
    vis[i + 1][j] = true;
}
```

---

## Right

```java
j + 1
```

Condition:

```java
if (j + 1 < m &&
    grid[i][j + 1] == 1 &&
    !vis[i][j + 1]) {

    q.add(new Node(i, j + 1, time + 1));
    vis[i][j + 1] = true;
}
```

---

## Up

```java
i - 1
```

Condition:

```java
if (i - 1 >= 0 &&
    grid[i - 1][j] == 1 &&
    !vis[i - 1][j]) {

    q.add(new Node(i - 1, j, time + 1));
    vis[i - 1][j] = true;
}
```

---

## Left

```java
j - 1
```

Condition:

```java
if (j - 1 >= 0 &&
    grid[i][j - 1] == 1 &&
    !vis[i][j - 1]) {

    q.add(new Node(i, j - 1, time + 1));
    vis[i][j - 1] = true;
}
```

---

# Important Pattern

Whenever you visit a new cell:

## 1. Add it to the queue

```java
q.add(new Node(newI, newJ, time + 1));
```

## 2. Immediately mark it as visited

```java
vis[newI][newJ] = true;
```

Always remember:

```text
Add to Queue
      ↓
Mark Visited
```

---

# Why Do We Use `time + 1`?

Current orange:

```text
Time = 0
```

When it rots an adjacent orange:

```text
Time = 1
```

Next level:

```text
Time = 2
```

Therefore:

```java
new Node(i, j, time + 1);
```

---

# Check Remaining Fresh Oranges

After BFS:

```java
for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {

        if (grid[i][j] == 1 && !vis[i][j]) {
            return -1;
        }
    }
}
```

Meaning:

```text
Fresh orange exists
        +
Not visited by BFS
        ↓
It can never become rotten
        ↓
Return -1
```

---

# Time Complexity

```text
O(n × m)
```

Every cell is visited at most once.

---

# Space Complexity

```text
O(n × m)
```

Space is used by:

* Queue
* Visited array

---

# Pattern to Remember

```text
Multi-Source BFS

1. Find all starting points
2. Add all starting points to queue
3. Mark all as visited
4. Start BFS
5. Visit four directions
6. Add valid neighbors
7. Mark neighbors as visited
8. Increase time/level
9. Check if any required cell remains unvisited
```
