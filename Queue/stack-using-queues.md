#Chef Builds Stack | Stack Using Queues (codechef)

## 📌 Problem Statement

Chef wants to implement a **Stack** data structure but he only has access to **Queues**.

Your task is to implement a stack using two queues and support the following operations:

- `push(x)` → Insert element `x` into the stack
- `pop()` → Remove and return the top element
- `top()` → Return the top element without removing it
- `empty()` → Return `true` if the stack is empty, otherwise `false`

---

## 🧠 Approach (Using Two Queues)

A stack follows **LIFO (Last In First Out)** order, while a queue follows **FIFO (First In First Out)** order.

To simulate stack behavior using queues:

- Use two queues: `q1` and `q2`
- During `push`:
  - Move all elements from `q1` to `q2`
  - Insert the new element into `q1`
  - Move all elements back from `q2` to `q1`

This ensures the **new element always stays at the front of q1**, which behaves like the **top of the stack**.

Thus:

- `push()` rearranges elements
- `pop()` simply removes from the front of `q1`

---

## ⚙️ Algorithm Steps

### Push Operation
1. Move all elements from `q1` to `q2`
2. Insert new element into `q1`
3. Move all elements back from `q2` to `q1`

### Pop Operation
- Remove element from `q1`

### Top Operation
- Return front element of `q1`

### Empty Operation
- Stack is empty if both queues are empty

---

## ✅ Java Solution

```java
import java.util.*;

class StackUsingQueues {

    Queue<Integer> q1 = new LinkedList<>();
    Queue<Integer> q2 = new LinkedList<>();

    public void push(int x) {

        while(!q1.isEmpty()){
            q2.add(q1.poll());
        }

        q1.add(x);

        while(!q2.isEmpty()){
            q1.add(q2.poll());
        }
    }

    public int pop() {
        return q1.poll();
    }

    public int top() {
        return q1.peek();
    }

    public boolean empty() {
        return (q1.isEmpty() && q2.isEmpty());
    }
}
```
⏱️ Complexity Analysis
Operation	Time Complexity
push()	O(n)
pop()	O(1)
top()	O(1)
empty()	O(1)

Space Complexity

O(n)

Two queues store the stack elements.

📊 Example

Operations

push(1)  
push(2)  
push(3)  
top()  
pop()  

Output

3
3

Explanation

Stack: [1,2,3]  
Top = 3  
Pop removes 3  
Key Concepts Used  
Queue Data Structure  
Stack Implementation  
LIFO vs FIFO  

Data Structure Simulation  
🚀 Learning Outcome

This problem demonstrates how one data structure can simulate another by carefully controlling the order of operations.
It strengthens understanding of queues, stacks, and data structure design techniques.

👨‍💻 Author

Shubham Patil

GitHub
https://github.com/Shubham6502
