# Last Stone Weight

## Problem
You are given an array of integers `stones` where each integer represents the weight of a stone.

Each turn:
- Take the two heaviest stones.
- If both stones have the same weight, both are destroyed.
- If they have different weights, the smaller stone is destroyed and the larger stone's new weight becomes `x - y`.
- Continue until at most one stone remains.

Return the weight of the last remaining stone. If no stones remain, return `0`.

---

## Approach
A **Max Heap (Priority Queue)** is the best choice because we repeatedly need the two largest stones.

### Steps
1. Insert all stones into a max heap.
2. While the heap contains at least two stones:
   - Remove the two largest stones.
   - If they are different, insert their difference back into the heap.
3. If the heap is empty, return `0`; otherwise, return the remaining stone.

---

## Algorithm
1. Create a max heap.
2. Add every stone into the heap.
3. Repeat while heap size is at least `2`:
   - Remove the largest stone `x`.
   - Remove the second largest stone `y`.
   - If `x != y`, insert `x - y` back into the heap.
4. Return the remaining stone or `0` if the heap is empty.

---

## Dry Run

### Input
```text
stones = [2,7,4,1,8,1]
```

### Max Heap
```text
[8,7,4,2,1,1]
```

| Removed Stones | Remaining Heap |
|---------------|----------------|
| 8, 7 → 1 | [4,2,1,1,1] |
| 4, 2 → 2 | [2,1,1,1] |
| 2, 1 → 1 | [1,1,1] |
| 1, 1 → 0 | [1] |

**Output**
```text
1
```

---

## Java Solution

```java
class Solution {
    public int lastStoneWeight(int[] stones) {
        PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());

        for (int stone : stones) {
            pq.add(stone);
        }

        while (pq.size() >= 2) {
            int x = pq.poll();
            int y = pq.poll();

            if (x != y) {
                pq.add(x - y);
            }
        }

        return pq.isEmpty() ? 0 : pq.poll();
    }
}
```

---

## Time Complexity
- Building the heap: **O(n log n)**
- Each smash operation takes **O(log n)**
- Overall: **O(n log n)**

---

## Space Complexity
- **O(n)** for the max heap.

---

## Key Idea
- Use a **Max Heap** to efficiently access the two heaviest stones.
- After smashing, only the weight difference (if any) is pushed back into the heap.
- Continue until one or zero stones remain.
