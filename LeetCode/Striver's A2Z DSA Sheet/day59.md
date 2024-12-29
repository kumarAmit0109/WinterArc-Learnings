# 271. Flatten Binary Tree to Linked List

## Problem Link

[Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/description/)

## Approach 1: Iterative Using Stack

### Idea/Intuition

The problem requires converting a binary tree into a flattened linked list in-place, where:

1. The flattened tree should use the right pointers to point to the next node in a pre-order traversal.
2. The left pointers should be set to `NULL`.

We use a stack to perform an iterative pre-order traversal and modify the tree structure on the go.

### Algorithm

1. If the root is `NULL`, return immediately.
2. Initialize a stack and push the root node onto it.
3. While the stack is not empty:
   - Pop the top node from the stack.
   - If the node has a right child, push it onto the stack.
   - If the node has a left child, push it onto the stack.
   - Set the left child of the current node to `NULL`.
   - Set the right child of the current node to the next node in the stack (if it exists).
4. Continue until the stack is empty.

### Time Complexity

- **O(n)**: Each node is visited once.

### Space Complexity

- **O(h)**: The stack can grow up to the height of the tree, where `h` is the height of the binary tree.

### Implementation

```cpp
class Solution {
public:
    void flatten(TreeNode* root) {
        if (root == NULL) {
            return;
        }

        stack<TreeNode*> st;
        st.push(root);

        while (!st.empty()) {
            TreeNode* stTop = st.top();
            st.pop();

            if (stTop->right) {
                st.push(stTop->right);
            }
            if (stTop->left) {
                st.push(stTop->left);
            }

            stTop->left = NULL;
            if (!st.empty()) {
                stTop->right = st.top();
            } else {
                stTop->right = NULL;
            }
        }
    }
};
```

## Approach 2: Reverse Pre-order Traversal

### Idea/Intuition

The approach performs a **reverse pre-order traversal** (process right subtree → process left subtree → process current node). The idea is to use a global variable `prev` to keep track of the previously processed node. This allows us to construct the linked list by attaching subtrees in the correct order.

### Algorithm

1. Initialize a global variable `prev` to `NULL`.
2. Define a recursive function `flatten` that:
   - **Base Case**: If the current node is `NULL`, return immediately.
   - Recursively flatten the right subtree by calling `flatten` on the current node's `right` child.
   - Recursively flatten the left subtree by calling `flatten` on the current node's `left` child.
   - Set the current node's `right` child to `prev` (the previously processed node).
   - Set the current node's `left` child to `NULL`.
   - Update `prev` to the current node.
3. Call `flatten` on the root of the binary tree.

### Time Complexity

- **O(n)**: Each node is visited once.

### Space Complexity

- **O(h)**: Recursive stack space, where `h` is the height of the binary tree.

### Implementation

```cpp
class Solution {
private:
    TreeNode* prev = NULL;

public:
    void flatten(TreeNode* root) {
        if (root == NULL) {
            return;
        }

        // Step 3: Flatten the right and left subtrees
        flatten(root->right);
        flatten(root->left);

        // Step 4: Attach the right subtree to the flattened left subtree
        root->right = prev;

        // Step 5: Attach the left subtree as right child and nullify the left pointer
        root->left = NULL;

        // Step 6: Update prev to the current node
        prev = root;
    }
};
```

## Approach 3: Morris Traversal

### Idea/Intuition

Morris Traversal avoids using extra space for recursion or stack by modifying the binary tree structure temporarily to perform a **preorder traversal**. It leverages threaded binary trees, where nodes are connected to their inorder successors.

### Algorithm

1. Start with the current node as the root of the binary tree.
2. Traverse the tree in a loop until the current node becomes `NULL`:
   - If the current node has a left child:
     1. Find the **rightmost node** in the current node's left subtree.
     2. Connect the rightmost node's `right` pointer to the current node's `right` child.
     3. Update the current node's `right` pointer to its `left` child.
     4. Set the current node's `left` pointer to `NULL`.
   - Move the current node to its `right` child.
3. Repeat until all nodes are processed.

### Time Complexity

- **O(n)**: Each node is visited a constant number of times.

### Space Complexity

- **O(1)**: Morris Traversal uses no extra space.

### Implementation

```cpp
class Solution {
public:
    void flatten(TreeNode* root) {
        TreeNode* current = root;

        while (current != NULL) {
            if (current->left != NULL) {
                // Step 1: Find the rightmost node in the left subtree
                TreeNode* predecessor = current->left;
                while (predecessor->right != NULL) {
                    predecessor = predecessor->right;
                }

                // Step 2: Connect the rightmost node's right pointer to current's right child
                predecessor->right = current->right;

                // Step 3: Update current's right child to its left child
                current->right = current->left;

                // Step 4: Nullify the left pointer
                current->left = NULL;
            }
            // Move to the right child
            current = current->right;
        }
    }
};
```

# 272. Binary Search Trees

## Problem Link

[Binary Search Trees](https://www.geeksforgeeks.org/problems/binary-search-trees/1?)

## Approach: Validate if Array Represents Inorder Traversal of a BST

### Idea/Intuition

Inorder traversal of a Binary Search Tree (BST) produces a strictly increasing sequence. The problem boils down to checking whether the given array is strictly increasing.

### Algorithm

1. Iterate through the array from index 1 to the end.
2. For each element, check if the previous element is greater than or equal to the current element.
3. If such a condition is found, return `false`.
4. If the loop completes, return `true`.

### Time Complexity

- **O(n)**: A single traversal of the array.

### Space Complexity

- **O(1)**: No extra space used apart from variables.

### Implementation

```cpp
class Solution {
public:
    bool isBSTTraversal(vector<int>& arr) {
        // Iterate through the array and check if it is strictly increasing
        for (int i = 1; i < arr.size(); i++) {
            // If a previous element is greater than or equal to the current element, return false
            if (arr[i - 1] >= arr[i]) {
                return false;
            }
        }
        // If we complete the loop, the array is strictly increasing
        return true;
    }
};
```

# 273. Search in a Binary Search Tree

## Problem Link

[Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/description/)

## Approach: Iterative Search

### Idea/Intuition

The problem leverages the properties of a Binary Search Tree (BST), where:

- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.

We use an iterative approach to traverse the tree and locate the node with the target value.

### Algorithm

1. Start from the root node.
2. While the current node is not `NULL`:
   - If the current node's value equals the target value, return the node.
   - If the current node's value is greater than the target value, move to the left subtree.
   - Otherwise, move to the right subtree.
3. If the node is not found, return `NULL`.

### Time Complexity

- **O(h)**: Where `h` is the height of the BST. In the worst case, `h = n` for a skewed tree, and in the best case, `h = log(n)` for a balanced tree.

### Space Complexity

- **O(1)**: No extra space is used.

### Implementation

```cpp
class Solution {
public:
    // Iterative Way
    TreeNode* searchBST(TreeNode* root, int val) {
        TreeNode* temp = root;
        while (temp != NULL) {
            if (temp->val == val) {
                return temp;
            } else if (temp->val > val) {
                temp = temp->left;
            } else {
                temp = temp->right;
            }
        }
        return NULL;
    }
};
```
