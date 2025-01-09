# 293. DFS Traversal of Graph

## Problem Link

[GeeksForGeeks - Depth First Traversal for a Graph](https://www.geeksforgeeks.org/problems/depth-first-traversal-for-a-graph/1)

## Approach: Depth-First Search (DFS)

### Idea/Intuition

- Start DFS traversal from node 0.
- Use recursion to explore each node and its unvisited neighbors.
- Mark each node as visited and store it in the result list during traversal.
- Continue until all reachable nodes are visited.

### Algorithm

1. Initialize a `visited` vector of size \(V\) (number of vertices), set to 0.
2. Start DFS from node 0:
   - Mark it as visited and add it to the result list.
3. For each neighbor of the current node:
   - If it's unvisited, recursively call DFS for that neighbor.
4. Return the result vector containing the DFS traversal order.

### Time Complexity

- **O(V + E)**:
  - Each vertex and edge is processed once during DFS traversal.

### Space Complexity

- **O(V)**:
  - For the `visited` array and the recursion stack.

### Implementation

```cpp
class Solution {
  public:
    // Function to return a list containing the DFS traversal of the graph.
    void solve(int indx, vector<vector<int>>& adj, vector<int>& visited, vector<int>& ans) {
        ans.push_back(indx);
        visited[indx] = 1;
        for (auto i : adj[indx]) {
            if (!visited[i]) {
                solve(i, adj, visited, ans);
            }
        }
    }

    vector<int> dfsOfGraph(vector<vector<int>>& adj) {
        vector<int> visited(adj.size(), 0); // Keeps track of visited nodes.
        vector<int> ans;
        solve(0, adj, visited, ans); // Start DFS from node 0.
        return ans;
    }
};
```

# 294. Number of Provinces

## Problem Link

[LeetCode - Number of Provinces](https://leetcode.com/problems/number-of-provinces/description/)

## Approach 1: Breadth-First Search (BFS)

### Idea/Intuition

- The problem can be treated as finding connected components in an undirected graph.
- Represent the input `isConnected` matrix as an adjacency list.
- Perform a BFS traversal for each unvisited node to explore its connected component.
- Count the number of BFS traversals, which corresponds to the number of connected components or provinces.

### Algorithm

1. Convert the adjacency matrix `isConnected` into an adjacency list.
   - Add edges for every `1` in the matrix (except self-loops).
2. Initialize a `visited` array to keep track of visited nodes.
3. For each unvisited node, perform a BFS:
   - Use a queue to explore nodes in the connected component.
   - Mark nodes as visited.
4. Increment the province count for each BFS traversal.
5. Return the count of provinces.

### Time Complexity

- **O(N²)**:
  - Conversion of the adjacency matrix to a list takes \(O(N²)\).
  - BFS traversal takes \(O(V + E)\), which is \(O(N²)\) for a dense graph.

### Space Complexity

- **O(N²)**:
  - For the adjacency list representation of the graph.
- **O(N)**:
  - For the `visited` array and BFS queue.

### Implementation

```cpp
class Solution {
public:
    void bfs(int node, unordered_map<int, list<int>> &adj, vector<int> &visited) {
        queue<int> q;
        q.push(node);
        visited[node] = 1;

        while (!q.empty()) {
            int current = q.front();
            q.pop();
            for (auto neigh : adj[current]) {
                if (!visited[neigh]) {
                    visited[neigh] = 1;
                    q.push(neigh);
                }
            }
        }
    }

    int findCircleNum(vector<vector<int>> &isConnected) {
        int n = isConnected.size(); // Number of nodes
        unordered_map<int, list<int>> adj;

        // Create adjacency list from the adjacency matrix
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (isConnected[i][j] == 1 && i != j) {
                    adj[i].push_back(j);
                }
            }
        }

        vector<int> visited(n, 0);
        int count = 0;

        // Perform BFS for each unvisited node
        for (int i = 0; i < n; ++i) {
            if (!visited[i]) {
                ++count;
                bfs(i, adj, visited);
            }
        }

        return count;
    }
};
```

## Approach 2: Depth-First Search (DFS)

### Idea/Intuition

- The problem is about identifying connected components in an undirected graph.
- Represent the input `isConnected` matrix as an adjacency list.
- Use a DFS traversal to explore all nodes in a connected component.
- Count the number of DFS calls, which corresponds to the number of provinces.

### Algorithm

1. Convert the adjacency matrix `isConnected` into an adjacency list.
   - Add edges for every `1` in the matrix (except self-loops).
2. Initialize a `visited` array to keep track of visited nodes.
3. For each unvisited node, perform a DFS:
   - Recursively visit all connected neighbors and mark them as visited.
4. Increment the province count for each DFS traversal.
5. Return the count of provinces.

### Time Complexity

- **O(N²)**:
  - Conversion of the adjacency matrix to a list takes \(O(N²)\).
  - DFS traversal takes \(O(V + E)\), which is \(O(N²)\) for a dense graph.

### Space Complexity

- **O(N²)**:
  - For the adjacency list representation of the graph.
- **O(N)**:
  - For the `visited` array and the recursion stack.

### Implementation

```cpp
class Solution {
public:
    void dfs(int node, unordered_map<int, list<int>> &adj, vector<int> &visited) {
        visited[node] = 1;
        for (auto neigh : adj[node]) {
            if (!visited[neigh]) {
                dfs(neigh, adj, visited);
            }
        }
    }

    int findCircleNum(vector<vector<int>> &isConnected) {
        int n = isConnected.size(); // Number of nodes
        unordered_map<int, list<int>> adj;

        // Create adjacency list from the adjacency matrix
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (isConnected[i][j] == 1 && i != j) {
                    adj[i].push_back(j);
                }
            }
        }

        vector<int> visited(n, 0);
        int count = 0;

        // Perform DFS for each unvisited node
        for (int i = 0; i < n; ++i) {
            if (!visited[i]) {
                ++count;
                dfs(i, adj, visited);
            }
        }

        return count;
    }
};
```

# 295. Number of Provinces

## Problem Link

[GeeksForGeeks - Number of Provinces](https://www.geeksforgeeks.org/problems/number-of-provinces/1)

## Approach: Union-Find (Disjoint Set Union)

### Idea/Intuition

- Use the Union-Find algorithm to determine the number of connected components in the graph.
- Treat each node as part of a disjoint set.
- Perform union operations for every edge (where \(adj[i][j] = 1\)).
- Use path compression to optimize the find operation and keep track of unique parents to count the provinces.

### Algorithm

1. **Initialization**:
   - Create a `parent` array where each node is its own parent initially.
   - Create a `rank` array to keep track of the tree height.
2. **Union Operation**:
   - For each edge in the adjacency matrix, merge the sets of the two nodes if they belong to different sets.
   - Use rank-based union to keep the tree flat.
3. **Count Provinces**:
   - Find the unique parents of all nodes after performing all union operations.
   - The number of unique parents corresponds to the number of provinces.

### Time Complexity

- **O(V²)**:
  - For processing all edges in the adjacency matrix.
- The find and union operations are nearly constant due to path compression and rank optimization.

### Space Complexity

- **O(V)**:
  - For storing the `parent` and `rank` arrays.

### Implementation

```cpp
class Solution {
  public:
    int findParent(int node, vector<int>& parent) {
        if (parent[node] == node) {
            return node;
        }
        return parent[node] = findParent(parent[node], parent); // Path compression
    }

    void unionNodes(int u, int v, vector<int>& parent, vector<int>& rank) {
        int parentU = findParent(u, parent);
        int parentV = findParent(v, parent);

        if (parentU != parentV) {
            if (rank[parentU] > rank[parentV]) {
                parent[parentV] = parentU;
            } else if (rank[parentU] < rank[parentV]) {
                parent[parentU] = parentV;
            } else {
                parent[parentV] = parentU;
                rank[parentU]++;
            }
        }
    }

    int numProvinces(vector<vector<int>> adj, int V) {
        vector<int> parent(V);
        vector<int> rank(V, 0);

        // Initialize parent array
        for (int i = 0; i < V; i++) {
            parent[i] = i;
        }

        // Perform union operations
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                if (adj[i][j] == 1) {
                    unionNodes(i, j, parent, rank);
                }
            }
        }

        // Count distinct parents
        unordered_set<int> uniqueParents;
        for (int i = 0; i < V; i++) {
            uniqueParents.insert(findParent(i, parent));
        }

        return uniqueParents.size();
    }
};
```
