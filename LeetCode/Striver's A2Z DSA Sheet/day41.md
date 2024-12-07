# 201. Implementation of Priority Queue using Binary Heap

### Problem Link

[GeeksforGeeks - Implementation of Priority Queue using Binary Heap](https://www.geeksforgeeks.org/problems/implementation-of-priority-queue-using-binary-heap/1)

## Approach: Extracting Maximum from Binary Heap

### Idea/Intuition

The priority queue uses a binary heap structure. In this approach, we extract the maximum element from the heap. The root of the binary heap always contains the maximum value in a max heap. When we extract the maximum element, we replace the root with the last element and restore the heap property by shifting down the new root.

### Algorithm

1. If the heap is empty (i.e., `s == -1`), return -1.
2. Otherwise, store the root element as `maxVal` (which is the maximum element).
3. Replace the root with the last element in the heap (i.e., `H[0] = H[s]`).
4. Decrement `s`, the size of the heap.
5. Call the `shiftDown()` function to restore the heap property, ensuring that the largest value is at the root.
6. Return the extracted maximum value.

### Time Complexity

- Extracting the maximum element requires shifting down the element to restore the heap property, which takes \(O(\log n)\), where \(n\) is the number of elements in the heap.

### Space Complexity

- The space complexity is \(O(n)\) for storing the heap elements.

### Implementation

```cpp
class Solution {
public:
    // Function to extract the maximum element from the heap
    int extractMax() {
        if (s == -1) {
            // Heap is empty
            return -1;
        }

        // The maximum element is the root of the heap
        int maxVal = H[0];

        // Replace the root with the last element
        H[0] = H[s];
        s--;

        // Restore the heap property by shifting down
        shiftDown(0);

        // Return the extracted maximum value
        return maxVal;
    }
};
```

# 202. Operations on Binary Min Heap

### Problem Link

[GeeksforGeeks - Operations on Binary Min Heap](https://www.geeksforgeeks.org/problems/operations-on-binary-min-heap/1)

## Approach: Extracting Minimum, Deleting Key, Inserting Key in Min Heap

### Idea/Intuition

In a binary min-heap, the root element always contains the minimum value. The heap is a complete binary tree where each parent is smaller than or equal to its children. Various operations such as extracting the minimum, deleting a key, inserting a new key, and decreasing a key value can be efficiently implemented.

### Algorithm

1. **Extract Min**:

   - If the heap is empty, return -1.
   - Remove the root element (which is the minimum value).
   - Replace the root with the last element in the heap.
   - Decrease the heap size.
   - Restore the heap property using `MinHeapify`.

2. **Delete Key**:

   - To delete a key at index `i`, decrease its value to negative infinity (or a very small number).
   - Extract the minimum value (which would be the key we wanted to delete).

3. **Insert Key**:

   - If the heap is full, return overflow.
   - Insert the new key at the end of the heap.
   - Restore the heap property by adjusting the position of the new key using `decreaseKey`.

4. **Decrease Key**:

   - Decrease the value of the key at index `i`.
   - Shift the key up until the heap property is restored.

5. **MinHeapify**:
   - To restore the heap property when the tree is out of balance, compare the parent node with its children and swap the nodes as necessary.

### Time Complexity

- **Extract Min**: \(O(\log n)\) (due to `MinHeapify`)
- **Delete Key**: \(O(\log n)\) (due to `extractMin` and `decreaseKey`)
- **Insert Key**: \(O(\log n)\) (due to `decreaseKey`)
- **MinHeapify**: \(O(\log n)\) (due to traversal down the heap)

### Space Complexity

- \(O(n)\) for storing the heap elements.

### Implementation

```cpp
int MinHeap::extractMin()
{
    if (heap_size == 0) {
        return -1;  // Heap is empty
    }

    // Root contains the minimum value
    int root = harr[0];

    // If there's more than one element, replace root with the last element
    harr[0] = harr[heap_size - 1];
    heap_size--;

    // Restore the heap property by calling MinHeapify
    MinHeapify(0);

    return root;
}

// Function to delete a key at ith index.
void MinHeap::deleteKey(int i)
{
    // Decrease the value at index i to a very small number (negative infinity)
    decreaseKey(i, INT_MIN);

    // Extract the minimum (which is now at the root)
    extractMin();
}

// Function to insert a value in Heap.
void MinHeap::insertKey(int k)
{
    if (heap_size == capacity) {
        cout << "Overflow: Could not insertKey\n";
        return;
    }

    // Insert the new key at the end
    heap_size++;
    harr[heap_size - 1] = k;

    // Restore the heap property by shifting the inserted element up
    decreaseKey(heap_size - 1, k);
}

// Function to change value at ith index and store that value at first index.
void MinHeap::decreaseKey(int i, int new_val)
{
    harr[i] = new_val;
    while (i != 0 && harr[parent(i)] > harr[i]) {
        swap(harr[i], harr[parent(i)]);
        i = parent(i);
    }
}

/* You may call below MinHeapify function in
   above codes. Please do not delete this code
   if you are not writing your own MinHeapify */
void MinHeap::MinHeapify(int i)
{
    int l = left(i);
    int r = right(i);
    int smallest = i;
    if (l < heap_size && harr[l] < harr[i]) smallest = l;
    if (r < heap_size && harr[r] < harr[smallest]) smallest = r;
    if (smallest != i) {
        swap(harr[i], harr[smallest]);
        MinHeapify(smallest);
    }
}
```

# 203. Does Array Represent Heap

### Problem Link

[GeeksforGeeks - Does Array Represent Heap](https://www.geeksforgeeks.org/problems/does-array-represent-heap4345/1)

## Approach: Check if an Array Represents a Max Heap

### Idea/Intuition

A max heap is a complete binary tree where each parent node is greater than or equal to its children. Given an array representation of a binary tree, the task is to check whether the array represents a valid max heap.

### Algorithm

1. **Loop through the nodes**:

   - For each node (except the leaf nodes), check if it satisfies the max heap property:
     - The value at the current node must be greater than or equal to its left and right children.
     - If it is smaller than either of the children, the array does not represent a valid max heap.

2. **Base Case**:
   - If all non-leaf nodes satisfy the max heap property, the array represents a valid max heap.

### Time Complexity

- **Time Complexity**: \(O(n)\), where `n` is the number of nodes in the heap. We visit each node once and check its left and right children.
- **Space Complexity**: \(O(1)\), as the check is performed in-place without using extra space.

### Implementation

```cpp
class Solution {
public:
    bool isMaxHeap(int arr[], int n)
    {
        // Loop through each node (except the leaf nodes) to check the heap property
        for (int i = 0; i < (n / 2); i++) {
            int left = 2 * i + 1;  // Left child index
            int right = 2 * i + 2; // Right child index

            // Check if the current node is greater than or equal to its left child
            if (left < n && arr[i] < arr[left]) {
                return false; // If the current node is smaller than the left child
            }

            // Check if the current node is greater than or equal to its right child
            if (right < n && arr[i] < arr[right]) {
                return false; // If the current node is smaller than the right child
            }
        }

        // If all checks pass, the array represents a valid Max Heap
        return true;
    }
};
```

# 204. Convert Min Heap to Max Heap

### Problem Link

[GeeksforGeeks - Convert Min Heap to Max Heap](https://www.geeksforgeeks.org/problems/convert-min-heap-to-max-heap-1666385109/1)

## Approach: Convert Min Heap to Max Heap Using Heapify

### Idea/Intuition

To convert a Min Heap to a Max Heap, we need to ensure that for every node, its value is greater than or equal to its children, and the entire tree follows the Max Heap property. We can achieve this by performing a series of heapify operations, starting from the last non-leaf node and moving upwards.

### Algorithm

1. **Heapify Function**:

   - This function ensures that the subtree rooted at a given node satisfies the Max Heap property by comparing the node with its left and right children. If the node violates the Max Heap property, it swaps with the largest of its children and recursively heapifies the affected subtree.

2. **Convert Min Heap to Max Heap**:
   - Start from the last non-leaf node (i.e., `N/2 - 1`) and move upwards to the root. For each node, perform the heapify operation to restore the Max Heap property.

### Time Complexity

- **Time Complexity**: \(O(n)\), where `n` is the number of nodes in the heap. We perform a heapify operation on each non-leaf node, which takes \(O(\log n)\), and there are \(n/2\) non-leaf nodes.
- **Space Complexity**: \(O(1)\), as the operations are done in place without requiring additional space.

### Implementation

```cpp
class Solution {
public:
    // Helper function to perform heapify
    void heapify(vector<int>& arr, int N, int i) {
        int largest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;

        // If left child exists and is larger than current largest
        if (left < N && arr[left] > arr[largest]) {
            largest = left;
        }

        // If right child exists and is larger than current largest
        if (right < N && arr[right] > arr[largest]) {
            largest = right;
        }

        // If largest is not the current node, swap and heapify the affected subtree
        if (largest != i) {
            swap(arr[i], arr[largest]);
            heapify(arr, N, largest); // Recursively heapify the affected subtree
        }
    }

    // Function to convert the min-heap to a max-heap
    void convertMinToMaxHeap(vector<int>& arr, int N) {
        // Start from the last non-leaf node and move upwards to the root
        for (int i = N / 2 - 1; i >= 0; i--) {
            heapify(arr, N, i); // Heapify each node
        }
    }
};
```

# 205. Kth Largest Element in an Array (Approach 1)

### Problem Link

[LeetCode - Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)

## Approach 1: Sorting the Array in Descending Order

### Idea/Intuition

In this approach, we simply sort the array in descending order and return the element at the \(k-1\) index (since arrays are 0-indexed).

### Algorithm

1. **Sort the array** in descending order using `greater<int>()` in the `sort()` function.
2. **Return the \(k-1\)th element** from the sorted array.

### Time Complexity

- **Time Complexity**: \(O(n \log n)\), where `n` is the length of the array. This is due to the sorting step.
- **Space Complexity**: \(O(1)\), as sorting is done in-place.

### Implementation

```cpp
int findKthLargest(vector<int>& nums, int k) {
    sort(begin(nums), end(nums), greater<int>());
    return nums[k - 1];
}
```

## Approach 2: Using a Min-Heap

### Idea/Intuition

In this approach, we use a **min-heap** of size `k` to keep track of the `k` largest elements. The top of the min-heap will always store the smallest element among the `k` largest elements. Once the heap reaches size `k`, we remove the smallest element (if needed), ensuring that we only keep the `k` largest elements.

### Algorithm

1. **Initialize a min-heap** of size `k`.
2. **Iterate through the array**:
   - Push each element into the heap.
   - If the heap size exceeds `k`, pop the smallest element.
3. **Return the top of the heap**, which will be the `k`th largest element.

### Time Complexity

- **Time Complexity**: \(O(n \log k)\), where `n` is the length of the array. Each insertion and removal from the heap takes \(O(\log k)\).
- **Space Complexity**: \(O(k)\), as we maintain a heap of size `k`.

### Implementation

```cpp
int findKthLargest(vector<int>& nums, int k) {
    // Min-heap to store k largest elements
    priority_queue<int, vector<int>, greater<int>> minh;

    // Add elements to the heap
    for (int i : nums) {
        minh.push(i);
        if (minh.size() > k) {
            minh.pop();
        }
    }

    // Return the top element of the heap
    return minh.top();
}
```

# Kth Largest Element in an Array (Approach 3)

### Problem Link

[LeetCode - Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)

---

## Approach 3: Using Quickselect (Partitioning Approach)

### Idea/Intuition

This approach utilizes the **Quickselect algorithm**, a variant of the quicksort algorithm, to find the `k`th largest element in the array. The algorithm works by partitioning the array around a pivot element, ensuring that elements larger than the pivot are on the right side, and elements smaller than the pivot are on the left. Based on the pivot’s position, we can efficiently narrow the search range.

1. **Partition the array** around a pivot element.
2. If the pivot is at the `k-1`th index, we have found the `k`th largest element.
3. If the pivot is greater than `k-1`, continue searching in the left partition.
4. If the pivot is less than `k-1`, continue searching in the right partition.
5. Repeat until the pivot is at the correct position.

This approach offers an average-case time complexity of \(O(n)\), making it efficient for large arrays.

### Algorithm

1. **Partition the array** around a pivot using the `partition` function.
2. **Check the pivot’s position**:
   - If the pivot is at index `k-1`, return it as the `k`th largest element.
   - Otherwise, adjust the search range by either moving left or right depending on the pivot’s position.
3. **Repeat the partitioning process** until the correct element is found.

### Time Complexity

- **Average Time Complexity**: \(O(n)\), where `n` is the size of the array.
- **Worst Time Complexity**: \(O(n^2)\) in the case of unbalanced partitioning, although this is rare in practice.
- **Space Complexity**: \(O(1)\), since quickselect uses constant extra space.

---

### Implementation

```cpp
int partition(vector<int>& nums, int left, int right) {
    int pivot = nums[left];  // Choose pivot element
    int low = left + 1;
    int high = right;

    while (low <= high) {
        if (nums[low] < pivot && nums[high] > pivot) {
            swap(nums[low], nums[high]);
            low++;
            high--;
        }

        if (nums[low] >= pivot) low++;
        if (nums[high] <= pivot) high--;
    }

    swap(nums[left], nums[high]);
    return high;  // Return the pivot index
}

int findKthLargest(vector<int>& nums, int k) {
    int n = nums.size();
    int left = 0, right = n - 1;
    int pivotIdx = 0;

    while (true) {
        pivotIdx = partition(nums, left, right);

        if (pivotIdx == k - 1) {
            break;
        } else if (pivotIdx > k - 1) {
            right = pivotIdx - 1;
        } else {
            left = pivotIdx + 1;
        }
    }

    return nums[pivotIdx];
}
```
