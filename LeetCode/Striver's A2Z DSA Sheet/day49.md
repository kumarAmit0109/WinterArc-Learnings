# 241. Binary Tree Preorder Traversal

## Problem Link

[LeetCode - Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)

## Iterative Preorder Traversal Using Stack

### Idea/Intuition

This approach uses an explicit stack to simulate the recursive call stack and perform the preorder traversal iteratively. The root node is processed first, followed by its left and right children.

### Algorithm

1. **Check for Empty Tree**:
   - If the root is `NULL`, return an empty list.
2. **Initialize Stack**:

   - Use a stack to hold nodes while traversing. Push the root node onto the stack.

3. **Process Nodes**:

   - While the stack is not empty:
     - Pop the node from the stack and process it by adding its value to the result.
     - Push the right child onto the stack, followed by the left child (so that the left child is processed first).

4. **Repeat**:
   - Continue this process until the stack is empty.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the binary tree. Each node is processed once.

### Space Complexity

- \(O(N)\), where \(N\) is the number of nodes in the binary tree. This is the space required for the stack in the worst case.

### Implementation

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ans;
        if (root == NULL) {
            return ans;
        }
        stack<TreeNode*> st;
        st.push(root);
        while (!st.empty()) {
            TreeNode* temp = st.top();
            st.pop();
            ans.push_back(temp->val);
            if (temp->right) {
                st.push(temp->right);
            }
            if (temp->left) {
                st.push(temp->left);
            }
        }
        return ans;
    }
};
```

# 242. Binary Tree Inorder Traversal

## Problem Link

[LeetCode - Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## Approach 1: Iterative Inorder Traversal Using Stack (Method 1)

### Idea/Intuition

In this approach, we use an explicit stack to simulate the recursive call stack and perform the inorder traversal iteratively. We first traverse all the way to the leftmost node, push each node onto the stack, and then process the node and move to the right subtree.

### Algorithm

1. **Traverse Left Subtree**:
   - Push all nodes along the left subtree onto the stack until we reach a `NULL` node.
2. **Process Node**:

   - Pop the node from the stack, process it by adding its value to the result, and move to its right subtree.

3. **Repeat**:
   - Continue this process until the stack is empty and we’ve processed all nodes.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the binary tree. Each node is processed once.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the binary tree. This is due to the space required for the stack.

### Implementation

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;  // Stack to keep track of nodes
        vector<int> ans;      // Vector to store the result
        TreeNode* temp = root;  // Start from the root node

        // Iterate as long as there are nodes to process
        while (temp != NULL || !st.empty()) {
            // Traverse the leftmost path and push nodes to the stack
            while (temp != NULL) {
                st.push(temp);
                temp = temp->left;
            }

            // Process the node at the top of the stack
            temp = st.top();
            st.pop();
            ans.push_back(temp->val);  // Add the node's value to the result

            // Move to the right child
            temp = temp->right;
        }

        return ans;
    }
};

```

## Approach 2: Iterative Inorder Traversal Using Stack (Method 2)

### Idea/Intuition

In this iterative approach, we use a stack to simulate the recursive calls. We traverse the left subtree first, push all nodes along the way to the stack, and process the nodes by popping from the stack and moving to the right subtree.

### Algorithm

1. **Traverse Left Subtree**:

   - Push all nodes along the left subtree onto the stack until we reach a `NULL` node.

2. **Process Node**:

   - Pop the node from the stack, process it by adding its value to the result, and move to its right subtree.

3. **Repeat**:
   - Continue this process until we encounter an empty stack and finish the traversal.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the binary tree. Each node is processed once.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the binary tree. This is due to the space required for the stack.

### Implementation

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;  // Stack to keep track of nodes
        TreeNode* node = root;  // Start from the root node
        vector<int> ans;  // Vector to store the result

        while (true) {
            if (node != NULL) {
                st.push(node);  // Push the current node to the stack
                node = node->left;  // Move to the left child
            } else {
                if (st.empty()) break;  // Break when the stack is empty

                node = st.top();  // Pop the node from the stack
                st.pop();
                ans.push_back(node->val);  // Add the node's value to the result
                node = node->right;  // Move to the right child
            }
        }

        return ans;
    }
};
```

# 243. Binary Tree Postorder Traversal

## Problem Link

[LeetCode - Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Approach: Two Stack Approach

### Idea/Intuition

This approach uses two stacks to simulate the postorder traversal of a binary tree. We first traverse the tree in a modified preorder manner, pushing nodes onto the first stack. Then, we process the nodes in postorder by popping from the second stack.

### Algorithm

1. **First Stack**:

   - Push the root node to the first stack.
   - While the first stack is not empty, pop the top node, push it to the second stack, and push its left and right children (if any) to the first stack.

2. **Second Stack**:
   - After processing all nodes in the first stack, the second stack contains the nodes in reverse postorder. Pop the nodes from the second stack and add their values to the result.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the binary tree. Each node is processed once.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the binary tree. This is due to the space used by the two stacks.

### Implementation

```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> ans;  // Vector to store the postorder traversal result
        if (root == NULL) {  // If the tree is empty, return an empty vector
            return ans;
        }

        stack<TreeNode*> st1, st2;  // Two stacks to simulate postorder traversal
        st1.push(root);  // Push the root to the first stack

        // Traverse the tree using the first stack
        while (!st1.empty()) {
            root = st1.top();
            st1.pop();  // Pop the node from the first stack
            st2.push(root);  // Push the node to the second stack

            // Push left and right children to the first stack
            if (root->left != NULL) {
                st1.push(root->left);
            }
            if (root->right != NULL) {
                st1.push(root->right);
            }
        }

        // Pop nodes from the second stack to get the postorder traversal
        while (!st2.empty()) {
            ans.push_back(st2.top()->val);
            st2.pop();
        }

        return ans;
    }
};
```
