# 107. Add 1 to a Number Represented as Linked List

### Problem Link

[GeeksforGeeks - Add 1 to a Number Represented as Linked List](https://www.geeksforgeeks.org/problems/add-1-to-a-number-represented-as-linked-list/1)

## Approach

1. **Reverse the List:**  
   Reverse the linked list to make addition easier, starting from the least significant digit.

2. **Add 1:**  
   Use the `addTwoNumbers` helper function to add the reversed list with a new list that represents the number `1`. This is done by initializing a new node with value `1` and using standard linked list addition logic.

3. **Reverse Again:**  
   Reverse the list back to restore the original order, and return the head of this final list.

### Time Complexity

- **O(n):** Each reversal and traversal in the addition step takes linear time.

### Space Complexity

-**O(n):** Additional nodes are created in the result list proportional to the length of the input list.

### Code

```cpp
Node* reverseIterative(Node* head) {
    if (head == NULL || head->next == NULL) {
        return head;
    }
    Node* curr = head;
    Node* prev = NULL;
    Node* frwd = NULL;
    while (curr != NULL) {
        frwd = curr->next;
        curr->next = prev;
        prev = curr;
        curr = frwd;
    }
    return prev;
}

Node* addTwoNumbers(Node* l1, Node* l2) {
    Node* head = new Node(-1);
    Node* curr = head;
    int carry = 0;
    while (l1 != nullptr || l2 != nullptr) {
        int sum = carry;
        if (l1 != NULL) {
            sum += l1->data;
            l1 = l1->next;
        }
        if (l2 != NULL) {
            sum += l2->data;
            l2 = l2->next;
        }
        carry = sum / 10;
        curr->next = new Node(sum % 10);
        curr = curr->next;
    }
    if (carry) curr->next = new Node(1);
    return head->next;
}

Node* addOne(Node *head) {
    Node* head1 = reverseIterative(head);
    Node* head2 = new Node(1);
    Node* ans = addTwoNumbers(head1, head2);
    return reverseIterative(ans);
}
```

## Approach 2: Reverse and Add with Carry Propagation

### Algorithm:

1. **Reverse the List:**  
   Start by reversing the linked list to handle addition from the least significant digit.

2. **Add 1 and Handle Carry:**  
   Traverse the list with a carry initialized to 1. For each node:

   - Add the carry to the node’s data.
   - Update carry as `carry = node->data / 10`.
   - Update the node's data to `node->data % 10`.
   - Continue this until the carry becomes 0 or the list ends.

3. **Reverse Again:**  
   Reverse the list back to its original order.

4. **Return Result:**  
   Return the modified list as the result.

### Time Complexity

- **O(n):** Each traversal and reversal takes linear time.

### Space Complexity

- **O(1):** Only constant extra space is used.

### Code

```cpp
Node* reverseList(Node* head) {
    Node* prev = NULL;
    Node* curr = head;
    while (curr != NULL) {
        Node* next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}

Node* addOne(Node* head) {
    // Step 1: Reverse the list
    head = reverseList(head);

    // Step 2: Add 1 and handle carry
    Node* curr = head;
    int carry = 1;
    while (curr != NULL && carry > 0) {
        int sum = curr->data + carry;
        curr->data = sum % 10;
        carry = sum / 10;
        if (curr->next == NULL && carry > 0) {
            curr->next = new Node(carry);
            carry = 0;
        }
        curr = curr->next;
    }

    // Step 3: Reverse the list back to original order
    head = reverseList(head);

    return head;
}
```

## Approach 3: Recursive Addition with Carry Propagation

### Algorithm:

1. **Recursive Addition (Helper Function):**  
   Define a helper function `addHelper(Node* temp)` that:
   - Recursively traverses to the end of the linked list.
   - Adds 1 to the last node and manages carry as the recursion unwinds.
   - Updates the node’s data based on carry, setting it to 0 if `data >= 10` and propagating carry if necessary.
2. **Carry Check After Recursion:**  
   After the recursion completes, check if there is a remaining carry. If so:

   - Create a new node with the carry value and link it to the head of the list.

3. **Return Result:**  
   Return the modified list with the final carry adjustment if needed.

### Time Complexity

- **O(n):** Each node is visited once during the recursive addition.

### Space Complexity

- **O(n):** Recursive stack space proportional to the list length.

### Code

```cpp
int addHelper(Node* temp) {
    if (temp == NULL) {
        return 1;  // Initial carry to add 1
    }

    int carry = addHelper(temp->next);  // Recurse to the end of the list
    temp->data += carry;  // Add carry to the current node's data

    if (temp->data < 10) return 0;  // No further carry needed
    temp->data = 0;  // Reset to 0 if data >= 10 and carry remains

    return 1;  // Carry 1 to the previous node
}

Node* addOne(Node* head) {
    int carry = addHelper(head);

    // If carry is still 1 after processing, add a new node at the head
    if (carry == 1) {
        Node* newNode = new Node(1);
        newNode->next = head;
        head = newNode;
    }

    return head;
}
```

# 108. Add Two Numbers

### Problem Link

[LeetCode - Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

## Approach: Iterative Addition with Carry

### Algorithm:

1. **Initialize a Dummy Node**:  
   Create a dummy node `head` to simplify edge cases and set `curr` to point to this dummy node. Initialize a `carry` variable as 0.

2. **Traverse Both Linked Lists**:

   - Loop through `l1` and `l2` until both are exhausted.
   - For each node, compute `sum` as the sum of values from `l1`, `l2`, and `carry`.
   - Set `carry = sum / 10` to get the new carry.
   - Assign `curr->next` to a new node with `sum % 10` as its value.
   - Move `curr` to `curr->next`.

3. **Handle Remaining Carry**:  
   If there’s a remaining carry after both lists are traversed, add a new node with the carry value.

4. **Return the Result**:  
   Return `head->next` to skip the dummy node and return the actual result.

### Time Complexity

- **O(max(m, n))**: Where `m` and `n` are the lengths of the two linked lists.

### Space Complexity

- **O(max(m, n))**: Space used by the new linked list.

### Code

```cpp
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
    ListNode* head = new ListNode(-1);  // Dummy head node
    ListNode* curr = head;
    int carry = 0;

    // Traverse both lists
    while (l1 != nullptr || l2 != nullptr) {
        int sum = carry;

        if (l1 != nullptr) {
            sum += l1->val;
            l1 = l1->next;
        }
        if (l2 != nullptr) {
            sum += l2->val;
            l2 = l2->next;
        }

        carry = sum / 10;
        curr->next = new ListNode(sum % 10);
        curr = curr->next;
    }

    // Handle final carry if present
    if (carry) {
        curr->next = new ListNode(1);
    }

    return head->next;  // Return the actual head of the result list
}
```

# 109. Delete All Occurrences of a Given Key in a Doubly Linked List

### Problem Link

[Naukri - Delete All Occurrences of a Given Key in a Doubly Linked List](https://www.naukri.com/code360/problems/delete-all-occurrences-of-a-given-key-in-a-doubly-linked-list_8160461)

## Approach: Traverse and Delete

### Algorithm:

1. **Traverse the List**:  
   Initialize a pointer `curr` to traverse the linked list starting from `head`.

2. **Check for Key**:

   - While `curr` is not `NULL`, check if `curr->data` is equal to the key `k`.
   - If it matches, delete the current node:
     - Adjust the pointers of the previous and next nodes to bypass the current node.
     - Delete the current node.
   - If it does not match, simply move to the next node.

3. **Return the Updated List**:  
   Return `head` after potentially updating the head if the first node is deleted.

### Time Complexity

- **O(n)**: Where `n` is the number of nodes in the doubly linked list.

### Space Complexity

- **O(1)**: No additional space is used.

### Code

```cpp
Node * deleteAllOccurrences(Node* head, int k) {
    Node* curr = head;

    while (curr != NULL) {
        if (curr->data == k) {

            Node* toDelete = curr;

            // Adjust previous node's next if it exists
            if (curr->prev != NULL) {
                curr->prev->next = curr->next;
            } else {
                // If there's no previous node, we are at the head
                head = curr->next;  // Update head
            }

            // Adjust next node's previous if it exists
            if (curr->next != NULL) {
                curr->next->prev = curr->prev;
            }

            curr = curr->next;
            delete toDelete;
        } else {
            curr = curr->next;
        }
    }

    return head;
```

# 110. Find Pairs with Given Sum in Doubly Linked List

### Problem Link

[GeeksforGeeks - Find Pairs with Given Sum in Doubly Linked List](https://www.geeksforgeeks.org/problems/find-pairs-with-given-sum-in-doubly-linked-list/1)

## Approach: Two Pointer Technique

### Algorithm:

1. **Initialize Pointers**:  
   Set `left` pointer to the head of the list and `right` pointer to the tail of the list (traverse to the end of the list using a while loop).

2. **Check for Pairs**:  
   While `left` is not equal to `right` and `left->prev` is not equal to `right`, perform the following:

   - If the sum of `left->data` and `right->data` is greater than the target, move the `right` pointer one step to the left (i.e., `right = right->prev`).
   - If the sum is less than the target, move the `left` pointer one step to the right (i.e., `left = left->next`).
   - If the sum equals the target, store the pair in the result vector and move both pointers:
     - Add the pair to the answer vector.
     - Move `left` one step to the right and `right` one step to the left.

3. **Return Result**:  
   Return the vector containing all pairs that sum to the target.

### Time Complexity

- **O(n)**: Where `n` is the number of nodes in the doubly linked list.

### Space Complexity

- **O(k)**: Where `k` is the number of pairs found.

### Code

```cpp
vector<pair<int, int>> findPairsWithGivenSum(Node *head, int target) {
    Node* left = head;
    Node* right = head;

    // Move right pointer to the end of the list
    while (right->next != NULL) {
        right = right->next;
    }

    vector<pair<int, int>> ans;
    while (left != right && left->prev != right) {
        if (left->data + right->data > target) {
            right = right->prev; // Move right pointer to the left
        } else if (left->data + right->data < target) {
            left = left->next; // Move left pointer to the right
        } else {
            ans.push_back(make_pair(left->data, right->data)); // Found a pair
            left = left->next; // Move both pointers
            right = right->prev;
        }
    }
    return ans; // Return the result vector
}
```
