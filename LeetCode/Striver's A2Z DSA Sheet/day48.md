# 236. Binary Tree Preorder Traversal

## Problem Link

[LeetCode - Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)

## Recursive Preorder Traversal

### Idea/Intuition

This approach uses recursion to traverse the binary tree in preorder (root -> left -> right). A helper function is used to perform the traversal and store the node values in a vector.

### Algorithm

1. **Base Case**:

   - If the current node is `NULL`, return from the function.

2. **Visit Root Node**:

   - Add the value of the current node to the result vector.

3. **Recur for Left Subtree**:

   - Call the function recursively for the left child.

4. **Recur for Right Subtree**:
   - Call the function recursively for the right child.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the binary tree. Each node is visited once.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the binary tree. This is due to the recursive stack.

### Implementation

```cpp
class Solution {
public:
    void solve(vector<int>& ans, TreeNode* root) {
        if (root == NULL) {
            return;
        }
        ans.push_back(root->val);
        solve(ans, root->left);
        solve(ans, root->right);
    }

    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ans;
        solve(ans, root);
        return ans;
    }
};
```

# 237. Binary Tree Inorder Traversal

## Problem Link

[LeetCode - Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)

## Recursive Inorder Traversal

### Idea/Intuition

This approach uses recursion to traverse the binary tree in inorder (left -> root -> right). A helper function is used to perform the traversal and store the node values in a vector.

### Algorithm

1. **Base Case**:

   - If the current node is `NULL`, return from the function.

2. **Recur for Left Subtree**:

   - Call the function recursively for the left child.

3. **Visit Root Node**:

   - Add the value of the current node to the result vector.

4. **Recur for Right Subtree**:
   - Call the function recursively for the right child.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the binary tree. Each node is visited once.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the binary tree. This is due to the recursive stack.

### Implementation

```cpp
class Solution {
public:
    void solve(vector<int>& ans, TreeNode* root) {
        if (root == NULL) {
            return;
        }
        solve(ans, root->left);
        ans.push_back(root->val);
        solve(ans, root->right);
    }

    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        solve(ans, root);
        return ans;
    }
};
```

# 238. Binary Tree Inorder Traversal

## Problem Link

[LeetCode - Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)

## Recursive Inorder Traversal

### Idea/Intuition

This approach uses recursion to traverse the binary tree in inorder (left -> root -> right). A helper function is used to perform the traversal and store the node values in a vector.

### Algorithm

1. **Base Case**:

   - If the current node is `NULL`, return from the function.

2. **Recur for Left Subtree**:

   - Call the function recursively for the left child.

3. **Visit Root Node**:

   - Add the value of the current node to the result vector.

4. **Recur for Right Subtree**:
   - Call the function recursively for the right child.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the binary tree. Each node is visited once.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the binary tree. This is due to the recursive stack.

### Implementation

```cpp
class Solution {
public:
    void solve(vector<int>& ans, TreeNode* root) {
        if (root == NULL) {
            return;
        }
        solve(ans, root->left);
        ans.push_back(root->val);
        solve(ans, root->right);
    }

    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        solve(ans, root);
        return ans;
    }
};
```

# 239. Tree Traversal

## Problem Link

[Naukri Code360 - Tree Traversal](https://www.naukri.com/code360/problems/tree-traversal_981269?)

## Pre-order, In-order, and Post-order Traversal

### Idea/Intuition

This approach performs three different tree traversals: pre-order, in-order, and post-order. Each traversal is implemented using a helper function that recursively visits nodes in the respective order.

### Algorithm

1. **Pre-order Traversal**:

   - Visit the root node first.
   - Recursively traverse the left subtree.
   - Recursively traverse the right subtree.

2. **In-order Traversal**:

   - Recursively traverse the left subtree.
   - Visit the root node.
   - Recursively traverse the right subtree.

3. **Post-order Traversal**:

   - Recursively traverse the left subtree.
   - Recursively traverse the right subtree.
   - Visit the root node.

4. **Return Results**:
   - Perform all three traversals and store their results in separate vectors.
   - Return the results as a vector of vectors.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the tree. Each node is visited once for each traversal.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the tree, due to the recursive call stack.

### Implementation

```cpp
class Solution {
public:
    // Helper function to perform Pre-order traversal
    void preOrder(TreeNode* node, vector<int>& result) {
        if (node == NULL) return;
        result.push_back(node->data);        // Visit root
        preOrder(node->left, result);        // Traverse left subtree
        preOrder(node->right, result);       // Traverse right subtree
    }

    // Helper function to perform In-order traversal
    void inOrder(TreeNode* node, vector<int>& result) {
        if (node == NULL) return;
        inOrder(node->left, result);         // Traverse left subtree
        result.push_back(node->data);        // Visit root
        inOrder(node->right, result);        // Traverse right subtree
    }

    // Helper function to perform Post-order traversal
    void postOrder(TreeNode* node, vector<int>& result) {
        if (node == NULL) return;
        postOrder(node->left, result);       // Traverse left subtree
        postOrder(node->right, result);      // Traverse right subtree
        result.push_back(node->data);        // Visit root
    }

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

# 240. Binary Tree Level Order Traversal

## Problem Link

[LeetCode - Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)

## Level Order Traversal Using Queue

### Idea/Intuition

This approach uses a breadth-first search (BFS) algorithm to traverse the binary tree level by level. A queue is utilized to process nodes from left to right, pushing the children of each node into the queue for the subsequent level.

### Algorithm

1. **Check for Empty Tree**:
   - If the root is `NULL`, return an empty list.
2. **Initialize Queue**:

   - Use a queue to hold nodes level by level. Start by pushing the root node into the queue.

3. **Process Nodes Level by Level**:

   - While the queue is not empty:
     - Get the size of the current level (number of nodes to process).
     - For each node at the current level, remove it from the queue, add its value to the result, and enqueue its left and right children if they exist.

4. **Repeat**:
   - Continue this process until all nodes are processed.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the binary tree. Every node is visited once.

### Space Complexity

- \(O(N)\), where \(N\) is the number of nodes in the binary tree. This is the space required for the queue to hold nodes at each level.

### Implementation

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if (root == NULL) {
            return ans;
        }

        queue<TreeNode*> que;
        que.push(root);
        while (!que.empty()) {
            int n = que.size();
            vector<int> temp;
            for (int i = 0; i < n; i++) {
                TreeNode* frnt = que.front();
                temp.push_back(frnt->val);
                que.pop();
                if (frnt->left) {
                    que.push(frnt->left);
                }
                if (frnt->right) {
                    que.push(frnt->right);
                }
            }
            ans.push_back(temp);
        }
        return ans;
    }
};
```
