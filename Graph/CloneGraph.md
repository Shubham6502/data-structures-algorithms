# Clone Graph - DFS + HashMap

## Definition

Clone Graph means creating a **deep copy** of every node and connection in the graph.

We use:

* **DFS** → To traverse the graph
* **HashMap** → To store `Original Node -> Cloned Node`

---

# Node Definition

```java
class Node {
    public int val;
    public List<Node> neighbors;

    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }

    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }

    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
```

---

# Complete Code

```java
class Solution {

    // Original Node -> Cloned Node
    Map<Node, Node> map;

    public void dfs(Node node, Node newNode) {

        for (Node neighbor : node.neighbors) {

            // If neighbor is not cloned
            if (!map.containsKey(neighbor)) {

                // Create cloned neighbor
                Node newNeighbor = new Node(neighbor.val);

                // Store original -> clone
                map.put(neighbor, newNeighbor);

                // Connect cloned nodes
                newNode.neighbors.add(newNeighbor);

                // DFS for neighbor
                dfs(neighbor, newNeighbor);

            } else {

                // Use already created clone
                newNode.neighbors.add(map.get(neighbor));
            }
        }
    }

    public Node cloneGraph(Node node) {

        // Empty graph
        if (node == null) {
            return null;
        }

        // Create clone of first node
        Node newNode = new Node(node.val);

        // Initialize HashMap
        map = new HashMap<>();

        // Store original -> clone
        map.put(node, newNode);

        // Start DFS
        dfs(node, newNode);

        return newNode;
    }
}
```

---

# Syntax Used

## HashMap Declaration

```java
Map<KeyType, ValueType> map = new HashMap<>();
```

Example:

```java
Map<Node, Node> map = new HashMap<>();
```

---

## Add to HashMap

```java
map.put(key, value);
```

Example:

```java
map.put(node, newNode);
```

---

## Get Value from HashMap

```java
map.get(key);
```

Example:

```java
Node clonedNode = map.get(node);
```

---

## Check if Key Exists

```java
map.containsKey(key);
```

Example:

```java
if (!map.containsKey(neighbor)) {
    // Node is not cloned
}
```

---

## Enhanced For Loop

Syntax:

```java
for (DataType variable : collection) {
    
}
```

Example:

```java
for (Node neighbor : node.neighbors) {
    
}
```

---

## Create New Node

Syntax:

```java
Node newNode = new Node(value);
```

Example:

```java
Node newNeighbor = new Node(neighbor.val);
```

---

## Add to List

Syntax:

```java
list.add(element);
```

Example:

```java
newNode.neighbors.add(newNeighbor);
```

---

# Algorithm

## Step 1: Check for Empty Graph

```java
if (node == null) {
    return null;
}
```

---

## Step 2: Create Clone of Starting Node

```java
Node newNode = new Node(node.val);
```

---

## Step 3: Store Original and Clone

```java
map.put(node, newNode);
```

Representation:

```text
Original Node -> Cloned Node
```

---

## Step 4: Traverse Neighbors

```java
for (Node neighbor : node.neighbors) {
    
}
```

---

## Step 5: If Neighbor is Not Cloned

```java
if (!map.containsKey(neighbor)) {
    
}
```

Create clone:

```java
Node newNeighbor = new Node(neighbor.val);
```

Store it:

```java
map.put(neighbor, newNeighbor);
```

Connect it:

```java
newNode.neighbors.add(newNeighbor);
```

Continue DFS:

```java
dfs(neighbor, newNeighbor);
```

---

## Step 6: If Neighbor is Already Cloned

```java
else {
    newNode.neighbors.add(map.get(neighbor));
}
```

We don't create another node.

We use the existing cloned node:

```java
map.get(neighbor)
```

---

# DFS Flow

```text
cloneGraph(node)
        |
        v
Create Clone
        |
        v
Store in HashMap
        |
        v
Start DFS
        |
        v
Visit Neighbor
        |
        +------> Already Cloned?
        |              |
       No             Yes
        |              |
        v              v
Create Clone      Use Existing Clone
        |
        v
Store in Map
        |
        v
Connect Nodes
        |
        v
Call DFS
```

---

# Important Logic

The most important line is:

```java
map.put(node, newNode);
```

This prevents creating multiple copies of the same node.

Without the `HashMap`, a graph containing a cycle can cause infinite recursion.

---

# Time Complexity

```text
O(V + E)
```

Where:

* `V` = Number of vertices (nodes)
* `E` = Number of edges

---

# Space Complexity

```text
O(V)
```

Space is used by:

* HashMap
* DFS recursion stack

---

# Key Pattern to Remember

```text
Graph Clone

1. Create clone
2. Store original -> clone in HashMap
3. Traverse neighbors
4. If not cloned:
   - Create clone
   - Store in HashMap
   - Connect nodes
   - DFS
5. If already cloned:
   - Connect existing clone
```
