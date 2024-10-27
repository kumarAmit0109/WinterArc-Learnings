# Problem: Height of Binary Tree After Subtree Removal Queries

**Problem Link**: [Height of Binary Tree After Subtree Removal Queries](https://leetcode.com/problems/height-of-binary-tree-after-subtree-removal-queries/)

## Approach

### Explanation Behind the Approach

1. **Understanding Heights and Depths**:
   - **Height** of a node is the longest path from that node to a leaf.
   - **Depth** of a node is the number of edges from the root to that node.
   - Removing a subtree rooted at a node affects the height of the tree if that node is part of the longest path from the root to any leaf.
2. **Precomputing Heights and Depths**:

   - To efficiently handle multiple queries, we precompute:
     - The **height** of each node in the tree.
     - The **depth** of each node.
   - This allows us to answer each query independently in constant time by looking up these values.

3. **Storing Maximum Heights by Depth**:
   - To determine how removing a node affects the overall tree height, we need the **two highest heights** at each depth level.
   - For each depth, we store the two maximum heights to handle cases where the node with the maximum height at that depth is removed.
4. **Answering Queries in Constant Time**:
   - For each query:
     - Retrieve the node’s depth and check its height.
     - If the node has the maximum height at its depth, use the second-highest height to determine the new height after removal.
     - If it doesn’t, the height remains the same as the highest at that depth.
   - This precomputed information allows each query to be processed in \( O(1) \).

### Algorithm Steps

1. **Calculate Heights and Depths of Each Node**:

   - For each node, calculate:
     - **Height**: The longest path from that node down to a leaf.
     - **Depth**: The distance from the root to that node.
   - Store these values in dictionaries (`heights` and `depths`).

2. **Store Maximum Heights at Each Depth**:

   - Organize nodes by depth, keeping track of the two highest heights at each depth.
   - For each depth level, store the heights of nodes at that depth in a sorted list. This allows quick access to the two highest heights for any depth.

3. **Answer Each Query**:

   - For each node in `queries`:
     - Retrieve the depth of the node.
     - Check if the node has the maximum height among nodes at its depth.
     - If it does, use the second-highest height to determine the new tree height.
     - Otherwise, use the highest height (since removing the node doesn’t affect the max height at that depth).
   - Append the result for each query to the `answer` vector.

4. **Return the Answer**:
   - Return the `answer` vector after processing all queries.

### Time Complexity

- **O(n + m)**: where \( n \) is the number of nodes in the tree and \( m \) is the number of queries.
  - **O(n)** for calculating heights and depths of each node.
  - **O(m)** for processing each query in constant time using precomputed data.

### Space Complexity

- **O(n)**: where \( n \) is the number of nodes in the binary tree.
  - Includes space for the recursive call stack (in the worst case), and the data structures for storing heights, depths, and maximum heights at each depth.

## Code Implementation

```cpp
class Solution {
public:
    vector<int> treeQueries(TreeNode* root, vector<int>& queries) {
        unordered_map<int, int> heights;       // Store the height of each node
        unordered_map<int, int> depths;        // Store the depth of each node
        unordered_map<int, vector<int>> depthMaxHeights;  // Store max heights for each depth level

        // Step 1: Precompute heights and depths for each node
        computeHeightsAndDepths(root, nullptr, 0, heights, depths);

        // Step 2: Organize max heights at each depth
        for (auto& [node, depth] : depths) {
            int h = heights[node];
            depthMaxHeights[depth].push_back(h);
        }
        for (auto& [depth, heightList] : depthMaxHeights) {
            sort(heightList.begin(), heightList.end(), greater<int>());  // Sort in descending order
        }

        // Step 3: Answer each query using precomputed data
        vector<int> answer;
        for (int query : queries) {
            int depth = depths[query];
            int maxHeightAfterRemoval;

            // Get the maximum height excluding the height of the current query node
            if (depthMaxHeights[depth].size() == 1) {
                maxHeightAfterRemoval = 0;  // Only one node at this depth
            } else if (heights[query] == depthMaxHeights[depth][0]) {
                maxHeightAfterRemoval = depthMaxHeights[depth][1];  // Query node has the max height at this depth
            } else {
                maxHeightAfterRemoval = depthMaxHeights[depth][0];  // Query node doesn't have the max height
            }

            // New tree height after removing the subtree
            answer.push_back(depth + maxHeightAfterRemoval);
        }

        return answer;
    }

private:
    // Function to compute height and depth for each node
    int computeHeightsAndDepths(TreeNode* node, TreeNode* parent, int depth,
                                unordered_map<int, int>& heights, unordered_map<int, int>& depths) {
        if (!node) return -1;
        depths[node->val] = depth;
        int leftHeight = computeHeightsAndDepths(node->left, node, depth + 1, heights, depths);
        int rightHeight = computeHeightsAndDepths(node->right, node, depth + 1, heights, depths);
        int currentHeight = max(leftHeight, rightHeight) + 1;
        heights[node->val] = currentHeight;
        return currentHeight;
    }
};
```
