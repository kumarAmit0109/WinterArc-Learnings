# 284. Predecessor and Successor in BST

## Problem Link

[Predecessor and Successor in BST](https://www.naukri.com/code360/problems/predecessor-and-successor-in-bst_893049?l)

## Approach : Iterative Search and Traversal

### Idea/Intuition

- The predecessor is the largest value smaller than the given key.
- The successor is the smallest value greater than the given key.
- Use a two-step process:
  1. Locate the node with the given key while keeping track of potential predecessors and successors.
  2. If the key node exists, traverse its left and right subtrees to find the closest values for predecessor and successor.

### Algorithm

1. Initialize `pred` and `succ` as `-1` to denote no predecessor or successor initially.
2. Traverse the BST to locate the key:
   - Update `succ` when moving to the left subtree.
   - Update `pred` when moving to the right subtree.
3. If the key node is found:
   - To find the predecessor:
     - Traverse the left subtree to find the maximum value.
   - To find the successor:
     - Traverse the right subtree to find the minimum value.
4. Return the pair `{pred, succ}`.

### Time Complexity

- **O(h)**: Traversing the tree, where `h` is the height of the tree.

### Space Complexity

- **O(1)**: Constant space, as no extra data structures are used apart from pointers.

### Implementation

```cpp
pair<int, int> predecessorSuccessor(TreeNode *root, int key) {
    int pred = -1, succ = -1;
    TreeNode *temp = root;

    // Step 1: Find the key
    while (temp && temp->data != key) {
        if (key < temp->data) {
            succ = temp->data; // Update possible successor
            temp = temp->left;
        } else {
            pred = temp->data; // Update possible predecessor
            temp = temp->right;
        }
    }

    if (!temp) {
        // Key not found
        return {pred, succ};
    }

    // Step 2: Find predecessor
    if (temp->left) {
        TreeNode *leftTree = temp->left;
        while (leftTree) {
            pred = leftTree->data;
            leftTree = leftTree->right;
        }
    }

    // Step 3: Find successor
    if (temp->right) {
        TreeNode *rightTree = temp->right;
        while (rightTree) {
            succ = rightTree->data;
            rightTree = rightTree->left;
        }
    }

    return {pred, succ};
}
```

# 285. Merge Two BSTs

## Problem Link

[Merge Two BSTs](https://www.naukri.com/code360/problems/h_920474?)

## Approach 1: Inorder Traversal and Merging

### Idea/Intuition

- Perform an **inorder traversal** on both BSTs to get their elements in sorted order.
- Merge the two sorted arrays obtained from the inorder traversals.
- Construct a new BST from the merged sorted array.

### Algorithm

1. Perform an **inorder traversal** on each BST to get their elements in sorted order.
2. Use a **two-pointer technique** to merge the two sorted arrays.
3. Convert the merged sorted array into a balanced BST using the middle element as the root and recursively building left and right subtrees.

### Time Complexity

- **O(m + n)**:
  - O(m) and O(n) for the inorder traversal of two BSTs.
  - O(m + n) for merging the sorted arrays.
- Total: **O(m + n)**.

### Space Complexity

- **O(m + n)**: Storing inorder traversals of both BSTs and the merged array.

### Implementation

```cpp
void inorderTraversal(TreeNode* root, vector<int>& result) {
    if (root == NULL) {
        return;
    }
    // Recursively traverse the left subtree
    inorderTraversal(root->left, result);
    // Store the current node's data
    result.push_back(root->data);
    // Recursively traverse the right subtree
    inorderTraversal(root->right, result);
}

vector<int> mergeSortedArrays(const vector<int>& arr1, const vector<int>& arr2) {
    vector<int> merged;
    int i = 0, j = 0;
    int n = arr1.size(), m = arr2.size();

    // Merge two sorted arrays
    while (i < n && j < m) {
        if (arr1[i] <= arr2[j]) {
            merged.push_back(arr1[i]);
            i++;
        } else {
            merged.push_back(arr2[j]);
            j++;
        }
    }

    // Add remaining elements from arr1
    while (i < n) {
        merged.push_back(arr1[i]);
        i++;
    }

    // Add remaining elements from arr2
    while (j < m) {
        merged.push_back(arr2[j]);
        j++;
    }

    return merged;
}

TreeNode* sortedArrayToBST(vector<int>& arr, int start, int end) {
    if (start > end) {
        return NULL;
    }
    // Middle element as root
    int mid = start + (end - start) / 2;
    TreeNode* root = new TreeNode(arr[mid]);
    // Recursively construct the left and right subtrees
    root->left = sortedArrayToBST(arr, start, mid - 1);
    root->right = sortedArrayToBST(arr, mid + 1, end);
    return root;
}

vector<int> mergeBST(TreeNode* root1, TreeNode* root2) {
    vector<int> inorder1, inorder2;

    // Get inorder traversal of both BSTs
    inorderTraversal(root1, inorder1);
    inorderTraversal(root2, inorder2);

    // Merge the two inorder traversals
    vector<int> mergedInorder = mergeSortedArrays(inorder1, inorder2);

    return mergedInorder;
}
```

## Approach 2: Convert BST to DLL, Merge, and Rebuild

### Idea/Intuition

1. Convert each BST into a **sorted Doubly Linked List (DLL)** using an in-order traversal.
2. Merge the two sorted DLLs into a single sorted DLL.
3. Convert the merged sorted DLL back into a height-balanced BST.
4. Perform an **in-order traversal** of the final BST to get the sorted result.

### Algorithm

1. **Convert BST to DLL**:
   - Perform an in-order traversal and link nodes as DLL.
2. **Merge DLLs**:
   - Use a two-pointer approach to merge the sorted DLLs.
3. **Convert DLL to BST**:
   - Use the middle element as the root to construct a height-balanced BST recursively.
4. **In-order Traversal**:
   - Perform in-order traversal on the merged BST to return the sorted result.

### Time Complexity

- **O(m + n)**:
  - O(m + n) for converting BSTs to DLLs.
  - O(m + n) for merging DLLs.
  - O(m + n) for converting DLL to BST.
  - O(m + n) for in-order traversal of the merged BST.

### Space Complexity

- **O(h1 + h2)**:
  - Stack space for the recursive in-order traversal of the two BSTs (where h1 and h2 are heights of the BSTs).

### Implementation

```cpp
// Function to convert a BST into a sorted Doubly Linked List (DLL)
void bstToDLL(TreeNode* root, TreeNode*& head, TreeNode*& prev) {
    if (root == NULL) {
        return;
    }
    // Recursively process the left subtree
    bstToDLL(root->left, head, prev);

    // Convert the current node
    if (prev == NULL) {
        // If this is the first node, set it as head
        head = root;
    } else {
        // Update the DLL pointers
        prev->right = root;
        root->left = prev;
    }
    prev = root;

    // Recursively process the right subtree
    bstToDLL(root->right, head, prev);
}

// Function to merge two sorted DLLs into one sorted DLL
TreeNode* mergeDLL(TreeNode* head1, TreeNode* head2) {
    TreeNode dummy;
    TreeNode* tail = &dummy;

    while (head1 && head2) {
        if (head1->data <= head2->data) {
            tail->right = head1;
            head1->left = tail;
            head1 = head1->right;
        } else {
            tail->right = head2;
            head2->left = tail;
            head2 = head2->right;
        }
        tail = tail->right;
    }

    if (head1) {
        tail->right = head1;
        head1->left = tail;
    }
    if (head2) {
        tail->right = head2;
        head2->left = tail;
    }

    return dummy.right;
}

// Function to convert a sorted DLL into a height-balanced BST
TreeNode* sortedDLLToBST(TreeNode*& head, int n) {
    if (n <= 0) {
        return NULL;
    }

    TreeNode* left = sortedDLLToBST(head, n / 2);

    TreeNode* root = head;
    root->left = left;

    head = head->right;

    root->right = sortedDLLToBST(head, n - n / 2 - 1);

    return root;
}

// Helper function to count the number of nodes in a DLL
int countNodes(TreeNode* head) {
    int count = 0;
    while (head) {
        count++;
        head = head->right;
    }
    return count;
}

// Helper function to perform in-order traversal and store the result in a vector
void inOrderTraversal(TreeNode* root, vector<int>& result) {
    if (root == NULL) {
        return;
    }
    inOrderTraversal(root->left, result);
    result.push_back(root->data);
    inOrderTraversal(root->right, result);
}

// Main function to merge two BSTs and return the result as a sorted vector
vector<int> mergeBST(TreeNode* root1, TreeNode* root2) {
    // Step 1: Convert both BSTs to sorted DLLs
    TreeNode *head1 = NULL, *prev1 = NULL;
    bstToDLL(root1, head1, prev1);
    if (prev1) prev1->right = NULL;

    TreeNode *head2 = NULL, *prev2 = NULL;
    bstToDLL(root2, head2, prev2);
    if (prev2) prev2->right = NULL;

    // Step 2: Merge the two sorted DLLs
    TreeNode* mergedHead = mergeDLL(head1, head2);

    // Step 3: Convert the merged DLL into a height-balanced BST
    int totalNodes = countNodes(mergedHead);
    TreeNode* mergedBST = sortedDLLToBST(mergedHead, totalNodes);

    // Step 4: Perform in-order traversal of the merged BST and return the result
    vector<int> result;
    inOrderTraversal(mergedBST, result);
    return result;
}
```

# 286. Two Sum IV - Input is a BST

## Problem Link

[Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/)

## Approach 1: Nested Traversal with Value Search

### Idea/Intuition

1. Use a helper function to search for a specific value (`target - current_node_value`) in the BST.
2. Traverse the BST recursively and for each node, check if there exists another node with the required complementary value.
3. Ensure the current node is not used to find its own complement.

### Algorithm

1. Define a helper function `findValue` to check if the `target` value exists in the BST, excluding the current node.
2. Use the main function `findTarget` to traverse the BST and invoke `findValue` for each node.
3. Combine the recursive traversal using `Solve` to check the left and right subtrees.

### Time Complexity

- **O(n\*h)** in the worst case:
  - For each node in the BST (`n` nodes), the `findValue` operation takes O(h), where `h` is the height of the tree.

### Space Complexity

- **O(h)**: Stack space for recursive calls, where `h` is the height of the BST.

### Implementation

```cpp
class Solution {
public:
    // Helper function to find a value in the BST
    bool findValue(TreeNode* root, TreeNode* curr, int target) {
        if (root == nullptr) {
            return false;
        }
        if (root != curr && root->val == target) {
            return true;
        }
        if (target < root->val) {
            return findValue(root->left, curr, target);
        }
        return findValue(root->right, curr, target);
    }

    // Main function to find if two elements sum to k
    bool findTarget(TreeNode* root, int k) {
        if (root == nullptr) {
            return false;
        }
        return Solve(root, root, k);
    }

    // Helper function to traverse the BST and solve the problem
    bool Solve(TreeNode* root, TreeNode* curr, int k) {
        if (curr == nullptr) {
            return false;
        }
        if (findValue(root, curr, k - curr->val)) {
            return true;
        }
        return Solve(root, curr->left, k) || Solve(root, curr->right, k);
    }
};
```

## Approach 2: Nested Traversal with Value Search (Repeated from Approach 1)

### Idea/Intuition

1. Use a helper function to search for a specific value (`target - current_node_value`) in the BST.
2. Traverse the BST recursively and for each node, check if there exists another node with the required complementary value.
3. Ensure the current node is not used to find its own complement.

### Algorithm

1. Define a helper function `findValue` to check if the `target` value exists in the BST, excluding the current node.
2. Use the main function `findTarget` to traverse the BST and invoke `findValue` for each node.
3. Combine the recursive traversal using `Solve` to check the left and right subtrees.

### Time Complexity

- **O(n\*h)** in the worst case:
  - For each node in the BST (`n` nodes), the `findValue` operation takes O(h), where `h` is the height of the tree.

### Space Complexity

- **O(h)**: Stack space for recursive calls, where `h` is the height of the BST.

### Implementation

```cpp
class Solution {
public:
    // Helper function to find a value in the BST
    bool findValue(TreeNode* root, TreeNode* curr, int target) {
        if (root == nullptr) {
            return false;
        }
        if (root != curr && root->val == target) {
            return true;
        }
        if (target < root->val) {
            return findValue(root->left, curr, target);
        }
        return findValue(root->right, curr, target);
    }

    // Main function to find if two elements sum to k
    bool findTarget(TreeNode* root, int k) {
        if (root == nullptr) {
            return false;
        }
        return Solve(root, root, k);
    }

    // Helper function to traverse the BST and solve the problem
    bool Solve(TreeNode* root, TreeNode* curr, int k) {
        if (curr == nullptr) {
            return false;
        }
        if (findValue(root, curr, k - curr->val)) {
            return true;
        }
        return Solve(root, curr->left, k) || Solve(root, curr->right, k);
    }
};
```
