# 269. Binary Tree Preorder Traversal

## Problem Link

[Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

## Approach: Morris Traversal (Iterative, No Extra Space)

### Idea/Intuition

The Morris Traversal algorithm is adapted for preorder traversal to avoid recursion or an explicit stack while maintaining constant space. Preorder traversal visits nodes in the order: root → left → right.

1. If the current node has no left child, visit it and move to the right.
2. If the current node has a left child:
   - Find its inorder predecessor (rightmost node in the left subtree).
   - If the predecessor's right pointer is `null`, link it to the current node, visit the current node, and move left.
   - If the predecessor's right pointer already points to the current node, restore the tree and move to the right.

### Algorithm

1. Start from the root and iterate while the current node is not `null`.
2. If the current node has no left child, add its value to the result and move right.
3. If the current node has a left child:
   - Find its inorder predecessor.
   - Modify the tree to create or restore the temporary link as needed.

### Time Complexity

- **O(n)**: Each node is visited at most twice.

### Space Complexity

- **O(1)**: No additional space is used apart from the result vector.

### Implementation

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        TreeNode* current = root;

        while (current != nullptr) {
            if (current->left == nullptr) {
                // No left child; visit the current node and move to the right
                result.push_back(current->val);
                current = current->right;
            } else {
                // Find the inorder predecessor (rightmost node in the left subtree)
                TreeNode* predecessor = current->left;
                while (predecessor->right != nullptr && predecessor->right != current) {
                    predecessor = predecessor->right;
                }

                if (predecessor->right == nullptr) {
                    // Create a temporary link to the current node
                    predecessor->right = current;
                    // Add the current node's value (visit the root before the left subtree)
                    result.push_back(current->val);
                    current = current->left;
                } else {
                    // Remove the temporary link
                    predecessor->right = nullptr;
                    // Move to the right subtree
                    current = current->right;
                }
            }
        }

        return result;
    }
};
```

# 270. Binary Tree Inorder Traversal

## Problem Link

[Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## Approach: Morris Traversal (Iterative, No Extra Space)

### Idea/Intuition

The Morris Traversal algorithm performs an inorder traversal without using extra space. It temporarily modifies the tree structure by creating links to predecessors. The algorithm ensures constant space usage and avoids recursion or an explicit stack.

1. If the current node does not have a left child, visit it and move to the right.
2. If the current node has a left child:
   - Find its inorder predecessor (rightmost node in the left subtree).
   - If the predecessor's right pointer is `null`, link it to the current node and move to the left child.
   - If the predecessor's right pointer already points to the current node, restore the tree, visit the current node, and move to the right.

### Algorithm

1. Start from the root and iterate while the current node is not `null`.
2. If the current node has no left child, add its value to the result and move right.
3. If the current node has a left child:
   - Find its inorder predecessor.
   - Modify the tree to create or restore the temporary link as needed.

### Time Complexity

- **O(n)**: Each node is visited at most twice.

### Space Complexity

- **O(1)**: No additional space is used apart from the result vector.

### Implementation

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;  // This will store the inorder traversal result
        TreeNode* current = root;

        while (current != nullptr) {
            if (current->left == nullptr) {
                // Case 1: No left subtree, visit the node and move to the right
                result.push_back(current->val);
                current = current->right;
            } else {
                // Case 2: There is a left subtree, find the inorder predecessor
                TreeNode* predecessor = current->left;
                // Find the rightmost node in the left subtree
                while (predecessor->right != nullptr && predecessor->right != current) {
                    predecessor = predecessor->right;
                }

                if (predecessor->right == nullptr) {
                    // If the right child of the predecessor is null, make a temporary link to the current node
                    predecessor->right = current;
                    current = current->left;  // Move to the left child
                } else {
                    // If the right child of the predecessor is already pointing to the current node, we've completed the left subtree
                    result.push_back(current->val);
                    predecessor->right = nullptr;  // Revert the changes (unlink the temporary link)
                    current = current->right;  // Move to the right child
                }
            }
        }
        return result;
    }
};
```
