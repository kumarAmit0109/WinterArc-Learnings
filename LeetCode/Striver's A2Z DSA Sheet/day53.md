# 253. Vertical Order Traversal of a Binary Tree

## Problem Link

[Vertical Order Traversal of a Binary Tree](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/)

## Approach: Level-wise Mapping with Horizontal Distance

### Idea/Intuition

1. Use a multilevel mapping:
   - Map horizontal distances (HD) to levels (LVL) and then to a sorted set of node values (`multiset`).
2. Perform a level-order traversal using a queue, maintaining HD and LVL for each node.
3. Populate the map during traversal:
   - Insert each node's value into the appropriate `multiset` for its HD and LVL.
   - Push child nodes into the queue with updated HD and LVL.
4. Extract the results from the map:
   - Combine all nodes' values column by column based on increasing HD and LVL.

### Algorithm

1. **Initialization**:
   - Use a map to store mappings: `map<int, map<int, multiset<int>>> m`.
   - Use a queue for level-order traversal: `queue<pair<TreeNode*, pair<int, int>>> que`.
2. **Traversal**:
   - Process each node, record its HD and LVL, and insert its value into the map.
   - Push its children with updated HD and LVL into the queue.
3. **Result Extraction**:
   - Traverse the map to extract sorted columns (HD-wise).
4. Return the final result as a 2D vector.

### Time Complexity

- **Traversal**: \(O(N \log N)\), where \(N\) is the number of nodes. Insertion and sorting within the map are logarithmic operations.
- **Result Extraction**: \(O(N)\).

### Space Complexity

- **Map Storage**: \(O(N)\), where \(N\) is the number of nodes.
- **Queue**: \(O(N)\) for level-order traversal.

### Implementation

```cpp
class Solution {
public:
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        // Map to store nodes by horizontal distance and level
        map<int, map<int, multiset<int>>> m;
        // Queue for BFS traversal
        queue<pair<TreeNode*, pair<int, int>>> que;

        // Start BFS with root node at HD = 0 and LVL = 0
        que.push(make_pair(root, make_pair(0, 0)));
        while (!que.empty()) {
            auto frnt = que.front();
            que.pop();

            TreeNode* node = frnt.first;
            int hd = frnt.second.first;  // Horizontal distance
            int lvl = frnt.second.second;  // Level

            // Insert node value into map
            m[hd][lvl].insert(node->val);

            // Process left and right children
            if (node->left) {
                que.push(make_pair(node->left, make_pair(hd - 1, lvl + 1)));
            }
            if (node->right) {
                que.push(make_pair(node->right, make_pair(hd + 1, lvl + 1)));
            }
        }

        // Construct the result
        vector<vector<int>> ans;
        for (auto& p : m) {
            vector<int> col;
            for (auto& q : p.second) {
                col.insert(col.end(), q.second.begin(), q.second.end());
            }
            ans.push_back(col);
        }
        return ans;
    }
};
```

# 254. Top View of Binary Tree

## Problem Link

[Top View of Binary Tree](https://www.geeksforgeeks.org/problems/top-view-of-binary-tree/1)

## Approach: Level-Order Traversal with Horizontal Distance Mapping

### Idea/Intuition

1. Perform a level-order traversal while maintaining the horizontal distance (HD) of each node from the root.
2. Use a map to store the first node encountered at each HD, as it represents the top view for that HD.
3. Extract the values from the map in ascending order of HD to construct the top view.

### Algorithm

1. **Initialization**:
   - Use a map to store the first node value for each HD: `unordered_map<int, int> topNodeMap`.
   - Use a queue for level-order traversal: `queue<pair<Node*, int>> q`.
2. **Traversal**:
   - Start with the root node at HD = 0.
   - For each node, if its HD is not yet in the map, add it.
   - Push its left child with HD - 1 and its right child with HD + 1 into the queue.
3. **Result Extraction**:
   - Sort the map by HD to arrange the nodes in left-to-right order.
   - Traverse the sorted map to construct the result vector.
4. Return the result vector.

### Time Complexity

- **Traversal**: \(O(N)\), where \(N\) is the number of nodes.
- **Map Sorting**: \(O(H \log H)\), where \(H\) is the range of horizontal distances.

### Space Complexity

- **Map Storage**: \(O(H)\), where \(H\) is the range of horizontal distances.
- **Queue**: \(O(N)\) for level-order traversal.

### Implementation

```cpp
class Solution {
  public:
    // Function to return a list of nodes visible from the top view
    // from left to right in Binary Tree.
    vector<int> topView(Node *root) {
        // Map to store the first node at each horizontal distance
        unordered_map<int, int> topNodeMap;
        // Queue for level order traversal
        queue<pair<Node*, int>> q;

        // Start with the root node and horizontal distance 0
        if (root == NULL) return {}; // Handle edge case where root is NULL

        q.push({root, 0});

        while (!q.empty()) {
            auto front = q.front();
            q.pop();

            Node* node = front.first;
            int hd = front.second;

            // If this horizontal distance is not yet visited, add it to the map
            if (topNodeMap.find(hd) == topNodeMap.end()) {
                topNodeMap[hd] = node->data;
            }

            // Add left and right children to the queue with updated horizontal distances
            if (node->left) {
                q.push({node->left, hd - 1});
            }
            if (node->right) {
                q.push({node->right, hd + 1});
            }
        }

        // Create a vector to store the top view in the correct order
        vector<int> result;

        // Sort the map by horizontal distance
        map<int, int> sortedMap(topNodeMap.begin(), topNodeMap.end());

        // Traverse the sorted map and collect the top view nodes
        for (auto it = sortedMap.begin(); it != sortedMap.end(); ++it) {
            result.push_back(it->second);
        }

        return result;
    }
};
```

# 255. Bottom View of Binary Tree

## Problem Link

[Bottom View of Binary Tree](https://www.geeksforgeeks.org/problems/bottom-view-of-binary-tree/1)

## Approach: Level-Order Traversal with Horizontal Distance Mapping

### Idea/Intuition

1. Perform a level-order traversal while maintaining the horizontal distance (HD) of each node from the root.
2. Use a map to store the node value corresponding to each HD.
   - For every HD, the map keeps updating to store the latest node value encountered, ensuring the "bottommost" node is recorded for that HD.
3. Extract the values from the map in ascending order of HD to construct the bottom view.

### Algorithm

1. **Initialization**:
   - Use a map to store the bottommost node value for each HD: `map<int, int> hdMap`.
   - Use a queue for level-order traversal: `queue<pair<Node*, int>> q`.
2. **Traversal**:
   - Start with the root node at HD = 0.
   - For each node, update the map with its HD and value.
   - Push its left child with HD - 1 and its right child with HD + 1 into the queue.
3. **Result Extraction**:
   - Traverse the map to extract the node values in ascending order of HD.
4. Return the result vector.

### Time Complexity

- **Traversal**: \(O(N)\), where \(N\) is the number of nodes.
- **Map Operations**: \(O(\log N)\) for each insertion.
- **Result Extraction**: \(O(H)\), where \(H\) is the range of horizontal distances.

### Space Complexity

- **Map Storage**: \(O(H)\), where \(H\) is the range of horizontal distances.
- **Queue**: \(O(N)\) for level-order traversal.

### Implementation

```cpp
class Solution {
public:
    vector<int> bottomView(Node *root) {
        if (root == nullptr) return {};  // If the tree is empty, return an empty vector

        // Map to store horizontal distance and corresponding node value
        map<int, int> hdMap;  // Maintains order of keys for horizontal distances

        // Queue for level-order traversal: node and its horizontal distance
        queue<pair<Node*, int>> q;

        // Start with the root node at horizontal distance 0
        q.push({root, 0});

        while (!q.empty()) {
            auto front = q.front();
            q.pop();

            Node* node = front.first;
            int hd = front.second;

            // Update the map with the current node at the horizontal distance
            // Overwriting ensures the bottommost node is stored
            hdMap[hd] = node->data;

            // Traverse left and right children, updating horizontal distances
            if (node->left) {
                q.push({node->left, hd - 1});
            }
            if (node->right) {
                q.push({node->right, hd + 1});
            }
        }

        // Result vector to store the bottom view
        vector<int> result;

        // Traverse the map in sorted order of horizontal distance
        for (auto& entry : hdMap) {
            result.push_back(entry.second);
        }

        return result;
    }
};
```
