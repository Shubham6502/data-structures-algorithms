# Count All K-Sum Paths in a Binary Tree(GFG)

## Problem
Given a binary tree and an integer `k`, count the number of **paths whose sum equals `k`**.

### Conditions
- A path can **start at any node**
- The path must go **downward (parent → child)**
- Paths do **not need to start at the root**

---
```
## Example

    10
   /  \
  5   -3
 / \     \
3   2     11

/ \
3 -2 1

k = 8
```

### Valid Paths


5 → 3
5 → 2 → 1
-3 → 11


Total paths = **3**

---

# Approach (Prefix Sum + HashMap)

### Key Idea

While traversing the tree we maintain:

- `currSum` → sum from root to current node
- `map` → stores how many times a prefix sum occurred
- `count` → number of valid paths

At each node we check:


currSum - k


If this value already exists in the map, then a path with sum `k` exists.

---

# Algorithm Steps

1. Traverse the tree using **DFS**
2. Update the running sum


currSum += node.data


3. If


currSum == k


increment the count.

4. Check if


currSum - k


exists in the map.

If yes, add its frequency to `count`.

5. Store the current prefix sum in the map.

6. Traverse left and right subtree.

7. **Backtrack** by removing the prefix sum when returning.

---

# Java Implementation

```java
import java.util.*;

class Solution {

    int count = 0;

    public void dfs(Node root, int k, int currSum, HashMap<Integer,Integer> map){

        if(root == null) return;

        currSum += root.data;

        if(currSum == k)
            count++;

        if(map.containsKey(currSum - k))
            count += map.get(currSum - k);

        map.put(currSum, map.getOrDefault(currSum,0) + 1);

        dfs(root.left, k, currSum, map);
        dfs(root.right, k, currSum, map);

        // Backtracking
        map.put(currSum, map.get(currSum) - 1);
    }

    public int countAllPaths(Node root, int k){

        HashMap<Integer,Integer> map = new HashMap<>();

        dfs(root, k, 0, map);

        return count;
    }
}

```

```
Dry Run Summary
Node	currSum	currSum-k	Path Found
10	10	2	❌
5	15	7	❌
3	18	10	✅ (5→3)
1	18	10	✅ (5→2→1)
11	18	10	✅ (-3→11)
```
Final count:

3
Time Complexity
O(N)

Each node is visited once.

Space Complexity
O(N)

HashMap storage

Recursion stack

Important Concept
Backtracking
map.put(currSum, map.get(currSum) - 1);

This removes the current prefix sum when we return to the parent node.

It ensures prefix sums are only used for the current path.
