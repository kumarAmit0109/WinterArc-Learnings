# 206. Kth Smallest Element in an Array

### Problem Link

[GeeksforGeeks - Kth Smallest Element](https://www.geeksforgeeks.org/problems/kth-smallest-element5635/1?)

## Approach 1: Sorting to Find the Kth Smallest Element

### Idea/Intuition

To find the Kth smallest element in an array, we can leverage sorting. Once the array is sorted in ascending order, the Kth smallest element will be at the (K-1)th index due to 0-based indexing.

### Algorithm

1. **Sort the Array**:

   - Use an efficient sorting algorithm (like quicksort or mergesort, implemented via `sort()` in most programming libraries) to sort the array in ascending order.

2. **Access the Kth Element**:
   - After sorting, the Kth smallest element is located at the (K-1)th index.

### Time Complexity

- **Sorting**: \(O(n \log n)\), where `n` is the size of the array.
- **Accessing Element**: \(O(1)\).
- **Overall Time Complexity**: \(O(n \log n)\).

### Space Complexity

- **Auxiliary Space**: \(O(1)\) for in-place sorting.

### Implementation

```cpp
int kthSmallest(vector<int> &arr, int k) {
    // Sort the array in ascending order
    sort(arr.begin(), arr.end());
    // Return the k-1 indexed element (0-based indexing)
    return arr[k - 1];
}
```

## Approach 2: Using Min-Heap to Find the Kth Smallest Element

### Idea/Intuition

A Min-Heap is a data structure that always keeps the smallest element at the top. By inserting all elements into a Min-Heap and then removing the smallest \(k-1\) elements, the top of the heap will contain the Kth smallest element.

### Algorithm

1. **Create a Min-Heap**:

   - Use a priority queue with a `greater<int>` comparator to implement the Min-Heap.

2. **Insert Elements into the Min-Heap**:

   - Push all elements of the array into the Min-Heap.

3. **Extract \(k-1\) Elements**:

   - Pop the top element of the heap \(k-1\) times to remove the smallest \(k-1\) elements.

4. **Retrieve the Top Element**:
   - The top of the Min-Heap after \(k-1\) pops is the Kth smallest element.

### Time Complexity

- **Heap Insertion**: \(O(n \log n)\), where `n` is the size of the array.
- **Heap Extraction**: \(O(k \log n)\).
- **Overall Time Complexity**: \(O(n \log n)\).

### Space Complexity

- **Heap Space**: \(O(n)\) for storing elements in the heap.

### Implementation

```cpp
int kthSmallest(vector<int> &arr, int k) {
    // Create a min-heap using priority_queue
    priority_queue<int, vector<int>, greater<int>> minHeap;

    // Push all elements of the array into the min-heap
    for (int num : arr) {
        minHeap.push(num);
    }

    // Remove the top k-1 elements from the min-heap
    for (int i = 0; i < k - 1; i++) {
        minHeap.pop();
    }

    // The top element of the min-heap is the kth smallest element
    return minHeap.top();
}
```

## Approach 3: QuickSelect Algorithm

### Idea/Intuition

The QuickSelect algorithm is a variation of QuickSort that efficiently finds the Kth smallest element by partially sorting the array. Instead of sorting the entire array, it narrows the search space to the partition containing the desired element.

### Algorithm

1. **Partition Function**:

   - Select a pivot element randomly.
   - Rearrange the array such that all elements smaller than the pivot are on its left, and all larger elements are on its right.
   - Return the final index of the pivot.

2. **QuickSelect Function**:
   - Recursively call the partition function to narrow down the search space.
   - If the pivot index equals \(k-1\), the Kth smallest element is found.
   - If the pivot index is less than \(k-1\), search the right partition.
   - Otherwise, search the left partition.

### Time Complexity

- **Average Case**: \(O(n)\), where \(n\) is the size of the array, as each partition reduces the search space.
- **Worst Case**: \(O(n^2)\), if the pivot selection is poor (rare with randomization).

### Space Complexity

- **Auxiliary Space**: \(O(1)\), as the algorithm works in place.

### Implementation

```cpp
int partition(vector<int> &arr, int left, int right) {
    int pivotIndex = left + rand() % (right - left + 1); // Randomly select pivot
    int pivot = arr[pivotIndex];
    swap(arr[pivotIndex], arr[right]); // Move pivot to the end
    int partitionIndex = left;

    for (int i = left; i < right; i++) {
        if (arr[i] < pivot) {
            swap(arr[i], arr[partitionIndex]);
            partitionIndex++;
        }
    }
    swap(arr[partitionIndex], arr[right]); // Move pivot to its correct position
    return partitionIndex;
}

// Helper function to find the kth smallest element
int quickSelect(vector<int> &arr, int left, int right, int k) {
    if (left <= right) {
        int partitionIndex = partition(arr, left, right);

        if (partitionIndex == k) {
            return arr[partitionIndex];
        } else if (partitionIndex < k) {
            return quickSelect(arr, partitionIndex + 1, right, k);
        } else {
            return quickSelect(arr, left, partitionIndex - 1, k);
        }
    }
    return -1; // This case won't occur if k is valid
}

// Function to find the kth smallest element
int kthSmallest(vector<int> &arr, int k) {
    return quickSelect(arr, 0, arr.size() - 1, k - 1); // k-1 for 0-based index
}
```

# 207. Merge K Sorted Arrays

### Problem Link

[GeeksforGeeks - Merge K Sorted Arrays](https://www.geeksforgeeks.org/problems/merge-k-sorted-arrays/1?)

## Approach 1: Flatten and Sort

### Idea/Intuition

To merge \(K\) sorted arrays, a straightforward solution is to combine all elements into a single array and sort it. Sorting ensures the final result is ordered.

### Algorithm

1. **Flatten the Arrays**:

   - Iterate through each array and push all elements into a temporary array.

2. **Sort the Array**:

   - Use a standard sorting algorithm to sort the temporary array.

3. **Return Result**:
   - The sorted array represents the merged \(K\) sorted arrays.

### Time Complexity

- **Flattening**: \(O(K^2)\), where \(K\) is the number of arrays, each of size \(K\).
- **Sorting**: \(O((K^2) \log (K^2))\).
- **Overall**: \(O(K^2 \log K^2)\).

### Space Complexity

- \(O(K^2)\) for the temporary array.

### Implementation

```cpp
vector<int> mergeKArrays(vector<vector<int>> arr, int K)
{
    vector<int> temp;

    // Push all elements into the temporary array
    for (int i = 0; i < K; i++) {
        for (int j = 0; j < K; j++) {
            temp.push_back(arr[i][j]);
        }
    }

    // Sort the array
    sort(temp.begin(), temp.end());
    return temp;
}
```

## Approach 2: Using Min-Heap

### Idea/Intuition

A Min-Heap can efficiently merge \(K\) sorted arrays by always extracting the smallest element and pushing the next element from the same array into the heap. This ensures that elements are processed in sorted order.

### Algorithm

1. **Min-Heap Structure**:

   - Create a structure `Node` to store the value, row index, and column index of each element in the heap.
   - Define a comparator to maintain a Min-Heap based on the smallest value.

2. **Initialization**:

   - Insert the first element from each array into the Min-Heap.

3. **Process the Heap**:

   - Extract the smallest element from the heap and append it to the result.
   - If the next element in the same array exists, push it into the heap.

4. **Repeat Until the Heap is Empty**:
   - Continue extracting and inserting elements until the heap is empty.

### Time Complexity

- **Heap Initialization**: \(O(K \log K)\), where \(K\) is the number of arrays.
- **Processing**: \(O(K^2 \log K)\), since there are \(K^2\) elements and each heap operation takes \(O(\log K)\).
- **Overall**: \(O(K^2 \log K)\).

### Space Complexity

- **Heap Space**: \(O(K)\), for storing \(K\) elements in the heap.

### Implementation

```cpp
class Solution
{
public:
    // Node structure to store heap elements
    struct Node {
        int value;
        int row;
        int col;

        Node(int v, int r, int c) : value(v), row(r), col(c) {}
    };

    // Comparator for the Min-Heap
    struct Compare {
        bool operator()(Node &a, Node &b) {
            return a.value > b.value;
        }
    };

    vector<int> mergeKArrays(vector<vector<int>> arr, int K)
    {
        priority_queue<Node, vector<Node>, Compare> minHeap;
        vector<int> result;

        // Push the first element of each array into the heap
        for (int i = 0; i < K; i++) {
            minHeap.push(Node(arr[i][0], i, 0));
        }

        // Process the heap
        while (!minHeap.empty()) {
            Node current = minHeap.top();
            minHeap.pop();
            result.push_back(current.value);

            // If the next element in the same row exists, push it into the heap
            if (current.col + 1 < K) {
                minHeap.push(Node(arr[current.row][current.col + 1], current.row, current.col + 1));
            }
        }

        return result;
    }
};
```

# 208. Merge K Sorted Lists

### Problem Link

[LeetCode - Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists)

## Approach 1: Collect, Sort, and Reconstruct

### Idea/Intuition

This approach collects all the values from the \(K\) sorted linked lists into a single array, sorts the array, and then reconstructs a single merged sorted linked list.

### Algorithm

1. **Collect All Values**:

   - Traverse through each linked list and collect all node values into a vector.

2. **Sort the Values**:

   - Sort the collected values using a sorting algorithm.

3. **Reconstruct the Linked List**:
   - Create a new linked list using the sorted values.

### Time Complexity

- **Collecting Values**: \(O(N)\), where \(N\) is the total number of nodes across all linked lists.
- **Sorting**: \(O(N \log N)\), to sort all values.
- **Reconstructing**: \(O(N)\), to create the new linked list.
- **Overall**: \(O(N \log N)\).

### Space Complexity

- **Auxiliary Space**: \(O(N)\), for storing all values in a vector.

### Implementation

```cpp
ListNode* mergeKLists(vector<ListNode*>& lists) {
    vector<int> allValues;

    // Collect all values from the k linked lists
    for (ListNode* list : lists) {
        while (list != nullptr) {
            allValues.push_back(list->val);
            list = list->next;
        }
    }

    // Sort all values
    sort(allValues.begin(), allValues.end());

    // Create the merged sorted linked list
    ListNode* dummy = new ListNode(-1);
    ListNode* current = dummy;

    for (int value : allValues) {
        current->next = new ListNode(value);
        current = current->next;
    }

    return dummy->next;
}
```

## Approach 2: Min-Heap for Efficient Merging

### Idea/Intuition

A Min-Heap can be used to efficiently merge \(K\) sorted linked lists by always extracting the smallest node among the current heads of the lists. This ensures that the merged list is constructed in sorted order.

### Algorithm

1. **Min-Heap Structure**:

   - Use a priority queue (Min-Heap) to store the nodes, ordered by their values.

2. **Initialization**:

   - Insert the head of each linked list into the Min-Heap.

3. **Process the Heap**:

   - Extract the smallest node from the heap and append it to the result.
   - If the extracted node has a next node, push it into the heap.

4. **Repeat Until the Heap is Empty**:
   - Continue extracting and inserting nodes until all lists are processed.

### Time Complexity

- **Heap Initialization**: \(O(K \log K)\), where \(K\) is the number of linked lists.
- **Processing**: \(O(N \log K)\), where \(N\) is the total number of nodes across all lists. Each heap operation (insertion or removal) takes \(O(\log K)\).
- **Overall**: \(O(N \log K)\).

### Space Complexity

- **Heap Space**: \(O(K)\), for storing up to \(K\) nodes in the heap.

### Implementation

```cpp
struct Compare {
    bool operator()(ListNode* a, ListNode* b) {
        return a->val > b->val;
    }
};

ListNode* mergeKLists(vector<ListNode*>& lists) {
    priority_queue<ListNode*, vector<ListNode*>, Compare> minHeap;

    // Push the head of each linked list into the min-heap
    for (ListNode* list : lists) {
        if (list != nullptr) {
            minHeap.push(list);
        }
    }

    // Dummy node to help with merging
    ListNode* dummy = new ListNode(-1);
    ListNode* current = dummy;

    // Process the min-heap
    while (!minHeap.empty()) {
        ListNode* smallest = minHeap.top();
        minHeap.pop();

        // Add the smallest node to the result
        current->next = smallest;
        current = current->next;

        // If there are more nodes in the same list, add them to the heap
        if (smallest->next != nullptr) {
            minHeap.push(smallest->next);
        }
    }

    // Return the merged list starting from the next of dummy node
    return dummy->next;
}
```

# 209. Replace Elements by Its Rank in the Array

### Problem Link

[GeeksforGeeks - Replace Elements by Its Rank in the Array](https://www.geeksforgeeks.org/problems/replace-elements-by-its-rank-in-the-array/)

## Approach 1: Sorting and HashMap

### Idea/Intuition

Sort the elements of the array to assign ranks based on their position in the sorted order. Use a hash map to store the rank of each unique element and replace each element in the original array with its rank.

### Algorithm

1. **Copy and Sort**:

   - Create a copy of the array and sort it.

2. **Map Ranks**:

   - Traverse the sorted array and assign ranks to each unique element using a hash map.

3. **Replace with Ranks**:
   - Iterate through the original array and replace each element with its corresponding rank from the map.

### Time Complexity

- **Sorting**: \(O(N log N)\), where \(N\) is the size of the array.
- **Mapping Ranks**: \(O(N)\), to assign ranks.
- **Replacement**: \(O(N)\), to replace elements with their ranks.
- **Overall**: \(O(N log N)\).

### Space Complexity

- **Auxiliary Space**: \(O(N)\), for the temporary sorted array and hash map.

### Implementation

```cpp
vector<int> replaceWithRank(vector<int> &arr, int N) {
    // Create a temporary vector and sort it
    vector<int> temp(arr);
    sort(temp.begin(), temp.end());

    // Create a map to assign ranks
    unordered_map<int, int> m;
    int rank = 1;
    for (int i = 0; i < N; i++) {
        if (m.find(temp[i]) == m.end()) {
            m[temp[i]] = rank++;
        }
    }

    // Replace elements in the array with their ranks
    vector<int> ans;
    for (int i = 0; i < N; i++) {
        ans.push_back(m[arr[i]]);
    }

    return ans;
}
```

## Approach 2: Min-Heap (Priority Queue)

### Idea/Intuition

Use a min-heap to extract elements in ascending order. Assign ranks as elements are popped and store them in a hash map. Replace each element in the original array with its rank.

### Algorithm

1. **Push to Min-Heap**:

   - Insert all elements of the array into a min-heap.

2. **Map Ranks**:

   - Extract elements from the heap and assign ranks to each unique element using a hash map.

3. **Replace with Ranks**:
   - Iterate through the original array and replace each element with its corresponding rank from the map.

### Time Complexity

- **Heap Operations**: \(O(N log N)\), for pushing and popping all elements.
- **Replacement**: \(O(N)\), to replace elements with their ranks.
- **Overall**: \(O(N log N)\).

### Space Complexity

- **Heap Space**: \(O(N)\), for the min-heap.
- **Auxiliary Space**: \(O(N)\), for the hash map.

### Implementation

```cpp
vector<int> replaceWithRank(vector<int> &arr, int N) {
    // Min-heap (priority queue)
    priority_queue<int, vector<int>, greater<int>> minHeap;

    // Push all elements into the min-heap
    for (int num : arr) {
        minHeap.push(num);
    }

    // Map to store ranks
    unordered_map<int, int> rankMap;
    int rank = 1;

    // Assign ranks while extracting from the min-heap
    while (!minHeap.empty()) {
        int current = minHeap.top();
        minHeap.pop();
        if (rankMap.find(current) == m.end()) {
            rankMap[current] = rank++;
        }
    }

    // Replace elements in the original array with their ranks
    vector<int> ans(N);
    for (int i = 0; i < N; i++) {
        ans[i] = rankMap[arr[i]];
    }

    return ans;
}
```

# 210. Task Scheduler

### Problem Link

[LeetCode - Task Scheduler](https://leetcode.com/problems/task-scheduler/)

## Approach 1: Priority Queue for Task Scheduling

### Idea/Intuition

Use a max-heap to prioritize tasks with the highest frequencies. Schedule tasks in chunks of \(n+1\) to ensure the cooling period is respected. Push remaining tasks back into the heap for subsequent cycles.

### Algorithm

1. **Frequency Map**:

   - Count the frequency of each task and store it in a map.

2. **Max-Heap**:

   - Use a priority queue to store frequencies in descending order.

3. **Process Cycles**:

   - In each cycle, schedule up to \(n+1\) tasks or as many as available.
   - Decrease their frequency and push remaining tasks back into the heap.

4. **Count Total Time**:
   - Add either \(n+1\) (cooling period + tasks) or the number of tasks processed if the heap becomes empty.

### Time Complexity

- **Frequency Map**: \(O(N)\), where \(N\) is the number of tasks.
- **Heap Operations**: \(O(D log 26)\), where \(D\) is the number of cycles.
- **Overall**: \(O(N + Dlog 26)\).

### Space Complexity

- **Auxiliary Space**: \(O(26)\), for the frequency map and heap.

### Implementation

```cpp
int leastInterval(vector<char>& tasks, int n) {
    vector<int> freqMap(26, 0); // Frequency map for 26 characters
    for (char ch : tasks) {
        freqMap[ch - 'A']++;
    }

    // Max-heap (priority queue) to store task frequencies
    priority_queue<int> pq;
    for (int freq : freqMap) {
        if (freq > 0) {
            pq.push(freq);
        }
    }

    int count = 0;

    while (!pq.empty()) {
        vector<int> temp;

        // Process up to (n + 1) tasks in one cycle
        for (int i = 0; i <= n; i++) {
            if (!pq.empty()) {
                temp.push_back(pq.top() - 1); // Decrement frequency
                pq.pop();
            }
        }

        // Push remaining tasks back into the queue
        for (int freq : temp) {
            if (freq > 0) {
                pq.push(freq);
            }
        }

        // If the queue is empty, add the actual tasks processed
        count += pq.empty() ? temp.size() : n + 1;
    }

    return count;
}
```

## Approach 2: Math-Based Optimization

### Idea/Intuition

Focus on the most frequent task. The minimum intervals depend on the chunks of this task, with idle slots filled by other tasks or left empty.

### Algorithm

1. **Frequency Map**:

   - Count the frequency of each task.

2. **Sort Tasks**:

   - Sort tasks by frequency in ascending order.

3. **Calculate Idle Slots**:

   - Compute chunks as \( freq[max] - 1 \).
   - Calculate idle slots as \( chunks times n \).
   - Reduce idle slots by filling them with other tasks.

4. **Return Result**:
   - If idle slots are negative, return the total task count. Otherwise, return \( tasks.size() + idle slots \).

### Time Complexity

- **Frequency Calculation**: \(O(N)\).
- **Sorting**: \(O(26 log 26)\).
- **Overall**: \(O(N + log 26)\).

### Space Complexity

- **Auxiliary Space**: \(O(26)\), for the frequency array.

### Implementation

```cpp
int leastInterval(vector<char>& tasks, int n) {
    int freq[26] = {0};
    for (char task : tasks) {
        freq[task - 'A']++;
    }
    sort(begin(freq), end(freq));
    int chunk = freq[25] - 1;
    int idle = chunk * n;

    for (int i = 24; i >= 0; i--) {
        idle -= min(chunk, freq[i]);
    }

    return idle < 0 ? tasks.size() : tasks.size() + idle;
}
```
