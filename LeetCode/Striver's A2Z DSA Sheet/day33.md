# 161. Implement Stack using Queues

### Problem Link

[LeetCode - Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)

## Approach 1: Two Queues

### Idea/Intuition

Use two queues to simulate stack operations. The main queue (`q1`) holds the elements of the stack, while the helper queue (`q2`) is used to manage the order of elements during the push operation.

### Algorithm

1. **Push Operation**:

   - Push the new element `x` into `q2`.
   - Move all elements from `q1` to `q2`.
   - Swap the names of `q1` and `q2`.

2. **Pop Operation**:

   - Remove and return the front element from `q1`.

3. **Top Operation**:

   - Return the front element of `q1` without removing it.

4. **Empty Operation**:
   - Check if `q1` is empty.

### Time Complexity

- **O(n)** for `push`, **O(1)** for `pop` and `top`.

### Space Complexity

- **O(n)**: Space used by the two queues.

### Code

```cpp
class MyStack {
private:
    queue<int> q1; // Main queue
    queue<int> q2; // Helper queue

public:
    MyStack() {}

    void push(int x) {
        q2.push(x);
        while (!q1.empty()) {
            q2.push(q1.front());
            q1.pop();
        }
        swap(q1, q2);
    }

    int pop() {
        if (q1.empty()) return -1; // Stack is empty
        int top = q1.front();
        q1.pop();
        return top;
    }

    int top() {
        if (q1.empty()) return -1; // Stack is empty
        return q1.front();
    }

    bool empty() {
        return q1.empty();
    }
};
```

## Approach 2: Single Queue

### Idea/Intuition

Use a single queue to implement stack operations by rotating the queue to ensure that the last pushed element is always at the front.

### Algorithm

1. **Push Operation**:

   - Push the new element `x` into the queue.
   - Rotate the queue to move all elements behind the newly added element to the back.

2. **Pop Operation**:

   - Remove and return the front element of the queue.

3. **Top Operation**:

   - Return the front element of the queue without removing it.

4. **Empty Operation**:
   - Check if the queue is empty.

### Time Complexity

- **O(n)** for `push`, **O(1)** for `pop` and `top`.

### Space Complexity

- **O(n)**: Space used by the queue.

### Code

```cpp
class MyStack {
private:
    queue<int> q;

public:
    MyStack() {}

    void push(int x) {
        q.push(x);
        for (int i = 0; i < q.size() - 1; ++i) {
            q.push(q.front());
            q.pop();
        }
    }

    int pop() {
        if (q.empty()) return -1; // Stack is empty
        int top = q.front();
        q.pop();
        return top;
    }

    int top() {
        if (q.empty()) return -1; // Stack is empty
        return q.front();
    }

    bool empty() {
        return q.empty();
    }
};
```

# 162. Implement Queue using Stacks

### Problem Link

[LeetCode - Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)

## Approach 1: Two Stacks for Queue Operations

### Idea/Intuition

Use two stacks to implement queue operations. The main stack (`s1`) holds the elements, and the helper stack (`s2`) is used to reverse the order when popping or peeking.

### Algorithm

1. **Push Operation**:

   - Push the new element `x` onto the main stack `s1`.

2. **Pop Operation**:

   - If `s2` is empty, transfer all elements from `s1` to `s2`.
   - Pop the top element from `s2`, which represents the front of the queue.

3. **Peek Operation**:

   - If `s2` is empty, transfer all elements from `s1` to `s2`.
   - Return the top element of `s2` without removing it.

4. **Empty Operation**:
   - The queue is empty if both `s1` and `s2` are empty.

### Time Complexity

- **O(1)** for `push`, **O(n)** for `pop` and `peek` in the worst case when transferring elements.

### Space Complexity

- **O(n)**: Space used by the two stacks.

### Code

```cpp
class MyQueue {
private:
    stack<int> s1; // Main stack for queue operations
    stack<int> s2; // Helper stack for transferring elements

public:
    MyQueue() {}

    void push(int x) {
        s1.push(x);
    }

    int pop() {
        if (s2.empty()) {
            while (!s1.empty()) {
                s2.push(s1.top());
                s1.pop();
            }
        }
        int front = s2.top();
        s2.pop();
        return front;
    }

    int peek() {
        if (s2.empty()) {
            while (!s1.empty()) {
                s2.push(s1.top());
                s1.pop();
            }
        }
        return s2.top();
    }

    bool empty() {
        return s1.empty() && s2.empty();
    }
};
```

## Approach 2: Single Stack for Queue Operations

### Idea/Intuition

This approach uses a single stack to implement queue operations by maintaining the order of elements. When pushing a new element, we rotate the existing elements to ensure the new element is at the front when popping or peeking.

### Algorithm

1. **Push Operation**:

   - Push the new element `x` onto the stack.
   - Rotate the stack by popping all elements and pushing them back to maintain the order.

2. **Pop Operation**:

   - Simply pop the top element from the stack, which represents the front of the queue.

3. **Peek Operation**:

   - Return the top element of the stack without removing it.

4. **Empty Operation**:
   - The queue is empty if the stack is empty.

### Time Complexity

- **O(n)** for `push` due to the rotation, **O(1)** for `pop` and `peek`.

### Space Complexity

- **O(n)**: Space used by the stack.

### Code

```cpp
class MyQueue {
private:
    stack<int> s;

public:
    MyQueue() {}

    void push(int x) {
        s.push(x);
        for (int i = 0; i < s.size() - 1; ++i) {
            s.push(s.top());
            s.pop();
        }
    }

    int pop() {
        if (s.empty()) return -1; // Queue is empty
        int front = s.top();
        s.pop();
        return front;
    }

    int peek() {
        if (s.empty()) return -1; // Queue is empty
        return s.top();
    }

    bool empty() {
        return s.empty();
    }
};
```

# 163. Implement Stack using Linked List

### Problem Link

[Naukri - Implement Stack using Linked List](https://www.naukri.com/code360/problems/implement-stack-with-linked-list_630475)

## Approach: Using Linked List for Stack Implementation

### Idea/Intuition

Utilize a singly linked list to implement stack operations. The top of the stack corresponds to the head of the linked list.

### Algorithm

1. **Push Operation**:

   - Create a new node with the given data.
   - Set the new node's next pointer to the current top.
   - Update the top pointer to the new node.

2. **Pop Operation**:

   - If the stack is not empty, store the current top node.
   - Update the top pointer to the next node.
   - Delete the old top node to free memory.

3. **Get Top Operation**:

   - Return the data of the top node if the stack is not empty; otherwise, return -1.

4. **Size and Empty Operations**:
   - Return the size of the stack and check if it is empty.

### Time Complexity

- **O(1)** for `push`, `pop`, and `getTop` operations.

### Space Complexity

- **O(n)**: Space used by the linked list nodes.

### Code

```cpp
class Stack
{
private:
    Node* top;
    int size;

public:
    Stack()
    {
        top = NULL;
        size = 0;
    }

    int getSize()
    {
        return size;
    }

    bool isEmpty()
    {
        return top == NULL;
    }

    void push(int data)
    {
        Node* newNode = new Node(data, top);
        top = newNode;
        size++;
    }

    void pop()
    {
        if (top != NULL)
        {
            Node* temp = top;
            top = top->next;
            delete temp;
            size--;
        }
    }

    int getTop()
    {
        if (top != NULL)
            return top->data;
        else
            return -1;
    }
};
```

# 164. Implement Queue using Linked List

### Problem Link

[Naukri - Implement Queue using Linked List](https://www.naukri.com/code360/problems/implement-queue-using-linked-list_8161235)

## Approach: Using Linked List for Queue Implementation

### Idea/Intuition

Utilize a singly linked list to implement queue operations. The front of the queue corresponds to the head of the linked list, and the rear corresponds to the tail.

### Algorithm

1. **Push Operation**:

   - Create a new node with the given data.
   - If the queue is empty (rear is NULL), set both front and rear to the new node.
   - If not empty, link the current rear's next pointer to the new node and update rear to the new node.

2. **Pop Operation**:
   - If the front is NULL, return -1 (queue is empty).
   - Store the data of the front node, update the front pointer to the next node, and delete the old front node.
   - If the front becomes NULL after the operation, set rear to NULL as well.

### Time Complexity

- **O(1)** for `push` and `pop` operations.

### Space Complexity

- **O(n)**: Space used by the linked list nodes.

### Code

```cpp
struct Queue {
    Node* front;
    Node* rear;
    void push(int);
    int pop();

    Queue() {
        front = rear = NULL;
    }
};

void Queue::push(int x) {
    Node* newNode = new Node(x);
    if (rear == NULL) {
        front = rear = newNode;
    } else {
        rear->next = newNode;
        rear = newNode;
    }
}

int Queue::pop() {
    if (front == NULL) {
        return -1;
    }
    int result = front->data;
    Node* temp = front;
    front = front->next;
    if (front == NULL) {
        rear = NULL;
    }
    delete temp;
    return result;
}
```

# 165. Valid Parentheses

### Problem Link

[LeetCode - Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/)

## Approach: Using Stack for Parenthesis Validation

### Idea/Intuition

Use a stack to keep track of opening parentheses. For each closing parenthesis, check if it matches the top of the stack.

### Algorithm

1. Initialize an empty stack to hold opening parentheses.
2. Iterate through each character in the string:
   - If it's an opening parenthesis ('(', '[', or '{'), push it onto the stack.
   - If it's a closing parenthesis (')', ']', or '}'):
     - Check if the stack is empty; if so, return false.
     - Check if the top of the stack matches the corresponding opening parenthesis. If it does, pop the stack; otherwise, return false.
3. At the end of the iteration, return true if the stack is empty (all parentheses matched), otherwise return false.

### Time Complexity

- **O(n)**: Each character is processed once.

### Space Complexity

- **O(n)**: In the worst case, all characters are opening parentheses.

### Code

```cpp
bool isValid(string s) {
    stack<char> st;

    for(int i = 0; i < s.length(); i++) {
        if(s[i] == '(' || s[i] == '[' || s[i] == '{') {
            st.push(s[i]);
        } else {
            if (st.empty()) {
                return false;
            }
            if ((s[i] == ')' && st.top() == '(') ||
                (s[i] == ']' && st.top() == '[') ||
                (s[i] == '}' && st.top() == '{')) {
                st.pop();
            } else {
                return false;
            }
        }
    }
    return st.empty();
}
```
