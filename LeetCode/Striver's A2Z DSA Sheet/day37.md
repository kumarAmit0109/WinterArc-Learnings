# 181. Remove K Digits

### Problem Link

[LeetCode - Remove K Digits](https://leetcode.com/problems/remove-k-digits/)

## Approach 1: Using Stack

### Idea/Intuition

In this approach, we use a stack to efficiently remove digits. We iterate through the digits of the number, and for each digit, we compare it with the top of the stack. If the current digit is smaller than the stack's top and we still have digits to remove, we pop the stack. After processing all digits, we remove any remaining digits from the stack if needed, and then construct the final result by reversing the stack. We also handle leading zeros by trimming them.

### Algorithm

1. **Iterate Over Digits**:
   - For each digit in the number, compare it with the top of the stack.
2. **Remove Larger Digits**:
   - If the current digit is smaller than the stack's top, pop the stack and decrement `k`.
3. **Remove Remaining Digits**:
   - After processing all digits, if `k` is still greater than 0, remove the remaining digits from the stack.
4. **Construct the Final String**:
   - Reverse the stack to get the correct order and handle any leading zeros by trimming them.

### Time Complexity

- **O(n)**: We process each digit once, where `n` is the length of the number.

### Space Complexity

- **O(n)**: Space used by the stack to store the digits.

### Code

```cpp
string removeKdigits(string num, int k) {
    stack<char> stack;

    for (char digit : num) {
        while (!stack.empty() && k > 0 && stack.top() > digit) {
            stack.pop();
            k--;
        }
        stack.push(digit);
    }

    // Remove remaining k digits from the end of the stack
    while (k > 0 && !stack.empty()) {
        stack.pop();
        k--;
    }

    // Construct the resulting string from the stack
    string result;
    while (!stack.empty()) {
        result += stack.top();
        stack.pop();
    }
    reverse(result.begin(), result.end()); // Reverse to get the correct order

    // Remove leading zeros
    size_t pos = result.find_first_not_of('0');
    result = (pos == string::npos) ? "0" : result.substr(pos);

    return result;
}
```

## Approach 2: Using Stack with Manual Leading Zero Removal

### Idea/Intuition

In this approach, we use a stack to efficiently remove digits while ensuring that the number remains as small as possible. We iterate through the digits of the number and for each digit, we remove elements from the stack that are larger than the current digit, as long as we have digits left to remove (`k > 0`). After processing all digits, we remove any remaining digits from the stack if necessary. Finally, we manually handle the removal of leading zeros.

### Algorithm

1. **Iterate Over Digits**:
   - For each digit in the number, compare it with the top of the stack. If the stack's top is greater and we still have digits to remove, pop the stack.
2. **Remove Remaining Digits**:
   - After iterating through the number, if `k` is still greater than 0, pop digits from the stack.
3. **Construct the Final String**:
   - Reverse the stack to get the digits in the correct order and remove leading zeros manually.

### Time Complexity

- **O(n)**: We process each digit once, where `n` is the length of the number.

### Space Complexity

- **O(n)**: Space used by the stack to store the digits.

### Code

```cpp
string removeKdigits(string num, int k) {
    stack<char> stk; // Stack to store the digits of the result

    // Iterate through each digit in the number
    for (char digit : num) {
        // Remove top elements from the stack while they are greater than the current digit
        // and we still have digits to remove (k > 0)
        while (!stk.empty() && k > 0 && stk.top() > digit) {
            stk.pop();
            k--; // Decrease k as we remove a digit
        }
        stk.push(digit); // Add the current digit to the stack
    }

    // If k is still greater than 0, remove remaining digits from the top of the stack
    while (k > 0 && !stk.empty()) {
        stk.pop();
        k--;
    }

    // Construct the result string from the stack
    string result;
    while (!stk.empty()) {
        result += stk.top();
        stk.pop();
    }
    reverse(result.begin(), result.end()); // Reverse the string to get the correct order

    // Remove leading zeros using a while loop
    int i = 0;
    while (i < result.size() && result[i] == '0') {
        i++;
    }

    // Create the final result string
    result = (i == result.size()) ? "0" : result.substr(i); // Handle case when all digits are removed

    return result;
}
```

# 182. Largest Rectangle in Histogram

### Problem Link

[LeetCode - Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

## Approach 1: Using Previous and Next Smaller Elements

### Idea/Intuition

For each bar in the histogram, calculate the largest rectangle it can contribute to by finding the previous smaller and next smaller bars. The width of the rectangle is determined by the indices of these smaller elements.

### Algorithm

1. **Find Previous Smaller Elements**:
   - Use a stack to determine the previous smaller element for each bar efficiently.
2. **Find Next Smaller Elements**:
   - Similarly, use a stack to find the next smaller element for each bar.
3. **Calculate the Maximum Area**:
   - For each bar, compute the width using the previous and next smaller indices and calculate the area.

### Time Complexity

- **O(n)**: Both `findPrevSmaller` and `findNextSmaller` functions process the array once.

### Space Complexity

- **O(n)**: Space for the stacks and the `prevSmaller` and `nextSmaller` arrays.

### Code

```cpp
vector<int> findPrevSmaller(const vector<int>& heights) {
    int n = heights.size();
    vector<int> prevSmaller(n, -1);
    stack<int> st;

    for (int i = 0; i < n; ++i) {
        while (!st.empty() && heights[st.top()] >= heights[i]) {
            st.pop();
        }
        if (!st.empty()) {
            prevSmaller[i] = st.top();
        }
        st.push(i);
    }
    return prevSmaller;
}

vector<int> findNextSmaller(const vector<int>& heights) {
    int n = heights.size();
    vector<int> nextSmaller(n, n);
    stack<int> st;

    for (int i = n - 1; i >= 0; --i) {
        while (!st.empty() && heights[st.top()] >= heights[i]) {
            st.pop();
        }
        if (!st.empty()) {
            nextSmaller[i] = st.top();
        }
        st.push(i);
    }
    return nextSmaller;
}

int largestRectangleArea(vector<int>& heights) {
    vector<int> prevSmaller = findPrevSmaller(heights);
    vector<int> nextSmaller = findNextSmaller(heights);
    int maxArea = 0;

    for (int i = 0; i < heights.size(); ++i) {
        int width = nextSmaller[i] - prevSmaller[i] - 1;
        int area = heights[i] * width;
        maxArea = max(maxArea, area);
    }

    return maxArea;
}
```

## Approach 2: Optimized Single Pass with Stack

### Idea/Intuition

This approach uses a stack to calculate the largest rectangle in a single pass. By maintaining indices of increasing heights in the stack, we ensure that when a smaller height is encountered, we can compute the largest rectangle for the popped elements.

### Algorithm

1. **Iterate Through the Heights**:
   - For each bar, check if it is smaller than the height at the top of the stack.
   - If it is, pop from the stack and calculate the area using the popped height as the smallest height in the rectangle.
2. **Handle Remaining Heights**:
   - At the end of the loop, ensure all heights in the stack are processed.
3. **Calculate Area**:
   - Use the indices in the stack to determine the width of rectangles.

### Time Complexity

- **O(n)**: Each bar is pushed and popped from the stack at most once.

### Space Complexity

- **O(n)**: Space used by the stack.

### Code

```cpp
int largestRectangleArea(vector<int>& heights) {
    int n = heights.size();
    stack<int> st;
    int maxArea = 0;

    for (int i = 0; i <= n; ++i) {
        int currentHeight = (i == n) ? 0 : heights[i];

        while (!st.empty() && currentHeight < heights[st.top()]) {
            int height = heights[st.top()];
            st.pop();
            int width = st.empty() ? i : (i - st.top() - 1);
            maxArea = max(maxArea, height * width);
        }

        st.push(i);
    }

    return maxArea;
}
```

# 183. Maximal Rectangle

### Problem Link

[LeetCode - Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)

## Approach 1: Brute Force Using Min-Height for Histogram Calculation

### Idea/Intuition

Treat each row of the matrix as the base of a histogram. Calculate the height array by accumulating the count of consecutive `1`s in each column. For each row, compute the maximal rectangle area by iterating through all possible subarrays of columns and determining the minimum height for each.

### Algorithm

1. Initialize a `heights` array of size `cols + 1` with all zeros.
2. Iterate through each row of the matrix:
   - Update the `heights` array:
     - Increment the value if the cell is `'1'`.
     - Reset to `0` if the cell is `'0'`.
   - For each index in the `heights` array, calculate the maximal area by:
     - Iterating over all possible subarrays of `heights` using nested loops.
     - Keeping track of the minimum height within the subarray.
     - Calculating the area as `minHeight * (j - i + 1)` and updating `maxArea`.

### Time Complexity

- **O(rows × cols²)**: Updating heights takes O(cols), and the nested loops for calculating the histogram area take O(cols²) per row.

### Space Complexity

- **O(cols)**: For storing the `heights` array.

### Code

```cpp
int maximalRectangle(vector<vector<char>>& matrix) {
    if (matrix.empty() || matrix[0].empty())
        return 0;

    int rows = matrix.size();
    int cols = matrix[0].size();
    vector<int> heights(cols + 1, 0); // Include an extra element for easier calculation
    int maxArea = 0;

    for (const auto& row : matrix) {
        // Update heights array
        for (int i = 0; i < cols; i++) {
            heights[i] = (row[i] == '1') ? heights[i] + 1 : 0;
        }

        // Calculate max area using brute force
        int n = heights.size(); // Number of bars in the histogram

        for (int i = 0; i < n; i++) {
            for (int j = i, minHeight = INT_MAX; j < n; j++) {
                minHeight = min(minHeight, heights[j]);
                int area = minHeight * (j - i + 1);
                maxArea = max(maxArea, area);
            }
        }
    }

    return maxArea;
}
```

## Approach 2: Optimized Using Histogram Approach for Each Row

### Idea/Intuition

The problem is converted into a series of histogram problems for each row of the matrix. The `largestRectangleArea` function calculates the largest rectangle in a histogram using a stack, and this is applied to a height array derived from the matrix.

### Algorithm

1. **Convert the Matrix into a Height Array**:
   - For each row in the matrix, update the height array to represent a histogram of consecutive 1s.
   - If `matrix[row][col] == '1'`, increment the corresponding `heights[col]`.
   - Otherwise, reset `heights[col]` to 0.
2. **Use the Largest Rectangle Area Function**:
   - Apply the histogram function (`largestRectangleArea`) on the height array to find the maximum rectangle area for that row.
3. **Update the Maximum Area**:
   - Track the largest rectangle area across all rows.

### Time Complexity

- **O(m × n)**: For a matrix of `m` rows and `n` columns, the histogram computation is applied for each row.
- The `largestRectangleArea` function runs in **O(n)** per row.

### Space Complexity

- **O(n)**: Space used by the height array and the stack.

### Code

```cpp
int largestRectangleArea(vector<int>& heights) {
    int n = heights.size();
    stack<int> st;
    int maxArea = 0;

    for (int i = 0; i <= n; ++i) {
        int currentHeight = (i == n) ? 0 : heights[i];

        while (!st.empty() && currentHeight < heights[st.top()]) {
            int height = heights[st.top()];
            st.pop();
            int width = st.empty() ? i : (i - st.top() - 1);
            maxArea = max(maxArea, height * width);
        }

        st.push(i);
    }

    return maxArea;
}

int maximalRectangle(vector<vector<char>>& matrix) {
    if (matrix.empty() || matrix[0].empty())
        return 0;

    int rows = matrix.size();
    int cols = matrix[0].size();
    vector<int> heights(cols, 0); // Histogram heights for each row
    int maxArea = 0;

    for (const auto& row : matrix) {
        for (int i = 0; i < cols; i++) {
            heights[i] = (row[i] == '1') ? heights[i] + 1 : 0;
        }
        maxArea = max(maxArea, largestRectangleArea(heights));
    }

    return maxArea;
}
```

# 184. Sliding Window Maximum

### Problem Link

[LeetCode - Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

## Approach 1: Brute Force

### Idea/Intuition

Iterate through each possible window of size `k` in the array. For each window, find the maximum element by iterating through the elements in the window. Append the maximum value to the result list.

### Algorithm

1. Initialize an empty result vector `ans`.
2. Loop through all valid starting indices `i` for windows of size `k` (i.e., `i` from `0` to `n - k`).
3. For each window:
   - Initialize `maxVal` to the first element of the window.
   - Iterate through the `k` elements of the window, updating `maxVal` if a larger element is found.
4. Append `maxVal` to the result.
5. Return the result vector.

### Time Complexity

- **O(n × k)**: For each starting index `i`, iterating through `k` elements in the window.

### Space Complexity

- **O(1)**: No additional data structures are used apart from the result vector.

### Code

```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    vector<int> ans;
    int n = nums.size();

    for (int i = 0; i <= n - k; ++i) {
        int maxVal = nums[i];
        for (int j = i; j < i + k; ++j) {
            maxVal = max(maxVal, nums[j]);
        }
        ans.push_back(maxVal);
    }

    return ans;
}
```

## Approach 2: Using Deque (Optimized)

### Idea/Intuition

Use a deque to efficiently keep track of indices of potential maximum elements for the sliding window. The deque stores indices in a way that:

1. Indices of elements not in the current window are removed from the front.
2. Indices of smaller elements (which cannot be the maximum) are removed from the back.

This ensures that the front of the deque always contains the index of the current window's maximum element.

### Algorithm

1. Initialize an empty deque `dq` and result vector `ans`.
2. Iterate through the array:
   - Remove indices of elements that are out of the current window (`i - k`).
   - Remove indices of elements smaller than the current element (as they are no longer useful).
   - Add the current element's index to the deque.
   - For indices `i >= k - 1`, append the element at the front of the deque (maximum of the current window) to the result vector.
3. Return the result vector.

### Time Complexity

- **O(n)**: Each element is added to and removed from the deque at most once.

### Space Complexity

- **O(k)**: Space for the deque, which can hold at most `k` elements.

### Code

```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    deque<int> dq; // Stores indices of potential maximum elements
    vector<int> ans;
    int n = nums.size();

    for (int i = 0; i < n; ++i) {
        // Remove indices of elements not in the current window
        if (!dq.empty() && dq.front() == i - k) {
            dq.pop_front();
        }

        // Remove indices of smaller elements (as they are useless)
        while (!dq.empty() && nums[dq.back()] < nums[i]) {
            dq.pop_back();
        }

        // Add the current element's index
        dq.push_back(i);

        // Store the maximum for the current window
        if (i >= k - 1) {
            ans.push_back(nums[dq.front()]);
        }
    }

    return ans;
}
```

# 185. Online Stock Span

### Problem Link

[LeetCode - Online Stock Span](https://leetcode.com/problems/online-stock-span/)

## Approach 1: Using a Vector to Store Prices

### Idea

- Use a vector to store all the stock prices.
- Traverse the vector backward from the current price to calculate the span.

### Algorithm

1. Maintain a vector `prices` to store all the prices.
2. For every new price:
   - Add it to the `prices` vector.
   - Traverse backward and count how many consecutive days have a price less than or equal to the current price.

### Complexity

- **Time Complexity**: \(O(n^2)\) in the worst case, where \(n\) is the number of days (traversal for each price).
- **Space Complexity**: \(O(n)\) for storing the prices.

### Implementation

```cpp
class StockSpanner {
public:
    vector<int> prices; // Store the prices

    StockSpanner() {
        prices = {}; // Initialize the prices vector
    }

    int next(int price) {
        prices.push_back(price); // Add the current price
        int span = 1; // Initialize the span as 1 (current day)

        // Traverse backward and calculate the span
        for (int i = prices.size() - 2; i >= 0; --i) {
            if (prices[i] <= price) {
                span++;
            } else {
                break;
            }
        }

        return span;
    }
};

/**
 * Your StockSpanner object will be instantiated and called as such:
 * StockSpanner* obj = new StockSpanner();
 * int param_1 = obj->next(price);
 */
```

## Approach 2: Using a Stack to Optimize Span Calculation

### Idea

- Use a stack to store pairs of prices and their indices.
- Eliminate unnecessary prices (those smaller than or equal to the current price) from the stack.

### Algorithm

1. Maintain a stack of pairs \((price, index)\).
2. For every new price:
   - Pop all elements from the stack where the price is less than or equal to the current price.
   - Calculate the span using the difference between the current index and the index of the top of the stack (if the stack is empty, the span is the entire range so far).
   - Push the current price and index onto the stack.

### Complexity

- **Time Complexity**: \(O(1)\) amortized for each `next` call since each price is pushed and popped at most once.
- **Space Complexity**: \(O(n)\), where \(n\) is the number of `next` calls, for storing prices and indices in the stack.

### Implementation

```cpp
class StockSpanner {
    stack<pair<int, int>> st; // Pair to store {price, index}
    int ind;

public:
    StockSpanner() {
        ind = -1; // Initialize index to -1
        while (!st.empty()) st.pop(); // Clear the stack if necessary
    }

    int next(int price) {
        ind += 1; // Increment the index

        // Remove all elements from the stack where the price is less than or equal to the current price
        while (!st.empty() && st.top().first <= price) {
            st.pop();
        }

        // Calculate the span
        int span = st.empty() ? ind + 1 : ind - st.top().second;

        // Push the current price with its index onto the stack
        st.push({price, ind});

        return span;
    }
};

/**
 * Your StockSpanner object will be instantiated and called as such:
 * StockSpanner* obj = new StockSpanner();
 * int param_1 = obj->next(price);
 */
```
