# 96. Reverse Linked List (Iterative)

### Problem Link

[LeetCode - Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/)

## Approach: Iterative Method

### Algorithm:

1. **Initialize Pointers:**  
   Create three pointers: `curr` pointing to the head of the linked list, `prev` initialized to `NULL`, and `frwd` initialized to `NULL`.

2. **Iterate Through the List:**  
   While `curr` is not `NULL`, do the following:

   - Store the next node in `frwd` (i.e., `frwd = curr->next`).
   - Reverse the link by pointing `curr->next` to `prev`.
   - Move `prev` and `curr` one step forward (i.e., `prev = curr` and `curr = frwd`).

3. **Return the New Head:**  
   After the loop ends, `prev` will be pointing to the new head of the reversed linked list.

### Time Complexity

- **O(n)**: We traverse the entire list once.

### Space Complexity

- **O(1)**: Only a constant amount of extra space is used.

### Code

```cpp
ListNode* reverseIterative(ListNode* head) {
    if (head == NULL || head->next == NULL) {
        return head;
    }
    ListNode* curr = head;
    ListNode* prev = NULL;
    ListNode* frwd = NULL;
    while (curr != NULL) {
        frwd = curr->next;  // Store the next node
        curr->next = prev;  // Reverse the link
        prev = curr;        // Move prev and curr one step forward
        curr = frwd;
    }
    return prev;  // New head of the reversed list
}

ListNode* reverseList(ListNode* head) {
    return reverseIterative(head);
}
```

# Reverse Linked List (Recursive)

### Problem Link

[LeetCode - Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/)

## Approach 1: Recursive Method

### Algorithm:

1. **Recursive Function:**  
   Define a recursive function `reverse2` that takes three parameters: a reference to the head pointer, `curr` (the current node), and `prev` (the previous node).

2. **Base Case:**  
   If `curr` is `NULL`, set the head to `prev` and return. This indicates that we have reached the end of the list and `prev` is the new head.

3. **Recur:**  
   Store the next node in `frwd` (i.e., `frwd = curr->next`). Call `reverse2` recursively with `head`, `frwd`, and `curr`.

4. **Reverse Link:**  
   After the recursive call, set `curr->next` to `prev`, effectively reversing the link.

### Time Complexity

- **O(n)**: We traverse the entire list once.

### Space Complexity

- **O(n)**: The recursion stack takes up space proportional to the number of nodes in the list.

### Code

```cpp
void reverse2(ListNode*& head, ListNode* curr, ListNode* prev) {
    if (curr == NULL) {
        head = prev;
        return;
    }
    ListNode* frwd = curr->next;  // Store the next node
    reverse2(head, frwd, curr);    // Recur with the next node
    curr->next = prev;             // Reverse the link
}

ListNode* reverseList(ListNode* head) {
    ListNode* curr = head;  // Current node
    ListNode* prev = NULL;  // Previous node
    reverse2(head, curr, prev);  // Start the recursion
    return head;  // Return the new head
}
```

## Approach 2: Recursive Method

### Algorithm:

1. **Base Case:**  
   If the head is `NULL` or if `head->next` is `NULL`, return `head`. This indicates that we have reached the end of the list or the list has only one node.

2. **Recursive Call:**  
   Call the `reverse` function recursively with `head->next` to reverse the remaining linked list.

3. **Reverse the Link:**  
   After the recursive call, set `head->next->next` to `head`, effectively reversing the link. Then set `head->next` to `NULL` to avoid cycles.

4. **Return the New Head:**  
   Finally, return `chotaHead`, which is the head of the reversed linked list.

### Time Complexity

- **O(n)**: We traverse the entire list once.

### Space Complexity

- **O(n)**: The recursion stack takes up space proportional to the number of nodes in the list.

### Code

```cpp
ListNode* reverse(ListNode* head) {
    // Base case
    if (head == NULL || head->next == NULL) {
        return head;
    }
    ListNode* chotaHead = reverse(head->next);  // Recursive call
    head->next->next = head;  // Reverse the link
    head->next = NULL;  // Avoid cycle
    return chotaHead;  // Return the new head
}

ListNode* reverseList(ListNode* head) {
    return reverse(head);  // Start the recursion
}
```
