# Find Champion II

**Problem Link**: [Find Champion II](https://leetcode.com/problems/find-champion-ii/)

## Approach: In-degree Calculation

### Idea

The champion is a node with no incoming edges (in-degree of 0) and all other nodes have at least one incoming edge pointing to them. The algorithm:

1. **Calculate In-degree**: Find the in-degree for each node.
2. **Find Champion**: A node with in-degree 0 is a potential champion, but only one such node should exist.

### Algorithm

1. Initialize an in-degree array where each index represents a node, and its value denotes the number of incoming edges.
2. Traverse all edges and update the in-degree array.
3. Find a node with in-degree 0. If there is more than one such node, return -1, indicating no unique champion.
4. Return the node with in-degree 0 if it is unique.

### Time Complexity

- **O(E + V)**:
  - E is the number of edges and V is the number of nodes.

### Space Complexity

- **O(V)**: Space for the in-degree array.

### Code

```cpp
class Solution {
public:
    int findChampion(int n, vector<vector<int>>& edges) {
        vector<int> inDegree(n, 0);

        // Calculate in-degrees of all nodes
        for (const auto& edge : edges) {
            inDegree[edge[1]]++;
        }

        // Find nodes with in-degree 0
        int champion = -1;
        for (int i = 0; i < n; ++i) {
            if (inDegree[i] == 0) {
                // If a second champion is found, return -1
                if (champion != -1) {
                    return -1;
                }
                champion = i;
            }
        }

        return champion;
    }
};
```
