# Problem: Cousins in Binary Tree II

[LeetCode Problem Link](https://leetcode.com/problems/cousins-in-binary-tree-ii/description/)

## Approach

1. **Level Order Traversal**:

   - Use a queue to perform a level order traversal of the binary tree.
   - Keep track of the cumulative sum of values at the previous level.

2. **Sibling Values Calculation**:

   - For each node at the current level, calculate the sum of its children's values (siblings).
   - Update the values of the children based on the current sibling values sum.

3. **Updating Node Values**:

   - For each node, update its value to the difference from the previous level sum.
   - Maintain the cumulative sum of sibling values for the next level.

4. **Return the Modified Tree**:
   - After processing all levels, return the modified tree.

### Time Complexity

- **Time**: \(O(n)\) where \(n\) is the number of nodes in the binary tree.
- **Space**: \(O(w)\) where \(w\) is the maximum width of the binary tree (used for the queue).

### C++ Code Implementation

```cpp
class Solution {
public:
    TreeNode* replaceValueInTree(TreeNode* root) {
        // Return nullptr if the tree is empty
        if (!root) return nullptr;

        // Queue for level order traversal
        queue<TreeNode*> nodeQueue;
        nodeQueue.push(root);

        // Variable to store the cumulative sum of values at the previous level
        int previousLevelSum = root->val;

        // Process the tree level by level
        while (!nodeQueue.empty()) {
            int levelSize = nodeQueue.size(); // Number of nodes at the current level
            int currentLevelSum = 0; // Sum of all sibling nodes' values at the current level

            // Traverse all nodes at the current level
            for (int i = 0; i < levelSize; ++i) {
                TreeNode* currentNode = nodeQueue.front(); // Get the current node
                nodeQueue.pop(); // Remove the current node from the queue

                // Calculate the sum of values of the current node's children (siblings)
                int siblingValuesSum = (currentNode->left ? currentNode->left->val : 0) +
                                       (currentNode->right ? currentNode->right->val : 0);

                // Update the left child's value and add it to the queue if it exists
                if (currentNode->left) {
                    currentNode->left->val = siblingValuesSum; // Set the left child's value
                    nodeQueue.push(currentNode->left); // Enqueue the left child
                }

                // Update the right child's value and add it to the queue if it exists
                if (currentNode->right) {
                    currentNode->right->val = siblingValuesSum; // Set the right child's value
                    nodeQueue.push(currentNode->right); // Enqueue the right child
                }

                // Accumulate the current level's sibling values sum
                currentLevelSum += siblingValuesSum;

                // Update the current node's value to the difference from the previous level sum
                currentNode->val = previousLevelSum - currentNode->val;
            }

            // Update previousLevelSum for the next level
            previousLevelSum = currentLevelSum;
        }

        return root; // Return the modified tree
    }
};
```
