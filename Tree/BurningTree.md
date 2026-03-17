# 🔥 Burning Tree Problem

## 🧩 Problem Statement

Given a binary tree and a target node, the tree starts burning from the target node.  
In each unit of time, the fire spreads to:

- Left child
- Right child
- Parent node

👉 Find the **minimum time required to burn the entire tree**.

---

## 🧠 Approach

To solve this problem, we follow **two main steps**:

### 1. Convert Tree to Graph (Parent Mapping)
- Use **DFS** to store each node’s parent in a `HashMap`
- Also, identify the **target node**

### 2. Apply BFS (Level Order Traversal)
- Start BFS from the target node
- Spread fire to:
  - Left child
  - Right child
  - Parent
- Track visited nodes to avoid cycles
- The maximum level reached = **answer**

---

## ⚙️ Algorithm Steps

1. Create a `HashMap<Node, Node>` to store parent relationships  
2. Run DFS to:
   - Fill parent map
   - Find target node  
3. Use a `Queue` for BFS  
4. Use a `Set` to track visited nodes  
5. Traverse level by level  
6. Return maximum time

---

## 💻 Code (Java)

```java
class Solution {
    Node targetNode;

    class Info {
        Node node;
        int level;

        Info(Node node, int level) {
            this.node = node;
            this.level = level;
        }
    }

    public void dfs(Node root, HashMap<Node, Node> map, int target) {
        if (root == null) return;

        if (root.data == target) targetNode = root;

        if (root.left != null) {
            map.put(root.left, root);
        }

        if (root.right != null) {
            map.put(root.right, root);
        }

        dfs(root.left, map, target);
        dfs(root.right, map, target);
    }

    public int minTime(Node root, int target) {
        HashMap<Node, Node> map = new HashMap<>();

        // Step 1: Build parent map and find target
        dfs(root, map, target);

        Queue<Info> q = new LinkedList<>();
        HashSet<Node> visited = new HashSet<>();

        q.add(new Info(targetNode, 0));
        visited.add(targetNode);

        int res = 0;

        // Step 2: BFS
        while (!q.isEmpty()) {
            Info curr = q.poll();
            Node currNode = curr.node;
            int level = curr.level;

            res = Math.max(res, level);

            if (currNode.left != null && !visited.contains(currNode.left)) {
                visited.add(currNode.left);
                q.add(new Info(currNode.left, level + 1));
            }

            if (currNode.right != null && !visited.contains(currNode.right)) {
                visited.add(currNode.right);
                q.add(new Info(currNode.right, level + 1));
            }

            Node parent = map.get(currNode);
            if (parent != null && !visited.contains(parent)) {
                visited.add(parent);
                q.add(new Info(parent, level + 1));
            }
        }

        return res;
    }
}
```
⏱️ Complexity Analysis
Type	Complexity
Time	O(N)
Space	O(N)

Each node is visited once in DFS and BFS

📌 Key Points

Tree is treated as an undirected graph

BFS ensures minimum time calculation

visited set prevents infinite loops

Parent mapping is critical step

🎯 Interview Tip

Explain like this:

"I first map each node to its parent to simulate an undirected graph. Then I perform BFS from the target node and calculate the maximum time required to reach all nodes."
```
 Example
        1
       / \
      2   3
     / \
    4   5
```
Target = 2

Burn sequence:

Time 0 → 2

Time 1 → 4, 5, 1

Time 2 → 3

👉 Output = 2
