# 256. Binary Tree Right Side View

## Problem Link

[Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view)

## Approach 1: Level-Order Traversal

### Idea/Intuition

1. Perform a level-order traversal of the binary tree, maintaining a queue to process nodes level by level.
2. For each level, store the rightmost element (i.e., the last node at each level).
3. After completing the traversal, the rightmost nodes from each level will form the right side view.

### Algorithm

1. Initialize a queue for level-order traversal and a vector `levels` to store node values at each level.
2. While processing the queue, for each level, add the last node to the `rightViewResult`.
3. Return the `rightViewResult` after the traversal.

### Time Complexity

- **Traversal**: \(O(N)\), where \(N\) is the number of nodes.

### Space Complexity

- **Queue**: \(O(N)\) for level-order traversal.
- **Result Storage**: \(O(H)\), where \(H\) is the height of the tree (i.e., the number of levels).

### Implementation

```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        if (!root) return {}; // If the tree is empty, return an empty array

        vector<vector<int>> levels; // To store the level order traversal
        queue<TreeNode*> q;         // Queue for traversal
        q.push(root);

        // Perform level order traversal
        while (!q.empty()) {
            int size = q.size();
            vector<int> level; // Temporary vector for the current level

            for (int i = 0; i < size; ++i) {
                TreeNode* node = q.front();
                q.pop();

                level.push_back(node->val); // Add the node's value to the level

                // Enqueue left and right children if they exist
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            levels.push_back(level); // Add the current level to levels
        }

        // Extract the right view from levels
        vector<int> rightViewResult;
        for (const auto& level : levels) {
            rightViewResult.push_back(level.back()); // Last element of each level
        }
        return rightViewResult;
    }
};
```

## Approach 2: Depth-First Search (DFS)

### Idea/Intuition

1. Use a DFS traversal to explore the tree, but prioritize the right subtree before the left subtree.
2. For each level, record the first node encountered (i.e., the first node of the rightmost branch).
3. This approach ensures that the rightmost nodes are visited first at each level.

### Algorithm

1. Start from the root node and traverse the tree using DFS.
2. If a node at a particular level is encountered for the first time, add it to the result vector.
3. Traverse the right subtree before the left subtree to ensure the rightmost node is captured first.

### Time Complexity

- **Traversal**: \(O(N)\), where \(N\) is the number of nodes.

### Space Complexity

- **Recursion Stack**: \(O(H)\), where \(H\) is the height of the tree.
- **Result Storage**: \(O(H)\), where \(H\) is the height of the tree.

### Implementation

```cpp
class Solution {
public:
    void solve(TreeNode* root, vector<int>& ans, int level) {
        if (root == NULL) {
            return;
        }

        // Add the first node encountered at this level
        if (ans.size() == level) {
            ans.push_back(root->val);
        }

        // First traverse the right subtree, then the left subtree
        solve(root->right, ans, level + 1);
        solve(root->left, ans, level + 1);
    }

    vector<int> rightSideView(TreeNode* root) {
        if (root == NULL) {
            return {};
        }
        vector<int> ans;
        solve(root, ans, 0);
        return ans;
    }
};
```

# 257. Symmetric Tree

## Problem Link

[Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

## Approach 1: Level Order Traversal with Palindrome Check

### Idea/Intuition

1. Perform a level-order traversal of the tree, keeping track of each level's elements.
2. For each level, treat the elements as an array and check if the array is symmetric (palindromic).
3. Null nodes are treated as a special case, and we mark them with a distinct value (e.g., 101) to differentiate them from actual node values.

### Algorithm

1. Start by checking if the tree is empty or consists of a single node.
2. Use a queue to traverse the tree level by level.
3. For each level, store the node values in a temporary array. If any value at symmetric positions is different, return false.
4. Continue this process until all levels are checked. If no asymmetry is found, return true.

### Time Complexity

- **Traversal**: \(O(N)\), where \(N\) is the number of nodes.

### Space Complexity

- **Queue and Temporary Storage**: \(O(N)\), where \(N\) is the number of nodes.

### Implementation

```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* root) {

        queue<TreeNode*> que;

        if (root == NULL || (root->left == NULL && root->right == NULL)) {
            return true;
        }
        que.push(root->left);
        que.push(root->right);

        while (!que.empty()) {
            int n = que.size();
            vector<int> temp(n, 101); // Initialize the temp vector with 101 to handle null nodes
            for (int i = 0; i < n; i++) {
                TreeNode* current = que.front();
                que.pop();
                if (current != nullptr) {
                    temp[i] = current->val;
                    que.push(current->left);
                    que.push(current->right);
                } else {
                    temp[i] = 101; // Mark null nodes with 101
                }
            }
            int s = 0, e = n - 1;
            while (s < e) {
                if (temp[s] != temp[e]) {
                    return false;
                }
                s++;
                e--;
            }
        }
        return true;
    }
};
```

## Approach 2: Level Order Traversal with Pairwise Comparison

### Idea/Intuition

1. Perform a level-order traversal using a queue.
2. For each pair of nodes (left and right), check if both are `null` (symmetric), or if one is `null` and the other is not (not symmetric).
3. If both nodes are non-null, compare their values and enqueue their children in the order: left’s left with right’s right, and left’s right with right’s left.
4. If any pair is asymmetric, return false. Otherwise, the tree is symmetric.

### Algorithm

1. Start by checking if the tree is empty. If it is, return `true`.
2. Use a queue to traverse the tree level by level, enqueuing left and right children in pairs.
3. For each pair, check if both are `null` (continue), one is `null` (return false), or both are non-null (compare values and enqueue children in the correct order).
4. If the traversal completes without returning `false`, return `true` as the tree is symmetric.

### Time Complexity

- **Traversal**: \(O(N)\), where \(N\) is the number of nodes.

### Space Complexity

- **Queue**: \(O(N)\), where \(N\) is the number of nodes.

### Implementation

```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* root) {

        if (root == nullptr) {
            return true;
        }
        queue<TreeNode*> que;
        que.push(root->left);
        que.push(root->right);

        while (!que.empty()) {
            TreeNode* leftNode = que.front();
            que.pop();
            TreeNode* rightNode = que.front();
            que.pop();

            if (leftNode == nullptr && rightNode == nullptr) {
                continue;
            }
            if (leftNode == nullptr || rightNode == nullptr) {
                return false;
            }
            if (leftNode->val != rightNode->val) {
                return false;
            }

            que.push(leftNode->left);
            que.push(rightNode->right);
            que.push(leftNode->right);
            que.push(rightNode->left);
        }
        return true;
    }
};
```

# 258. Root to Leaf Paths

## Problem Link

[Root to Leaf Paths](https://www.geeksforgeeks.org/problems/root-to-leaf-paths/1)

## Approach: Depth-First Search (DFS)

### Idea/Intuition

1. We traverse the tree using DFS. At each node, we maintain a list of nodes visited so far in the path.
2. When we reach a leaf node (a node with no left or right child), we add the current path to the result.
3. For non-leaf nodes, we continue traversing both the left and right subtrees.
4. Backtrack by removing the current node from the path after exploring both subtrees.

### Algorithm

1. Define a helper function `dfs` that takes the current node, the path so far, and a list to store all paths.
2. If the node is `NULL`, return.
3. Add the current node to the path.
4. If the current node is a leaf node, add the path to the result.
5. Otherwise, recursively explore the left and right subtrees.
6. After exploring both subtrees, backtrack by removing the current node from the path.
7. The main function `Paths` initializes the result list and the current path, and calls `dfs` to collect all root-to-leaf paths.

### Time Complexity

- **DFS Traversal**: \(O(N)\), where \(N\) is the number of nodes in the tree, because each node is visited once.

### Space Complexity

- **Recursive Stack**: \(O(H)\), where \(H\) is the height of the tree (in the worst case, \(H = N\) for a skewed tree).
- **Result Storage**: \(O(N)\) for storing the paths.

### Implementation

```cpp
class Solution {
public:
    // Helper function to perform DFS and collect root-to-leaf paths
    void dfs(Node* root, vector<int>& currentPath, vector<vector<int>>& result) {
        // Base case: If root is NULL, return
        if (!root) return;

        // Add current node to the path
        currentPath.push_back(root->data);

        // If it's a leaf node, add the current path to the result
        if (!root->left && !root->right) {
            result.push_back(currentPath);
        } else {
            // Recur for left and right subtrees
            dfs(root->left, currentPath, result);
            dfs(root->right, currentPath, result);
        }

        // Backtrack: Remove the current node from the path
        currentPath.pop_back();
    }

    // Function to return a list of root-to-leaf paths
    vector<vector<int>> Paths(Node* root) {
        vector<vector<int>> result;  // To store all root-to-leaf paths
        vector<int> currentPath;     // To store the current path

        dfs(root, currentPath, result);

        return result;
    }
};
```
