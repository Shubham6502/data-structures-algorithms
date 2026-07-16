# 🚗 Car Fleet

**LeetCode 853 - Medium**

## 📝 Problem Statement

There are `n` cars traveling toward the same destination (`target`).

- Each car has a starting `position[i]`.
- Each car moves at a constant `speed[i]`.
- A car **cannot overtake** another car.
- If a faster car catches a slower car before reaching the target, they form a **fleet** and travel together at the slower car's speed.

Return the **number of car fleets** that will arrive at the destination.

---

## 💡 Idea

Instead of simulating every car's movement, calculate **how long each car takes to reach the target**.

```
Time = (target - position) / speed
```

### Key Observation

- Sort cars by **position in descending order** (closest to target first).
- While moving from front to back:
  - If the current car takes **more time** than the fleet ahead, it **cannot catch up**, so it forms a **new fleet**.
  - Otherwise, it catches the fleet ahead and becomes part of that fleet.

---

# Algorithm

1. Store every car as `(position, speed)`.
2. Sort by position in descending order.
3. Compute each car's arrival time.
4. Maintain a stack containing the arrival time of each fleet.
5. If

```
currentTime > stack.peek()
```

push it because it forms a new fleet.

Otherwise, ignore it because it joins the fleet ahead.

6. Return the stack size.

---

# Dry Run

## Input

```text
target = 12

position = [10,8,0,5,3]
speed    = [2 ,4,1,1,3]
```

---

## Step 1 : Pair Position & Speed

| Position | Speed |
|----------|------|
|10|2|
|8|4|
|0|1|
|5|1|
|3|3|

---

## Step 2 : Sort by Position (Descending)

Closest cars come first.

| Position | Speed |
|----------|------|
|10|2|
|8|4|
|5|1|
|3|3|
|0|1|

---

## Step 3 : Calculate Arrival Time

| Position | Speed | Time |
|----------|------|------|
|10|2|1|
|8|4|1|
|5|1|7|
|3|3|3|
|0|1|12|

---

# Stack Visualization

### Car 1

```
Position = 10
Time = 1

Stack

1
```

Fleet Count = **1**

---

### Car 2

```
Position = 8
Time = 1

Top = 1

1 <= 1
```

This car catches the fleet ahead.

```
Stack

1
```

Fleet Count = **1**

---

### Car 3

```
Position = 5
Time = 7

Top = 1

7 > 1
```

Cannot catch the fleet ahead.

Create a new fleet.

```
Stack

7
1
```

Fleet Count = **2**

---

### Car 4

```
Position = 3
Time = 3

Top = 7

3 <= 7
```

This car catches the fleet with arrival time **7**.

```
Stack

7
1
```

Fleet Count = **2**

---

### Car 5

```
Position = 0
Time = 12

Top = 7

12 > 7
```

Cannot catch any fleet.

```
Stack

12
7
1
```

Fleet Count = **3**

---

# Visual Representation

```
Target (12)
│
│
│  Position 10 (Time = 1)
│      🚗
│
│  Position 8  (Time = 1)
│      🚗
│
│  ───────────────► Fleet 1
│
│
│  Position 5  (Time = 7)
│      🚙
│
│  Position 3  (Time = 3)
│      🚗
│
│  🚗 catches 🚙
│
│  ───────────────► Fleet 2
│
│
│  Position 0 (Time = 12)
│      🚘
│
│  Cannot catch anyone
│
│  ───────────────► Fleet 3
│
▼ Target
```

Final Fleets

```
Fleet 1 → Cars at positions 10 and 8

Fleet 2 → Cars at positions 5 and 3

Fleet 3 → Car at position 0
```

Answer

```
3
```

---

# Why Does This Work?

Suppose the fleet ahead reaches the target in **7 hours**.

If the current car reaches in:

- **3 hours** → It is faster and catches the fleet before the target.
- **7 hours** → Meets exactly at the target, still one fleet.
- **12 hours** → Too slow to catch, forms a new fleet.

Therefore,

```
currentTime <= topTime
```

➡️ Joins the existing fleet.

```
currentTime > topTime
```

➡️ Forms a new fleet.

---

# Complexity Analysis

### Time Complexity

- Sorting → **O(n log n)**
- Stack Traversal → **O(n)**

Overall:

```
O(n log n)
```

---

### Space Complexity

- Car Array → **O(n)**
- Stack → **O(n)**

Overall:

```
O(n)
```

---

# Java Solution

```java
class Solution {
    public int carFleet(int target, int[] position, int[] speed) {

        ArrayList<int[]> cars = new ArrayList<>();

        for (int i = 0; i < position.length; i++) {
            cars.add(new int[]{position[i], speed[i]});
        }

        Collections.sort(cars, (a, b) -> b[0] - a[0]);

        Stack<Double> stack = new Stack<>();

        for (int[] car : cars) {

            double time = (double) (target - car[0]) / car[1];

            if (stack.isEmpty() || time > stack.peek()) {
                stack.push(time);
            }
        }

        return stack.size();
    }
}
```
