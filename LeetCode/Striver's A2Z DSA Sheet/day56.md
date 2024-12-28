# 262. All Nodes Distance K in Binary Tree

## Problem Link

[All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/)

## Approach 1: Parent Mapping + BFS Traversal

### Idea/Intuition

1. **Map Parent Nodes**:
   - Use recursion to traverse the binary tree and create a map of child-to-parent relationships for every node. This helps in moving both up (to the parent) and down (to the children) in the tree during traversal.
2. **BFS to Find Nodes at Distance K**:
   - Perform a level-order traversal (BFS) starting from the target node, moving to its neighbors (left, right, and parent), and stopping once all nodes at distance `k` are processed.

### Algorithm

1. Create a `parent` map to store the parent pointers of each node in the tree.
2. Recursively traverse the tree to populate the `parent` map.
3. Perform a BFS traversal starting from the target node:
   - Track visited nodes to avoid revisiting.
   - Decrease `k` at each level.
   - When `k` becomes 0, collect all nodes in the queue as they are at the desired distance.
4. Return the collected nodes as the result.

### Time Complexity

- **Parent Mapping**: \(O(N)\), where \(N\) is the number of nodes in the tree.
- **BFS Traversal**: \(O(N)\), as each node is visited at most once.
- **Total**: \(O(N)\).

### Space Complexity

- **Parent Map**: \(O(N)\), to store parent pointers.
- **Visited Set and Queue**: \(O(N)\), for BFS traversal.
- **Total**: \(O(N)\).

### Implementation

```cpp
class Solution {
public:
    unordered_map<TreeNode*, TreeNode*> parent; // Map to store parent pointers

    // Helper function to populate the parent map
    void addParent(TreeNode* root) {
        if (!root) return;

        if (root->left) {
            parent[root->left] = root;
            addParent(root->left);
        }
        if (root->right) {
            parent[root->right] = root;
            addParent(root->right);
        }
    }

    // Helper function to collect nodes at distance k
    void collectKDistanceNodes(TreeNode* target, int k, vector<int>& result) {
        queue<TreeNode*> que;
        unordered_set<int> visited; // To track visited nodes

        que.push(target);
        visited.insert(target->val);

        while (!que.empty() && k > 0) {
            int size = que.size();
            while (size--) {
                TreeNode* curr = que.front();
                que.pop();

                // Process left child
                if (curr->left && !visited.count(curr->left->val)) {
                    que.push(curr->left);
                    visited.insert(curr->left->val);
                }

                // Process right child
                if (curr->right && !visited.count(curr->right->val)) {
                    que.push(curr->right);
                    visited.insert(curr->right->val);
                }

                // Process parent
                if (parent.count(curr) && !visited.count(parent[curr]->val)) {
                    que.push(parent[curr]);
                    visited.insert(parent[curr]->val);
                }
            }
            k--; // Decrease the distance
        }

        // Collect all nodes at distance k
        while (!que.empty()) {
            result.push_back(que.front()->val);
            que.pop();
        }
    }

    vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
        vector<int> result;
        addParent(root); // Build the parent map
        collectKDistanceNodes(target, k, result); // Collect nodes at distance k
        return result;
    }
};
```

## Approach 2: Convert Tree to Graph and BFS Traversal

### Idea/Intuition

1. **Convert Tree to Graph**:
   - Represent the binary tree as an undirected graph where each node is connected to its parent and children. This allows us to explore both upwards and downwards easily during traversal.
2. **BFS to Find Nodes at Distance K**:
   - Perform a level-order traversal (BFS) from the target node. At each level, decrement the distance `k` until `k == 0`, where all nodes in the queue represent the nodes at distance `k`.

### Algorithm

1. Use a recursive function to convert the tree into an adjacency list representation of a graph.
2. Perform BFS starting from the target node:
   - Use a queue to track the current level of nodes.
   - Use a set to keep track of visited nodes and avoid revisiting.
   - When `k` becomes 0, collect all nodes in the queue as they represent the desired result.
3. Return the collected nodes as the result.

### Time Complexity

- **Graph Conversion**: \(O(N)\), where \(N\) is the number of nodes in the tree.
- **BFS Traversal**: \(O(N)\), as each node is visited at most once.
- **Total**: \(O(N)\).

### Space Complexity

- **Graph Representation**: \(O(N)\), for the adjacency list.
- **Visited Set and Queue**: \(O(N)\), for BFS traversal.
- **Total**: \(O(N)\).

### Implementation

```cpp
class Solution {
public:
    // Helper function to convert the tree into a graph
    void convertToGraph(TreeNode* node, TreeNode* parent, unordered_map<int, vector<int>>& graph) {
        if (!node) return;

        if (parent) {
            graph[node->val].push_back(parent->val);
            graph[parent->val].push_back(node->val);
        }

        convertToGraph(node->left, node, graph);
        convertToGraph(node->right, node, graph);
    }

    vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
        unordered_map<int, vector<int>> graph;
        convertToGraph(root, nullptr, graph); // Convert tree into graph

        queue<int> q;
        unordered_set<int> visited;
        vector<int> result;

        q.push(target->val);
        visited.insert(target->val);

        while (!q.empty() && k >= 0) {
            int size = q.size();

            // If we've reached distance k, collect all nodes in this level
            if (k == 0) {
                while (!q.empty()) {
                    result.push_back(q.front());
                    q.pop();
                }
                break;
            }

            // Process the current level
            while (size--) {
                int curr = q.front();
                q.pop();

                for (int neighbor : graph[curr]) {
                    if (!visited.count(neighbor)) {
                        q.push(neighbor);
                        visited.insert(neighbor);
                    }
                }
            }
            k--;
        }

        return result;
    }
};
```

# 263. Amount of Time for Binary Tree to Be Infected

## Problem Link

[Amount of Time for Binary Tree to Be Infected](https://leetcode.com/problems/amount-of-time-for-binary-tree-to-be-infected/description/)

## Approach 1: BFS with Parent Mapping

### Idea/Intuition

1. The problem involves finding the time it takes for an infection to spread throughout a binary tree starting from a given node.
2. To simulate this process:
   - Use Breadth-First Search (BFS) to traverse the tree.
   - Maintain a mapping of each node to its parent for bidirectional traversal (child-to-parent and parent-to-child).
3. Perform BFS starting from the `start` node, traversing all unvisited nodes (left child, right child, and parent).

### Algorithm

1. **Step 1**: Build a parent mapping for all nodes in the tree:
   - Perform a level-order traversal to find the parent of each node and store it in a map.
   - Identify the node corresponding to the `start` value.
2. **Step 2**: Perform a BFS starting from the `start` node:
   - Use a queue to track nodes to be processed and a map to track visited nodes.
   - Traverse all unvisited neighboring nodes (left child, right child, and parent).
   - Increment the time whenever new nodes are infected in the current BFS level.
3. Return the total time after the BFS completes.

### Time Complexity

- **Building the Parent Map**: \(O(N)\), where \(N\) is the number of nodes in the tree.
- **BFS Traversal**: \(O(N)\), as each node is visited once.
- **Total**: \(O(N)\).

### Space Complexity

- **Parent Map**: \(O(N)\).
- **Visited Map**: \(O(N)\).
- **Queue**: \(O(N)\).
- **Total**: \(O(N)\).

### Implementation

```cpp
class Solution {
public:
    int amountOfTime(TreeNode* root, int start) {
        // Map to store parent of each node
        unordered_map<TreeNode*, TreeNode*> parentMap;
        // Queue for level-order traversal
        queue<TreeNode*> q;
        // Map to track visited nodes
        unordered_map<TreeNode*, bool> visited;

        // Step 1: Find the start node and fill parent map
        TreeNode* startNode = nullptr;
        q.push(root);
        parentMap[root] = nullptr;

        while (!q.empty()) {
            TreeNode* current = q.front();
            q.pop();

            if (current->val == start) {
                startNode = current;
            }

            if (current->left) {
                parentMap[current->left] = current;
                q.push(current->left);
            }

            if (current->right) {
                parentMap[current->right] = current;
                q.push(current->right);
            }
        }

        // Step 2: Perform BFS starting from the start node
        q.push(startNode);
        visited[startNode] = true;
        int time = 0;

        while (!q.empty()) {
            int size = q.size();
            bool infected = false;

            for (int i = 0; i < size; i++) {
                TreeNode* current = q.front();
                q.pop();

                // Process left child
                if (current->left && !visited[current->left]) {
                    visited[current->left] = true;
                    q.push(current->left);
                    infected = true;
                }

                // Process right child
                if (current->right && !visited[current->right]) {
                    visited[current->right] = true;
                    q.push(current->right);
                    infected = true;
                }

                // Process parent
                if (parentMap[current] && !visited[parentMap[current]]) {
                    visited[parentMap[current]] = true;
                    q.push(parentMap[current]);
                    infected = true;
                }
            }

            if (infected) {
                time++;
            }
        }

        return time;
    }
};
```

## Approach 2: Convert Binary Tree to Graph and Use BFS

### Idea/Intuition

1. A binary tree can be transformed into an undirected graph, where:
   - Each node in the binary tree becomes a vertex in the graph.
   - Edges connect a node to its parent and its children.
2. Perform BFS on this graph starting from the `start` node to calculate the maximum time required for infection to spread to all nodes.

### Algorithm

1. **Convert Binary Tree to Graph**:
   - Traverse the binary tree using recursion.
   - Add edges between a node and its children (if they exist) and between the node and its parent.
2. **Perform BFS on the Graph**:
   - Start from the `start` node and visit all its neighbors.
   - Keep track of visited nodes to prevent revisiting.
   - Use BFS level-by-level traversal, incrementing the time after processing all nodes at each level.

### Time Complexity

- **Tree to Graph Conversion**: \(O(N)\), where \(N\) is the number of nodes in the tree.
- **BFS Traversal**: \(O(N)\), as each node is visited once.
- **Total**: \(O(N)\).

### Space Complexity

- **Graph Representation**: \(O(N)\), for the adjacency list.
- **Visited Set**: \(O(N)\), for tracking visited nodes.
- **Queue**: \(O(N)\), for BFS traversal.
- **Total**: \(O(N)\).

### Implementation

```cpp
class Solution {
public:
    // Function to convert the tree into an undirected graph
    void convertToGraph(TreeNode* node, TreeNode* parent, unordered_map<int, vector<int>>& graph) {
        if (node == nullptr) return;

        // Add the current node to the graph
        if (graph.find(node->val) == graph.end()) {
            graph[node->val] = {};
        }

        // Connect the current node with its parent
        if (parent != nullptr) {
            graph[node->val].push_back(parent->val);
            graph[parent->val].push_back(node->val);
        }

        // Recurse for left and right children
        convertToGraph(node->left, node, graph);
        convertToGraph(node->right, node, graph);
    }

    // Function to calculate the time to infect all nodes using BFS
    int amountOfTime(TreeNode* root, int start) {
        // Step 1: Convert the tree to an undirected graph
        unordered_map<int, vector<int>> graph;
        convertToGraph(root, nullptr, graph);

        // Step 2: Perform BFS to calculate the maximum distance from the start node
        queue<int> q;
        unordered_set<int> visited;
        q.push(start);
        visited.insert(start);

        int minutes = 0;

        while (!q.empty()) {
            int levelSize = q.size();

            // Process all nodes at the current level
            for (int i = 0; i < levelSize; i++) {
                int current = q.front();
                q.pop();

                // Visit all neighbors
                for (int neighbor : graph[current]) {
                    if (visited.find(neighbor) == visited.end()) {
                        q.push(neighbor);
                        visited.insert(neighbor);
                    }
                }
            }

            // Increment time after processing one level
            if (!q.empty()) {
                minutes++;
            }
        }

        return minutes;
    }
};
```

# 264. Count Complete Tree Nodes

## Problem Link

[Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

## Approach 1: Recursive Inorder Traversal to Count Nodes

### Idea/Intuition

1. Use an inorder traversal to visit each node in the binary tree.
2. Maintain a counter to count the nodes during traversal.
3. Traverse the left subtree, count the current node, and then traverse the right subtree recursively.

### Algorithm

1. **Base Case**: If the current node is `NULL`, return without incrementing the counter.
2. **Recursive Call**:
   - Traverse the left subtree.
   - Increment the counter for the current node.
   - Traverse the right subtree.
3. Start the traversal from the root node and count the nodes.

### Time Complexity

- **Time Complexity**: \(O(N)\), where \(N\) is the number of nodes in the tree, as we visit each node once.
- **Space Complexity**: \(O(H)\), where \(H\) is the height of the tree, due to the recursive call stack.

### Implementation

```cpp
class Solution {
public:
    // Helper function to perform an inorder traversal and count nodes
    void inorderTraversal(TreeNode* root, int& count) {
        // Base case: if the current node is NULL, return
        if (root == NULL) {
            return;
        }

        // Traverse the left subtree
        inorderTraversal(root->left, count);

        // Increment the counter for the current node
        count++;

        // Traverse the right subtree
        inorderTraversal(root->right, count);
    }
    // Function to count the nodes in a complete binary tree
    int countNodes(TreeNode* root) {
        int count = 0;
        inorderTraversal(root, count);
        return count;
    }
};
```

## Approach 2: Recursive Postorder Traversal with Aggregation

### Idea/Intuition

1. Use a postorder traversal to visit each node in the binary tree.
2. For each node, calculate the total number of nodes in its left and right subtrees.
3. The total number of nodes for the current node is the sum of nodes in its left and right subtrees plus one (for the current node itself).

### Algorithm

1. **Base Case**: If the current node is `NULL`, return 0 as there are no nodes.
2. **Recursive Call**:
   - Recursively calculate the number of nodes in the left subtree.
   - Recursively calculate the number of nodes in the right subtree.
3. Return the sum of the left and right subtree counts plus one.

### Time Complexity

- **Time Complexity**: \(O(N)\), where \(N\) is the number of nodes in the tree, as we visit each node once.
- **Space Complexity**: \(O(H)\), where \(H\) is the height of the tree, due to the recursive call stack.

### Implementation

```cpp
class Solution {
public:
    // Helper function to calculate the total number of nodes
    int solve(TreeNode* root) {
        // Base case: if the current node is NULL, return 0
        if (root == NULL) {
            return 0;
        }

        // Recursively calculate nodes in the left and right subtrees
        int left = solve(root->left);
        int right = solve(root->right);

        // Return the total count including the current node
        return left + right + 1;
    }

    // Function to count the nodes in a complete binary tree
    int countNodes(TreeNode* root) {
        return solve(root);  // Start calculation from the root
    }
};
```

## Approach 3: Optimized Recursive Approach Using Tree Height

### Idea/Intuition

1. Use the property of a complete binary tree:
   - If the heights of the leftmost and rightmost paths are equal, the tree is a full binary tree.
   - The number of nodes in a full binary tree can be calculated using the formula \((2^h) - 1\), where \(h\) is the height.
2. If the heights are not equal, recursively count the nodes in the left and right subtrees and add 1 for the current root node.

### Algorithm

1. Define two helper functions to calculate the height of the leftmost and rightmost paths:
   - Traverse the left subtree until reaching a `NULL` node to compute the left height.
   - Traverse the right subtree until reaching a `NULL` node to compute the right height.
2. For the given root:
   - Compute the left and right heights.
   - If the heights are equal, use the formula \((1 << h) - 1\) to calculate the total nodes in the tree.
   - Otherwise, recursively count the nodes in the left and right subtrees and return the total.

### Time Complexity

- **Time Complexity**: \(O(\log^2 N)\), where \(N\) is the number of nodes in the tree:
  - Calculating the height takes \(O(\log N)\), and this is done for each recursive call, leading to \(O(\log N) \times O(\log N) = O(\log^2 N)\).
- **Space Complexity**: \(O(\log N)\), due to the recursive call stack.

### Implementation

```cpp
class Solution {
public:
    // Helper function to calculate the height of the leftmost path
    int leftHeight(TreeNode* root) {
        int ans = 0;
        while (root != NULL) {
            root = root->left;
            ans++;
        }
        return ans;
    }

    // Helper function to calculate the height of the rightmost path
    int rightHeight(TreeNode* root) {
        int ans = 0;
        while (root != NULL) {
            root = root->right;
            ans++;
        }
        return ans;
    }

    // Function to count the nodes in a complete binary tree
    int countNodes(TreeNode* root) {
        if (root == NULL) {
            return 0;
        }

        // Calculate the left and right heights
        int lh = leftHeight(root);
        int rh = rightHeight(root);

        if (lh == rh) {
            // If the left height is equal to the right height, the tree is a full binary tree
            return (1 << lh) - 1;  // (2^h) - 1
        } else {
            // Otherwise, recursively count the nodes in the left and right subtrees
            return countNodes(root->left) + 1 + countNodes(root->right);
        }
    }
};
```
