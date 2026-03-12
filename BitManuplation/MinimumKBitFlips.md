# Minimum K Consecutive Bit Flips (GFG)

## Problem

You are given a binary array `arr` and an integer `k`.
You can choose any `k` consecutive elements and **flip** them.

Flipping means:

```
0 → 1
1 → 0
```

Return the **minimum number of flips** required to convert the entire array to **all 1s**.
If it is impossible, return **-1**.

---

## Example

Input

```
arr = [1,0,1,0]
k = 2
```

Process

```
1 0 1 0
  ↑ flip(1,2)

1 1 0 0
    ↑ flip(2,3)

1 1 1 1
```

Output

```
2
```

---

## Key Idea (Greedy + Sliding Window)

Instead of flipping `k` elements every time (which is slow), we track flips logically.

We maintain:

* **flip** → indicates whether the current position is affected by an odd number of flips.
* **isFlipped[i]** → records if a flip operation started at index `i`.

When we move forward:

* If `i >= k`, the flip window that started at `i-k` stops affecting us.
* We remove that flip effect using XOR.

Condition to start a new flip:

```
if(arr[i] == flip)
```

Meaning the **effective value becomes 0**, so we must flip.

---

## Algorithm

1. Initialize `steps = 0`, `flip = 0`.
2. Create `isFlipped[]` array to track flip start positions.
3. Traverse the array:

   * Remove expired flip effect.
   * If the current effective bit is `0`, start a flip.
4. If flipping exceeds the array boundary → return `-1`.

---

## Java Implementation

```java
class Solution {
    public int kBitFlips(int[] arr, int k) {
        
        int steps = 0;
        int flip = 0;
        int[] isFlipped = new int[arr.length];

        for(int i = 0; i < arr.length; i++){

            if(i >= k){
                flip ^= isFlipped[i - k];
            }

            if(flip == arr[i]){

                if(i + k > arr.length) return -1;

                steps++;
                flip ^= 1;
                isFlipped[i] = 1;
            }
        }

        return steps;
    }
}
```

---

## Complexity

```
Time Complexity  : O(n)
Space Complexity : O(n)
```

---

## Key Insight

Instead of physically flipping `k` elements every time (**O(n × k)**), we use a **flip state variable** and track flip windows, reducing the complexity to **O(n)**.
