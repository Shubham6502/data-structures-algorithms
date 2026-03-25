# 🌳 Minimum Height Trees (MHT) (GFG-POTD)

## 📌 Problem Statement
Given a tree with `V` vertices and `edges`, find all possible root nodes such that the height of the tree is minimum.

👉 Return all such root nodes.

---

## 💡 Intuition

- A tree can have **1 or 2 centers**.
- These centers give **minimum height** when chosen as root.

### Key Idea:
- Start removing **leaf nodes** (nodes with degree = 1)
- Continue layer by layer
- The **last remaining nodes** are the answer

---

## 🔄 Approach (Topological BFS)

1. Build adjacency list
2. Maintain degree array
3. Add all leaf nodes (degree = 1) into queue
4. Remove leaves level by level
5. Stop when ≤ 2 nodes remain
6. Remaining nodes = MHT roots

---

## ⚠️ Edge Case

- If `V == 1`, return `[0]`

---

## ✅ Code (Java)

```java
import java.util.*;

class Solution {
    public ArrayList<Integer> minHeightRoot(int V, int[][] edges) {
        
        ArrayList<Integer> ans = new ArrayList<>();
        
        // Edge case
        if (V == 1) {
            ans.add(0);
            return ans;
        }

        int[] deg = new int[V];
        List<List<Integer>> adj = new ArrayList<>();

        // Initialize adjacency list
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }

        // Build graph
        for (int[] e : edges) {
            deg[e[0]]++;
            deg[e[1]]++;
            
            adj.get(e[0]).add(e[1]);
            adj.get(e[1]).add(e[0]);
        }

        // Queue for leaf nodes
        Queue<Integer> q = new LinkedList<>();

        // Add initial leaves
        for (int i = 0; i < V; i++) {
            if (deg[i] == 1) {
                q.offer(i);
            }
        }

        int remaining = V;

        // Remove leaves layer by layer
        while (remaining > 2) {
            int size = q.size();
            remaining -= size;

            for (int i = 0; i < size; i++) {
                int node = q.poll();

                for (int nei : adj.get(node)) {
                    deg[nei]--;

                    if (deg[nei] == 1) {
                        q.offer(nei);
                    }
                }
            }
        }

        // Remaining nodes are MHT roots
        while (!q.isEmpty()) {
            ans.add(q.poll());
        }

        return ans;
    }
}
```
Complexity
Time Complexity: O(V + E)
Space Complexity: O(V + E)
