# Koko Eating Bananas (Binary Search on Answer)

## Idea

We are **not searching inside the array**.\
We are searching for the **minimum eating speed `k`** such that Koko can
finish all the bananas within `h` hours.

The possible values of `k` are:

``` text
1, 2, 3, ..., max(piles)
```

As the eating speed increases, the total hours required **never
increase**. This monotonic property makes the problem perfect for
**Binary Search on Answer**.

------------------------------------------------------------------------

# Code

``` java
class Solution {
    public boolean canFinish(int[] piles, int mid, int h) {
        int currHr = 0;

        for (int i = 0; i < piles.length; i++) {
            currHr += (piles[i] + mid - 1) / mid;
        }

        return currHr <= h;
    }

    public int minEatingSpeed(int[] piles, int h) {
        Arrays.sort(piles);

        int low = 1;
        int high = piles[piles.length - 1];

        while (low < high) {
            int mid = (low + high) / 2;

            if (canFinish(piles, mid, h)) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }

        return high;
    }
}
```

------------------------------------------------------------------------

# Step 1: Define the Search Space

``` java
int low = 1;
int high = piles[piles.length - 1];
```

Suppose:

``` text
piles = [30,11,23,4,20]
```

After sorting:

``` text
[4,11,20,23,30]
```

Then:

``` text
low  = 1
high = 30
```

This means our possible answers are:

``` text
1 2 3 4 ... 30
```

> **Note:** Sorting is only used to get the maximum pile. A better
> approach is to find the maximum using a loop.

``` java
int high = 0;
for (int pile : piles) {
    high = Math.max(high, pile);
}
```

This avoids sorting and gives the optimal complexity.

------------------------------------------------------------------------

# Step 2: Binary Search

``` java
while (low < high)
```

Initially:

``` text
low = 1
high = 30
```

First iteration:

``` text
mid = (1 + 30) / 2 = 15
```

Now ask:

> **If Koko eats 15 bananas per hour, can she finish within `h` hours?**

The answer is determined by the `canFinish()` function.

------------------------------------------------------------------------

# Step 3: Understanding `canFinish()`

``` java
currHr = 0;
```

For every pile, calculate the number of hours required.

Example:

``` text
piles = [30,11,23,4,20]
mid = 15
```

### Pile = 30

``` text
Hours = ceil(30 / 15) = 2
Formula:
(30 + 15 - 1) / 15 = 44 / 15 = 2
```

Current hours:

``` text
currHr = 2
```

------------------------------------------------------------------------

### Pile = 11

``` text
Hours = ceil(11 / 15) = 1
(11 + 15 - 1) / 15 = 25 / 15 = 1
```

``` text
currHr = 3
```

------------------------------------------------------------------------

### Pile = 23

``` text
Hours = 2
currHr = 5
```

------------------------------------------------------------------------

### Pile = 4

``` text
Hours = 1
currHr = 6
```

------------------------------------------------------------------------

### Pile = 20

``` text
Hours = 2
currHr = 8
```

Final:

``` text
currHr = 8
```

If:

``` text
h = 5
```

Then:

``` text
8 <= 5 → false
```

So eating **15 bananas/hour is too slow**.

------------------------------------------------------------------------

# Step 4: Binary Search Decision

## Case 1: `canFinish()` returns `true`

``` java
high = mid;
```

Why?

If Koko can finish at speed `mid`, then she can also finish at any
**higher** speed.

``` text
mid
mid + 1
mid + 2
...
high
```

All are valid.

But we need the **minimum** speed.

So we continue searching the **left half**.

------------------------------------------------------------------------

## Case 2: `canFinish()` returns `false`

``` java
low = mid + 1;
```

Why?

If Koko **cannot** finish at speed `mid`, then any smaller speed will
also fail.

``` text
1
2
3
...
mid
```

All are invalid.

So we discard them and search the **right half**.

------------------------------------------------------------------------

# Complete Dry Run

Input:

``` text
piles = [30,11,23,4,20]
h = 5
```

### Iteration 1

``` text
low = 1
high = 30
mid = 15

Hours = 8

8 > 5

low = 16
```

------------------------------------------------------------------------

### Iteration 2

``` text
low = 16
high = 30
mid = 23

Hours = 6

6 > 5

low = 24
```

------------------------------------------------------------------------

### Iteration 3

``` text
low = 24
high = 30
mid = 27

Hours = 6

6 > 5

low = 28
```

------------------------------------------------------------------------

### Iteration 4

``` text
low = 28
high = 30
mid = 29

Hours = 6

6 > 5

low = 30
```

------------------------------------------------------------------------

Now:

``` text
low = 30
high = 30
```

Loop ends.

``` java
return high;
```

Output:

``` text
30
```

------------------------------------------------------------------------

# Why does `(pile + mid - 1) / mid` work?

It computes:

``` text
ceil(pile / mid)
```

using only integer arithmetic.

  Pile     Speed   Normal Division   Ceiling            Formula
  ------ ------- ----------------- --------- ------------------
  11           4              2.75         3     (11+4-1)/4 = 3
  8            4                 2         2      (8+4-1)/4 = 2
  7            4              1.75         2      (7+4-1)/4 = 2
  30          15                 2         2   (30+15-1)/15 = 2

This is faster and avoids floating-point calculations.

------------------------------------------------------------------------

# Time Complexity

### Current Code

-   `Arrays.sort()` → **O(n log n)**
-   Binary Search → **O(log M)** iterations (`M = max(piles)`)
-   `canFinish()` → **O(n)** per iteration

Overall:

``` text
O(n log n + n log M)
```

------------------------------------------------------------------------

### Optimized Version (Without Sorting)

Find the maximum pile using a loop instead of sorting.

Overall:

``` text
O(n log M)
```

This is the optimal solution.

------------------------------------------------------------------------

# Key Takeaways

-   Binary search is performed on the **answer**, not on the array.
-   Search space is **1 to max(piles)**.
-   `canFinish()` checks whether a speed is feasible.
-   If a speed works, search left for a smaller valid speed.
-   If a speed fails, search right for a larger speed.
-   Use `(pile + speed - 1) / speed` to efficiently compute
    `ceil(pile / speed)`.
