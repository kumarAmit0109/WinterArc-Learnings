# 280. Validate Binary Search Tree

## Problem Link

[Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree)

## Approach 1: Inorder Traversal and Check Sorted Order

### Idea/Intuition

The inorder traversal of a Binary Search Tree (BST) results in a sorted sequence of node values. This property can be used to validate whether a given binary tree is a BST:

- Perform an inorder traversal to collect the node values in a list.
- Verify if the list is sorted in strictly increasing order.

### Algorithm

1. Perform an inorder traversal of the tree and store the node values in a list.
2. Check if the list is sorted in strictly increasing order:
   - Compare each element with the previous one.
   - If any element is less than or equal to the previous element, the tree is not a valid BST.
3. Return `true` if the list is sorted, otherwise return `false`.

### Time Complexity

- **O(n)**: Traverses all `n` nodes of the tree once during the inorder traversal.

### Space Complexity

- **O(n)**: Stores the node values of the tree in a list.

### Implementation

```cpp
class Solution {
public:
    void inorder(TreeNode* node, vector<int>& inorderTraversal) {
        if (!node) return; // Base case: null node
        inorder(node->left, inorderTraversal); // Traverse the left subtree
        inorderTraversal.push_back(node->val); // Visit the current node
        inorder(node->right, inorderTraversal); // Traverse the right subtree
    }

    bool isValidBST(TreeNode* root) {
        vector<int> inorderTraversal; // To store the inorder sequence
        inorder(root, inorderTraversal);

        // Check if the inorder sequence is sorted
        for (int i = 1; i < inorderTraversal.size(); i++) {
            if (inorderTraversal[i] <= inorderTraversal[i - 1]) {
                return false; // Not a valid BST
            }
        }

        return true; // Valid BST
    }
};
```

## Approach 2: Recursive Range Validation

### Idea/Intuition

To validate if a binary tree is a BST:

- Each node must satisfy the BST property:
  - All nodes in the left subtree must have values less than the node's value.
  - All nodes in the right subtree must have values greater than the node's value.
- Use a recursive function to validate each node by maintaining an allowable range (`minVal` and `maxVal`).

### Algorithm

1. Start with the root node and an initial range of `(-∞, ∞)`.
2. For each node:
   - Check if its value lies within the allowable range.
   - Recur for the left child with an updated range `(-∞, node->val)`.
   - Recur for the right child with an updated range `(node->val, ∞)`.
3. If all nodes satisfy the condition, the tree is a valid BST; otherwise, it is not.

### Time Complexity

- **O(n)**: Visits each node exactly once.

### Space Complexity

- **O(h)**: Recursive stack space, where `h` is the height of the tree.

### Implementation

```cpp
class Solution {
public:
    bool validate(TreeNode* node, long long minVal, long long maxVal) {
        if (node == NULL) return true; // Base case: null node is valid

        // Check if the current node's value strictly lies within the allowed range
        if (node->val > minVal && node->val < maxVal) {
            // Recursively validate the left and right subtrees
            return validate(node->left, minVal, node->val) &&
                   validate(node->right, node->val, maxVal);
        } else {
            return false;
        }
    }

    bool isValidBST(TreeNode* root) {
        return validate(root, LLONG_MIN, LLONG_MAX); // Use long long limits
    }
};
```

# 281. Lowest Common Ancestor of a Binary Search Tree

## Problem Link

[Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

## Approach 1: Path Tracing and Comparison

### Idea/Intuition

The approach involves finding paths from the root to the two given nodes `p` and `q` and then comparing these paths to determine the lowest common ancestor (LCA). The LCA is the last common node in the two paths.

### Algorithm

1. Define a helper function `findPath`:
   - Recursively search for the target node in the left and right subtrees.
   - Maintain a vector `path` to track the nodes visited during the search.
   - Backtrack by removing nodes from the path if the target is not found in a subtree.
2. In the `lowestCommonAncestor` function:
   - Use `findPath` to find the path from the root to `p` and `q`.
   - Compare the two paths to find the last common node.
3. Return the last common node as the LCA.

### Time Complexity

- **O(n)**: Each node is visited once for both `p` and `q`, where `n` is the number of nodes in the tree.

### Space Complexity

- **O(h)**: Space used for recursion and storing paths, where `h` is the height of the tree.

### Implementation

```cpp
class Solution {
public:
    bool findPath(TreeNode* root, TreeNode* target, vector<TreeNode*>& path) {
        if (!root) return false;

        // Add the current node to the path
        path.push_back(root);

        // If the target node is found
        if (root == target) return true;

        // Recursively check left and right subtrees
        if (findPath(root->left, target, path) || findPath(root->right, target, path)) {
            return true;
        }

        // Backtrack if the target node is not found in this subtree
        path.pop_back();
        return false;
    }

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        // Vectors to store paths
        vector<TreeNode*> pathP, pathQ;

        // Find paths from root to p and q
        if (!findPath(root, p, pathP) || !findPath(root, q, pathQ)) {
            return nullptr; // If either p or q is not present in the tree
        }

        // Find the last common node in the paths
        int i = 0;
        while (i < pathP.size() && i < pathQ.size() && pathP[i] == pathQ[i]) {
            i++;
        }

        return pathP[i - 1]; // The last common node
    }
};
```

## Approach 2: Iterative Traversal

### Idea/Intuition

This approach leverages the properties of a Binary Search Tree (BST). The lowest common ancestor (LCA) is the node where the paths to the two given nodes `p` and `q` split. Using an iterative approach, we traverse the tree to find this split point.

### Algorithm

1. Start from the root of the tree.
2. Traverse the tree iteratively:
   - If both `p` and `q` are smaller than the current node, move to the left subtree.
   - If both `p` and `q` are greater than the current node, move to the right subtree.
   - Otherwise, the current node is the LCA, as it is where the paths to `p` and `q` diverge.
3. Return the current node as the LCA.

### Time Complexity

- **O(h)**: The height of the tree determines the number of nodes visited during traversal.

### Space Complexity

- **O(1)**: No additional space is used apart from a few variables.

### Implementation

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        while (root) {
            // If both p and q are smaller than root, move to the left subtree
            if (p->val < root->val && q->val < root->val) {
                root = root->left;
            }
            // If both p and q are greater than root, move to the right subtree
            else if (p->val > root->val && q->val > root->val) {
                root = root->right;
            }
            // Otherwise, the current node is the LCA
            else {
                return root;
            }
        }
        return nullptr; // If the tree is empty
    }
};
```

# 282. Validate Binary Search Tree

## Problem Link

[Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree)

## Approach 1: Recursive Range Validation

### Idea/Intuition

To validate if a binary tree is a BST:

- Each node must satisfy the BST property:
  - All nodes in the left subtree must have values less than the node's value.
  - All nodes in the right subtree must have values greater than the node's value.
- Use a recursive function to validate each node by maintaining an allowable range (`minVal` and `maxVal`).

### Algorithm

1. Start with the root node and an initial range of `(-∞, ∞)`.
2. For each node:
   - Check if its value lies within the allowable range.
   - Recur for the left child with an updated range `(-∞, node->val)`.
   - Recur for the right child with an updated range `(node->val, ∞)`.
3. If all nodes satisfy the condition, the tree is a valid BST; otherwise, it is not.

### Time Complexity

- **O(n)**: Visits each node exactly once.

### Space Complexity

- **O(h)**: Recursive stack space, where `h` is the height of the tree.

### Implementation

```cpp
class Solution {
public:
    bool validate(TreeNode* node, long long minVal, long long maxVal) {
        if (node == NULL) return true; // Base case: null node is valid

        // Check if the current node's value strictly lies within the allowed range
        if (node->val > minVal && node->val < maxVal) {
            // Recursively validate the left and right subtrees
            return validate(node->left, minVal, node->val) &&
                   validate(node->right, node->val, maxVal);
        } else {
            return false;
        }
    }

    bool isValidBST(TreeNode* root) {
        return validate(root, LLONG_MIN, LLONG_MAX); // Use long long limits
    }
};
```

# 283. Construct Binary Search Tree from Preorder Traversal

## Problem Link

[Construct Binary Search Tree from Preorder Traversal](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/)

## Approach 1: Iterative Insertion

### Idea/Intuition

- Build the BST by inserting nodes sequentially from the `preorder` traversal list.
- Start with an empty tree and insert each value into the BST, ensuring that:
  - Values smaller than the root go to the left subtree.
  - Values larger than the root go to the right subtree.

### Algorithm

1. Define a helper function `insertNode` to recursively insert a value into the BST.
2. Initialize an empty tree with a `nullptr` root.
3. Iterate over each value in the `preorder` list and insert it into the BST using `insertNode`.
4. Return the constructed BST.

### Time Complexity

- **O(n²)**: Each insertion takes O(h), where `h` is the height of the tree. In the worst case, the tree becomes skewed (height ≈ n).

### Space Complexity

- **O(n)**: Space for recursion stack during insertion in the worst case.

### Implementation

```cpp
class Solution {
public:
    TreeNode* insertNode(TreeNode* root, int value) {
        if (root == nullptr) {
            return new TreeNode(value); // Create a new node if the current position is empty
        }

        if (value < root->val) {
            // If value is smaller, go to the left subtree
            root->left = insertNode(root->left, value);
        } else {
            // If value is greater, go to the right subtree
            root->right = insertNode(root->right, value);
        }
        return root;
    }

    TreeNode* bstFromPreorder(vector<int>& preorder) {
        if (preorder.empty()) return nullptr; // If the preorder is empty, return null

        TreeNode* root = nullptr;
        for (int val : preorder) {
            root = insertNode(root, val); // Insert each value into the BST
        }
        return root;
    }
};
```

## Approach 2: Preorder and Inorder Reconstruction

### Idea/Intuition

- A BST's inorder traversal is sorted, while its preorder traversal gives the root-first order.
- Use the `preorder` array to determine the root nodes and recursively divide the tree into left and right subtrees based on the sorted `inorder` traversal.

### Algorithm

1. Create an `inorder` array by sorting the `preorder` array (as it represents the inorder traversal of the BST).
2. Create a mapping of the indices of elements in the `inorder` array for efficient lookup.
3. Use a recursive function `buildTree` to construct the BST:
   - Select the current root node using the `preorder` array.
   - Divide the `inorder` array into left and right subtrees based on the root's position.
   - Recursively build the left and right subtrees.
4. Return the constructed BST.

### Time Complexity

- **O(n log n)**: Sorting the `inorder` array takes O(n log n), and constructing the tree takes O(n).
- **O(n)** if the `inorder` array is provided directly.

### Space Complexity

- **O(n)**: Space for the map and recursion stack.

### Implementation

```cpp
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder, int& preIndex, int inStart, int inEnd, unordered_map<int, int>& inMap) {
        if (inStart > inEnd) return nullptr; // Base case: no elements in this subtree

        // Pick the current node from preorder traversal
        int currentVal = preorder[preIndex++];
        TreeNode* node = new TreeNode(currentVal);

        // Find the position of this node in the inorder traversal
        int inPos = inMap[currentVal];

        // Recursively build left and right subtrees
        node->left = buildTree(preorder, inorder, preIndex, inStart, inPos - 1, inMap);
        node->right = buildTree(preorder, inorder, preIndex, inPos + 1, inEnd, inMap);

        return node;
    }

    TreeNode* bstFromPreorder(vector<int>& preorder) {
        vector<int> inorder = preorder;          // Copy preorder to get inorder
        sort(inorder.begin(), inorder.end());   // Sort inorder as it is BST

        unordered_map<int, int> inMap;          // Store index mapping for inorder
        for (int i = 0; i < inorder.size(); i++) {
            inMap[inorder[i]] = i;
        }

        int preIndex = 0; // Index to track current node in preorder
        return buildTree(preorder, inorder, preIndex, 0, inorder.size() - 1, inMap);
    }
};
```

## Approach 3: Recursive Range-Based Construction

### Idea/Intuition

- A BST can be constructed from preorder traversal by maintaining bounds (`min` and `max`) for each node to ensure it satisfies the BST property.
- Start with the entire range `[-∞, ∞]` and recursively narrow the range as nodes are added.

### Algorithm

1. Use a helper function `solve` to construct the BST:
   - Check if the current value lies within the range `[mini, maxi]`. If not, return `nullptr`.
   - Create a new node for the current value and move the index forward.
   - Recursively construct the left subtree with the range `[mini, root->val]`.
   - Recursively construct the right subtree with the range `[root->val, maxi]`.
2. Start the recursion with the full range `[INT_MIN, INT_MAX]` and the initial index `0`.
3. Return the constructed BST.

### Time Complexity

- **O(n)**: Each node is processed exactly once.

### Space Complexity

- **O(n)**: Space required for the recursion stack.

### Implementation

```cpp
class Solution {
public:
    TreeNode* solve(vector<int>& preorder, int mini, int maxi, int& indx) {
        if (indx >= preorder.size()) {
            return nullptr; // Base case: out of bounds
        }

        // Check if the current value fits in the range
        if (preorder[indx] < mini || preorder[indx] > maxi) {
            return nullptr;
        }

        // Create the root node
        TreeNode* root = new TreeNode(preorder[indx++]);

        // Recursively construct the left and right subtrees
        root->left = solve(preorder, mini, root->val, indx);
        root->right = solve(preorder, root->val, maxi, indx);

        return root;
    }

    TreeNode* bstFromPreorder(vector<int>& preorder) {
        int indx = 0; // Start index for preorder traversal
        return solve(preorder, INT_MIN, INT_MAX, indx);
    }
};
```
