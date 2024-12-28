# 265. Unique Binary Tree Requirements

## Problem Link

[Unique Binary Tree Requirements](https://www.geeksforgeeks.org/problems/unique-binary-tree-requirements/1)

## Approach: Check Valid Traversal Pair

### Idea/Intuition

To construct a unique binary tree, certain combinations of traversal sequences are required. The tree can be uniquely constructed if one traversal sequence specifies the structure and another specifies the order of node visits. The following conditions determine the feasibility:

1. **Preorder + Inorder** or **Inorder + Preorder**:

   - Possible to uniquely construct the tree as both traversals complement each other.

2. **Inorder + Postorder** or **Postorder + Inorder**:

   - Also sufficient to uniquely construct the tree as one specifies structure and the other specifies node order.

3. **Preorder + Postorder**:
   - Insufficient to uniquely construct the tree, as multiple trees can share the same preorder and postorder sequences without a unique mapping.

### Algorithm

1. If the pair of traversals is either `(a = 1, b = 2)` or `(a = 2, b = 1)`, return `true` (Preorder + Inorder or vice versa).
2. If the pair is `(a = 2, b = 3)` or `(a = 3, b = 2)`, return `true` (Inorder + Postorder or vice versa).
3. Otherwise, return `false`.

### Time Complexity

- **O(1)**: The decision is made based on a constant number of conditions.

### Space Complexity

- **O(1)**: No extra space is used.

### Implementation

```cpp
class Solution {
public:
    bool isPossible(int a, int b) {
        // We can only construct a unique binary tree in these cases:
        if ((a == 1 && b == 2) || (a == 2 && b == 1)) {
            return true;  // Preorder + Inorder or Inorder + Preorder
        }
        if ((a == 2 && b == 3) || (a == 3 && b == 2)) {
            return true;  // Inorder + Postorder or Postorder + Inorder
        }

        return false;  // Preorder + Postorder is not sufficient to create a unique binary tree
    }
};
```

# 266. Construct Binary Tree from Inorder and Postorder Traversal

## Problem Link

[Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

## Approach: Recursive Construction with Index Mapping

### Idea/Intuition

The problem involves reconstructing a binary tree from two traversal sequences:

- **Inorder Traversal**: Left -> Root -> Right
- **Postorder Traversal**: Left -> Right -> Root

The last element in the `postorder` array is the root of the current subtree. Using this information:

1. Find the root's index in the `inorder` array to divide it into left and right subtrees.
2. Use the size of the left subtree to determine the boundaries for recursion in the `postorder` array.

### Algorithm

1. Create a map for quick lookup of indices in the `inorder` array.
2. Recursively determine the root node and construct left and right subtrees using:
   - Root value from `postorder`.
   - Subtree boundaries from the `inorder` and `postorder` arrays.

### Time Complexity

- **O(n)**: Each node is processed once, and the index lookup in the map takes constant time.

### Space Complexity

- **O(n)**: For the index map and the recursion stack.

### Implementation

```cpp
class Solution {
public:
    TreeNode* buildTreeHelper(vector<int>& postorder, int postStart, int postEnd, vector<int>& inorder, int inStart, int inEnd,
                              unordered_map<int, int>& inMap) {
        // Base case: if there are no elements to construct the tree
        if (postStart > postEnd || inStart > inEnd) {
            return nullptr;
        }

        // The root of the current subtree is the last element in postorder
        int rootVal = postorder[postEnd];
        TreeNode* root = new TreeNode(rootVal);

        // Find the index of the root in the inorder traversal
        int rootIndex = inMap[rootVal];

        // Calculate the number of nodes in the left subtree
        int leftSubtreeSize = rootIndex - inStart;

        // Recursively build the right and left subtrees
        root->right = buildTreeHelper(postorder, postStart + leftSubtreeSize, postEnd - 1,
                                      inorder, rootIndex + 1, inEnd, inMap);
        root->left = buildTreeHelper(postorder, postStart, postStart + leftSubtreeSize - 1,
                                      inorder, inStart, rootIndex - 1, inMap);

        return root;
    }

    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        // Step 1: Create a map for quick look-up of indices in inorder array
        unordered_map<int, int> inMap;
        for (int i = 0; i < inorder.size(); ++i) {
            inMap[inorder[i]] = i;
        }

        // Step 2: Call the helper function to recursively build the tree
        return buildTreeHelper(postorder, 0, postorder.size() - 1, inorder, 0, inorder.size() - 1, inMap);
    }
};
```

# 267. Construct Binary Tree from Preorder and Inorder Traversal

## Problem Link

[Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

## Approach: Recursive Construction with Index Mapping

### Idea/Intuition

The problem involves reconstructing a binary tree from two traversal sequences:

- **Preorder Traversal**: Root -> Left -> Right
- **Inorder Traversal**: Left -> Root -> Right

The first element in the `preorder` array is the root of the current subtree. Using this information:

1. Find the root's index in the `inorder` array to divide it into left and right subtrees.
2. Use the size of the left subtree to determine the boundaries for recursion in the `preorder` array.

### Algorithm

1. Create a map for quick lookup of indices in the `inorder` array.
2. Recursively determine the root node and construct left and right subtrees using:
   - Root value from `preorder`.
   - Subtree boundaries from the `inorder` and `preorder` arrays.

### Time Complexity

- **O(n)**: Each node is processed once, and the index lookup in the map takes constant time.

### Space Complexity

- **O(n)**: For the index map and the recursion stack.

### Implementation

```cpp
class Solution {
public:
    TreeNode* buildTreeHelper(vector<int>& preorder, int preStart, int preEnd,
                              vector<int>& inorder, int inStart, int inEnd,
                              unordered_map<int, int>& inMap) {
        // Base case: if there is no range to process
        if (preStart > preEnd || inStart > inEnd) {
            return nullptr;
        }

        // Step 3: The root is the first element in the current preorder range
        int rootVal = preorder[preStart];
        TreeNode* root = new TreeNode(rootVal);

        // Step 4: Find the index of the root in the inorder array
        int rootIndex = inMap[rootVal];

        // Step 5: Calculate the size of the left subtree
        int leftSubtreeSize = rootIndex - inStart;

        // Step 6: Recursively build the left and right subtrees
        root->left = buildTreeHelper(preorder, preStart + 1, preStart + leftSubtreeSize,
                                      inorder, inStart, rootIndex - 1, inMap);
        root->right = buildTreeHelper(preorder, preStart + leftSubtreeSize + 1, preEnd,
                                       inorder, rootIndex + 1, inEnd, inMap);

        // Return the constructed root node
        return root;
    }

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        // Step 1: Create a hashmap for the inorder indices
        unordered_map<int, int> inMap;
        for (int i = 0; i < inorder.size(); ++i) {
            inMap[inorder[i]] = i;
        }

        // Step 2: Define the helper function
        return buildTreeHelper(preorder, 0, preorder.size() - 1, inorder, 0, inorder.size() - 1, inMap);
    }
};
```
