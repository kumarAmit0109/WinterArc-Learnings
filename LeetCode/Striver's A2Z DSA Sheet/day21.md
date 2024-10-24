# 101. Palindrome Linked List

### Problem Link

[LeetCode - Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

## Approach 1: Using a Vector to Store the Values

### Algorithm:

1. Traverse the linked list and store the values of the nodes in a vector.
2. Use two pointers to check if the vector is a palindrome by comparing the first and last elements.
3. If a mismatch is found, return `false`, otherwise return `true`.

### Time Complexity:

- **O(n)**: We traverse the linked list once and compare the values in O(n).

### Space Complexity:

- **O(n)**: We use extra space for storing the node values in a vector.

### Code:

```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        vector<int> arr;
        ListNode* temp = head;
        while (temp != NULL) {
            arr.push_back(temp->val);
            temp = temp->next;
        }

        int strt = 0, end = arr.size() - 1;
        while (strt <= end) {
            if (arr[strt] != arr[end]) {
                return false;
            }
            strt++;
            end--;
        }
        return true;
    }
};
```

## Approach 2: Using a Stack to Compare Values

### Algorithm:

1. Traverse the linked list and push all the node values onto a stack.
2. Traverse the list again, comparing each node's value with the value at the top of the stack.
3. If a mismatch is found, return `false`, otherwise return `true`.

### Time Complexity:

- **O(n)**: We traverse the linked list twice.

### Space Complexity:

- **O(n)**: We use extra space for the stack.

### Code:

```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        stack<int> st;
        ListNode* temp = head;

        while (temp != NULL) {
            st.push(temp->val);
            temp = temp->next;
        }

        temp = head;
        while (!st.empty()) {
            if (st.top() != temp->val) {
                return false;
            }
            st.pop();
            temp = temp->next;
        }

        return true;
    }
};
```

## Approach 3: Reverse the Second Half and Compare

### Algorithm:

1. Use the slow and fast pointer technique to find the middle of the linked list.
2. Reverse the second half of the list.
3. Compare the first half with the reversed second half.
4. If all values match, return `true`; otherwise, return `false`.
5. Restore the reversed second half to its original form.

### Time Complexity:

- **O(n)**: We traverse the list, reverse part of it, and compare.

### Space Complexity:

- **O(1)**: We only use pointers without additional space.

### Code:

```cpp
class Solution {
public:
    ListNode* reverse(ListNode* head) {
        ListNode* prev = NULL;
        ListNode* curr = head;
        while (curr != NULL) {
            ListNode* next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }

    bool isPalindrome(ListNode* head) {
        if (!head || !head->next) return true;

        // Step 1: Find the middle
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast->next && fast->next->next) {
            slow = slow->next;
            fast = fast->next->next;
        }

        // Step 2: Reverse the second half
        ListNode* secondHalf = reverse(slow->next);

        // Step 3: Compare first and second halves
        ListNode* firstHalf = head;
        ListNode* secondHalfCopy = secondHalf;
        bool palindrome = true;
        while (secondHalf != NULL) {
            if (firstHalf->val != secondHalf->val) {
                palindrome = false;
                break;
            }
            firstHalf = firstHalf->next;
            secondHalf = secondHalf->next;
        }

        // Step 4: Restore the second half
        slow->next = reverse(secondHalfCopy);

        return palindrome;
    }
};
```

# 102. Odd Even Linked List

### Problem Link

[LeetCode - Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/description/)

## Approach 1: Using Two Pointers (Odd and Even Lists)

### Algorithm:

1. **Initial Setup**:  
   Create a dummy node to store the even-positioned nodes in a separate list.  
   Use a pointer (`odd`) to track odd-positioned nodes in the original list and another pointer (`even_head`) to maintain the head of the even list.

2. **Traverse the Linked List**:  
   Traverse through the linked list, maintaining a counter starting from 1.  
   If the counter is odd, leave the node in its current position and move to the next node.  
   If the counter is even, remove the node from the original list and attach it to the even list.

3. **Merge Lists**:  
   After completing the traversal, append the even-positioned nodes (i.e., `even_head`) to the end of the odd-positioned nodes.

4. **Edge Cases**:  
   If the list is empty or contains only one node, return the list as-is.

### Time Complexity:

- **O(n)**: We traverse the list once, and each operation (moving a node) takes constant time.

### Space Complexity:

- **O(1)**: We only use a constant amount of extra space.

### Code:

```cpp
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if (!head || !head->next) return head; // Return if less than 2 nodes

        // Pointers for odd, even, and even head
        ListNode* odd = head;
        ListNode* even = head->next;
        ListNode* even_head = even;

        // Traverse the list
        while (even && even->next) {
            odd->next = even->next;
            odd = odd->next;
            even->next = odd->next;
            even = even->next;
        }

        // Attach even list to the end of odd list
        odd->next = even_head;
        return head;
    }
};
```

# 103. Remove Nth Node From End of List

### Problem Link

[LeetCode - Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

## Approach 1: Length Calculation Approach

### Algorithm:

1. **Find Length of the Linked List:**

   - Traverse the entire linked list to find the total length.

2. **Calculate the Position to Remove:**

   - The node to be removed is at the position `length - n + 1` from the start of the list.

3. **Remove the Node:**
   - Traverse the list again to that position and remove the node by adjusting pointers.

### Time Complexity:

- **O(n):** Traversal of the linked list twice.

### Space Complexity:

- **O(1):** Constant space is used.

### Code:

```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    int length = 0;
    ListNode* temp = head;

    // First pass to calculate length
    while (temp != NULL) {
        length++;
        temp = temp->next;
    }

    // Special case: removing the head
    if (length == n) {
        return head->next;
    }

    // Second pass to find the node to remove
    temp = head;
    for (int i = 1; i < length - n; i++) {
        temp = temp->next;
    }

    // Remove the nth node
    temp->next = temp->next->next;

    return head;
}
```

## Approach 2: Two Pointers Approach

### Algorithm:

1. **Initialize Two Pointers:**

   - Use two pointers, `first` and `second`, both initially pointing to the head of the linked list.

2. **Move First Pointer Ahead by N Nodes:**

   - Move the `first` pointer ahead by `n` nodes.

3. **Move Both Pointers Until the First Reaches the End:**

   - Then, move both `first` and `second` pointers one step at a time until the `first` pointer reaches the end of the list.

4. **Remove the Node:**
   - The `second` pointer will now be just before the node to be removed. Adjust the pointers to skip that node.

### Time Complexity:

- **O(n):** Single traversal of the linked list.

### Space Complexity:

- **O(1):** Constant space is used.

### Code:

```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode* dummy = new ListNode(0);
    dummy->next = head;
    ListNode* first = dummy;
    ListNode* second = dummy;

    // Move first pointer ahead by n+1 nodes
    for (int i = 1; i <= n + 1; i++) {
        first = first->next;
    }

    // Move both pointers until the first reaches the end
    while (first != NULL) {
        first = first->next;
        second = second->next;
    }

    // Remove the nth node
    second->next = second->next->next;

    return dummy->next;
}
```

# 104. Sort List

### Problem Link

[LeetCode - Sort List](https://leetcode.com/problems/sort-list/description/)

## Approach 1: Array Sort

### Algorithm:

1. **Store Node Values:**  
   Create an empty array to store the node values. Iterate the linked list using a temp pointer to the head and push the value of the temp node into the array. Move temp to the next node.

2. **Sort the Array:**  
   Sort the array containing node values in ascending order.

3. **Convert Back to Linked List:**  
   Convert the sorted array back to a linked list by reassigning the values from the sorted array and overwriting them sequentially according to their order in the array.

### Time Complexity:

- **O(n log n):** Sorting the array requires O(n log n) time.

### Space Complexity:

- **O(n):** Space used for the array.

### Code:

```cpp
ListNode* sortList(ListNode* head) {
    vector<int> values;
    ListNode* temp = head;

    // Step 1: Store Node Values
    while (temp) {
        values.push_back(temp->val);
        temp = temp->next;
    }

    // Step 2: Sort the Array
    sort(values.begin(), values.end());

    // Step 3: Convert Back to Linked List
    temp = head;
    for (int value : values) {
        temp->val = value;
        temp = temp->next;
    }

    return head;
}
```

## Approach 2: Merge Sort

### Algorithm:

1. **Base Case:**  
   If the linked list contains zero or one element, it is already sorted. Return the head node.

2. **Split the List:**  
   Find the middle of the linked list using a slow and a fast pointer. Split the linked list into two halves at the middle node.

3. **Recursion:**  
   Recursively apply merge sort to both halves obtained in the previous step.

4. **Merge Sorted Lists:**  
   Merge the sorted halves obtained from the recursive calls into a single sorted linked list. Update the head pointer to the beginning of the newly sorted list.

5. **Return:**  
   Once the merging is complete, return the head of the sorted linked list.

### Time Complexity:

- **O(n log n):** Linear traversal for splitting and merging.

### Space Complexity:

- **O(log n):** Space used in the call stack for recursion.

### Code:

```cpp
ListNode* sortList(ListNode* head) {
    // Step 1: Base Case
    if (!head || !head->next) {
        return head; // List is already sorted
    }

    // Step 2: Split the List
    ListNode* slow = head;
    ListNode* fast = head->next;

    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }

    ListNode* mid = slow->next; // Middle node
    slow->next = nullptr; // Split the list into two halves

    // Step 3: Recursion
    ListNode* left = sortList(head); // Sort the left half
    ListNode* right = sortList(mid); // Sort the right half

    // Step 4: Merge Sorted Lists
    return merge(left, right);
}

ListNode* merge(ListNode* left, ListNode* right) {
    ListNode dummy; // Dummy node to simplify the merging process
    ListNode* tail = &dummy;

    while (left && right) {
        if (left->val < right->val) {
            tail->next = left;
            left = left->next;
        } else {
            tail->next = right;
            right = right->next;
        }
        tail = tail->next;
    }

    tail->next = left ? left : right; // Append remaining nodes
    return dummy.next; // Return the merged list
}
```

# 105. Given a linked list of 0s, 1s, and 2s, sort it

### Problem Link

[GeeksforGeeks - Given a linked list of 0s, 1s, and 2s, sort it](https://www.geeksforgeeks.org/problems/given-a-linked-list-of-0s-1s-and-2s-sort-it/1)

## Approach 1: Counting Method

### Algorithm:

1. **Count Elements:**  
   Traverse the linked list and count the number of 0s, 1s, and 2s.

2. **Update Node Values:**  
   In a second pass, update the node values based on the counts of 0s, 1s, and 2s.
   Fill the linked list with the counted values in order.

### Time Complexity:

- **O(n):** We traverse the linked list twice.

### Space Complexity:

- **O(1):** No additional space used except for counters.

### Code:

```cpp
Node* segregate(Node* head) {
    int count[3] = {0}; // Array to store counts of 0s, 1s, and 2s
    Node* temp = head;

    // Step 1: Count the number of 0s, 1s, and 2s
    while (temp) {
        count[temp->data]++;
        temp = temp->next;
    }

    // Step 2: Update node values based on counts
    temp = head;
    for (int i = 0; i < 3; i++) {
        while (count[i] > 0) {
            temp->data = i; // Update the current node data
            temp = temp->next;
            count[i]--;
        }
    }
    return head; // Return the head of the sorted linked list
}
```

## Approach 2: Three Dummy Nodes

### Algorithm:

1. **Create Dummy Nodes:**  
   Initialize three dummy nodes (zeroHead, oneHead, twoHead) to hold the nodes for 0s, 1s, and 2s respectively.

2. **Separate the Nodes:**  
   Traverse the linked list and link the nodes to the appropriate dummy nodes based on their values.

3. **Join the Lists:**  
   After creating separate lists, connect the last node of zeroHead to oneHead and then to twoHead to form the sorted list.

4. **Return the Sorted List:**  
   Return the next of the zeroHead, which points to the head of the sorted linked list.

### Time Complexity:

- **O(n):** We traverse the linked list once.

### Space Complexity:

- **O(1):** Only constant space is used for dummy nodes.

### Code:

```cpp
Node* segregate(Node* head) {
    Node *zeroHead = new Node(0), *oneHead = new Node(0), *twoHead = new Node(0);
    Node *zeroTail = zeroHead, *oneTail = oneHead, *twoTail = twoHead;
    Node* temp = head;

    // Step 1: Separate the nodes into three lists
    while (temp) {
        if (temp->data == 0) {
            zeroTail->next = temp;
            zeroTail = zeroTail->next;
        } else if (temp->data == 1) {
            oneTail->next = temp;
            oneTail = oneTail->next;
        } else {
            twoTail->next = temp;
            twoTail = twoTail->next;
        }
        temp = temp->next;
    }

    // Step 2: Join the three lists
    zeroTail->next = oneHead->next; // Connect zero list to one list
    oneTail->next = twoHead->next; // Connect one list to two list
    twoTail->next = nullptr; // End the list

    // Clean up dummy heads
    Node* sortedHead = zeroHead->next;
    delete zeroHead;
    delete oneHead;
    delete twoHead;

    return sortedHead; // Return the head of the sorted linked list
}
```
