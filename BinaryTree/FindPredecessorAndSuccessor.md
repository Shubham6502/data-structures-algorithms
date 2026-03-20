# 📘 BST: Find Predecessor and Successor

## 🔹 Problem Statement
Given a Binary Search Tree (BST) and a key, find:
- **Predecessor (pre)** → largest value smaller than key  
- **Successor (succ)** → smallest value greater than key  

---

## 🧠 Approach

### Case 1: Key is found
- Predecessor → Maximum value in left subtree  
- Successor → Minimum value in right subtree  

### Case 2: Key is not found
- If `root.data > key` → current node can be successor → move left  
- If `root.data < key` → current node can be predecessor → move right  

---

## ✅ Correct Java Code

```java
import java.util.*;

class Solution {
    Node pre = null;
    Node succ = null;

    public void findSucPreUtil(Node root, int key) {
        if (root == null) return;

        if (root.data == key) {

            // Find predecessor (max in left subtree)
            if (root.left != null) {
                Node temp = root.left;
                while (temp.right != null) {
                    temp = temp.right;
                }
                pre = temp;
            }

            // Find successor (min in right subtree)
            if (root.right != null) {
                Node temp = root.right;
                while (temp.left != null) {
                    temp = temp.left;
                }
                succ = temp;
            }

            return;
        }

        else if (root.data > key) {
            succ = root;
            findSucPreUtil(root.left, key);
        } 
        else {
            pre = root;
            findSucPreUtil(root.right, key);
        }
    }

    public ArrayList<Node> findPreSuc(Node root, int key) {
        ArrayList<Node> ans = new ArrayList<>();
        findSucPreUtil(root, key);

        ans.add(pre);
        ans.add(succ);
        return ans;
    }
}
```
⏱️ Time Complexity

O(h) → where h is height of BST
