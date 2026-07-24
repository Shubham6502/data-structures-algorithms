# Remove Nth Node From End of List

## 🧩 Problem

Given the head of a linked list and an integer `n`, remove the **nth node from the end** of the list and return the head of the modified list.

---

## 💡 Approach (Two Pointers + Dummy Node)

We use:

- A **dummy node** to handle edge cases (like deleting the head).
- Two pointers:
  - `fast`
  - `slow`

### Idea

1. Create a dummy node pointing to the head.
2. Move the `fast` pointer `n` steps ahead.
3. Move both `fast` and `slow` together until `fast` reaches the last node.
4. Now `slow` is just before the node to delete.
5. Skip the target node using:
   ```java
   slow.next = slow.next.next;
   ```
6. Return `dummy.next`.

---

## 🔍 Why Use a Dummy Node?

Without a dummy node, deleting the **first node** becomes difficult.

Example:

```
Head
 ↓
1 → 2 → 3

n = 3
```

We need to remove `1`.

After using a dummy node:

```
Dummy → 1 → 2 → 3
```

Now `slow` stops at the dummy node, making deletion easy.

---

## 📝 Dry Run

### Input

```
head = 1 → 2 → 3 → 4 → 5
n = 2
```

---

### Step 1: Create Dummy

```
dummy → 1 → 2 → 3 → 4 → 5
```

```
slow = dummy
fast = dummy
```

---

### Step 2: Move Fast `n` Steps

Move `fast` 2 times.

```
dummy → 1 → 2 → 3 → 4 → 5
          ↑
        fast
```

---

### Step 3: Move Both Together

Move both until `fast.next == null`.

| Fast | Slow |
|------|------|
| 3 | 1 |
| 4 | 2 |
| 5 | 3 |

Final position:

```
dummy → 1 → 2 → 3 → 4 → 5
                 ↑     ↑
               slow   fast
```

`slow` is just before the node to delete.

---

### Step 4: Delete Node

```
slow.next = slow.next.next;
```

Before:

```
3 → 4 → 5
```

After:

```
3 ─────→ 5
```

Final list:

```
1 → 2 → 3 → 5
```

---

## 🌳 Pointer Visualization

Initial:

```
dummy → 1 → 2 → 3 → 4 → 5
↑
S,F
```

After moving `fast` by `n`:

```
dummy → 1 → 2 → 3 → 4 → 5
↑         ↑
S         F
```

After moving together:

```
dummy → 1 → 2 → 3 → 4 → 5
                ↑         ↑
                S         F
```

Delete:

```
3 → 4 → 5

↓

3 ─────→ 5
```

---

## ✅ Java Code

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {

        ListNode node = new ListNode(0);
        node.next = head;

        ListNode fast = node;
        ListNode slow = node;

        // Move fast pointer n steps
        for(int i = 0; i < n; i++) {
            if(fast.next != null)
                fast = fast.next;
        }

        // Move both pointers
        while(fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }

        // Remove target node
        slow.next = slow.next.next;

        return node.next;
    }
}
```

---

## ⏱️ Complexity Analysis

### Time Complexity

- Move `fast` pointer: **O(n)**
- Move both pointers: **O(L - n)**

Overall:

```
O(L)
```

where `L` is the length of the linked list.

---

### Space Complexity

Only a few pointers are used.

```
O(1)
```

---

## 🔑 Key Points to Remember

- Use a **dummy node** to handle deletion of the head node.
- Keep a gap of `n` nodes between `fast` and `slow`.
- When `fast` reaches the last node, `slow` will be just before the node to delete.
- Delete the node in one step:

```java
slow.next = slow.next.next;
```

- Return `dummy.next`, not `head`.

---

