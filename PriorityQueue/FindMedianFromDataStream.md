# Find Median from Data Stream

## Problem
Design a data structure that supports the following operations efficiently:

- `addNum(int num)` → Add a number to the data stream.
- `findMedian()` → Return the median of all inserted numbers.

---

## Approach: Two Heaps

We use **two priority queues (heaps)**.

- **Max Heap (`maxHeap`)**
  - Stores the smaller half of the numbers.
  - The largest element of this half remains at the top.

- **Min Heap (`minHeap`)**
  - Stores the larger half of the numbers.
  - The smallest element of this half remains at the top.

### Why Two Heaps?

This allows us to:
- Insert elements in **O(log n)**.
- Retrieve the median in **O(1)**.

---

## Algorithm

### `addNum(int num)`

1. Insert the new number into the **max heap**.
2. Move the largest element from the max heap to the min heap.
3. If the min heap becomes larger than the max heap, move its smallest element back to the max heap.

This ensures:

- `maxHeap.size() >= minHeap.size()`
- Size difference is never greater than **1**.
- Every element in `maxHeap` is less than or equal to every element in `minHeap`.

---

### `findMedian()`

There are two cases:

#### Case 1: Odd number of elements

If `maxHeap` has one extra element, the median is simply:

```java
maxHeap.peek()
```

#### Case 2: Even number of elements

The median is the average of the two middle elements:

```java
(maxHeap.peek() + minHeap.peek()) / 2.0
```

---

## Dry Run

### Insert 5

```
maxHeap = [5]
minHeap = []

Median = 5
```

---

### Insert 10

After balancing:

```
maxHeap = [5]
minHeap = [10]

Median = (5 + 10) / 2 = 7.5
```

---

### Insert 2

After balancing:

```
maxHeap = [5,2]
minHeap = [10]

Median = 5
```

---

### Insert 8

After balancing:

```
maxHeap = [5,2]
minHeap = [8,10]

Median = (5 + 8) / 2 = 6.5
```

---

### Insert 12

After balancing:

```
maxHeap = [8,5,2]
minHeap = [10,12]

Median = 8
```

---

## Complexity Analysis

| Operation | Time Complexity | Space Complexity |
|-----------|-----------------|------------------|
| addNum() | **O(log n)** | O(n) |
| findMedian() | **O(1)** | O(1) |

---

## Key Points

- Max Heap stores the **smaller half**.
- Min Heap stores the **larger half**.
- Max Heap always has either:
  - the same number of elements as Min Heap, or
  - exactly one extra element.
- Median can always be obtained from the heap tops without sorting the entire data stream.

---

## Java Code

```java
class MedianFinder {

    PriorityQueue<Integer> minHeap;
    PriorityQueue<Integer> maxHeap;

    public MedianFinder() {
        minHeap = new PriorityQueue<>();
        maxHeap = new PriorityQueue<>(Collections.reverseOrder());
    }

    public void addNum(int num) {
        maxHeap.add(num);
        minHeap.add(maxHeap.poll());

        if (minHeap.size() > maxHeap.size()) {
            maxHeap.add(minHeap.poll());
        }
    }

    public double findMedian() {
        return maxHeap.size() > minHeap.size()
                ? maxHeap.peek()
                : (maxHeap.peek() + minHeap.peek()) / 2.0;
    }
}
```

---

## Interview Tip

**Why insert into the max heap first?**

By always inserting into the max heap first and then transferring its largest element to the min heap, we automatically maintain the ordering:

```
All elements in maxHeap <= All elements in minHeap
```

Finally, we rebalance the heaps so that the max heap never has fewer elements than the min heap. This guarantees constant-time median retrieval.
