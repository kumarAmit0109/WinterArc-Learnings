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

# 97. Reverse Linked List (Recursive)

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

# 98. Linked List Cycle

### Problem Link

[LeetCode - Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/description/)

## Approach 1: Using a Hash Map

### Algorithm:

1. **Traverse the List:**  
   As we traverse the linked list, use a hash map (or set) to mark each node as visited.

2. **Check for Cycles:**  
   Before adding a node to the map, check if the current node is already marked as visited. If it is, return `true`, indicating that a cycle is present.

3. **Return False:**  
   If we finish traversing the list without finding a cycle, return `false`.

### Time Complexity

- **O(n)**: We traverse the entire list once.

### Space Complexity

- **O(n)**: We use additional space for the hash map.

### Code

```cpp
bool hasCycle(ListNode *head) {
    unordered_set<ListNode*> visited;
    ListNode* current = head;

    while (current != NULL) {
        // Check if the current node is already visited
        if (visited.find(current) != visited.end()) {
            return true;  // Cycle found
        }
        // Mark the current node as visited
        visited.insert(current);
        current = current->next;  // Move to the next node
    }
    return false;  // No cycle found
}
```

## Approach 2: Tortoise and Hare Algorithm

### Algorithm:

1. **Initialize Pointers:**  
   Create two pointers, `slow` and `fast`. Initialize both to the head of the linked list.

2. **Traverse with Two Pointers:**  
   Move `slow` one step at a time and `fast` two steps at a time. If they meet at any point, a cycle exists.

3. **Check for End of List:**  
   If `fast` or `fast->next` becomes `NULL`, return `false`, indicating that there is no cycle.

### Time Complexity

- **O(n)**: We traverse the list at most twice.

### Space Complexity

- **O(1)**: No additional space is used.

### Code

```cpp
bool hasCycle(ListNode *head) {
    ListNode *fast = head;
    ListNode *slow = head;

    while (fast != NULL && fast->next != NULL) {
        fast = fast->next->next;  // Move fast pointer two steps
        slow = slow->next;  // Move slow pointer one step

        if (fast == slow) {
            return true;  // Cycle found
        }
    }
    return false;  // No cycle found
}
```

# 99. Linked List Cycle II

### Problem Link

[LeetCode - Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

## Approach 1: Using HashMap

### Algorithm / Intuition

The starting point of a loop in the linked list is the first node we visit twice during its traversal. It's the point where we realize that we are no longer moving forward in the list but rather entering a cycle.

### Steps:

1. **Traverse the Linked List:**  
   Use a temporary node starting from the head and iterate through the list until reaching null.

2. **Track Visited Nodes:**  
   Keep track of visited nodes using a map data structure.

   **Note:** Storing the entire node in the map is essential to distinguish between nodes with identical values but different positions in the list. This ensures accurate loop detection and not just duplicate value checks.

3. **Detect Cycle:**  
   If a previously visited node is encountered again, return that node as it indicates the start of the loop in the linked list.

4. **No Cycle:**  
   If traversal completes and we reach the end of the list (null), return null.

### Time Complexity

- **O(n)**: We traverse the list once.

### Space Complexity

- **O(n)**: We use extra space for the map.

### Code

```cpp
ListNode* detectCycle(ListNode *head) {
    unordered_map<ListNode*, bool> visited;
    ListNode* temp = head;

    while (temp) {
        if (visited[temp]) {
            return temp;  // Start of the cycle detected
        }
        visited[temp] = true;
        temp = temp->next;
    }
    return NULL;  // No cycle
}
```

## Approach 2: Tortoise and Hare Algorithm

### Algorithm:

1. **Detect Cycle Presence:**

   - Use two pointers, `slow` and `fast`. Both start from the head.
   - `slow` moves one step at a time, while `fast` moves two steps at a time.
   - If there is a cycle, `slow` and `fast` will eventually meet. If they never meet, there is no cycle.

2. **Find the Start of the Cycle:**
   - Once the cycle is detected (i.e., `slow` equals `fast`), reset the `slow` pointer to the head.
   - Now, move both `slow` and `fast` one step at a time. The point where they meet again is the **starting node of the cycle**.

### Time Complexity

- **O(n)**: The list is traversed a couple of times but remains linear in complexity.

### Space Complexity

- **O(1)**: Only a constant amount of extra space is used.

### Code

```cpp
ListNode* detectCycle(ListNode* head) {
    if (head == NULL || head->next == NULL) {
        return NULL;
    }

    ListNode* slow = head;
    ListNode* fast = head;
    bool isCycle = false;

    // Step 1: Detect if a cycle is present
    while (fast != NULL && fast->next != NULL) {
        slow = slow->next;
        fast = fast->next->next;

        if (slow == fast) {
            isCycle = true;
            break;
        }
    }

    // Step 2: If no cycle, return NULL
    if (!isCycle) {
        return NULL;
    }

    // Step 3: Find the starting point of the cycle
    slow = head;
    while (slow != fast) {
        slow = slow->next;
        fast = fast->next;
    }

    return slow;  // Starting node of the cycle
}
```

# 100. Find Length of Loop in Linked List

### Problem Link

[GeeksforGeeks - Find Length of Loop](https://www.geeksforgeeks.org/problems/find-length-of-loop/1)

## Approach 1: Brute Force Hashing

### Algorithm:

1. **Initialize Traversal:**  
   Start traversing the linked list using a pointer (`temp`), starting from the head node.

2. **Track Nodes with Timer Values:**  
   Maintain a hashmap (`visited`) where each node (`Node*`) is stored as the key and the timer value (step count) as the value.  
   As we traverse the list, store the current node and its associated timer value in the hashmap.

3. **Detect Loop:**  
   If a node is encountered that has already been visited (i.e., exists in the hashmap), it indicates a loop.  
   The length of the loop is determined by subtracting the timer value of the previously stored instance from the current timer value.

4. **Return 0 for No Loop:**  
   If the traversal reaches the end of the list (`null`) without finding any repeated nodes, return 0 indicating no loop is present.

### Time Complexity:

- **O(n):** We traverse the linked list once.

### Space Complexity:

- **O(n):** We use extra space for storing nodes in the hashmap.

### Code:

```cpp
#include <unordered_map>

int countNodesinLoop(Node* head) {
    std::unordered_map<Node*, int> visited;
    Node* temp = head;
    int timer = 0;

    while (temp != NULL) {
        // If the node is already visited, calculate the loop length
        if (visited.find(temp) != visited.end()) {
            return timer - visited[temp];
        }
        // Mark the node with the current timer value
        visited[temp] = timer;
        temp = temp->next;
        timer++;
    }
    // No loop found
    return 0;
}
```

## Approach 2: Tortoise and Hare Algorithm

### Algorithm:

1. **Detect Loop with Tortoise and Hare:**  
   Use two pointers, `slow` and `fast`. The `slow` pointer moves one step at a time, while the `fast` pointer moves two steps at a time.  
   If `slow` and `fast` meet, it confirms the presence of a loop in the linked list.

2. **Find the Loop Starting Point:**  
   Once a loop is detected, reset the `slow` pointer to the head of the linked list. Move both `slow` and `fast` pointers one step at a time.  
   The point where they meet again is the starting point of the loop.

3. **Count the Length of the Loop:**  
   After determining the starting point of the loop, traverse the loop to count the number of nodes in the cycle.  
   Start from the loop's entry point, move around the loop, and stop once you reach the starting point again. The number of nodes traversed will be the length of the loop.

### Time Complexity:

- **O(n):** Linear traversal of the linked list.

### Space Complexity:

- **O(1):** Only constant space is used for the two pointers.

### Code:

```cpp
int countNodesinLoop(Node* head) {
    if (head == NULL || head->next == NULL) {
        return 0;  // No loop if the list is empty or has only one node
    }

    Node* slow = head;
    Node* fast = head;

    // Step 1: Detect Loop with Tortoise and Hare
    while (fast != NULL && fast->next != NULL) {
        slow = slow->next;
        fast = fast->next->next;

        if (slow == fast) {
            // Loop detected
            break;
        }
    }

    // No loop found
    if (fast == NULL || fast->next == NULL) {
        return 0;
    }

    // Step 2: Find the Loop Starting Point
    slow = head;
    while (slow != fast) {
        slow = slow->next;
        fast = fast->next;
    }

    // Now slow and fast are both at the starting point of the loop

    // Step 3: Count the Length of the Loop
    Node* temp = slow;
    int length = 1;
    while (temp->next != slow) {
        length++;
        temp = temp->next;
    }

    return length;  // Return the length of the loop
}
```
