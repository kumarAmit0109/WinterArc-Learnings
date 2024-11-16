# Most Beautiful Item for Each Query

**Problem Link**: [Most Beautiful Item for Each Query](https://leetcode.com/problems/most-beautiful-item-for-each-query/)

## Approach 1: Binary Search with Prefix Maximum

### Idea

1. **Sort Items by Price**:
   - Sort the `items` array by price in ascending order.
2. **Compute Prefix Maximum Beauty**:

   - Traverse through sorted `items`, updating each item’s beauty to the maximum beauty seen up to that price.

3. **Binary Search for Each Query**:
   - For each `queryPrice` in `queries`, use binary search to find the maximum beauty within the price limit.

### Algorithm

1. Sort `items` based on the price.
2. For each item, update its beauty to be the maximum beauty up to that price.
3. For each `queryPrice`, apply binary search to locate the highest beauty value within the price range.

### Time Complexity

- **O((n + m) log n)**, where `n` is the length of `items` and `m` is the number of queries.

### Space Complexity

- **O(1)**, excluding input and output storage.

### Code Implementation

```cpp
class Solution {
public:
    int customBinarySearch(const vector<vector<int>>& items, int queryPrice) {
        int left = 0, right = items.size() - 1;
        int maxBeauty = 0;

        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (items[mid][0] <= queryPrice) {
                maxBeauty = items[mid][1];
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        return maxBeauty;
    }

    vector<int> maximumBeauty(vector<vector<int>>& items, vector<int>& queries) {
        int n = items.size();
        int m = queries.size();
        vector<int> result(m);

        sort(begin(items), end(items));

        int maxBeautySeen = items[0][1];
        for (int i = 1; i < n; ++i) {
            maxBeautySeen = max(maxBeautySeen, items[i][1]);
            items[i][1] = maxBeautySeen;
        }

        for (int i = 0; i < m; ++i) {
            int queryPrice = queries[i];
            result[i] = customBinarySearch(items, queryPrice);
        }

        return result;
    }
};
```

## Approach 2: Map with Upper Bound Search

### Idea

1. **Sort Items by Price**:

   - Sort the `items` array based on price in ascending order.

2. **Create a Price-to-Max-Beauty Map**:

   - Traverse through `items` and, for each item, update `maxBeauty` to store the maximum beauty encountered up to that price in a map `priceToMaxBeauty`.
   - For each unique price in `items`, store the maximum beauty up to that price in `priceToMaxBeauty`.

3. **Process Each Query with Upper Bound**:
   - For each query, use the `upper_bound` function to find the largest price in `priceToMaxBeauty` that is less than or equal to the query price.
   - If no such price exists, append 0 to the result; otherwise, append the corresponding maximum beauty.

### Algorithm

1. Sort `items` based on price.
2. Traverse `items`, updating `maxBeauty` for each price and storing it in `priceToMaxBeauty`.
3. For each query:
   - Use `upper_bound` to find the highest price that doesn’t exceed the query.
   - Append the maximum beauty up to that price, or 0 if no valid price is found.

### Time Complexity

- **O(n log n + m log n)**, where `n` is the length of `items` and `m` is the number of queries.

### Space Complexity

- **O(n)**, for storing the `priceToMaxBeauty` map.

### Code Implementation

```cpp
class Solution {
public:
    vector<int> maximumBeauty(vector<vector<int>>& items, vector<int>& queries) {
        // Step 1: Sort items based on price
        sort(items.begin(), items.end());

        // Step 2: Create a map to store max beauty up to each price
        map<int, int> priceToMaxBeauty;
        int maxBeauty = 0;

        for (const auto& item : items) {
            int price = item[0];
            int beauty = item[1];
            maxBeauty = max(maxBeauty, beauty);
            priceToMaxBeauty[price] = maxBeauty;
        }

        // Step 3: Process each query
        vector<int> answer;
        for (int query : queries) {
            auto it = priceToMaxBeauty.upper_bound(query);
            if (it == priceToMaxBeauty.begin()) {
                answer.push_back(0);
            } else {
                --it;
                answer.push_back(it->second);
            }
        }

        return answer;
    }
};
```
