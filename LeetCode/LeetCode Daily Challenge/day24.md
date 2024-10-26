# Problem: Flip Equivalent Binary Trees

[LeetCode Problem Link](https://leetcode.com/problems/flip-equivalent-binary-trees/)

## Approach: Recursive Comparison

1. **Base Cases**:

   - If both nodes are null, they are equivalent.
   - If one node is null or their values differ, they are not equivalent.

2. **Check for Flip Equivalence**:

   - Recursively check both possibilities:
     - Without flipping: Compare the left child of the first tree with the left child of the second tree and the right child of the first tree with the right child of the second tree.
     - With flipping: Compare the left child of the first tree with the right child of the second tree and the right child of the first tree with the left child of the second tree.

3. **Return the Result**:
   - Return true if either of the above checks returns true; otherwise, return false.

### Time Complexity

- **O(n)**: where \(n\) is the number of nodes in the binary tree, as each node is visited once.

### Space Complexity

- **O(h)**: where \(h\) is the height of the binary tree, due to the recursion stack.

### C++ Code Implementation

```cpp
class Solution {
public:
    bool flipEquiv(TreeNode* root1, TreeNode* root2) {
        // If both nodes are null, they are equivalent
        if (!root1 && !root2) return true;
        // If one node is null or their values differ, they are not equivalent
        if (!root1 || !root2 || root1->val != root2->val) return false;

        // Check both possibilities: without flip or with flip
        return (flipEquiv(root1->left, root2->left) && flipEquiv(root1->right, root2->right)) ||
               (flipEquiv(root1->left, root2->right) && flipEquiv(root1->right, root2->left));
    }
};
```
