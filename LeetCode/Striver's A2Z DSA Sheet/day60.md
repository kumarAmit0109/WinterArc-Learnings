# 274. Minimum Element in BST

## Problem Link

[Minimum Element in BST](https://www.geeksforgeeks.org/problems/minimum-element-in-bst/1)

## Approach: Iterative Traversal to Find the Leftmost Node

### Idea/Intuition

In a Binary Search Tree (BST), the smallest element resides at the leftmost node because all values decrease as we move left. We simply traverse the tree iteratively, going as far left as possible.

### Algorithm

1. Start from the root node.
2. If the root is `NULL`, return `-1` indicating an empty BST.
3. Traverse left until a node with no left child is encountered.
4. Return the value of this leftmost node, which is the minimum element in the BST.

### Time Complexity

- **O(h)**: `h` is the height of the tree. In the worst case, this is `O(n)` for a skewed tree and `O(log n)` for a balanced tree.

### Space Complexity

- **O(1)**: No extra space is used.

### Implementation

```cpp
class Solution {
  public:
    int minValue(Node* root) {
        // Ensure the tree is not empty
        if (root == NULL) {
            return -1; // Indicating no elements in BST
        }

        // Traverse to the leftmost node
        Node* current = root;
        while (current->left != NULL) {
            current = current->left;
        }

        // Return the value of the leftmost node
        return current->data;
    }
};
```

# 275. Implementing Ceil in BST

## Problem Link

[Implementing Ceil in BST](https://www.geeksforgeeks.org/problems/implementing-ceil-in-bst/1)

## Approach: Iterative Traversal to Find Ceil

### Idea/Intuition

The ceil of a value in a BST is the smallest element in the BST that is greater than or equal to the given value. This can be efficiently found by traversing the tree:

1. If the current node's value is equal to the input, that value is the ceil.
2. If the current node's value is less than the input, move to the right subtree since all values in the left subtree will also be smaller.
3. If the current node's value is greater than the input, update the potential ceil and move to the left subtree for a closer match.

### Algorithm

1. Initialize `ceil` to `-1`.
2. Start traversal from the root:
   - If the current node's value equals the input, return the value as the ceil.
   - If the current node's value is less than the input, move to the right subtree.
   - If the current node's value is greater than the input, update `ceil` to the current node's value and move to the left subtree.
3. Continue until the traversal is complete or the node is `NULL`.
4. Return the value stored in `ceil`.

### Time Complexity

- **O(h)**: `h` is the height of the tree. In the worst case, this is `O(n)` for a skewed tree and `O(log n)` for a balanced tree.

### Space Complexity

- **O(1)**: No extra space is used.

### Implementation

```cpp
int findCeil(Node* root, int input) {
    if (root == NULL) return -1;

    int ceil = -1;

    while (root) {
        if (root->data == input) {
            // If the current node's value is equal to the input, it is the ceil.
            return root->data;
        }

        if (root->data < input) {
            // Move to the right subtree if input is greater than current node's value.
            root = root->right;
        } else {
            // Update ceil to current node's value and move to the left subtree.
            ceil = root->data;
            root = root->left;
        }
    }

    return ceil;
}
```

# 276. Floor in BST

## Problem Link

[Floor in BST](https://www.geeksforgeeks.org/problems/floor-in-bst/1?)

## Approach: Iterative Traversal to Find Floor

### Idea/Intuition

The floor of a value in a BST is the largest element in the BST that is less than or equal to the given value. This can be efficiently found by traversing the tree:

1. If the current node's value is equal to the input, that value is the floor.
2. If the current node's value is greater than the input, move to the left subtree since all values in the right subtree will also be greater.
3. If the current node's value is less than the input, update the potential floor and move to the right subtree for a closer match.

### Algorithm

1. Initialize `floor` to `-1`.
2. Start traversal from the root:
   - If the current node's value equals the input, return the value as the floor.
   - If the current node's value is greater than the input, move to the left subtree.
   - If the current node's value is less than the input, update `floor` to the current node's value and move to the right subtree.
3. Continue until the traversal is complete or the node is `NULL`.
4. Return the value stored in `floor`.

### Time Complexity

- **O(h)**: `h` is the height of the tree. In the worst case, this is `O(n)` for a skewed tree and `O(log n)` for a balanced tree.

### Space Complexity

- **O(1)**: No extra space is used.

### Implementation

```cpp
class Solution {
public:
    int floor(Node* root, int x) {
        int floor = -1; // Initialize floor to -1

        while (root) {
            if (root->data == x) {
                // If the current node's value matches x, it is the floor.
                return root->data;
            }

            if (root->data < x) {
                // Update floor to current node's value and move to the right subtree.
                floor = root->data;
                root = root->right;
            } else {
                // Move to the left subtree if current node's value is greater than x.
                root = root->left;
            }
        }

        return floor; // Return the computed floor value
    }
};
```
