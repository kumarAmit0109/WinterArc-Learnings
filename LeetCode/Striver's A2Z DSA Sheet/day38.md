# 186. The Celebrity Problem

**Problem Link**: [https://www.geeksforgeeks.org/problems/the-celebrity-problem/1](https://www.geeksforgeeks.org/problems/the-celebrity-problem/1)

## Approach 1: Using Indegree and Outdegree

### Idea

- Calculate the **indegree** (number of people who know a person) and **outdegree** (number of people a person knows) for each individual.
- A celebrity has an indegree of \( n-1 \) and outdegree of \( 0 \).

### Algorithm

1. Initialize two vectors: `indegree` and `outdegree` of size \( n \).
2. Iterate over the matrix:
   - If `mat[i][j] == 1`, increment `outdegree[i]` and `indegree[j]`.
3. Iterate through all individuals:
   - If any individual satisfies `indegree[i] == n-1` and `outdegree[i] == 0`, they are the celebrity.
4. Return the index of the celebrity or `-1` if no celebrity exists.

### Complexity

- **Time Complexity**: \( O(n^2) \) for traversing the matrix.
- **Space Complexity**: \( O(n) \) for `indegree` and `outdegree`.

### Implementation

```cpp
class Solution {
  public:
    // Function to find if there is a celebrity in the party or not.
    int celebrity(vector<vector<int>>& mat) {
        int n = mat.size();
        vector<int> indegree(n, 0);  // To store the number of people who know a person
        vector<int> outdegree(n, 0); // To store the number of people a person knows

        // Step 1: Calculate indegree and outdegree for each person
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (mat[i][j] == 1) {
                    outdegree[i]++;   // i knows j
                    indegree[j]++;    // j is known by i
                }
            }
        }

        // Step 2: Find the person with indegree n-1 and outdegree 0
        for (int i = 0; i < n; i++) {
            if (indegree[i] == n - 1 && outdegree[i] == 0) {
                return i; // Celebrity found
            }
        }

        return -1; // No celebrity found
    }
};
```

## Approach 2: Using a Stack

### Idea

- Use a **stack** to narrow down the potential celebrity by comparing pairs of individuals:
  - If `A` knows `B`, then `A` cannot be the celebrity.
  - If `A` does not know `B`, then `B` cannot be the celebrity.
- After finding a candidate, verify whether they are a celebrity by checking the matrix.

### Algorithm

1. Push all individuals into a stack.
2. While the stack has more than one person:
   - Pop two individuals, `A` and `B`.
   - If `A` knows `B`, push `B` back into the stack.
   - Otherwise, push `A` back into the stack.
3. The remaining person in the stack is the candidate.
4. Verify the candidate:
   - Ensure the candidate is known by everyone.
   - Ensure the candidate knows no one.

### Complexity

- **Time Complexity**: \(O(n)\), where \(n\) is the number of people.
- **Space Complexity**: \(O(n)\), for the stack.

### Implementation

```cpp
class Solution {
  public:
    // Function to find if there is a celebrity in the party or not.
    int celebrity(vector<vector<int>>& mat) {
        int n = mat.size();
        stack<int> s;

        // Step 1: Push all persons into the stack
        for (int i = 0; i < n; i++) {
            s.push(i);
        }

        // Step 2: Find the potential celebrity
        while (s.size() > 1) {
            int A = s.top(); s.pop();
            int B = s.top(); s.pop();

            if (mat[A][B] == 1) {
                // A knows B, so A can't be a celebrity
                s.push(B);
            } else {
                // A doesn't know B, so B can't be a celebrity
                s.push(A);
            }
        }

        // Step 3: Verify if the remaining candidate is a celebrity
        int candidate = s.top();

        // Check if the candidate is known by everyone and knows no one
        for (int i = 0; i < n; i++) {
            if (i != candidate && (mat[candidate][i] == 1 || mat[i][candidate] == 0)) {
                return -1; // Not a celebrity
            }
        }

        return candidate; // Celebrity found
    }
};
```

# 187. LRU Cache

**Problem Link**: [https://leetcode.com/problems/lru-cache/description/](https://leetcode.com/problems/lru-cache/description/)

## Approach 1: Using a Vector to Store Key-Value Pairs

### Idea

- Use a **vector** to store key-value pairs.
- Maintain the order of keys in the vector to reflect their usage:
  - Move the accessed key to the back when `get` is called.
  - Remove the least recently used key (front of the vector) when the cache is full in `put`.

### Algorithm

1. For `get`:
   - Iterate through the vector to find the key.
   - If found, move the key-value pair to the back and return its value.
   - If not found, return `-1`.
2. For `put`:
   - Check if the key already exists in the vector.
   - If it exists, update its value and move it to the back.
   - If it does not exist:
     - If the cache is full, remove the least recently used key (front of the vector).
     - Add the new key-value pair to the back.

### Complexity

- **Time Complexity**:
  - `get`: \(O(n)\), where \(n\) is the number of elements in the cache, due to iteration over the vector.
  - `put`: \(O(n)\) for finding and erasing an existing key.
- **Space Complexity**: \(O(n)\), where \(n\) is the capacity of the cache.

### Implementation

```cpp
class LRUCache {
public:
    vector<pair<int, int>> cache; // Vector to store key-value pairs
    int capacity; // Maximum capacity of the cache

    LRUCache(int capacity) {
        this->capacity = capacity;
    }

    int get(int key) {
        // Iterate over the cache to find the key
        for (int i = 0; i < cache.size(); i++) {
            if (cache[i].first == key) {
                int value = cache[i].second;
                // Move the accessed key-value pair to the back to indicate recent use
                pair<int, int> temp = cache[i];
                cache.erase(cache.begin() + i);
                cache.push_back(temp);
                return value;
            }
        }
        return -1; // Key not found
    }

    void put(int key, int value) {
        // Check if the key already exists
        for (int i = 0; i < cache.size(); i++) {
            if (cache[i].first == key) {
                // Update the value and move the pair to the back
                cache.erase(cache.begin() + i);
                cache.push_back({key, value});
                return;
            }
        }
        // If the cache is full, remove the least recently used (front of the vector)
        if (cache.size() == capacity) {
            cache.erase(cache.begin());
        }
        // Add the new key-value pair to the back
        cache.push_back({key, value});
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

## Approach 2: Using a Doubly Linked List and a Map

### Idea

- Use a **doubly linked list (DLL)** to store keys in the order of their usage (most recent at the front).
- Use a **map** to store key-value pairs, where the key maps to an iterator pointing to its position in the DLL.
- The most recently used key is moved to the front of the list, and the least recently used key is evicted when the cache exceeds its capacity.

### Algorithm

1. **get**:

   - If the key exists in the cache, move it to the front (most recently used) and return its value.
   - If the key does not exist, return `-1`.

2. **put**:
   - If the key already exists, update its value and move it to the front.
   - If the key is new, add it to the front of the DLL and the map.
   - If the cache exceeds its capacity, evict the least recently used key (remove the key at the back of the DLL).

### Complexity

- **Time Complexity**:
  - `get`: \(O(1)\) for accessing and updating the map and DLL.
  - `put`: \(O(1)\) for both adding a new key and updating the DLL.
- **Space Complexity**: \(O(n)\), where \(n\) is the capacity of the cache.

### Implementation

```cpp
class LRUCache {
public:
    list<int> dll; // Doubly linked list to store keys in LRU order
    map<int, pair<list<int>::iterator, int>> cache; // Map: key -> (iterator to dll node, value)
    int capacity; // Maximum capacity of the cache

    LRUCache(int capacity) {
        this->capacity = capacity;
    }

    // Helper function to move a key to the front (most recently used)
    void makeMostRecentlyUsed(int key) {
        dll.erase(cache[key].first); // Remove the key from its current position
        dll.push_front(key);        // Add the key to the front of the list
        cache[key].first = dll.begin(); // Update the iterator in the map
    }

    int get(int key) {
        if (!cache.count(key)) // If key is not in the cache, return -1
            return -1;

        makeMostRecentlyUsed(key); // Move the key to the front
        return cache[key].second; // Return the value associated with the key
    }

    void put(int key, int value) {
        if (cache.count(key)) {
            // If key already exists, update its value and move it to the front
            cache[key].second = value;
            makeMostRecentlyUsed(key);
        } else {
            // If key is new, add it to the cache
            dll.push_front(key);
            cache[key] = {dll.begin(), value};
            capacity--;
        }

        // If capacity is exceeded, evict the least recently used key
        if (capacity < 0) {
            int lruKey = dll.back(); // Key at the back is least recently used
            cache.erase(lruKey);    // Remove it from the map
            dll.pop_back();         // Remove it from the list
            capacity++;
        }
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key, value);
 */
```

# 188. LFU Cache

**Problem Link**: [https://leetcode.com/problems/lfu-cache/](https://leetcode.com/problems/lfu-cache/)

---

## Approach: Using Map and Frequency List

### Idea

- Use a **map of lists** to store keys grouped by their frequency. Each frequency corresponds to a list of `{key, value, frequency}` tuples.
- Use an **unordered map** to store iterators pointing to the position of each key in its frequency list.
- When a key is accessed:
  - Update its frequency.
  - Move it to the new frequency group.
- If the cache is full, remove the least frequently used key (lowest frequency group, least recently used key in that group).

### Algorithm

1. **get**:

   - If the key exists, retrieve its value.
   - Update its frequency using a helper function `makeMostFrequentlyUsed`.
   - Return the value.
   - If the key does not exist, return `-1`.

2. **put**:
   - If the key already exists:
     - Update its value and frequency.
   - If the key is new:
     - Add it to the frequency group with frequency `1`.
     - If the cache is full, evict the least frequently used key.

### Complexity

- **Time Complexity**:
  - `get`: \(O(1)\) due to hash map and list operations.
  - `put`: \(O(1)\) for insertion and \(O(1)\) for eviction.
- **Space Complexity**: \(O(n)\), where \(n\) is the capacity of the cache.

### Implementation

```cpp
class LFUCache {
private:
    int cap; // Cache capacity
    int size; // Current size of the cache
    unordered_map<int, list<vector<int>>::iterator> mp; // Key -> iterator pointing to the element in the frequency list
    map<int, list<vector<int>>> freq; // Frequency -> list of {key, value, frequency}

public:
    LFUCache(int capacity) {
        cap = capacity;
        size = 0;
    }

    void makeMostFrequentlyUsed(int key) {
        auto &vec = *(mp[key]);
        int value = vec[1];
        int f = vec[2];

        freq[f].erase(mp[key]); // Remove the element from its current frequency list

        if (freq[f].empty())
            freq.erase(f); // Erase the frequency list if it's empty

        f++; // Increase frequency
        freq[f].push_front(vector<int>({key, value, f})); // Add the element to the updated frequency list
        mp[key] = freq[f].begin(); // Update the map with the new position
    }

    int get(int key) {
        if (mp.find(key) == mp.end())
            return -1; // Key not found

        int value = (*(mp[key]))[1]; // Get the value
        makeMostFrequentlyUsed(key); // Update frequency

        return value;
    }

    void put(int key, int value) {
        if (cap == 0)
            return; // No capacity

        if (mp.find(key) != mp.end()) {
            auto &vec = *(mp[key]);
            vec[1] = value; // Update the value
            makeMostFrequentlyUsed(key); // Update frequency
        } else {
            if (size < cap) {
                size++;
                freq[1].push_front(vector<int>({key, value, 1})); // Add new element with frequency 1
                mp[key] = freq[1].begin();
            } else {
                // Remove LFU element
                auto &leastFreqList = freq.begin()->second;
                int keyToDelete = (leastFreqList.back())[0];

                leastFreqList.pop_back(); // Remove least recently used in the least frequency list

                if (leastFreqList.empty())
                    freq.erase(freq.begin()->first); // Remove the frequency list if it's empty

                freq[1].push_front(vector<int>({key, value, 1})); // Add the new element with frequency 1
                mp.erase(keyToDelete); // Erase the old key
                mp[key] = freq[1].begin();
            }
        }
    }
};

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache* obj = new LFUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

# 189. Longest Substring Without Repeating Characters

**Problem Link**: [https://leetcode.com/problems/longest-substring-without-repeating-characters](https://leetcode.com/problems/longest-substring-without-repeating-characters)

## Approach 1: Using Nested Loops and an Unordered Set

### Idea

- Use a **nested loop** to consider all possible substrings starting from each index.
- Use an **unordered set** to store characters and ensure that each character in the substring is unique.
- Break the inner loop as soon as a duplicate character is found.
- Track the length of the longest substring found.

### Algorithm

1. Initialize `max_len` to store the length of the longest substring.
2. For each starting index `i`, iterate through the string with an inner loop and check if a character is already in the set.
3. If the character is not in the set, add it and update the maximum length.
4. If a duplicate is found, break the inner loop and continue with the next starting index.

### Complexity

- **Time Complexity**: \(O(n^2)\), where \(n\) is the length of the string. The inner loop may iterate up to \(n\) times for each starting index.
- **Space Complexity**: \(O(n)\), where \(n\) is the size of the set used to store characters.

### Implementation

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n = s.size();
        int max_len = 0;

        for (int i = 0; i < n; ++i) {
            unordered_set<char> set;
            for (int j = i; j < n; ++j) {
                if (set.find(s[j]) != set.end()) {
                    break;
                }
                set.insert(s[j]);
                max_len = max(max_len, j - i + 1);
            }
        }
        return max_len;
    }
};
```

## Approach 2: Using Sliding Window with an Unordered Set

### Idea

- Use a **sliding window** technique with two pointers (`left` and `right`).
- Maintain an **unordered set** to track characters in the current window.
- If a duplicate is found, move the `left` pointer to the right of the last occurrence of the duplicate character.

### Algorithm

1. Initialize `left` pointer to 0 and `max_len` to 0.
2. Iterate over the string with the `right` pointer.
3. If the character at `right` is already in the set, move the `left` pointer to the right of the last occurrence of that character by removing elements from the set.
4. Insert the current character at `right` into the set and update `max_len` with the length of the current window.

### Complexity

- **Time Complexity**: \(O(n)\), where \(n\) is the length of the string. Each character is processed once by both `left` and `right` pointers.
- **Space Complexity**: \(O(k)\), where \(k\) is the size of the set, which at most holds \(n\) unique characters.

### Implementation

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_set<char> set;
        int left = 0, max_len = 0;

        for (int right = 0; right < s.size(); ++right) {
            while (set.find(s[right]) != set.end()) {
                set.erase(s[left]);
                left++;
            }
            set.insert(s[right]);
            max_len = max(max_len, right - left + 1);
        }
        return max_len;
    }
};
```

## Approach 3: Using Sliding Window with an Unordered Map

### Idea

- Use a **sliding window** technique with two pointers (`left` and `right`).
- Maintain an **unordered map** to store the most recent index of each character.
- If a duplicate is found, move the `left` pointer to the right of the last occurrence of that character using the stored index.

### Algorithm

1. Initialize `left` pointer to 0 and `max_len` to 0.
2. Iterate over the string with the `right` pointer.
3. If the character at `right` has been seen before (exists in the map), move the `left` pointer to the index of the last occurrence of that character plus one.
4. Update the map with the new index of the character at `right`.
5. Calculate the length of the current window and update `max_len` accordingly.

### Complexity

- **Time Complexity**: \(O(n)\), where \(n\) is the length of the string. Each character is processed once by both `left` and `right` pointers.
- **Space Complexity**: \(O(k)\), where \(k\) is the size of the map, which at most holds \(n\) unique characters.

### Implementation

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> map;
        int left = 0, max_len = 0;

        for (int right = 0; right < s.size(); ++right) {
            if (map.find(s[right]) != map.end()) {
                left = max(left, map[s[right]] + 1);
            }
            map[s[right]] = right;
            max_len = max(max_len, right - left + 1);
        }
        return max_len;
    }
};
```

# 190. Max Consecutive Ones III

**Problem Link**: [https://leetcode.com/problems/max-consecutive-ones-iii/description/](https://leetcode.com/problems/max-consecutive-ones-iii/description/)

## Approach 1: Brute Force with Nested Loops

### Idea

- Iterate through all possible subarrays using nested loops.
- Count the number of `0`s in the current subarray.
- If the number of `0`s exceeds `k`, stop expanding the current subarray.
- Keep track of the maximum subarray length where the number of `0`s is at most `k`.

### Algorithm

1. Use an outer loop to set the starting index of the subarray.
2. Use an inner loop to expand the subarray by incrementing the ending index.
3. Count the number of `0`s in the subarray.
4. If the number of `0`s exceeds `k`, break the inner loop.
5. Update the maximum length of the subarray.

### Complexity

- **Time Complexity**: \(O(n^2)\), where \(n\) is the size of the array. The outer and inner loops iterate over the array.
- **Space Complexity**: \(O(1)\), no extra data structures are used.

### Implementation

```cpp
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        int n = nums.size();
        int maxLength = 0;

        // Outer loop for the starting point of the subarray
        for (int start = 0; start < n; ++start) {
            int zeroCount = 0;

            // Inner loop for the ending point of the subarray
            for (int end = start; end < n; ++end) {
                if (nums[end] == 0) {
                    ++zeroCount;
                }

                // If zeroCount exceeds k, break out of the inner loop
                if (zeroCount > k) {
                    break;
                }

                // Update the maximum length
                maxLength = max(maxLength, end - start + 1);
            }
        }

        return maxLength;
    }
};
```

## Approach 2: Sliding Window

### Idea

- Use a sliding window to dynamically adjust the subarray while maintaining the condition that the number of `0`s does not exceed `k`.
- Expand the window by moving the `right` pointer.
- If the count of `0`s exceeds `k`, shrink the window by moving the `left` pointer until the condition is satisfied.
- Track the maximum length of the subarray during the process.

### Algorithm

1. Initialize two pointers, `left` and `right`, to define the sliding window.
2. Use a variable `zeroCount` to count the number of `0`s in the current window.
3. For each `right` pointer:
   - If `nums[right]` is `0`, increment `zeroCount`.
   - While `zeroCount > k`, increment `left` and decrement `zeroCount` if `nums[left]` is `0`.
4. Update the maximum length of the subarray as `right - left + 1`.

### Complexity

- **Time Complexity**: \(O(n)\), where \(n\) is the size of the array. Both `left` and `right` pointers traverse the array once.
- **Space Complexity**: \(O(1)\), no extra data structures are used.

### Implementation

```cpp
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        int left = 0; // Left pointer for the sliding window
        int maxLength = 0; // To track the maximum length of consecutive 1's
        int zeroCount = 0; // To count the number of 0's in the current window

        for (int right = 0; right < nums.size(); ++right) {
            // If the current element is 0, increment the zero count
            if (nums[right] == 0) {
                ++zeroCount;
            }

            // If zero count exceeds k, shrink the window from the left
            while (zeroCount > k) {
                if (nums[left] == 0) {
                    --zeroCount;
                }
                ++left;
            }

            // Calculate the maximum length of the window
            maxLength = max(maxLength, right - left + 1);
        }

        return maxLength;
    }
};
```
