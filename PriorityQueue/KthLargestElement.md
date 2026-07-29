# Kth Largest Element in a Stream (LeetCode 703)

## Problem Statement

Design a class to find the **kth largest element** in a stream of numbers.

Implement the following:

- `KthLargest(int k, int[] nums)` initializes the object with an integer `k` and the initial stream of integers.
- `int add(int val)` adds a new element to the stream and returns the **kth largest element**.

---

## Approach

Use a **Min Heap (PriorityQueue)** of size `k`.

### Key Idea

- Keep only the **k largest elements** in the heap.
- The **smallest element** among these `k` elements is exactly the **kth largest element**.
- Whenever the heap size becomes greater than `k`, remove the smallest element.

---

## Algorithm

### Constructor

1. Create a Min Heap.
2. Store `k`.
3. Insert every element from `nums`.
4. If heap size exceeds `k`, remove the smallest element.

### add(val)

1. Insert `val` into the heap.
2. If heap size exceeds `k`, remove the smallest element.
3. Return the top of the heap (`peek()`), which is the kth largest element.

---

## Dry Run

### Input

```text
k = 3
nums = [4, 5, 8, 2]
```

### Building Heap

Insert `4`

```
[4]
```

Insert `5`

```
[4,5]
```

Insert `8`

```
[4,5,8]
```

Insert `2`

```
[2,4,8,5]
```

Heap size becomes 4 (>3)

Remove smallest (`2`)

```
[4,5,8]
```

Current kth largest = **4**

---

### add(3)

Insert

```
[3,4,8,5]
```

Remove smallest (`3`)

```
[4,5,8]
```

Return

```
4
```

---

### add(5)

Insert

```
[4,5,8,5]
```

Remove smallest (`4`)

```
[5,5,8]
```

Return

```
5
```

---

### add(10)

Insert

```
[5,5,8,10]
```

Remove smallest (`5`)

```
[5,10,8]
```

Return

```
5
```

---

### add(9)

Insert

```
[5,9,8,10]
```

Remove smallest (`5`)

```
[8,9,10]
```

Return

```
8
```

---

### add(4)

Insert

```
[4,8,10,9]
```

Remove smallest (`4`)

```
[8,9,10]
```

Return

```
8
```

---

## Code

```java
class KthLargest {

    PriorityQueue<Integer> minHeap;
    int k;

    public KthLargest(int k, int[] nums) {
        minHeap = new PriorityQueue<>();
        this.k = k;

        for (int num : nums) {
            minHeap.add(num);

            if (minHeap.size() > k) {
                minHeap.poll();
            }
        }
    }

    public int add(int val) {
        minHeap.add(val);

        if (minHeap.size() > k) {
            minHeap.poll();
        }

        return minHeap.peek();
    }
}
```

---

## Complexity Analysis

### Constructor

- Insert `n` elements.
- Heap size never exceeds `k`.

**Time Complexity:** `O(n log k)`

**Space Complexity:** `O(k)`

---

### add(val)

Insertion into heap:

```
O(log k)
```

Possible removal:

```
O(log k)
```

Overall:

**Time Complexity:** `O(log k)`

**Space Complexity:** `O(k)`

---

## Why Min Heap?

Suppose:

```
k = 3

Numbers:
2, 4, 7, 9, 15, 20
```

Keep only the 3 largest numbers:

```
[9, 15, 20]
```

The smallest element among them is:

```
9
```

Which is exactly the **3rd largest element**.

The Min Heap always keeps this value at the top (`peek()`), allowing us to retrieve the answer in **O(1)** time.

---

## Key Points

- Use a **Min Heap** of size `k`.
- Remove the smallest element whenever the heap size exceeds `k`.
- The heap always stores the **k largest elements** seen so far.
- The root (`peek()`) is always the **kth largest element**.
- Efficient for continuously processing a stream of numbers.
