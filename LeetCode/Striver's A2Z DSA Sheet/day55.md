# 259. Lowest Common Ancestor of a Binary Tree

## Problem Link

[Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

## Approach: Recursive Traversal

### Idea/Intuition

1. Start with the root and check if the current node matches either of the target nodes `p` or `q`. If so, return the root.
2. Recursively traverse the left and right subtrees.
3. If both left and right subtrees return non-null values, the current node is the lowest common ancestor.
4. If one subtree returns a non-null value, propagate that upwards as the potential ancestor.
5. If both subtrees return null, it means neither `p` nor `q` are found in that subtree.

### Algorithm

1. If the current node is `NULL`, return `NULL`.
2. If the current node is `p` or `q`, return the current node.
3. Recursively search the left and right subtrees.
4. If both left and right subtrees return non-null values, the current node is the LCA.
5. Otherwise, return the non-null result from either the left or right subtree.

### Time Complexity

- **Traversal**: \(O(N)\), where \(N\) is the number of nodes in the tree. In the worst case, we need to visit all nodes.

### Space Complexity

- **Recursion Stack**: \(O(H)\), where \(H\) is the height of the tree (in the worst case, the height is \(N\) if the tree is skewed).

### Implementation

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == NULL) {
            return NULL;
        }
        if ((root->val == p->val) || (root->val == q->val)) {
            return root;
        }
        TreeNode* leftAns = lowestCommonAncestor(root->left, p, q);
        TreeNode* rightAns = lowestCommonAncestor(root->right, p, q);

        if (leftAns != NULL && rightAns != NULL) {
            return root;
        }
        if (leftAns != NULL && rightAns == NULL) {
            return leftAns;
        }
        if (rightAns != NULL && leftAns == NULL) {
            return rightAns;
        }
        return NULL;
    }
};
```

# 261. Children's Sum Property

## Problem Link

[Children's Sum Property](https://www.geeksforgeeks.org/problems/children-sum-parent/1)

## Approach 1: Efficient Recursive Check with Sum Calculation

### Idea/Intuition

1. For each node in the tree, check if the sum of its left and right children is equal to its value.
2. Recursively check the left and right subtrees to ensure that the children sum property holds for all nodes.
3. Use a helper function that returns a pair of a boolean (indicating if the subtree satisfies the sum property) and an integer (the total sum of the subtree including the node itself).

### Algorithm

1. If the current node is `NULL`, return `true` with a sum of `0` because an empty tree trivially satisfies the sum property.
2. If the node is a leaf (both left and right children are `NULL`), return `true` with the node's value as the sum.
3. Recursively check the left and right subtrees. If either subtree does not satisfy the sum property, the entire tree violates the property.
4. Check if the current node’s value is equal to the sum of its left and right children. If so, return `true` with the sum of the current node and its children.
5. If the property is violated at any point, return `false`.

### Time Complexity

- **Time Complexity**: \(O(N)\), where \(N\) is the number of nodes in the tree, because each node is visited once.
- **Space Complexity**: \(O(H)\), where \(H\) is the height of the tree, due to the recursive call stack.

### Implementation

```cpp
class Solution {
public:
    // Helper function to check whether the tree follows the children sum property
    Pair<bool, int> isSumTreeFast(Node* root) {
        // Base case: if node is NULL, return true and sum as 0
        if (root == NULL) {
            return make_pair(true, 0);
        }

        // If the node is a leaf, it's trivially a sum tree, return its value
        if (root->left == NULL && root->right == NULL) {
            return make_pair(true, root->data);
        }

        // Recursively check the left and right subtrees
        Pair<bool, int> leftAns = isSumTreeFast(root->left);
        Pair<bool, int> rightAns = isSumTreeFast(root->right);

        // Check the sum property at the current node
        bool isLeftSumTree = leftAns.first;
        bool isRightSumTree = rightAns.first;

        int leftSum = leftAns.second;
        int rightSum = rightAns.second;

        // The node must be equal to the sum of its left and right children
        bool isSumTree = (root->data == leftSum + rightSum);

        // If the current node, its left, and right children all satisfy the sum property
        Pair<bool, int> ans;
        if (isLeftSumTree && isRightSumTree && isSumTree) {
            ans.first = true;
            ans.second = root->data + leftSum + rightSum;  // Sum for this node
        } else {
            ans.first = false;
        }

        return ans;
    }

    // Function to check whether the tree satisfies the children sum property
    bool isSumProperty(Node* root) {
        // Get the result from isSumTreeFast function
        return isSumTreeFast(root).first;
    }
};
```

## Approach 2: Recursive Check for Sum Property

### Idea/Intuition

1. For each node in the tree, check if the value of the node is equal to the sum of its left and right children's values.
2. If the current node is a leaf, it trivially satisfies the sum property (as it has no children).
3. Recursively check all nodes to ensure the property holds for every node in the tree.

### Algorithm

1. If the current node is `NULL` (base case), return `true` because an empty subtree trivially satisfies the sum property.
2. If the node is a leaf node (both left and right children are `NULL`), return `true` because the property holds for leaf nodes.
3. For non-leaf nodes, get the values of the left and right children. If either child is `NULL`, consider its value as `0`.
4. Check if the current node's value equals the sum of its left and right children.
5. Recursively check both the left and right subtrees. If both subtrees satisfy the sum property, return `true`; otherwise, return `false`.

### Time Complexity

- **Time Complexity**: \(O(N)\), where \(N\) is the number of nodes in the tree, as each node is visited once.
- **Space Complexity**: \(O(H)\), where \(H\) is the height of the tree, due to the recursive call stack.

### Implementation

```cpp
class Solution {
  public:
    // Function to check whether all nodes of a tree have the value
    // equal to the sum of their child nodes.
    int isSumProperty(Node* root) {
        // If the root is NULL, it trivially satisfies the property
        if (root == NULL) {
            return 1;
        }

        // If the node is a leaf node (no left or right children), it satisfies the property
        if (root->left == NULL && root->right == NULL) {
            return 1;
        }

        // Initialize left and right child data values
        int leftData = 0, rightData = 0;

        // If left child exists, get its data
        if (root->left != NULL) {
            leftData = root->left->data;
        }

        // If right child exists, get its data
        if (root->right != NULL) {
            rightData = root->right->data;
        }

        // Check if the current node's data is equal to the sum of its children
        if (root->data != leftData + rightData) {
            return 0;  // If the sum is not equal to the current node's value, return false
        }

        // Recur for left and right subtrees
        int leftCheck = isSumProperty(root->left);
        int rightCheck = isSumProperty(root->right);

        // If both subtrees satisfy the property, return true
        if (leftCheck && rightCheck) {
            return 1;
        }

        return 0;  // Otherwise return false
    }
};
```
