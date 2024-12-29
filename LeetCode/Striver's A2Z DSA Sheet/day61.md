# 277. Insert into a Binary Search Tree

## Problem Link

[Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree)

## Approach 1: Recursive Insertion

### Idea/Intuition

The task is to insert a new value into a Binary Search Tree (BST) while maintaining its properties:

1. All values in the left subtree of a node are smaller than the node's value.
2. All values in the right subtree of a node are greater than the node's value.

The recursive approach leverages these properties to find the correct position for the new node:

- Recur into the left subtree if the new value is less than the current node's value.
- Recur into the right subtree if the new value is greater than the current node's value.
- Once we reach a `nullptr`, create a new node and attach it to the tree.

### Algorithm

1. If the `root` is `nullptr`, create a new node with the given value and return it.
2. Compare the value with the current node's value:
   - If `val < root->val`, recursively call the function for the left subtree.
   - If `val > root->val`, recursively call the function for the right subtree.
3. After insertion, return the unchanged root node.

### Time Complexity

- **O(h)**: `h` is the height of the tree. In the worst case, this is `O(n)` for a skewed tree and `O(log n)` for a balanced tree.

### Space Complexity

- **O(h)**: Due to the recursion stack, where `h` is the height of the tree.

### Implementation

```cpp
class Solution {
public:
    // Recursive Approach
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (root == nullptr) {
            // Create a new node and return it as the root/subtree
            return new TreeNode(val);
        }

        if (val < root->val) {
            // Recur into the left subtree
            root->left = insertIntoBST(root->left, val);
        } else {
            // Recur into the right subtree
            root->right = insertIntoBST(root->right, val);
        }

        return root; // Return the unchanged root node
    }
};
```

## Approach 2: Iterative Insertion

### Idea/Intuition

Instead of recursion, we can use an iterative approach to find the correct position for the new value:

1. Traverse the tree starting from the root.
2. Depending on the comparison of the new value with the current node's value:
   - Move to the left child if the value is smaller.
   - Move to the right child if the value is larger.
3. When a `nullptr` child is reached, create a new node and attach it as the left or right child of the current node.

### Algorithm

1. If the `root` is `nullptr`, create a new node and return it.
2. Use a pointer (`current`) to traverse the tree starting from the root.
3. In a loop:
   - If `val < current->val`, check if the left child is `nullptr`. If so, insert the new node there; otherwise, move to the left child.
   - If `val > current->val`, check if the right child is `nullptr`. If so, insert the new node there; otherwise, move to the right child.
4. After insertion, return the unchanged root.

### Time Complexity

- **O(h)**: `h` is the height of the tree. In the worst case, this is `O(n)` for a skewed tree and `O(log n)` for a balanced tree.

### Space Complexity

- **O(1)**: No recursion stack is used.

### Implementation

```cpp
class Solution {
public:
    // Iterative Approach
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (root == nullptr) {
            return new TreeNode(val);
        }

        TreeNode* current = root;
        while (true) {
            if (val < current->val) {
                if (current->left == nullptr) {
                    current->left = new TreeNode(val);
                    break;
                }
                current = current->left;
            } else {
                if (current->right == nullptr) {
                    current->right = new TreeNode(val);
                    break;
                }
                current = current->right;
            }
        }

        return root; // Return the unchanged root node
    }
};
```

# 278. Delete Node in a BST

## Problem Link

[Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/description/)

## Approach: Recursive Deletion

### Idea/Intuition

The process of deleting a node from a Binary Search Tree (BST) involves three primary cases:

1. **Node to delete is not found**: Return the tree as is.
2. **Node to delete has no children or one child**: Replace the node with its child (or null if it has no children).
3. **Node to delete has two children**:
   - Find the **in-order successor** (smallest node in the right subtree).
   - Replace the node's value with the in-order successor's value.
   - Recursively delete the in-order successor.

### Algorithm

1. Start at the root and search for the node with the given `key`.
2. Depending on the value of `key`:
   - If `key < root->val`, recurse into the left subtree.
   - If `key > root->val`, recurse into the right subtree.
3. When the node to delete is found:
   - If it has no children, return `nullptr`.
   - If it has one child, return that child.
   - If it has two children:
     - Find the in-order successor.
     - Replace the node's value with the in-order successor's value.
     - Recur into the right subtree to delete the in-order successor.
4. Return the modified tree.

### Time Complexity

- **O(h)**: Where `h` is the height of the tree. In the worst case, this is `O(n)` for a skewed tree and `O(log n)` for a balanced tree.

### Space Complexity

- **O(h)**: Recursion stack space.

### Implementation

```cpp
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (root == nullptr) {
            // Node not found, return the root as is
            return root;
        }

        // Search for the node to delete
        if (key < root->val) {
            root->left = deleteNode(root->left, key);
        } else if (key > root->val) {
            root->right = deleteNode(root->right, key);
        } else {
            // Node to delete is found

            // Case 1: Node has no children or only one child
            if (root->left == nullptr) {
                TreeNode* temp = root->right;
                delete root;
                return temp;
            } else if (root->right == nullptr) {
                TreeNode* temp = root->left;
                delete root;
                return temp;
            }

            // Case 2: Node has two children
            // Find the in-order successor (smallest node in the right subtree)
            TreeNode* successor = getMinNode(root->right);
            // Replace the value of the node to delete with the successor's value
            root->val = successor->val;
            // Delete the successor
            root->right = deleteNode(root->right, successor->val);
        }

        return root;
    }

private:
    TreeNode* getMinNode(TreeNode* node) {
        while (node->left != nullptr) {
            node = node->left;
        }
        return node;
    }
};
```

# 279. Kth Smallest Element in a BST

## Problem Link

[Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/)

## Approach 1: Inorder Traversal and Array Storage

### Idea/Intuition

In a Binary Search Tree (BST), an **inorder traversal** yields the nodes in sorted order. We can perform an inorder traversal to collect all the elements in a vector, and then directly access the `(k-1)`th element as the k-th smallest element.

### Algorithm

1. Initialize an empty vector `elements` to store the inorder traversal.
2. Perform an inorder traversal:
   - Recur into the left subtree.
   - Add the current node's value to the vector.
   - Recur into the right subtree.
3. After the traversal, return `elements[k-1]` (0-based indexing).
4. This ensures we retrieve the k-th smallest element directly.

### Time Complexity

- **O(n)**: The inorder traversal visits all `n` nodes.
- Accessing the k-th element is **O(1)**.

### Space Complexity

- **O(n)**: Space required to store the inorder traversal.

### Implementation

```cpp
class Solution {
public:
    void inorderTraversal(TreeNode* node, vector<int>& elements) {
        if (node == nullptr) {
            return;
        }
        // Traverse the left subtree
        inorderTraversal(node->left, elements);
        // Add the current node's value
        elements.push_back(node->val);
        // Traverse the right subtree
        inorderTraversal(node->right, elements);
    }

    int kthSmallest(TreeNode* root, int k) {
        vector<int> inorderElements;
        inorderTraversal(root, inorderElements);
        // The k-th smallest element is at index k - 1 in the sorted array
        return inorderElements[k - 1];
    }
};
```

## Approach 2: Inorder Traversal with Early Termination

### Idea/Intuition

An **inorder traversal** of a BST gives nodes in sorted order. Instead of storing all elements in a vector, we count nodes during the traversal and terminate as soon as the k-th node is found, reducing space usage.

### Algorithm

1. Define a helper function `solve` to perform the inorder traversal.
2. Use a counter `count` to track the number of nodes visited.
3. During the traversal:
   - Recur into the left subtree.
   - Increment the counter when visiting a node.
   - If the counter matches `k`, return the current node's value.
   - Recur into the right subtree.
4. Return the value of the k-th smallest element.

### Time Complexity

- **O(n)**: In the worst case, we may traverse all `n` nodes.

### Space Complexity

- **O(h)**: Space required for the recursion stack, where `h` is the height of the BST.

### Implementation

```cpp
class Solution {
public:
    int solve(TreeNode* root, int &count, int k) {
        // Base case
        if (root == nullptr) {
            return -1;
        }

        // Traverse the left subtree
        int left = solve(root->left, count, k);
        if (left != -1) {
            return left;
        }

        // Visit the current node
        count++;
        if (count == k) {
            return root->val;
        }

        // Traverse the right subtree
        return solve(root->right, count, k);
    }

    int kthSmallest(TreeNode* root, int k) {
        int count = 0;
        return solve(root, count, k);
    }
};
```
