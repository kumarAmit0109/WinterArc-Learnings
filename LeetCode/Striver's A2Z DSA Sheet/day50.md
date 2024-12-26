# 244. Binary Tree Postorder Traversal (one Stack)

## Problem Link

[LeetCode - Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Approach 1: Iterative Postorder Traversal with One Stack

### Idea/Intuition

This approach uses a single stack and a `prev` pointer to traverse the binary tree in postorder (left-right-root). We move down the tree and push the nodes onto the stack. When we reach a leaf node or return from a child, we process the node and move back up the tree.

### Algorithm

1. **Edge Case**:

   - If the tree is empty (`root == nullptr`), return an empty vector.

2. **Traversing the Tree**:

   - Push the root onto the stack.
   - At each step, if the current node has no children or both its children have already been processed, visit it and pop it from the stack.
   - Otherwise, push the right child first, then the left child, so that we can visit the left child before the right child in postorder.

3. **Process Nodes**:
   - When the node is ready to be visited, add its value to the result vector.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the binary tree. Each node is processed once.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the binary tree. This is due to the space used by the stack.

### Implementation

```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        if (root == nullptr) return {};  // Edge case: empty tree

        stack<TreeNode*> st;
        vector<int> result;
        TreeNode* prev = nullptr;

        st.push(root);  // Start with the root node

        while (!st.empty()) {
            TreeNode* curr = st.top();

            // Check if we're moving down the tree or returning from a child
            if (!curr->left && !curr->right ||
                prev && (prev == curr->left || prev == curr->right)) {
                // If no children or we've already processed the children, visit the node
                result.push_back(curr->val);
                st.pop();
                prev = curr;
            } else {
                // Otherwise, go down the tree, visiting right child first
                if (curr->right) st.push(curr->right);
                if (curr->left) st.push(curr->left);
            }
        }

        return result;
    }
};
```

## Approach 2: Iterative Postorder Traversal with One Stack

### Idea/Intuition

This approach uses a single stack to simulate postorder traversal. We push nodes onto the stack, traverse left, and only visit the node when its right child has been processed.

### Algorithm

1. **Edge Case**:

   - If the tree is empty (`root == nullptr`), return an empty vector.

2. **First Stack**:
   - Traverse the tree, pushing nodes onto the stack and moving to the left child.
3. **Processing Nodes**:

   - If the current node's right child is `nullptr`, visit the current node and add its value to the result vector.
   - If the right child exists, push it onto the stack and continue to its left child.

4. **Continue the Process**:
   - Continue processing the stack, popping nodes in postorder as the traversal completes.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the binary tree. Each node is processed once.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the binary tree. This is due to the space used by the stack.

### Implementation

```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> postorder;
        if (!root) return postorder;

        stack<TreeNode*> st;
        TreeNode* curr = root;

        while (curr != nullptr || !st.empty()) {
            if (curr != nullptr) {
                // Push current node to the stack and move to its left child
                st.push(curr);
                curr = curr->left;
            } else {
                // Peek the top node of the stack
                TreeNode* temp = st.top()->right;
                if (temp == nullptr) {
                    // If the right child is null, process the current node
                    temp = st.top();
                    st.pop();
                    postorder.push_back(temp->val);

                    // Check if the node on top of the stack has the current node as its right child
                    while (!st.empty() && temp == st.top()->right) {
                        temp = st.top();
                        st.pop();
                        postorder.push_back(temp->val);
                    }
                } else {
                    // Move to the right child
                    curr = temp;
                }
            }
        }

        return postorder;
    }
};
```

# 245. Tree Traversals (Pre-order, In-order, Post-order)

## Problem Link

[Naukri - Tree Traversals](https://www.naukri.com/code360/problems/tree-traversals_981269?)

## Approach: Iterative Traversal

### Idea/Intuition

The goal is to perform Pre-order, In-order, and Post-order tree traversals iteratively using stacks. Each traversal uses its specific method to handle nodes, ensuring that the traversal order is respected.

### Algorithm

1. **Pre-order Traversal**:

   - Visit the root first, then recursively traverse the left subtree and then the right subtree. We simulate the recursive approach iteratively using a stack. The right child is pushed first so that the left child is processed first.

2. **In-order Traversal**:

   - Traverse the left subtree, visit the root, then traverse the right subtree. We simulate this process iteratively by pushing nodes to the stack until reaching the leftmost node. Once the leftmost node is reached, we visit it, and then move to its right subtree.

3. **Post-order Traversal**:

   - Traverse the left subtree, then the right subtree, and finally visit the root. This is the most complex traversal because we need to track when we are returning from the left or right subtree. We use a stack to simulate post-order traversal iteratively.

4. **Return All Three Traversals**:
   - The function `getTreeTraversal` performs all three traversals and returns the results as a vector of vectors.

### Time Complexity

- **Pre-order**: \(O(N)\) where \(N\) is the number of nodes in the tree.
- **In-order**: \(O(N)\) where \(N\) is the number of nodes in the tree.
- **Post-order**: \(O(N)\) where \(N\) is the number of nodes in the tree.

### Space Complexity

- **Pre-order**: \(O(H)\) where \(H\) is the height of the tree, for storing the stack.
- **In-order**: \(O(H)\) where \(H\) is the height of the tree, for storing the stack.
- **Post-order**: \(O(H)\) where \(H\) is the height of the tree, for storing the stack.

### Implementation

```cpp
class Solution {
public:
    // Iterative Pre-order traversal
    void preOrder(TreeNode* root, vector<int>& result) {
        if (root == NULL) return;
        stack<TreeNode*> st;
        st.push(root);

        while (!st.empty()) {
            TreeNode* node = st.top();
            st.pop();
            result.push_back(node->data);  // Visit root

            // Push right child first so that left child is processed first
            if (node->right) st.push(node->right);
            if (node->left) st.push(node->left);
        }
    }

    // Iterative In-order traversal
    void inOrder(TreeNode* root, vector<int>& result) {
        stack<TreeNode*> st;
        TreeNode* node = root;

        while (node != NULL || !st.empty()) {
            // Traverse to the leftmost node
            while (node != NULL) {
                st.push(node);
                node = node->left;
            }

            // Process the node
            node = st.top();
            st.pop();
            result.push_back(node->data);

            // Move to the right child
            node = node->right;
        }
    }

    // Iterative Post-order traversal using one stack
    void postOrder(TreeNode* root, vector<int>& result) {
        if (root == NULL) return;

        stack<TreeNode*> st;
        TreeNode* prev = NULL;
        st.push(root);

        while (!st.empty()) {
            TreeNode* curr = st.top();

            // Traverse the tree: post-order requires checking if both children are processed
            if (prev == NULL || prev->left == curr || prev->right == curr) {
                // Going down the tree: push left and right children if available
                if (curr->left) st.push(curr->left);
                else if (curr->right) st.push(curr->right);
            }
            // If we are coming back from the left or right child, process the node
            else if (curr->left == prev) {
                if (curr->right) st.push(curr->right);
            } else {
                result.push_back(curr->data);  // Visit root
                st.pop();
            }

            prev = curr;
        }
    }

    // Function to return all three traversals as a vector of vectors
    vector<vector<int>> getTreeTraversal(TreeNode* root) {
        vector<int> inOrderResult, preOrderResult, postOrderResult;

        // Perform the three traversals
        preOrder(root, preOrderResult);
        inOrder(root, inOrderResult);
        postOrder(root, postOrderResult);

        // Return the results as a vector of vectors
        return {inOrderResult, preOrderResult, postOrderResult};
    }
};
```

# 246. Maximum Depth of Binary Tree

## Problem Link

[LeetCode - Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)

## Approach: Recursive Depth Calculation

### Idea/Intuition

The maximum depth of a binary tree can be determined using a recursive approach:

1. For any node, the depth is the maximum of the depths of its left and right subtrees plus 1 (to account for the current node).
2. If the tree is empty, the depth is 0.

### Algorithm

1. Define a recursive function `solve`:

   - If the current node is `NULL`, return 0.
   - Recursively compute the depth of the left and right subtrees.
   - Return `max(left, right) + 1`, where `left` and `right` are the depths of the left and right subtrees.

2. Use the `solve` function in `maxDepth` to compute the depth of the binary tree starting from the root.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the tree. Each node is visited once.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the tree. This is the space used by the recursion stack.

### Implementation

```cpp
class Solution {
public:
    // Recursive function to calculate depth
    int solve(TreeNode* root) {
        if (root == NULL) {
            return 0; // Base case: empty tree has depth 0
        }
        // Recursively find the depth of left and right subtrees
        int left = solve(root->left);
        int right = solve(root->right);
        // Return the maximum depth between left and right subtrees plus 1
        return max(left, right) + 1;
    }

    int maxDepth(TreeNode* root) {
        return solve(root); // Start the recursion from the root
    }
};
```
