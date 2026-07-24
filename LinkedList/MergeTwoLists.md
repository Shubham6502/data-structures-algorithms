# Merge Two Sorted Linked Lists (Recursive)

## 🧩 Problem

You are given the heads of two **sorted linked lists** `list1` and `list2`.

Merge the two lists into one sorted linked list by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

---

## 💡 Approach (Recursion)

### Base Cases

If one list is empty, simply return the other list because it is already sorted.

```java
if(list1 == null) return list2;
if(list2 == null) return list1;
```

---

### Recursive Step

Compare the current nodes of both lists.

### Case 1: `list1.val < list2.val`

- `list1` should come first.
- Merge the remaining nodes of `list1` with `list2`.
- Attach the result to `list1.next`.

```java
list1.next = mergeTwoLists(list1.next, list2);
return list1;
```

---

### Case 2: `list2.val <= list1.val`

- `list2` should come first.
- Merge `list1` with the remaining nodes of `list2`.
- Attach the result to `list2.next`.

```java
list2.next = mergeTwoLists(list1, list2.next);
return list2;
```

---

## 📝 Dry Run

### Input

```
list1: 1 -> 2 -> 4
list2: 1 -> 3 -> 4
```

### Recursive Calls

```
merge(1,1)
│
├── 1 is not smaller
│
└── Take list2(1)
      ↓
merge(1,3)
│
├── 1 < 3
│
└── Take list1(1)
      ↓
merge(2,3)
│
├── 2 < 3
│
└── Take list1(2)
      ↓
merge(4,3)
│
├── 3 < 4
│
└── Take list2(3)
      ↓
merge(4,4)
│
├── 4 is not smaller
│
└── Take list2(4)
      ↓
merge(4,null)
      ↓
Return remaining list1(4)
```

### Final Output

```
1 -> 1 -> 2 -> 3 -> 4 -> 4
```

---

## 🌳 Recursion Tree

```
merge(1,1)
       |
   take list2
       |
merge(1,3)
       |
   take list1
       |
merge(2,3)
       |
   take list1
       |
merge(4,3)
       |
   take list2
       |
merge(4,4)
       |
   take list2
       |
merge(4,null)
       |
return 4
```

---

## ✅ Java Code

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {

        if(list1 == null) return list2;
        if(list2 == null) return list1;

        if(list1.val < list2.val) {
            list1.next = mergeTwoLists(list1.next, list2);
            return list1;
        } else {
            list2.next = mergeTwoLists(list1, list2.next);
            return list2;
        }
    }
}
```

---

## ⏱️ Complexity Analysis

### Time Complexity

Each node from both lists is visited exactly once.

```
O(n + m)
```

- `n` = length of `list1`
- `m` = length of `list2`

---

### Space Complexity

The recursion stack stores one call for each processed node.

```
O(n + m)
```

*(If an iterative solution is used, the extra space becomes **O(1)**.)*

---

## 🔑 Key Points to Remember

- Handle base cases first.
- Compare the current nodes of both lists.
- Attach the smaller node to the answer.
- Recursively merge the remaining lists.
- Return the smaller node as the new head.
- The recursion naturally builds the merged list in sorted order.

---

