# 91. Introduction to Doubly Linked List

### Problem Link

[GeeksforGeeks - Introduction to Doubly Linked List](https://www.geeksforgeeks.org/problems/introduction-to-doubly-linked-list/1)

## Approach: Iterative Construction

### Idea

We iterate through the given vector of integers and create nodes one by one. Each node is linked to the previous node to form a doubly linked list.

### Time Complexity

- **O(n)**: We traverse the input array of size `n` once.

### Space Complexity

- **O(n)**: We create `n` nodes in the doubly linked list.

### Code

```cpp
Node* constructDLL(vector<int>& arr) {
    if (arr.empty()) return nullptr;  // Handle edge case for empty array

    Node* head = new Node(arr[0]);  // Create the first node
    Node* prev = head;  // Track the previous node

    for (int i = 1; i < arr.size(); ++i) {
        Node* current = new Node(arr[i]);  // Create a new node
        prev->next = current;  // Link previous node to the current one
        current->prev = prev;  // Link current node back to the previous one
        prev = current;  // Update previous node to the current one
    }

    return head;  // Return the head of the constructed list
}
```

# 92. Insert a Node in a Doubly Linked List

### Problem Link

[GeeksforGeeks - Insert a Node in a Doubly Linked List](https://www.geeksforgeeks.org/problems/insert-a-node-in-doubly-linked-list/1)

## Approach: Iterative Traversal

### Idea

We traverse the list to the given position and insert a new node there by updating the `next` and `prev` pointers of adjacent nodes.

### Time Complexity

- **O(n)**: We may need to traverse the entire list.

### Space Complexity

- **O(1)**: Only a constant amount of extra space is used.

### Code

```cpp
Node* addNode(Node* head, int pos, int data) {
    Node* newNode = new Node(data);  // Create the new node

    if (!head) return newNode;  // Handle the case when the list is empty

    Node* current = head;

    // Traverse to the node at position `pos`
    for (int i = 0; i < pos && current->next != nullptr; ++i) {
        current = current->next;
    }

    // Insert the new node after `current`
    newNode->next = current->next;
    newNode->prev = current;

    if (current->next) {
        current->next->prev = newNode;
    }

    current->next = newNode;

    return head;  // Return the updated head of the list
}
```

# 93. Delete Node in a Doubly Linked List

### Problem Link

[GeeksforGeeks - Delete Node in a Doubly Linked List](https://www.geeksforgeeks.org/problems/delete-node-in-doubly-linked-list/1)

## Approach: Traversing to the Node and Adjusting Pointers

### Idea

We traverse the doubly linked list to the given position `x`. Once we reach the node, we update the adjacent pointers to skip the node, effectively deleting it.

### Time Complexity

- **O(n)**: In the worst case, we may need to traverse the entire list to find the node to be deleted.

### Space Complexity

- **O(1)**: Only a constant amount of extra space is used.

### Code

```cpp
Node* deleteNode(Node* head, int x) {
    // If the list is empty
    if (!head) return nullptr;

    Node* current = head;

    // Traverse to the node at position `x`
    for (int i = 1; i < x && current != nullptr; ++i) {
        current = current->next;
    }

    // If the node to be deleted is the head
    if (current == head) {
        head = head->next;
        if (head) head->prev = nullptr;
    }
    // If the node to be deleted is in the middle or end
    else {
        if (current->next) current->next->prev = current->prev;
        if (current->prev) current->prev->next = current->next;
    }

    // Free the memory of the deleted node
    delete current;

    return head;  // Return the updated head of the list
}
```

# 94. Reverse a Doubly Linked List

### Problem Link

[GeeksforGeeks - Reverse a Doubly Linked List](https://www.geeksforgeeks.org/problems/reverse-a-doubly-linked-list/1)

## Approach: Iterative Reversal of the List

### Idea

We will traverse the doubly linked list and reverse the `next` and `prev` pointers for each node. Finally, we will return the new head of the reversed list.

### Time Complexity

- **O(n)**: We traverse the entire list once.

### Space Complexity

- **O(1)**: Only a constant amount of extra space is used.

### Code

```cpp
DLLNode* reverseDLL(DLLNode* head) {
    // If the list is empty or has only one node
    if (!head || !head->next) return head;

    DLLNode* current = head;
    DLLNode* temp = nullptr;

    // Traverse the list and swap the next and prev pointers
    while (current) {
        // Swap the next and prev pointers
        temp = current->prev;
        current->prev = current->next;
        current->next = temp;

        // Move to the next node (which is the previous node before swap)
        current = current->prev;  // Move to the original next node
    }

    // The head will be the last non-null node processed
    if (temp) head = temp->prev;

    return head;  // Return the new head of the reversed list
}
```

# 95. Middle of the Linked List

### Problem Link

[LeetCode - Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/description/)

## Approach 1: Counting Nodes

### Algorithm:

1. **Count the Nodes:**  
   Initialize a pointer `temp` to the head of the linked list and a variable `count` to hold the number of nodes. Traverse the linked list using `temp`, incrementing `count` by one at each node until `temp` becomes null. The final value of `count` will represent the length of the linked list.

2. **Calculate Mid:**  
   Compute `mid` as `count / 2` where `count` is the length of the linked list. `mid` represents the position of the middle node.

3. **Find the Middle Node:**  
   Reset the `temp` pointer back to the head and traverse the list, moving `temp` `mid` number of times. The node pointed to by `temp` after this traversal is the middle node of the linked list.

### Time Complexity

- **O(n)**: We traverse the list to count the nodes and then again to find the middle.

### Space Complexity

- **O(1)**: Only a constant amount of extra space is used.

### Code

```cpp
ListNode* middleNode(ListNode* head) {
    // Step 1: Count the number of nodes
    ListNode* temp = head;
    int count = 0;
    while (temp) {
        count++;
        temp = temp->next;
    }

    // Step 2: Calculate the middle index
    int mid = count / 2;

    // Step 3: Reset temp and find the middle node
    temp = head;
    for (int i = 0; i < mid; i++) {
        temp = temp->next;
    }

    return temp;  // Middle node
}
```
