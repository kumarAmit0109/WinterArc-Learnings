# 234. Introduction to Trees

### Problem Link

[GeeksforGeeks - Introduction to Trees](https://www.geeksforgeeks.org/problems/introduction-to-trees/1)

## Approach: Formula-Based Calculation

### Idea/Intuition

For a complete binary tree, the maximum number of nodes in a tree of level `i` is given by the formula:
\[ Total Nodes = 2^i - 1 \]

This formula calculates the maximum number of nodes directly without any iteration or recursion.

### Algorithm

1. **Formula Application**:
   - Use the formula \( (1 << i) - 1 \) to calculate maximum number of nodes on level `i` of a binary tree.

### Time Complexity

- **Computation**: \( O(1) \), as it involves a single mathematical operation.
- **Overall**: \( O(1) \).

### Space Complexity

- **Auxiliary Space**: \( O(1) \), no extra space is used.

### Implementation

```cpp
int countNodes(int i) {
    return (1 << i-1); // maximum nodes in a complete binary tree of level i
}
```

# 235. Binary Tree Representation

### Problem Link

[GeeksforGeeks - Binary Tree Representation](https://www.geeksforgeeks.org/problems/binary-tree-representation/1?)

## Approach

### Idea/Intuition

The goal is to create a binary tree from a given array `vec` of size 7 using level-order traversal. A queue is utilized to assign left and right children to each node while iterating through the array.

### Algorithm

1. Initialize a queue and push the root node (`root0`).
2. Traverse the array `vec` starting from index `1`.
3. For each node popped from the queue:
   - Assign the next element in `vec` as its left child and push this child into the queue.
   - If there are more elements in `vec`, assign the next element as its right child and push this child into the queue.
4. Repeat until all elements in `vec` are processed.

### Time Complexity

- **O(N)**: Each node is visited once.

### Space Complexity

- **O(N)**: Space used by the queue.

### Implementation

```cpp
class Solution {
public:
    void create_tree(node* root0, vector<int>& vec) {
        queue<node*> q;
        q.push(root0);
        int index = 1;

        while (!q.empty() && index < vec.size()) {
            node* current = q.front();
            q.pop();

            // Assign left child
            current->left = newNode(vec[index++]);
            q.push(current->left);

            // Assign right child if possible
            if (index < vec.size()) {
                current->right = newNode(vec[index++]);
                q.push(current->right);
            }
        }
    }
};
```
