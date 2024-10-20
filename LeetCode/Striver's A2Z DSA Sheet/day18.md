# 86. Introduction to Linked List

### Problem Link

[GeeksforGeeks - Introduction to Linked List](https://www.geeksforgeeks.org/problems/introduction-to-linked-list/0)

## Approach: Constructing a Linked List from a Vector

### Idea

1. We need to **create a linked list** where each element in the vector corresponds to a node in the list.
2. **Iterate through the vector** to construct the linked list one node at a time.
3. **Maintain head and tail pointers** to connect the nodes correctly as we traverse the array.

### Time Complexity

- **O(n)**: We traverse the array exactly once to create the linked list.

### Space Complexity

- **O(n)**: For creating the nodes of the linked list.

### Code

```cpp
Node* constructLL(vector<int>& arr) {
    if (arr.empty()) return nullptr;  // Handle edge case where the input vector is empty.

    // Create the head node with the first element.
    Node* head = new Node(arr[0]);
    Node* tail = head;

    // Iterate through the rest of the array to build the linked list.
    for (int i = 1; i < arr.size(); i++) {
        Node* newNode = new Node(arr[i]);  // Create a new node for each element.
        tail->next = newNode;  // Link the new node to the previous one.
        tail = newNode;  // Move the tail pointer to the new node.
    }

    return head;  // Return the head of the linked list.
}
```

# 87. Linked List Insertion

### Problem Link

[GeeksforGeeks - Linked List Insertion](https://www.geeksforgeeks.org/problems/linked-list-insertion-1587115620/0)

## Approach: Insert at the End of the Linked List

### Idea

1. If the **head is `nullptr`**, create a new node and make it the head.
2. Otherwise, **traverse to the last node** in the list.
3. **Insert the new node** at the end by updating the `next` pointer of the last node.

### Time Complexity

- **O(n)**: In the worst case, we traverse the entire linked list to reach the end.

### Space Complexity

- **O(1)**: No extra space required other than the new node.

### Code

```cpp
Node* insertAtEnd(Node* head, int x) {
    Node* newNode = new Node(x);  // Create a new node with value x.

    if (head == nullptr) {
        return newNode;  // If the list is empty, the new node becomes the head.
    }

    Node* temp = head;
    while (temp->next != nullptr) {
        temp = temp->next;  // Traverse to the last node.
    }

    temp->next = newNode;  // Link the last node to the new node.
    return head;  // Return the head of the modified list.
}
```

# 88. Delete Node in a Linked List

### Problem Link

[LeetCode - Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/description/)

## Approach: Copy Value and Bypass Node

### Idea

To delete a node from a linked list without access to the head, we can copy the value of the next node to the current node and then bypass the next node.

### Time Complexity

- **O(1)**: The operation takes constant time since we are only modifying a few pointers and values.

### Space Complexity

- **O(1)**: No extra space is used.

### Code

```cpp
void deleteNode(ListNode* node) {
    node->val = node->next->val;      // Copy the value from the next node.
    node->next = node->next->next;    // Bypass the next node.
}
```

# 89. Count Nodes of Linked List

### Problem Link

[GeeksforGeeks - Count Nodes of Linked List](https://www.geeksforgeeks.org/problems/count-nodes-of-linked-list/0)

## Approach: Iterative Traversal

### Idea

We traverse the linked list from the head to the end, counting the number of nodes.

### Time Complexity

- **O(n)**: We traverse all `n` nodes of the linked list.

### Space Complexity

- **O(1)**: No extra space is used.

### Code

```cpp
int getCount(struct Node* head) {
    int count = 0;  // Initialize count to 0
    struct Node* current = head;

    while (current != nullptr) {
        count++;            // Increment count for each node
        current = current->next;  // Move to the next node
    }

    return count;  // Return the total node count
}
```

# 90. Search in Linked List

### Problem Link

[GeeksforGeeks - Search in Linked List](https://www.geeksforgeeks.org/problems/search-in-linked-list-1664434326/1)

## Approach: Iterative Traversal

### Idea

We traverse the linked list from the head to the end, checking if any node contains the target value (`key`).

### Time Complexity

- **O(n)**: We traverse all `n` nodes in the worst case.

### Space Complexity

- **O(1)**: No extra space is used.

### Code

```cpp
bool searchKey(int n, struct Node* head, int key) {
    struct Node* current = head;

    while (current != nullptr) {
        if (current->data == key) {
            return true;  // Key found
        }
        current = current->next;  // Move to the next node
    }

    return false;  // Key not found
}
```
