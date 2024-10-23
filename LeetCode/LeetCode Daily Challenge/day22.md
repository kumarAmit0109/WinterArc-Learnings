# Problem: Kth Largest Sum in a Binary Tree

[LeetCode Problem Link](https://leetcode.com/problems/kth-largest-sum-in-a-binary-tree)

## Approach

1. **Level Order Traversal**:

   - Use a queue to perform a level order traversal of the binary tree.
   - Calculate the sum of values at each level.

2. **Min-Heap for K Largest Sums**:

   - Maintain a min-heap of size \(k\) to track the largest sums.
   - If the size of the heap is less than \(k\), push the current level sum into the heap.
   - If the size exceeds \(k\), compare the current level sum with the smallest element in the heap. If the current sum is larger, pop the smallest and push the current sum.

3. **Return the Kth Largest Sum**:
   - After completing the traversal, the top element of the min-heap will be the \(k\)th largest sum.

### Time Complexity

- **Time**: \(O(n + k \log k)\) where \(n\) is the number of nodes in the binary tree and \(k\) is the size of the heap.
- **Space**: \(O(k)\) for the min-heap.

### C++ Code Implementation

```cpp
class Solution {
public:
    long long kthLargestLevelSum(TreeNode* root, int k) {
        if (!root) return 0;

        queue<TreeNode*> q;
        priority_queue<long long, vector<long long>, greater<long long>> minHeap;
        q.push(root);

        while (!q.empty()) {
            long long levelSum = 0;
            int size = q.size();

            // Calculate the sum of the current level
            for (int i = 0; i < size; ++i) {
                TreeNode* node = q.front(); q.pop();
                levelSum += node->val;

                // Add child nodes to the queue
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }

            // Maintain a min-heap of size k
            if (minHeap.size() < k) {
                minHeap.push(levelSum);
            } else if (levelSum > minHeap.top()) {
                minHeap.pop();
                minHeap.push(levelSum);
            }
        }

        // The top of the min-heap is the kth largest sum
        return minHeap.top();
    }
};
```
