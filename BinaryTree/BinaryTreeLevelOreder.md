# Binary Tree Level Order Traversal

## Problem
Given the `root` of a binary tree, return the **level order traversal** of its nodes' values (i.e., from left to right, level by level).

---

## Approach (BFS using Queue)

We use **Breadth-First Search (BFS)** with a queue to traverse the tree level by level.

### Steps
1. If the root is `null`, return an empty list.
2. Create a queue and insert the root node.
3. While the queue is not empty:
   - Store the current queue size (`n`), which represents the number of nodes in the current level.
   - Create a list to store the values of the current level.
   - Process exactly `n` nodes:
     - Remove a node from the queue.
     - Add its value to the current level list.
     - Add its left child to the queue (if it exists).
     - Add its right child to the queue (if it exists).
   - Add the current level list to the final result.
4. Return the result.

---

## Dry Run

### Input
```text
        3
       / \
      9  20
         / \
        15  7
```

### Execution

**Initial Queue**

```text
[3]
```

### Level 0

Queue Size = 1

Process:
- Remove 3
- Add 9 and 20

Current Level:

```text
[3]
```

Queue:

```text
[9, 20]
```

---

### Level 1

Queue Size = 2

Process:
- Remove 9
- Remove 20
- Add 15 and 7

Current Level:

```text
[9, 20]
```

Queue:

```text
[15, 7]
```

---

### Level 2

Queue Size = 2

Process:
- Remove 15
- Remove 7

Current Level:

```text
[15, 7]
```

Queue:

```text
[]
```

---

### Final Output

```text
[
  [3],
  [9,20],
  [15,7]
]
```

---

## Java Solution

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {

        List<List<Integer>> res = new ArrayList<>();

        if (root == null)
            return res;

        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);

        while (!q.isEmpty()) {

            int n = q.size();
            List<Integer> list = new ArrayList<>();

            for (int i = 0; i < n; i++) {

                TreeNode curr = q.poll();

                list.add(curr.val);

                if (curr.left != null)
                    q.add(curr.left);

                if (curr.right != null)
                    q.add(curr.right);
            }

            res.add(list);
        }

        return res;
    }
}
```

---

## Complexity Analysis

### Time Complexity

- Each node is visited exactly once.
- **O(n)**

### Space Complexity

- Queue stores nodes of one level.
- In the worst case, it can hold approximately `n/2` nodes.
- **O(n)**

---

## Why This Works

- BFS naturally processes nodes **level by level**.
- The queue keeps track of nodes to visit next.
- `queue.size()` tells us how many nodes belong to the current level, ensuring each level is grouped correctly.

---

## Key Takeaways

- Use **BFS** whenever the problem asks for **level-wise traversal**.
- Store the queue size before processing a level.
- Process exactly that many nodes before moving to the next level.
- This is the **optimal solution** with:
  - **Time:** `O(n)`
  - **Space:** `O(n)`
