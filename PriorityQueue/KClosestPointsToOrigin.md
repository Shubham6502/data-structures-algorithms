# 973. K Closest Points to Origin

## Problem
Given an array of points where `points[i] = [xi, yi]` represents a point on the X-Y plane and an integer `k`, return the `k` closest points to the origin `(0,0)`.

The distance between a point `(x, y)` and the origin is:

\[
\sqrt{x^2 + y^2}
\]

Since the square root does not affect the comparison of distances, we compare using:

\[
x^2 + y^2
\]

---

## Approach
Use a **Min Heap (Priority Queue)** to store each point along with its squared distance from the origin.

### Why Min Heap?
- The point with the **smallest distance** should always be available at the top.
- After inserting all points into the heap, simply remove the first `k` points.

---

## Algorithm

1. Create a `Pair` class containing:
   - `dist` → Squared distance from the origin.
   - `point` → Coordinates of the point.
2. Create a **Min Heap** based on `dist`.
3. Traverse all points:
   - Compute `dist = x² + y²`.
   - Insert `(dist, point)` into the heap.
4. Remove the first `k` elements from the heap.
5. Store them in the answer array.
6. Return the result.

---

## Dry Run

### Input
```text
points = [[3,3],[5,-1],[-2,4]]
k = 2
```

### Step 1: Compute Distances

| Point | Squared Distance |
|-------|------------------:|
| (3,3) | 18 |
| (5,-1) | 26 |
| (-2,4) | 20 |

### Step 2: Insert into Min Heap

```text
        (18)
       /    \
    (26)   (20)
```

Top of the heap = `(3,3)` because it has the smallest distance.

### Step 3: Remove k = 2 Elements

**First Poll**
```text
(3,3)
```

Remaining Heap
```text
(20)
 |
(26)
```

**Second Poll**
```text
(-2,4)
```

Answer:

```text
[[3,3],[-2,4]]
```

---

## Java Solution

```java
class Solution {

    static class Pair {
        int dist;
        int[] point;

        Pair(int dist, int[] point) {
            this.dist = dist;
            this.point = point;
        }
    }

    public int[][] kClosest(int[][] points, int k) {

        PriorityQueue<Pair> pq =
            new PriorityQueue<>((a, b) -> Integer.compare(a.dist, b.dist));

        for (int[] point : points) {
            int dist = point[0] * point[0] + point[1] * point[1];
            pq.add(new Pair(dist, point));
        }

        int[][] ans = new int[k][2];

        for (int i = 0; i < k; i++) {
            ans[i] = pq.poll().point;
        }

        return ans;
    }
}
```

---

## Time Complexity

- Computing distances: **O(n)**
- Inserting all points into the Min Heap: **O(n log n)**
- Removing `k` points: **O(k log n)**

**Overall Time Complexity:**

```text
O(n log n)
```

---

## Space Complexity

- Priority Queue stores all `n` points.

```text
O(n)
```

---

## Key Idea

- Calculate the **squared distance** (`x² + y²`) for every point.
- Store each point with its distance in a **Min Heap**.
- The heap always keeps the closest point at the top.
- Remove the top `k` points to get the answer.
- No need to calculate the square root because it does not change the relative order of distances.
