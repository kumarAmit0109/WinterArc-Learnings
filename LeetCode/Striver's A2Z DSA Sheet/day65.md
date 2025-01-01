# 290. Print Adjacency List

## Problem Link

[GeeksForGeeks - Print Adjacency List](https://www.geeksforgeeks.org/problems/print-adjacency-list-1587115620/1)

## Approach: Constructing the Adjacency List

### Idea/Intuition

1. Use a vector of vectors to represent the adjacency list for the graph.
2. Traverse through the given edges and populate the adjacency list for each vertex.
3. For every edge `(u, v)`, add `v` to `u`'s list and `u` to `v`'s list since the graph is undirected.

### Algorithm

1. Initialize an adjacency list with `V` empty vectors, where `V` is the number of vertices.
2. Iterate through the list of edges:
   - For each edge `(u, v)`, add `v` to `u`'s list and `u` to `v`'s list.
3. Return the adjacency list.

### Time Complexity

- **O(V + E)**:
  - Building the adjacency list involves adding each edge exactly twice, which is proportional to the number of edges.

### Space Complexity

- **O(V + E)**:
  - The adjacency list stores all vertices and their respective connections.

### Implementation

```cpp
class Solution {
public:
    // Function to return the adjacency list for each vertex.
    vector<vector<int>> printGraph(int V, vector<pair<int, int>>& edges) {
        // Initialize adjacency list with V vertices.
        vector<vector<int>> adj(V);

        // Iterate through all edges.
        for (auto edge : edges) {
            int u = edge.first;  // First vertex of the edge.
            int v = edge.second; // Second vertex of the edge.

            // Add edge to both vertices' adjacency lists.
            adj[u].push_back(v);
            adj[v].push_back(u);
        }

        return adj;
    }
};
```

# 291. Count Connected Components

## Problem Link

[NeetCode - Count Connected Components](https://neetcode.io/problems/count-connected-components)

## Approach: BFS for Connected Components

### Idea/Intuition

- Represent the graph using an adjacency list.
- Use BFS to explore all nodes in each connected component.
- Track visited nodes with a `visited` array.
- For each unvisited node, start a BFS, marking all nodes in that connected component.
- Increment the count for each BFS start, indicating a new connected component.

### Algorithm

1. Create an adjacency list from the input edges.
2. Maintain a `visited` vector to track visited nodes.
3. For each node:
   - If it is not visited, perform a BFS starting from that node.
   - Mark all reachable nodes in the component as visited.
   - Increment the component count.
4. Return the count of connected components.

### Time Complexity

- **O(V + E)**:
  - Building the adjacency list takes \(O(E)\).
  - BFS processes all vertices and edges once.

### Space Complexity

- **O(V + E)**:
  - Adjacency list stores all edges.
  - `visited` array and BFS queue use \(O(V)\).

### Implementation

```cpp
class Solution {
public:
    void bfs(int node, vector<vector<int>>& adj, vector<int>& visited) {
        queue<int> q;
        q.push(node);
        visited[node] = 1;

        while (!q.empty()) {
            int curr = q.front();
            q.pop();
            for (int neighbor : adj[curr]) {
                if (!visited[neighbor]) {
                    q.push(neighbor);
                    visited[neighbor] = 1;
                }
            }
        }
    }

    int countComponents(int n, vector<vector<int>>& edges) {
        vector<vector<int>> adj(n); // Adjacency list
        for (auto& edge : edges) {
            adj[edge[0]].push_back(edge[1]);
            adj[edge[1]].push_back(edge[0]);
        }

        vector<int> visited(n, 0);
        int components = 0;

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                components++;
                bfs(i, adj, visited);
            }
        }

        return components;
    }
};
```

# 292. BFS Traversal of Graph

## Problem Link

[GeeksForGeeks - BFS Traversal of Graph](https://www.geeksforgeeks.org/problems/bfs-traversal-of-graph/1)

## Approach: Breadth-First Search (BFS)

### Idea/Intuition

- Use a queue to perform BFS starting from the first node (0).
- Maintain a `visited` vector to ensure each node is processed only once.
- Traverse all neighbors of the current node and add unvisited ones to the queue.
- Continue until the queue is empty, storing the nodes in the traversal order.

### Algorithm

1. Initialize a queue `que` and a vector `visited` of size \(V\), set to 0.
2. Start BFS from node 0:
   - Mark it as visited and enqueue it.
3. While the queue is not empty:
   - Dequeue a node, add it to the result.
   - Traverse its neighbors:
     - If a neighbor is unvisited, mark it visited and enqueue it.
4. Return the result vector.

### Time Complexity

- **O(V + E)**:
  - Each vertex and edge is processed once.

### Space Complexity

- **O(V)**:
  - `visited` array and BFS queue.

### Implementation

```cpp
class Solution {
  public:
    // Function to return Breadth First Traversal of the given graph.
    vector<int> bfsOfGraph(vector<vector<int>> &adj) {
        vector<int> ans;
        vector<int> visited(adj.size(), 0);
        queue<int> que;

        // Start BFS from node 0.
        que.push(0);
        visited[0] = 1;

        while (!que.empty()) {
            int front = que.front();
            que.pop();
            ans.push_back(front);

            // Traverse all neighbors.
            for (int neighbor : adj[front]) {
                if (!visited[neighbor]) {
                    que.push(neighbor);
                    visited[neighbor] = 1;
                }
            }
        }

        return ans;
    }
};
```
