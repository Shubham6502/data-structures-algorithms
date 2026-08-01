# Same Tree (LeetCode 100)

## Problem Statement

Given the roots of two binary trees `p` and `q`, return `true` if they are the same, otherwise return `false`.

Two binary trees are considered the same if they are:

- Structurally identical.
- Have the same value at every corresponding node.

---

## Approach

We use **Recursion (Depth-First Search)** to compare both trees simultaneously.

### Algorithm

1. If both nodes are `null`, return `true`.
2. If one node is `null` and the other is not, return `false`.
3. If the node values are different, return `false`.
4. Recursively compare:
   - Left subtree
   - Right subtree
5. Return `true` only if **both** subtrees are identical.

---

## Java Solution

```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {

        if (p == null && q == null)
            return true;

        if (p == null || q == null)
            return false;

        if (p.val != q.val)
            return false;

        boolean isLeft = isSameTree(p.left, q.left);
        boolean isRight = isSameTree(p.right, q.right);

        return isLeft && isRight;
    }
}
```

---

## Dry Run

### Example

```
Tree 1:          Tree 2:

    1               1
   / \             / \
  2   3           2   3
```

### Execution

```
Compare 1 and 1 → Same

Compare Left:
2 and 2 → Same

Compare Right:
3 and 3 → Same

Both left and right subtrees are identical.

Answer = true
```

---

### Example 2

```
Tree 1:          Tree 2:

    1               1
   /                 \
  2                   2
```

One tree has a left child while the other has a right child.

```
Answer = false
```

---

## Time Complexity

- **O(n)**

Each node is visited exactly once.

---

## Space Complexity

- **O(h)**

Where `h` is the height of the tree (recursive call stack).

- Balanced Tree: **O(log n)**
- Skewed Tree: **O(n)**

---

## Key Points

- Compare **node values**, not node references.
- If either node is `null` while the other isn't, return `false`.
- Both left and right subtrees must match, so use **AND (`&&`)**.
- This is a classic recursive DFS tree comparison problem.
