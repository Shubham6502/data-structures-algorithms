# Binary Tree Vertical Order Traversal

## Problem
Given the root of a binary tree, return the **vertical order traversal** of its nodes.

Nodes are grouped according to their **horizontal distance (HD)** from the root.

### Horizontal Distance Rules
- Root → `HD = 0`
- Left child → `HD - 1`
- Right child → `HD + 1`

Nodes with the same HD belong to the same vertical line.

---

## Example

Input (Level Order):


[1,2,3,4,5,6,7,N,N,N,8,N,9,N,10,11,N]


Output:


[[4], [2], [1,5,6,11], [3,8,9], [7], [10]]


---

## Approach

1. Use **Breadth First Search (BFS)**.
2. Store `(node, horizontal distance)` in a queue.
3. Use a **TreeMap** to keep vertical lines sorted from left to right.
4. Add node values into the list corresponding to their HD.
5. Push left and right children into the queue with updated HD.

---

## Algorithm Steps

1. Initialize:
   - `TreeMap<Integer, ArrayList<Integer>>`
   - `Queue<Pair>`
2. Push root with HD = `0`
3. While queue is not empty:
   - Remove node
   - Store value in map
   - Push left child with `hd - 1`
   - Push right child with `hd + 1`
4. Extract values from TreeMap into result list.

---

## Java Implementation

```java
/*
class Node {
    int data;
    Node left;
    Node right;

    Node(int data) {
        this.data = data;
        left = null;
        right = null;
    }
}
*/

class Solution {

    class Pair {
        int hd;
        Node node;

        Pair(int hd, Node node) {
            this.hd = hd;
            this.node = node;
        }
    }

    public ArrayList<ArrayList<Integer>> verticalOrder(Node root) {

        ArrayList<ArrayList<Integer>> result = new ArrayList<>();
        if (root == null) return result;

        TreeMap<Integer, ArrayList<Integer>> map = new TreeMap<>();
        Queue<Pair> queue = new LinkedList<>();

        queue.add(new Pair(0, root));

        while (!queue.isEmpty()) {

            Pair curr = queue.poll();
            Node node = curr.node;
            int hd = curr.hd;

            map.putIfAbsent(hd, new ArrayList<>());
            map.get(hd).add(node.data);

            if (node.left != null)
                queue.add(new Pair(hd - 1, node.left));

            if (node.right != null)
                queue.add(new Pair(hd + 1, node.right));
        }

        for (ArrayList<Integer> list : map.values())
            result.add(list);

        return result;
    }
}
```
Complexity
Time Complexity
O(N log N)

N nodes in tree

TreeMap operations cost log N

Space Complexity
O(N)

Queue and map store nodes.

Key Concept

This problem uses a common Binary Tree Pattern:

BFS + Horizontal Distance + Ordered Map

This same idea is used in:

Vertical Order Traversal

Top View

Bottom View

Diagonal Traversal
