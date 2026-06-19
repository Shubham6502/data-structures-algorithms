# Largest Altitude

## Problem Statement

You are given an integer array `gain` where `gain[i]` represents the net gain in altitude between points.

The biker starts at altitude `0`. Return the **highest altitude** reached during the trip.

### Example

**Input:**

```java
gain = [-5,1,5,0,-7]
```

**Output:**

```java
1
```

**Explanation:**
Altitudes during the trip:

```
0 → -5 → -4 → 1 → 1 → -6
```

The highest altitude reached is `1`.


---

## Java Solution

```java
class Solution {
    public int largestAltitude(int[] gain) {
        int max = 0;
        int sum = 0;

        for (int i = 0; i < gain.length; i++) {
            sum += gain[i];
            max = Math.max(sum, max);
        }

        return max;
    }
}
```

---

## Complexity Analysis

* **Time Complexity:** O(n)

  * We traverse the array once.

* **Space Complexity:** O(1)

  * Only two extra variables are used.

---

