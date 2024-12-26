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
