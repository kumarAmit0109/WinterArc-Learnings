# Shortest Distance After Road Addition Queries I

**Problem Link**: [Shortest Distance After Road Addition Queries I](https://leetcode.com/problems/shortest-distance-after-road-addition-queries-i/)

## Approach 1: BFS

### Idea

We maintain an adjacency list to represent the graph. For each query, we add the road (edge) and then use a **Breadth-First Search (BFS)** to find the shortest path from node `0` to node `n-1`. BFS guarantees that we find the shortest path in an unweighted graph.

### Algorithm

1. Initialize the adjacency list for the graph with edges between consecutive nodes.
2. For each query, add the road to the graph.
3. Use BFS to compute the shortest path from node `0` to node `n-1`.
4. Return the results for all queries.

### Time Complexity

- **O(n + m + k \* (n + m))**:
  - **n**: Number of nodes.
  - **m**: Number of edges.
  - **k**: Number of queries (each query adds an edge and performs BFS).

### Space Complexity

- **O(n + m)**: Space for the adjacency list.

### Code

```cpp
class Solution {
public:
    int bfs(int n, unordered_map<int, vector<int>>& adj) {
        queue<int> que;
        que.push(0);
        vector<bool> visited(n, false);
        visited[0] = true;

        int level = 0;
        while (!que.empty()) {
            int size = que.size();
            while (size--) {
                int node = que.front();
                que.pop();

                if (node == n - 1) {
                    return level; // Found the destination node
                }

                for (int neighbor : adj[node]) {
                    if (!visited[neighbor]) {
                        que.push(neighbor);
                        visited[neighbor] = true;
                    }
                }
            }
            level++;
        }
        return -1; // If destination node is unreachable
    }

    vector<int> shortestDistanceAfterQueries(int n, vector<vector<int>>& queries) {
        // Initialize the adjacency list
        unordered_map<int, vector<int>> adj;
        for (int i = 0; i < n - 1; i++) {
            adj[i].push_back(i + 1);
        }

        int k = queries.size();
        vector<int> res(k);
        for (int i = 0; i < k; i++) {
            // Add the edge from the query to the adjacency list
            adj[queries[i][0]].push_back(queries[i][1]);
            // Find the shortest path from node 0 to node n-1
            res[i] = bfs(n, adj);
        }
        return res;
    }
};
```

## Approach 2: Dijkstra's Algorithm

### Idea

In this approach, we use **Dijkstra's Algorithm** to find the shortest path from node `0` to node `n-1`. The graph is weighted (all edges have a weight of 1), and Dijkstra's algorithm is suitable for finding the shortest path in graphs with non-negative weights.

### Algorithm

1. Initialize the adjacency list with the edges.
2. For each query, add the edge to the graph.
3. Use Dijkstra’s algorithm to find the shortest distance from node `0` to node `n-1`.
4. Return the results for all queries.

### Time Complexity

- **O(k _ (n _ log n + m))**:
  - **k**: Number of queries.
  - **n**: Number of nodes.
  - **m**: Number of edges.

### Space Complexity

- **O(n + m)**: Space for the adjacency list and the priority queue.

### Code

```cpp
class Solution {
public:
    int dijkstra(int n, unordered_map<int, vector<pair<int, int>>>& adj) {
        vector<int> dist(n, INT_MAX);
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;

        // Start from node 0
        dist[0] = 0;
        pq.push({0, 0}); // {distance, node}

        while (!pq.empty()) {
            auto [currentDist, node] = pq.top();
            pq.pop();

            // If we've already found a shorter path to this node, skip
            if (currentDist > dist[node]) {
                continue;
            }

            for (auto& [neighbor, weight] : adj[node]) {
                if (dist[node] + weight < dist[neighbor]) {
                    dist[neighbor] = dist[node] + weight;
                    pq.push({dist[neighbor], neighbor});
                }
            }
        }

        // Return the shortest distance to the last node, or -1 if unreachable
        return dist[n - 1] == INT_MAX ? -1 : dist[n - 1];
    }

    vector<int> shortestDistanceAfterQueries(int n, vector<vector<int>>& queries) {
        // Initialize the adjacency list
        unordered_map<int, vector<pair<int, int>>> adj;
        for (int i = 0; i < n - 1; i++) {
            adj[i].push_back({i + 1, 1}); // Weight is 1
        }

        int k = queries.size();
        vector<int> res(k);
        for (int i = 0; i < k; i++) {
            // Add the edge from the query to the adjacency list
            adj[queries[i][0]].push_back({queries[i][1], 1}); // Weight is 1
            // Find the shortest path from node 0 to node n-1 using Dijkstra
            res[i] = dijkstra(n, adj);
        }
        return res;
    }
};
```
