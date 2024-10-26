# 111. Remove Duplicates from a Sorted Doubly Linked List

### Problem Link

[Naukri - Remove Duplicates from a Sorted Doubly Linked List](https://www.naukri.com/code360/problems/remove-duplicates-from-a-sorted-doubly-linked-list_2420283)

## Approach: Iterative with Pointer Adjustments

### Algorithm:

1. **Initialize Pointer**:

   - Start from the `head` and set `temp` as the current node.

2. **Traverse and Remove Duplicates**:

   - For each node, check if the next node has the same data as the current node.
   - If a duplicate is found, store the `next` node in a temporary pointer `duplicate`.
   - Adjust the `next` pointer of the current node (`temp`) to skip over the duplicate node.
   - Free the memory of the duplicate node.

3. **Move to the Next Unique Node**:

   - Move `temp` to the next node in the list.

4. **Return Result**:
   - Return the modified list's `head`.

### Time Complexity

- **O(n)**: Where `n` is the number of nodes in the list.

### Space Complexity

- **O(1)**: In-place modification of the list.

### Code

```cpp
Node* removeDuplicates(Node *head) {
    Node* temp = head;

    while (temp != NULL && temp->next != NULL) {
        Node* nextNode;
        while ((nextNode = temp->next) && nextNode->data == temp->data) {
            Node* duplicate = nextNode;
            temp->next = nextNode->next;
            if (nextNode->next != NULL) {
                nextNode->next->prev = temp;
            }
            free(duplicate);
        }
        temp = temp->next;
    }

    return head;
}
```

# 112. Reverse Nodes in k-Group

### Problem Link

[LeetCode - Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

## Approach 1: Recursive Group Reversal

### Algorithm:

1. **Recursive Function (`solve`)**:

   - Reverse the first `k` nodes.
   - If there are at least `k` nodes remaining, recursively call `solve` on the next part of the list.
   - Connect the reversed nodes to the result of the recursive call.

2. **Helper Function (`reverseKGroup`)**:
   - Count the total nodes in the list.
   - If there are at least `k` nodes, call `solve` to reverse in groups of `k`.
   - If not, return the head as is.

### Time Complexity

- **O(n)**

### Space Complexity

- **O(n/k)**: Recursive call stack depth.

### Code

```cpp
ListNode* solve(ListNode* head, int k, int cnt) {
    if (head == NULL) {
        return head;
    }

    ListNode* nxt = NULL;
    ListNode* curr = head;
    ListNode* prev = NULL;
    int count = 0;

    while (curr != NULL && count < k) {
        nxt = curr->next;
        curr->next = prev;
        prev = curr;
        curr = nxt;
        count++;
    }

    if (nxt != NULL && cnt - count >= k) {
        head->next = solve(nxt, k, cnt - count);
    } else {
        head->next = nxt;
    }

    return prev;
}

ListNode* reverseKGroup(ListNode* head, int k) {
    int cnt = 0;
    ListNode* temp = head;

    while (temp != NULL) {
        cnt++;
        temp = temp->next;
    }

    if (cnt >= k) {
        return solve(head, k, cnt);
    } else {
        return head;
    }
}
```

## Approach 2: Iterative Group Reversal with Segment Breaking

### Algorithm:

1. **Segment Division**:

   - Traverse to the `k`th node, preserve the next pointer as `nextNode`, and set the `k`th node’s next pointer to `null` to separate it from the rest of the list.

2. **Reverse Each Segment**:

   - Reverse the separated segment using a helper function, then attach it back to the rest of the list.

3. **Continue with Remaining Groups**:
   - Repeat this for all segments of `k` nodes, connecting the end of the previous segment to the beginning of the current reversed segment.
   - If a segment has fewer than `k` nodes, leave it unmodified.

### Time Complexity

- **O(n)**

### Space Complexity

- **O(1)**

### Code

```cpp
ListNode* reverseLinkedList(ListNode* head) {
    ListNode* prev = NULL;
    ListNode* curr = head;
    while (curr != NULL) {
        ListNode* nextNode = curr->next;
        curr->next = prev;
        prev = curr;
        curr = nextNode;
    }
    return prev;
}

ListNode* reverseKGroup(ListNode* head, int k) {
    ListNode* temp = head;
    ListNode* newHead = NULL;
    ListNode* prevLast = NULL;

    while (temp != NULL) {
        ListNode* kthNode = temp;
        for (int i = 1; i < k && kthNode != NULL; i++) {
            kthNode = kthNode->next;
        }

        if (kthNode == NULL) {
            if (prevLast != NULL) prevLast->next = temp;
            break;
        }

        ListNode* nextNode = kthNode->next;
        kthNode->next = NULL;

        ListNode* reversedHead = reverseLinkedList(temp);

        if (newHead == NULL) newHead = reversedHead;

        if (prevLast != NULL) prevLast->next = reversedHead;

        prevLast = temp;
        temp = nextNode;
    }

    return newHead != NULL ? newHead : head;
}
```

# 113. Rotate List

### Problem Link

[LeetCode - Rotate List](https://leetcode.com/problems/rotate-list/)

## Approach 1: Iterative Move of Last Element to Front

### Algorithm:

- For each rotation, find the last element, detach it, and attach it to the front of the list.
- Repeat this for `k` rotations.

### Time Complexity

- **O(k \* n)**

### Space Complexity

- **O(1)**

## Approach 2: Reverse Segments with k Modulo Length

### Algorithm:

1. **Compute Length**:

   - Count the total number of nodes. If `k >= count`, set `k = k % count`.

2. **Reverse Entire List**:

   - Reverse the whole list.

3. **Reverse Segments**:
   - Traverse to the `k`th node and split the list into two segments.
   - Reverse both segments and reconnect them.

### Time Complexity

- **O(n)**

### Space Complexity

- **O(1)**

### Code

```cpp
ListNode* reverseIterative(ListNode* head){
    if(head == NULL || head->next == NULL) return head;
    ListNode* curr = head;
    ListNode* prev = NULL;
    ListNode* frwd = NULL;
    while(curr != NULL) {
        frwd = curr->next;
        curr->next = prev;
        prev = curr;
        curr = frwd;
    }
    return prev;
}

ListNode* rotateRight(ListNode* head, int k) {
    if(head == NULL) return NULL;
    int count = 0;
    ListNode* temp = head;
    while(temp != NULL) {
        count++;
        temp = temp->next;
    }
    if(k >= count) k = k % count;
    if(k == 0) return head;

    head = reverseIterative(head);
    temp = head;
    count = 1;
    while(count < k) {
        temp = temp->next;
        count++;
    }
    ListNode* part = temp->next;
    ListNode* left = head;
    ListNode* right = part;
    temp->next = NULL;

    left = reverseIterative(left);
    right = reverseIterative(right);
    temp = left;
    while(temp->next != NULL) temp = temp->next;
    temp->next = right;
    return left;
}
```

## Approach 3: Convert to Circular Linked List

### Algorithm:

1. **Calculate Length**:

   - Traverse the list to calculate its length. If `k >= length`, set `k = k % length`.

2. **Create Circular List**:

   - Connect the last node to the head, forming a circular linked list.

3. **Break Circular Link**:
   - Traverse to the `(length - k)`th node, then break the circular link by setting the next node as the new head and the current node's `next` to `NULL`.

### Time Complexity

- **O(n)**

### Space Complexity

- **O(1)**

### Code

```cpp
ListNode* rotateRight(ListNode* head, int k) {
    if(head == NULL || head->next == NULL || k == 0) return head;

    // Step 1: Calculate the length of the list
    int length = 1;
    ListNode* tail = head;
    while(tail->next != NULL) {
        tail = tail->next;
        length++;
    }

    // Step 2: Form a circular list
    tail->next = head;

    // Step 3: Find the new head and break the circle
    k = k % length;
    int stepsToNewHead = length - k;
    ListNode* newTail = tail;
    while(stepsToNewHead--) {
        newTail = newTail->next;
    }

    ListNode* newHead = newTail->next;
    newTail->next = NULL;

    return newHead;
}
```

# 114. Flatten a Linked List

### Problem Link

[Naukri - Flatten a Linked List](https://www.naukri.com/code360/problems/flatten-a-linked-list_1112655?)

## Approach 1: Using Array Storage and Sorting

### Algorithm:

1. **Initialize an Array**:

   - Create an empty array to store data from the linked list.

2. **Traverse the List**:

   - Traverse through each `next` node and within each `next` node, traverse its `child` nodes.
   - For each node accessed, add its data to the array.

3. **Sort the Array**:

   - Sort the array in ascending order.

4. **Rebuild the List**:
   - Using the sorted array, create a new flattened linked list.

### Time Complexity

- **O(n \* m)** (where `n` is the number of nodes with `next` pointers and `m` is the length of each child list traversal) + **O(k log k)** for sorting, where `k` is the number of nodes.

### Space Complexity

- **O(k)** (for storing node values in the array)

### Code

```cpp
Node* flattenLinkedList(Node* head) {
    if (!head) return head;

    // Step 1: Store all node values in an array
    std::vector<int> values;
    Node* temp = head;
    while (temp) {
        Node* child = temp;
        while (child) {
            values.push_back(child->data);
            child = child->child;
        }
        temp = temp->next;
    }

    // Step 2: Sort the values array
    std::sort(values.begin(), values.end());

    // Step 3: Create a new flattened linked list from sorted values
    Node* newHead = new Node(values[0]);
    Node* curr = newHead;
    for (int i = 1; i < values.size(); i++) {
        curr->child = new Node(values[i]);
        curr = curr->child;
    }

    return newHead;
}
```

## Approach 2: Traversing Child Nodes and Connecting to Main List

### Algorithm:

1. **Traverse the Main List**:

   - Traverse each `next` node in the main list.

2. **Connect Child Lists**:

   - For each node, if there’s a `child` list, traverse the child nodes and connect the end of the `child` list to the next main list node. This creates a single linked list.

3. **Sort the Flattened List**:
   - After merging all nodes into a single list, use a sorting approach to reorder nodes in ascending order.

### Time Complexity

- **O(n \* m)** for traversing the main and child lists, where `n` is the number of main list nodes and `m` is the child list length per node, plus **O(k log k)** for sorting, where `k` is the total number of nodes.

### Space Complexity

- **O(1)** (no extra space required aside from the input structure)

### Code

```cpp
Node* flattenLinkedList(Node* head) {
    if (!head) return head;

    // Traverse each node in the main list
    Node* curr = head;
    while (curr) {
        Node* child = curr->child;

        // Traverse to the end of the child list if it exists
        if (child) {
            while (child->child) {
                child = child->child;
            }
            // Connect the last node in the child list to the next main list node
            child->child = curr->next;
            curr->next = curr->child;
            curr->child = nullptr;
        }
        curr = curr->next;
    }

    // Sort the entire list after merging
    Node* sortedHead = head;
    Node* i = sortedHead;
    while (i) {
        Node* j = i->next;
        while (j) {
            if (i->data > j->data) {
                std::swap(i->data, j->data);
            }
            j = j->next;
        }
        i = i->next;
    }

    return sortedHead;
}
```

## Approach 3: Recursive Merge-Based Flattening

### Algorithm:

1. **Base Case**:

   - If `head` is `NULL`, return `head`.
   - If `head->next` is `NULL`, return `head`.

2. **Recursive Flattening**:

   - Recursively call `flattenLinkedList` on `head->next`, which returns the flattened list of the remaining nodes.

3. **Merge Operations**:

   - Use a merge function to merge `head` and `flattenLinkedList(head->next)` based on their `data` values.

4. **Return**:
   - Return the merged head of the flattened linked list.

### Time Complexity

- **O(n \* m)** (where `n` is the number of nodes with a `next` pointer and `m` is the length of each child list)

### Space Complexity

- **O(n)** (for recursive call stack)

### Code

```cpp
Node* merge(Node* a, Node* b) {
    if (!a) return b;
    if (!b) return a;

    Node* result;
    if (a->data < b->data) {
        result = a;
        result->child = merge(a->child, b);
    } else {
        result = b;
        result->child = merge(a, b->child);
    }
    return result;
}

Node* flattenLinkedList(Node* head) {
    if (!head || !head->next) {
        return head;
    }

    // Recursively flatten the rest of the list
    head->next = flattenLinkedList(head->next);

    // Merge current node with the flattened `next` node list
    head = merge(head, head->next);

    return head;
}
```

# 115. Copy List with Random Pointer

### Problem Link

[LeetCode - Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

## Approach 1: Using a Map

### Algorithm:

1. **Create a Map:**  
   Create a map to hold the mapping between the original nodes and the cloned nodes.

2. **Clone the List:**  
   Traverse the original list and populate the map with the original nodes as keys and their corresponding cloned nodes as values.

3. **Update Random Pointers:**  
   Use the map to update the random pointers of the cloned list based on the original list's random pointers.

### Time Complexity:

- **O(n):** We traverse the linked list twice.

### Space Complexity:

- **O(n):** We use a map to store the node mappings.

### Code:

```cpp
Node* copyRandomList(Node* head) {
    if (head == NULL) return NULL;

    Node* current = head;
    Node* copyHead = new Node(current->val);
    Node* copyCurrent = copyHead;
    unordered_map<Node*, Node*> mapping;

    mapping[current] = copyCurrent;

    // Create the copy linked list
    while (current->next) {
        current = current->next;
        copyCurrent->next = new Node(current->val);
        copyCurrent = copyCurrent->next;
        mapping[current] = copyCurrent;
    }

    // Update random pointers
    current = head;
    copyCurrent = copyHead;

    while (current) {
        copyCurrent->random = mapping[current->random];
        current = current->next;
        copyCurrent = copyCurrent->next;
    }

    return copyHead;
}

```

## Approach 2: Optimized Copy with Node Interleaving

### Algorithm:

1. **Create Copy Nodes:**  
   Traverse the original list and create new cloned nodes right after each original node. For example, if the original node is `A`, create a new node `A'` right after `A`.

2. **Update Random Pointers:**  
   After creating the interleaved list, traverse the list again to update the random pointers of the cloned nodes. The random pointer of the cloned node can be set by accessing the random pointer of the original node and moving to the next node.

3. **Separate the Cloned List:**  
   Finally, separate the cloned list from the original list by restoring the original next pointers and extracting the cloned nodes.

### Time Complexity:

- **O(n):** We traverse the linked list three times.

### Space Complexity:

- **O(1):** We do not use any additional space apart from the cloned nodes.

### Code:

```cpp
Node* copyRandomList(Node* head) {
    // Create the copy linked list
    if (head == NULL) {
        return NULL;
    }

    Node* temp1 = head->next;
    Node* cpyHead = new Node(head->val);
    Node* temp2 = cpyHead;

    while (temp1 != NULL) {
        temp2->next = new Node(temp1->val);
        temp2 = temp2->next;
        temp1 = temp1->next;
    }

    Node* curr1 = head;
    Node* nxt1 = NULL;
    Node* curr2 = cpyHead;
    Node* nxt2 = NULL;

    // Make connections between main and copy linked lists
    while (curr1 != NULL && curr2 != NULL) {
        nxt1 = curr1->next;
        curr1->next = curr2;
        curr1 = nxt1;

        nxt2 = curr2->next;
        curr2->next = curr1;
        curr2 = nxt2;
    }

    // Copy random pointers
    curr1 = head;
    while (curr1 != NULL) {
        if (curr1->next != NULL)
            curr1->next->random = curr1->random ? curr1->random->next : curr1->random;
        curr1 = curr1->next->next;
    }

    // Revert connections
    temp1 = head;
    temp2 = cpyHead;
    while (temp1 != NULL && temp2 != NULL) {
        temp1->next = temp2->next;
        temp1 = temp1->next;
        if (temp1 != NULL) {
            temp2->next = temp1->next;
        }
        temp2 = temp2->next;
    }

    return cpyHead;
}
```
