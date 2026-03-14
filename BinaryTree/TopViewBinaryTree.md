# Top View of a Binary Tree (GFG)

## Problem
The **Top View** of a binary tree is the set of nodes visible when the tree is viewed from the **top**.

For every **horizontal distance (HD)** from the root, we select the **first node encountered**.

---

# Key Concept

Each node has a **Horizontal Distance (HD)** from the root.

Rules:


Root → HD = 0
Left Child → HD - 1
Right Child → HD + 1


Example:
```
    1
   / \
  2   3
 / \   \
4   5   6
```

Horizontal Distances:


Node HD
1 0
2 -1
3 1
4 -2
5 0
6 2


Top View Result:


4 2 1 3 6


---

# Approach

Use **Breadth First Search (BFS)** because it processes nodes **level by level**.

Steps:

1. Use a **Queue** for BFS traversal.
2. Store `(node, horizontal distance)` in the queue.
3. Use a **TreeMap / HashMap** to store `HD → node value`.
4. If a horizontal distance is **not already present**, store the node.
5. Continue BFS traversal.

---

# Why BFS Instead of DFS?

BFS visits nodes **level by level**, so the **topmost node is visited first**.

DFS may go deep first and store a **lower node instead of the top node**, which leads to incorrect results.

Therefore:


Top View → BFS


---

# Java Implementation

```java
class Pair {
    Node node;
    int hd;

    Pair(Node node, int hd){
        this.node = node;
        this.hd = hd;
    }
}

class Solution {

    static ArrayList<Integer> topView(Node root) {

        ArrayList<Integer> result = new ArrayList<>();

        if(root == null)
            return result;

        TreeMap<Integer,Integer> map = new TreeMap<>();
        Queue<Pair> q = new LinkedList<>();

        q.add(new Pair(root,0));

        while(!q.isEmpty()){

            Pair p = q.poll();
            Node node = p.node;
            int hd = p.hd;

            if(!map.containsKey(hd)){
                map.put(hd,node.data);
            }

            if(node.left != null){
                q.add(new Pair(node.left,hd-1));
            }

            if(node.right != null){
                q.add(new Pair(node.right,hd+1));
            }
        }

        result.addAll(map.values());

        return result;
    }
}
```
Complexity

Time Complexity

O(N)

Every node is visited once.

Space Complexity

O(N)

Used for queue and map.

Interview Pattern

Many Binary Tree View Problems follow similar logic.

Problem	Traversal Used
Top View	BFS
Bottom View	BFS
Vertical Order	BFS
Left View	BFS / DFS
Right View	BFS / DFS
Important Interview Points

Use Horizontal Distance (HD).

Use BFS traversal.

Store the first node for each HD.

Use TreeMap to maintain sorted order.

Key Takeaway
Top View = BFS + Horizontal Distance + First Node at each HD
