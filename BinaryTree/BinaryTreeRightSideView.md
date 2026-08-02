# Binary Tree Right Side View

## Problem

Given the `root` of a binary tree, imagine yourself standing on the **right side** of it. Return the values of the nodes you can see from top to bottom.

---

## Approach 1: Breadth-First Search (BFS)

### Idea

Traverse the tree **level by level** using a queue.

At every level, the **last node processed** is the node visible from the right side.

### Algorithm

1. If the tree is empty, return an empty list.
2. Add the root node to the queue.
3. While the queue is not empty:
   - Store the current queue size.
   - Process all nodes of that level.
   - When processing the **last node** of the level (`i == size - 1`), add its value to the result.
4. Return the result.

---

### Java Solution (BFS)

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {

        List<Integer> res = new ArrayList<>();

        if (root == null)
            return res;

        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);

        while (!q.isEmpty()) {

            int size = q.size();

            for (int i = 0; i < size; i++) {

                TreeNode curr = q.poll();

                if (i == size - 1)
                    res.add(curr.val);

                if (curr.left != null)
                    q.offer(curr.left);

                if (curr.right != null)
                    q.offer(curr.right);
            }
        }

        return res;
    }
}
```

---

### Complexity Analysis

**Time Complexity:** `O(n)`

- Every node is visited exactly once.

**Space Complexity:** `O(n)`

- Queue stores nodes level by level.

---

# Approach 2: Depth-First Search (DFS) (Right → Left)

## Key Idea

Instead of traversing level by level, traverse

```text
Root → Right → Left
```

Since we visit the **right child first**, the **first node encountered at every level** is the node visible from the right side.

---

## Why does this line work?

```java
if (level == res.size())
```

This is the most important line in the solution.

`res.size()` tells us

> **How many levels have already been added to the result.**

If

```java
level == res.size()
```

then it means

> **This is the first node we have visited at this level.**

Since we always visit the **right subtree before the left subtree**, this first node is automatically the rightmost node.

Therefore, we add it to the answer.

---

## Example

### Input

```text
          1
        /   \
       2     3
        \     \
         5     4
```

Expected Output

```text
[1,3,4]
```

---

## Dry Run

### Initial State

```text
res = []
```

---

### Visit Node 1

```text
level = 0
res.size() = 0

0 == 0
```

Add 1.

```text
res = [1]
```

---

### Visit Node 3

```text
level = 1
res.size() = 1

1 == 1
```

Add 3.

```text
res = [1,3]
```

---

### Visit Node 4

```text
level = 2
res.size() = 2

2 == 2
```

Add 4.

```text
res = [1,3,4]
```

---

### Backtrack to Node 2

```text
level = 1
res.size() = 3

1 == 3
```

False.

Do not add 2 because level 1 already has its rightmost node (3).

---

### Visit Node 5

```text
level = 2
res.size() = 3

2 == 3
```

False.

Do not add 5 because level 2 already has its rightmost node (4).

---

## DFS Traversal Order

```text
1 → 3 → 4 → 2 → 5
```

Notice that the **first node visited at every level** is

| Level | First Node Visited |
|--------|--------------------|
| 0 | 1 |
| 1 | 3 |
| 2 | 4 |

These are exactly the nodes visible from the right side.

---

## Java Solution (DFS)

```java
class Solution {

    public List<Integer> rightSideView(TreeNode root) {

        List<Integer> res = new ArrayList<>();
        dfs(root, 0, res);
        return res;
    }

    private void dfs(TreeNode root, int level, List<Integer> res) {

        if (root == null)
            return;

        if (level == res.size())
            res.add(root.val);

        dfs(root.right, level + 1, res);
        dfs(root.left, level + 1, res);
    }
}
```

---

## Complexity Analysis

**Time Complexity:** `O(n)`

- Every node is visited exactly once.

**Space Complexity:** `O(h)`

Where `h` is the height of the tree.

- Balanced Tree → `O(log n)`
- Skewed Tree → `O(n)`

---

# Comparison

| Approach | Time | Space | Extra Data Structure |
|----------|------|-------|----------------------|
| BFS | O(n) | O(n) | Queue |
| DFS | O(n) | O(h) | Recursion Stack |

---

# Key Takeaways

- **BFS:** Traverse level by level and take the last node of each level.
- **DFS:** Traverse **Root → Right → Left**.
- `level == res.size()` means:
  - This is the **first node** visited at this level.
  - Since we visit the right subtree first, it is the **rightmost visible node**.
- Both approaches are optimal with **O(n)** time complexity.
- DFS is often considered the more elegant solution because it avoids maintaining a queue and naturally captures the right-side view.
