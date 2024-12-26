# 250. Same Tree

## Problem Link

[LeetCode - Same Tree](https://leetcode.com/problems/same-tree/description/)

## Approach: Recursive Comparison

### Idea/Intuition

The problem can be solved by recursively comparing the two trees:

1. If both nodes are `NULL`, they are the same.
2. If one node is `NULL` and the other is not, the trees are not the same.
3. If both nodes are non-`NULL`, recursively compare their left and right subtrees, ensuring their values are also equal.

### Algorithm

1. Define a recursive function `isSameTree` that takes two nodes as arguments.
2. If both nodes are `NULL`, return `true`.
3. If one of the nodes is `NULL` and the other is not, return `false`.
4. Recursively check the left and right subtrees:
   - Compare the current node values.
   - Combine the results of the left and right subtree comparisons with a logical `AND`.

### Time Complexity

- \(O(N)\), where \(N\) is the total number of nodes in the smaller of the two trees. Each node is visited once.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the tree. This is the space used by the recursion stack.

### Implementation

```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        // If both nodes are NULL, they are the same
        if (p == NULL && q == NULL) {
            return true;
        }

        // If one node is NULL and the other is not, they are not the same
        if ((p == NULL && q != NULL) || (p != NULL && q == NULL)) {
            return false;
        }

        // Recursively check the left and right subtrees
        bool leftAns = isSameTree(p->left, q->left);
        bool rightAns = isSameTree(p->right, q->right);

        // Combine the results and compare current node values
        return leftAns && rightAns && (p->val == q->val);
    }
};
``
```

# 251. Binary Tree Zigzag Level Order Traversal

## Problem Link

[LeetCode - Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/)

## Approach: Level Order Traversal with Directional Flag

### Idea/Intuition

1. Use a queue to perform a level order traversal of the binary tree.
2. At each level, alternate the traversal direction between left-to-right and right-to-left using a flag.
3. Use an array to store nodes of the current level in the required order.
4. Push the child nodes of the current level into the queue for processing the next level.

### Algorithm

1. Initialize an empty queue and push the root node into it.
2. Use a boolean flag to track the traversal direction:
   - `true`: left-to-right.
   - `false`: right-to-left.
3. While the queue is not empty:
   - Compute the number of nodes at the current level.
   - Use a temporary array to store node values at the current level in the desired order.
   - Pop nodes from the queue, insert their values into the temporary array, and push their child nodes into the queue.
4. Add the temporary array to the result and toggle the flag for the next level.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the binary tree. Each node is visited once.

### Space Complexity

- \(O(N)\), for the result storage and queue.

### Implementation

```cpp
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if (root == NULL) {
            return {};
        }

        queue<TreeNode*> que;
        que.push(root);
        // Flag: 1 for left-to-right, 0 for right-to-left
        bool flag = 1;

        while (!que.empty()) {
            int n = que.size();
            vector<int> temp(n);

            for (int i = 0; i < n; i++) {
                int index = flag ? i : n - i - 1; // Determine insertion index
                TreeNode* frnt = que.front();
                que.pop();

                temp[index] = frnt->val;

                // Push child nodes into the queue
                if (frnt->left) {
                    que.push(frnt->left);
                }
                if (frnt->right) {
                    que.push(frnt->right);
                }
            }

            // Add the current level to the result
            ans.push_back(temp);

            // Toggle the traversal direction
            flag = !flag;
        }

        return ans;
    }
};
```

# 252. Boundary Traversal of a Binary Tree

## Problem Link

[Boundary Traversal](https://www.naukri.com/code360/problems/boundary-traversal_790725)

## Approach: Divide the Boundary into Three Parts

### Idea/Intuition

1. **Left Boundary**: Traverse from the root's left child down to the bottom-left leaf node, excluding leaf nodes.
2. **Leaf Nodes**: Collect all the leaf nodes in a left-to-right order.
3. **Right Boundary**: Traverse from the root's right child down to the bottom-right leaf node, excluding leaf nodes, in reverse order.

### Algorithm

1. **Check Leaf**: A helper function to determine if a node is a leaf.
2. **Left Boundary**:
   - Traverse the left boundary, adding nodes to the result, excluding leaves.
3. **Leaf Nodes**:
   - Traverse the entire tree to collect leaf nodes using a recursive helper function.
4. **Right Boundary**:
   - Traverse the right boundary, collect nodes, and reverse their order before adding them to the result.
5. Combine the results of the above three steps in order: root, left boundary, leaf nodes, and right boundary.

### Time Complexity

- \(O(N)\), where \(N\) is the number of nodes in the binary tree. Each node is visited once.

### Space Complexity

- \(O(H)\), where \(H\) is the height of the binary tree, for recursion stack space.

### Implementation

```cpp
#include <bits/stdc++.h>
using namespace std;

// TreeNode structure
template <typename T>
struct TreeNode {
    T data;
    TreeNode* left;
    TreeNode* right;
    TreeNode(T x) : data(x), left(NULL), right(NULL) {}
};

// Helper function to check if a node is a leaf node
bool isLeaf(TreeNode<int>* node) {
    return node != NULL && node->left == NULL && node->right == NULL;
}

// Function to add left boundary nodes
void addLeftBoundary(TreeNode<int>* node, vector<int>& result) {
    while (node != NULL) {
        if (!isLeaf(node)) {
            result.push_back(node->data);
        }
        if (node->left != NULL) {
            node = node->left;
        } else {
            node = node->right;
        }
    }
}

// Function to add leaf nodes (in left-to-right order)
void addLeafNodes(TreeNode<int>* node, vector<int>& result) {
    if (node == NULL) return;

    if (isLeaf(node)) {
        result.push_back(node->data);
    } else {
        addLeafNodes(node->left, result);
        addLeafNodes(node->right, result);
    }
}

// Function to add right boundary nodes in reverse order
void addRightBoundary(TreeNode<int>* node, vector<int>& result) {
    vector<int> temp;
    while (node != NULL) {
        if (!isLeaf(node)) {
            temp.push_back(node->data);
        }
        if (node->right != NULL) {
            node = node->right;
        } else {
            node = node->left;
        }
    }

    // Add right boundary in reverse order
    reverse(temp.begin(), temp.end());
    result.insert(result.end(), temp.begin(), temp.end());
}

// Main function to traverse boundary nodes
vector<int> traverseBoundary(TreeNode<int>* root) {
    vector<int> result;

    if (root == NULL) return result;  // If the tree is empty, return an empty result

    // 1. Add the root node
    result.push_back(root->data);

    // 2. Add the left boundary excluding leaf nodes
    if (root->left != NULL) {
        addLeftBoundary(root->left, result);
    }

    // 3. Add all the leaf nodes
    addLeafNodes(root, result);

    // 4. Add the right boundary excluding leaf nodes, in reverse order
    if (root->right != NULL) {
        addRightBoundary(root->right, result);
    }

    return result;
}
```
