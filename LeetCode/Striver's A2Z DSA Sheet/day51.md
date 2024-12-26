# 247. Balanced Binary Tree

## Problem Link

[LeetCode - Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/description/)

## Approach 1: Recursive Approach with Separate Height Calculation

### Idea/Intuition

To check if a binary tree is balanced:

1. A tree is balanced if:
   - The left and right subtrees are balanced.
   - The difference in their heights is at most 1.
2. Use a recursive function `isBalanced` to check the balance status and calculate the height of the subtrees separately.

### Algorithm

1. Define a helper function `height`:
   - Base case: If the node is `NULL`, return 0.
   - Recursively calculate the height of the left and right subtrees.
   - Return the maximum of the two heights plus 1.
2. In the `isBalanced` function:
   - Base case: If the node is `NULL`, return true.
   - Recursively check if the left and right subtrees are balanced.
   - Calculate the height difference between the left and right subtrees.
   - Return true if the subtrees are balanced and the height difference is at most 1.

### Time Complexity

- \(O(N^2)\), where \(N\) is the number of nodes in the tree. Each node's height is recalculated multiple times.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the tree. This is the space used by the recursion stack.

### Implementation

```cpp
class Solution {
public:
    // Helper function to calculate height of a tree
    int height(TreeNode* root) {
        if (root == NULL) {
            return 0; // Base case: height of an empty tree is 0
        }
        int leftHeight = height(root->left);
        int rightHeight = height(root->right);

        return max(leftHeight, rightHeight) + 1; // Return max height of subtrees + 1
    }

    bool isBalanced(TreeNode* root) {
        if (root == NULL) {
            return true; // Base case: empty tree is balanced
        }

        // Check balance status of left and right subtrees
        bool left = isBalanced(root->left);
        bool right = isBalanced(root->right);

        // Calculate height difference
        int leftHeight = height(root->left);
        int rightHeight = height(root->right);

        // Return true if both subtrees are balanced and height difference is <= 1
        return left && right && (abs(leftHeight - rightHeight) <= 1);
    }
};
```

## Approach 2: Optimized Recursive Approach with Pair

### Idea/Intuition

To check if a binary tree is balanced:

1. A tree is balanced if:
   - The left and right subtrees are balanced.
   - The difference in their heights is at most 1.
2. Use a single recursive function to compute both:
   - The height of the tree.
   - Whether the tree is balanced.

### Algorithm

1. Define a recursive function `solve`:
   - Base case: If the node is `NULL`, return a pair `{0, true}` (height 0 and balanced).
   - Recursively calculate the height and balance status of the left and right subtrees.
   - Determine the current node's balance status:
     - True if both subtrees are balanced and the height difference is at most 1.
   - Compute the height of the current node as `max(leftHeight, rightHeight) + 1`.
   - Return the height and balance status as a pair.
2. In the `isBalanced` function, call `solve` on the root and return the balance status.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the tree. Each node is visited once.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the tree. This is the space used by the recursion stack.

### Implementation

```cpp
class Solution {
public:
    // Recursive function to calculate height and balance status
    pair<int, bool> solve(TreeNode* root) {
        if (root == NULL) {
            return {0, true}; // Base case: height 0 and balanced
        }

        // Recursive calls for left and right subtrees
        pair<int, bool> leftAns = solve(root->left);
        pair<int, bool> rightAns = solve(root->right);

        // Compute balance status for current node
        bool isBalanced = leftAns.second && rightAns.second &&
                          (abs(leftAns.first - rightAns.first) <= 1);

        // Compute height for current node
        int height = max(leftAns.first, rightAns.first) + 1;

        return {height, isBalanced};
    }

    bool isBalanced(TreeNode* root) {
        if (root == NULL) {
            return true; // Empty tree is balanced
        }
        return solve(root).second; // Return the balance status
    }
};
```

# 248. Diameter of Binary Tree

## Problem Link

[LeetCode - Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/description/)

## Approach 1: Recursive Approach with Separate Height Calculation

### Idea/Intuition

The diameter of a binary tree is the longest path between any two nodes in the tree, which may or may not pass through the root.

1. Calculate the diameter for the left and right subtrees recursively.
2. Compute the height of the left and right subtrees to determine the diameter passing through the root.
3. The result for a node is the maximum of:
   - The diameter of the left subtree.
   - The diameter of the right subtree.
   - The sum of the heights of the left and right subtrees.

### Algorithm

1. Define a helper function `height` to compute the height of a tree:
   - Base case: If the node is `NULL`, return 0.
   - Recursively calculate the height of the left and right subtrees and return the maximum height + 1.
2. In the `diameterOfBinaryTree` function:
   - Base case: If the node is `NULL`, return 0.
   - Recursively calculate the diameter of the left and right subtrees.
   - Compute the height of the left and right subtrees.
   - Calculate the diameter passing through the current node as the sum of the heights of the left and right subtrees.
   - Return the maximum of the above three values.

### Time Complexity

- \(O(N^2)\), where \(N\) is the number of nodes in the tree. Each node's height is recalculated multiple times.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the tree. This is the space used by the recursion stack.

### Implementation

```cpp
class Solution {
public:
    // Helper function to calculate the height of a tree
    int height(TreeNode* root) {
        if (root == NULL) {
            return 0; // Base case: height of an empty tree is 0
        }
        int leftHeight = height(root->left);
        int rightHeight = height(root->right);

        return max(leftHeight, rightHeight) + 1; // Return max height of subtrees + 1
    }

    int diameterOfBinaryTree(TreeNode* root) {
        if (root == NULL) {
            return 0; // Base case: diameter of an empty tree is 0
        }

        // Recursive call for left and right subtrees
        int leftDiameter = diameterOfBinaryTree(root->left);
        int rightDiameter = diameterOfBinaryTree(root->right);

        // Calculate heights of left and right subtrees
        int leftHeight = height(root->left);
        int rightHeight = height(root->right);

        // Diameter passing through the current node
        int diameterThroughRoot = leftHeight + rightHeight;

        // Return the maximum of the three possible diameters
        return max(diameterThroughRoot, max(leftDiameter, rightDiameter));
    }
};
```

## Approach 2: Optimized Recursive Approach with Pair

### Idea/Intuition

To optimize the calculation of the diameter, the height and the diameter can be calculated simultaneously:

1. Use a pair to store both the height and the diameter for each subtree.
2. At each node:
   - Compute the height as the maximum height of the left and right subtrees plus one.
   - Compute the diameter as the maximum of:
     - The diameter of the left subtree.
     - The diameter of the right subtree.
     - The sum of the heights of the left and right subtrees (diameter through the current node).

### Algorithm

1. Define a helper function `solve` that returns a pair of integers:
   - `first`: the height of the tree.
   - `second`: the diameter of the tree.
2. In the helper function:
   - Base case: If the node is `NULL`, return `{0, 0}`.
   - Recursively compute the results for the left and right subtrees.
   - Calculate the height as `max(leftHeight, rightHeight) + 1`.
   - Calculate the diameter as the maximum of:
     - The diameter of the left subtree.
     - The diameter of the right subtree.
     - The sum of the heights of the left and right subtrees.
   - Return the pair `{height, diameter}`.
3. In the `diameterOfBinaryTree` function, call `solve` and return the diameter.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the tree. Each node is visited once.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the tree. This is the space used by the recursion stack.

### Implementation

```cpp
class Solution {
public:
    // Helper function to calculate height and diameter
    pair<int, int> solve(TreeNode* root) {
        if (root == NULL) {
            return {0, 0}; // Base case: height and diameter of an empty tree are 0
        }

        // Recursively solve for left and right subtrees
        pair<int, int> leftAns = solve(root->left);
        pair<int, int> rightAns = solve(root->right);

        // Height of the current node
        int height = max(leftAns.first, rightAns.first) + 1;

        // Diameter through the current node
        int diameterThroughRoot = leftAns.first + rightAns.first;

        // Maximum diameter
        int diameter = max(diameterThroughRoot, max(leftAns.second, rightAns.second));

        return {height, diameter};
    }

    int diameterOfBinaryTree(TreeNode* root) {
        pair<int, int> ans = solve(root);
        return ans.second; // The diameter is stored in the second value of the pair
    }
};
```

# 249. Binary Tree Maximum Path Sum

## Problem Link

[Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

## Approach 1: Recursive DFS with Global Maximum Tracking

### Idea/Intuition

1. Traverse the binary tree using Depth-First Search (DFS).
2. At each node, calculate three potential path sums:
   - **Current path sum**: Including the node and its left and right children.
   - **Single-side path**: Maximum of the left or right subtree plus the node's value.
   - **Node-only path**: Value of the node itself.
3. Update a global variable to store the maximum path sum encountered so far.
4. Return the maximum sum achievable from the current node to its parent (considering only one side).

### Algorithm

1. **Base Case**:
   - If the node is `NULL`, return 0.
2. **Recursive Case**:
   - Calculate the maximum path sum for the left and right subtrees.
   - Compute the possible path sums:
     - Current path sum.
     - Single-side path sum.
     - Node-only path sum.
   - Update the global maximum path sum.
3. Return the maximum single-side path sum or the node-only path sum to the parent.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the binary tree. Each node is visited once.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the binary tree, for recursion stack space.

### Implementation

```cpp
class Solution {
public:
    int maxSum = INT_MIN;  // Global variable to store the maximum path sum

    // Helper function to calculate the maximum path sum
    int findMaxPath(TreeNode* node) {
        if (node == NULL)
            return 0;

        // Recursively calculate the maximum path sum for the left and right children
        int leftSum = max(0, findMaxPath(node->left));  // Ignore negative paths
        int rightSum = max(0, findMaxPath(node->right));

        // Case where the current node is the turning point: sum of node, left, and right
        int currentPathSum = leftSum + rightSum + node->val;

        // Update the global maximum path sum
        maxSum = max(maxSum, currentPathSum);

        // Return the maximum sum we can achieve from this node to its parent (only one side)
        return max(leftSum, rightSum) + node->val;
    }

    int maxPathSum(TreeNode* root) {
        findMaxPath(root);
        return maxSum;  // Return the global maximum path sum
    }
};
```

## Approach 2: Recursive DFS with Parameter Passing for Global Maximum

### Idea/Intuition

1. Use recursive DFS to calculate the maximum path sum for each node.
2. At each node, compute:
   - The sum of the current node's value and its left and right child contributions.
   - The maximum path sum that can be contributed to its parent (choose the larger contribution between left or right).
3. Use a reference variable to keep track of the global maximum path sum during recursion.

### Algorithm

1. **Base Case**:
   - If the node is `NULL`, return 0.
2. **Recursive Case**:
   - Calculate the maximum path sum for the left and right subtrees, ignoring negative paths by taking the maximum with 0.
   - Compute the current path sum using the node's value and the left and right contributions.
   - Update the global maximum path sum if the current path sum is larger.
   - Return the maximum contribution (node + max of left/right subtree) to the parent.
3. **Final Step**:
   - Start the recursion from the root and return the global maximum path sum.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the binary tree. Each node is visited once.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the binary tree, for recursion stack space.

### Implementation

```cpp
class Solution {
public:
    // Helper function to calculate the maximum path sum
    int findMaxPathSum(TreeNode* node, int& maxi) {
        if (node == NULL) {
            return 0;  // If node is NULL, it doesn't contribute to the path sum
        }

        // Recursively find the maximum path sum for left and right children
        int leftSum = max(0, findMaxPathSum(node->left, maxi));  // Ignore negative paths
        int rightSum = max(0, findMaxPathSum(node->right, maxi));  // Ignore negative paths

        // Current node's path sum considering both left and right subtrees
        int currentSum = node->val + leftSum + rightSum;

        // Update the global maximum path sum
        maxi = max(maxi, currentSum);

        // Return the maximum path sum from this node to its parent (only one branch can be taken)
        return node->val + max(leftSum, rightSum);
    }

    // Main function to get the maximum path sum
    int maxPathSum(TreeNode* root) {
        int maxi = INT_MIN;  // Initialize maxi to the smallest possible integer value

        // Start the recursion from the root node
        findMaxPathSum(root, maxi);

        // Return the final maximum path sum
        return maxi;
    }
};
```
