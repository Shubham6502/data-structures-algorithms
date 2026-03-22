# 🍊 Rotten Oranges Problem (BFS Approach)

## 📌 Problem Statement
You are given a grid where:
- `0` = Empty cell  
- `1` = Fresh orange  
- `2` = Rotten orange  

Every minute, a rotten orange can rot its adjacent fresh oranges (up, down, left, right).

👉 Return the **minimum time** required to rot all oranges.  
👉 If not possible, return `-1`.

---

## 🚀 Approach (Multi-Source BFS)

- Add all rotten oranges to queue initially
- Spread rot in all 4 directions
- Track time using BFS
- At the end, check if any fresh orange remains

---

## ✅ Java Code

```java
class Solution {

    class Node {
        int i, j, time;
        Node(int i, int j, int time) {
            this.i = i;
            this.j = j;
            this.time = time;
        }
    }

    public int orangesRot(int[][] mat) {
        int m = mat.length;
        int n = mat[0].length;
        int ans = 0;

        Queue<Node> q = new LinkedList<>();
        boolean[][] visited = new boolean[m][n];

        // Step 1: Add all rotten oranges
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (mat[i][j] == 2) {
                    q.add(new Node(i, j, 0));
                    visited[i][j] = true;
                }
            }
        }

        // Step 2: BFS traversal
        while (!q.isEmpty()) {
            Node curr = q.poll();
            int i = curr.i;
            int j = curr.j;
            int time = curr.time;

            ans = Math.max(ans, time);

            // Up
            if (i - 1 >= 0 && !visited[i - 1][j] && mat[i - 1][j] == 1) {
                q.add(new Node(i - 1, j, time + 1));
                visited[i - 1][j] = true;
            }

            // Left
            if (j - 1 >= 0 && !visited[i][j - 1] && mat[i][j - 1] == 1) {
                q.add(new Node(i, j - 1, time + 1));
                visited[i][j - 1] = true;
            }

            // Down
            if (i + 1 < m && !visited[i + 1][j] && mat[i + 1][j] == 1) {
                q.add(new Node(i + 1, j, time + 1));
                visited[i + 1][j] = true;
            }

            // Right
            if (j + 1 < n && !visited[i][j + 1] && mat[i][j + 1] == 1) {
                q.add(new Node(i, j + 1, time + 1));
                visited[i][j + 1] = true;
            }
        }

        // Step 3: Check remaining fresh oranges
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (mat[i][j] == 1 && !visited[i][j]) {
                    return -1;
                }
            }
        }

        return ans;
    }
}
```

⏱ Time Complexity
O(m × n)
📦 Space Complexity
O(m × n) (queue + visited array)
