# 177. Trapping Rain Water

### Problem Link

[LeetCode - Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

## Approach 1: Brute Force

### Idea/Intuition

In this brute-force approach, for each element in the array, we calculate the maximum height to its left and right. Using these values, we determine the amount of water that can be trapped at each index.

### Algorithm

1. **Initialize Water Trapped Counter**:

   - Start with `waterTrapped = 0`.

2. **Iterate Through Each Element**:

   - For each index `i`:
     - Find the maximum height to the left of `i` (`leftMax`).
     - Find the maximum height to the right of `i` (`rightMax`).
     - Calculate the water trapped at index `i` as `min(leftMax, rightMax) - height[i]` and add it to `waterTrapped`.

3. **Return Result**:
   - Return the accumulated `waterTrapped` value.

### Time Complexity

- **O(n^2)**: For each element, we check all elements to its left and right.

### Space Complexity

- **O(1)**: Constant space used, excluding input and output.

### Code

```cpp
int trap(vector<int>& height) {
    int n = height.size();
    if (n == 0) return 0;

    int waterTrapped = 0;
    for (int i = 0; i < n; ++i) {
        int leftMax = 0;
        for (int j = 0; j <= i; ++j) {
            leftMax = max(leftMax, height[j]);
        }

        int rightMax = 0;
        for (int j = i; j < n; ++j) {
            rightMax = max(rightMax, height[j]);
        }

        waterTrapped += min(leftMax, rightMax) - height[i];
    }

    return waterTrapped;
}
```

## Approach 2: Using LeftMax and RightMax Arrays

### Idea/Intuition

This approach involves creating two auxiliary arrays, `leftMax` and `rightMax`, to store the maximum heights to the left and right of each element. By calculating the minimum of these values for each position, we can determine the amount of water trapped at each position.

### Algorithm

1. **Initialize Arrays**:

   - Create two arrays, `leftMax` and `rightMax`, each of size `n`.
   - Set `leftMax[0] = height[0]` and `rightMax[n-1] = height[n-1]`.

2. **Fill LeftMax Array**:

   - For each element `i` from 1 to `n-1`, set `leftMax[i]` to the maximum of `leftMax[i-1]` and `height[i]`.

3. **Fill RightMax Array**:

   - For each element `i` from `n-2` to 0, set `rightMax[i]` to the maximum of `rightMax[i+1]` and `height[i]`.

4. **Calculate Water Trapped**:
   - For each element, calculate the water trapped as `min(leftMax[i], rightMax[i]) - height[i]` and accumulate it in `waterTrapped`.

### Time Complexity

- **O(n)**: Two passes are required to fill `leftMax` and `rightMax` arrays, and one pass to calculate water trapped.

### Space Complexity

- **O(n)**: Additional space for `leftMax` and `rightMax` arrays.

### Code

```cpp
int trap(vector<int>& height) {
    int n = height.size();
    if (n == 0) return 0;

    vector<int> leftMax(n), rightMax(n);
    leftMax[0] = height[0];
    rightMax[n - 1] = height[n - 1];

    for (int i = 1; i < n; ++i) {
        leftMax[i] = max(leftMax[i - 1], height[i]);
    }

    for (int i = n - 2; i >= 0; --i) {
        rightMax[i] = max(rightMax[i + 1], height[i]);
    }

    int waterTrapped = 0;
    for (int i = 0; i < n; ++i) {
        waterTrapped += min(leftMax[i], rightMax[i]) - height[i];
    }

    return waterTrapped;
}
```

## Approach 3: Two-Pointer Method

### Idea/Intuition

This approach uses two pointers and maintains the maximum heights encountered from the left and right as we traverse the array. By moving the pointers towards each other, we can efficiently calculate the water trapped without additional arrays.

### Algorithm

1. **Initialize Pointers and Variables**:

   - Set two pointers: `i` at the beginning and `j` at the end of the array.
   - Initialize `left_max` and `right_max` with the heights at `i` and `j`, respectively.
   - Initialize `sum` to store the accumulated water trapped.

2. **Traverse with Two Pointers**:

   - While `i < j`:
     - If `left_max` is less than or equal to `right_max`, calculate the water trapped at `i` as `left_max - height[i]`, add it to `sum`, move `i` right, and update `left_max`.
     - Otherwise, calculate the water trapped at `j` as `right_max - height[j]`, add it to `sum`, move `j` left, and update `right_max`.

3. **Return Result**:
   - Return the accumulated `sum` value.

### Time Complexity

- **O(n)**: Each element is processed once.

### Space Complexity

- **O(1)**: Constant space used.

### Code

```cpp
int trap(vector<int>& height) {
    int i = 0, left_max = height[0], sum = 0;
    int j = height.size() - 1, right_max = height[j];

    while (i < j) {
        if (left_max <= right_max) {
            sum += (left_max - height[i]);
            i++;
            left_max = max(left_max, height[i]);
        } else {
            sum += (right_max - height[j]);
            j--;
            right_max = max(right_max, height[j]);
        }
    }

    return sum;
}
```

# 178. Sum of Subarray Minimums

### Problem Link

[LeetCode - Sum of Subarray Minimums](https://leetcode.com/problems/sum-of-subarray-minimums/)

## Approach 1: Brute Force

### Idea/Intuition

This brute-force approach calculates the minimum value for each subarray in the input array. For every possible subarray, we keep track of the minimum element and accumulate it to get the total sum.

### Algorithm

1. **Initialize Variables**:

   - Start with `total = 0` and a constant `MOD = 1e9 + 7` to keep results within limits.

2. **Iterate Over All Subarrays**:

   - For each starting index `i`, initialize `minVal` as `arr[i]`.
   - For each ending index `j` from `i` to `n-1`:
     - Update `minVal` to the minimum between `minVal` and `arr[j]`.
     - Add `minVal` to `total`, applying `MOD` to keep it within bounds.

3. **Return Result**:
   - Return `total`.

### Time Complexity

- **O(n^2)**: Each element participates in calculating the minimum of each possible subarray.

### Space Complexity

- **O(1)**: Constant space usage.

### Code

```cpp
int sumSubarrayMins(vector<int>& arr) {
    int n = arr.size();
    const int MOD = 1e9 + 7;
    long long total = 0;

    for (int i = 0; i < n; ++i) {
        int minVal = arr[i];
        for (int j = i; j < n; ++j) {
            minVal = min(minVal, arr[j]);
            total = (total + minVal) % MOD;
        }
    }

    return total;
}
```

## Approach 2: Monotonic Stack with Previous and Next Minimums

### Idea/Intuition

This approach uses monotonic stacks to efficiently compute the contribution of each element as the minimum of its subarrays by determining the nearest smaller elements to the left and right. Each element's contribution to the result is based on how many subarrays it serves as the minimum for.

### Algorithm

1. **Initialize Variables**:

   - Create `prevMin` and `nextMin` arrays to store the indices of the previous and next smaller elements for each index.
   - Initialize a stack `s` to help compute these arrays.
   - Use `MOD = 1e9 + 7` to keep results within limits.

2. **Compute `prevMin` Array**:

   - Traverse from left to right.
   - For each index `i`, pop elements from the stack while the top of the stack is greater than or equal to `arr[i]` to maintain the monotonic stack property.
   - Set `prevMin[i]` to the index at the top of the stack if it's non-empty; otherwise, set it to `-1`.
   - Push the current index `i` onto the stack.

3. **Compute `nextMin` Array**:

   - Clear the stack and traverse from right to left.
   - For each index `i`, pop elements from the stack while the top of the stack is greater than `arr[i]`.
   - Set `nextMin[i]` to the index at the top of the stack if it's non-empty; otherwise, set it to `n` (out of bounds).
   - Push the current index `i` onto the stack.

4. **Calculate the Result**:

   - For each index `i`, compute the number of subarrays in which `arr[i]` is the minimum using `leftCount = i - prevMin[i]` and `rightCount = nextMin[i] - i`.
   - Add the contribution of `arr[i]` to `total` as `(long long)arr[i] * leftCount * rightCount`, applying `MOD`.

5. **Return Result**:
   - Return `total`.

### Time Complexity

- **O(n)**: Each element is processed a constant number of times.

### Space Complexity

- **O(n)**: Space used by `prevMin`, `nextMin`, and the stack.

### Code

```cpp
int sumSubarrayMins(vector<int>& arr) {
    int n = arr.size();
    const int MOD = 1e9 + 7;
    vector<int> prevMin(n), nextMin(n);
    stack<int> s;

    // Calculate prevMin array
    for (int i = 0; i < n; ++i) {
        while (!s.empty() && arr[s.top()] >= arr[i]) {
            s.pop();
        }
        prevMin[i] = s.empty() ? -1 : s.top();
        s.push(i);
    }

    // Clear the stack to reuse it for nextMin calculation
    while (!s.empty()) {
        s.pop();
    }

    // Calculate nextMin array
    for (int i = n - 1; i >= 0; --i) {
        while (!s.empty() && arr[s.top()] > arr[i]) {
            s.pop();
        }
        nextMin[i] = s.empty() ? n : s.top();
        s.push(i);
    }

    // Calculate the result
    long long total = 0;
    for (int i = 0; i < n; ++i) {
        int leftCount = i - prevMin[i];
        int rightCount = nextMin[i] - i;
        total = (total + (long long)arr[i] * leftCount * rightCount) % MOD;
    }

    return total;
}
```

# 179. Asteroid Collision

### Problem Link

[LeetCode - Asteroid Collision](https://leetcode.com/problems/asteroid-collision/)

## Approach: Using a Vector to Simulate Collisions

### Idea/Intuition

In this approach, we use a vector (which we named `stack`) to simulate the movement and collisions of asteroids. The idea is to handle asteroids moving in different directions and resolve collisions when necessary.

### Algorithm

1. **Initialize the Vector**:

   - Create an empty vector `stack` to track the asteroids in motion.

2. **Iterate Over Asteroids**:

   - For each asteroid:
     - If the asteroid is moving to the right (positive value), add it to the vector.
     - If the asteroid is moving to the left (negative value), check for potential collisions:
       - If the top of the vector (which represents an asteroid moving right) is smaller than the current asteroid, remove it from the vector and continue checking for further collisions.
       - If both asteroids are of equal size, remove the right-moving asteroid (they destroy each other).
       - If the asteroid in the vector is larger, the current asteroid is destroyed.
     - If no collision happens, add the current asteroid to the vector.

3. **Return the Remaining Asteroids**:
   - After processing all asteroids, the vector will contain the asteroids that survived.

### Time Complexity

- **O(n)**: Each asteroid is processed at most once, and each operation on the vector is constant time.

### Space Complexity

- **O(n)**: Space used by the vector to store the asteroids.

### Code

```cpp
vector<int> asteroidCollision(vector<int>& asteroids) {
    vector<int> stack;

    for (int asteroid : asteroids) {
        bool destroyed = false;

        while (!stack.empty() && stack.back() > 0 && asteroid < 0) {
            if (stack.back() < -asteroid) {
                stack.pop_back();
                continue;
            } else if (stack.back() == -asteroid) {
                stack.pop_back();
            }
            destroyed = true;
            break;
        }

        if (!destroyed) {
            stack.push_back(asteroid);
        }
    }

    return stack;
}
```

# 180. Sum of Subarray Ranges

### Problem Link

[LeetCode - Sum of Subarray Ranges](https://leetcode.com/problems/sum-of-subarray-ranges/)

## Approach 1: Brute Force Method

### Idea/Intuition

In this approach, we iterate over all possible subarrays and for each subarray, calculate the difference between the maximum and minimum values. We then sum these differences to obtain the result.

### Algorithm

1. **Iterate Over All Subarrays**:
   - For each subarray starting from index `i`, calculate the minimum and maximum values.
2. **Sum the Differences**:

   - For each subarray, calculate the difference between the maximum and minimum values and accumulate it into the result.

3. **Return the Final Result**:
   - Return the accumulated sum.

### Time Complexity

- **O(n^2)**: We iterate over all possible subarrays, and for each subarray, we calculate the maximum and minimum values.

### Space Complexity

- **O(1)**: We only use a constant amount of space for variables.

### Code

```cpp
long long subArrayRanges(vector<int>& nums) {
    long long ans = 0;
    int n = nums.size();

    // Iterate over all possible subarrays
    for (int i = 0; i < n; ++i) {
        int minVal = nums[i], maxVal = nums[i];

        // For each subarray starting at i, find the range (max - min)
        for (int j = i; j < n; ++j) {
            minVal = min(minVal, nums[j]);
            maxVal = max(maxVal, nums[j]);
            ans += (maxVal - minVal);
        }
    }

    return ans;
}
```

## Approach 2: Optimized Using Stack

### Idea/Intuition

This approach uses stacks to efficiently calculate the sum of subarray minimums and maximums. We calculate the contribution of each element to the total sum by determining how many subarrays it is part of as the minimum and maximum.

### Algorithm

1. **Calculate Sum of Subarray Minimums**:

   - Use two arrays, `prevMin` and `nextMin`, to keep track of the closest elements that are smaller to the left and right of each element. This helps calculate the number of subarrays where the element is the minimum.

2. **Calculate Sum of Subarray Maximums**:

   - Similarly, use `prevMax` and `nextMax` to keep track of the closest elements that are larger to the left and right of each element. This helps calculate the number of subarrays where the element is the maximum.

3. **Calculate the Final Result**:
   - Subtract the sum of subarray minimums from the sum of subarray maximums to get the final result.

### Time Complexity

- **O(n)**: We process each element in the array a constant number of times using stacks.

### Space Complexity

- **O(n)**: Space used by the arrays `prevMin`, `nextMin`, `prevMax`, and `nextMax`, and the stack.

### Code

```cpp
long long subArrayRanges(vector<int>& nums) {
    int n = nums.size();

    // Calculate sum of subarray minimums
    vector<int> prevMin(n), nextMin(n);
    stack<int> s;

    // Calculate prevMin array
    for (int i = 0; i < n; ++i) {
        while (!s.empty() && nums[s.top()] >= nums[i]) {
            s.pop();
        }
        prevMin[i] = s.empty() ? -1 : s.top();
        s.push(i);
    }

    // Clear the stack to reuse it for nextMin calculation
    while (!s.empty()) {
        s.pop();
    }

    // Calculate nextMin array
    for (int i = n - 1; i >= 0; --i) {
        while (!s.empty() && nums[s.top()] > nums[i]) {
            s.pop();
        }
        nextMin[i] = s.empty() ? n : s.top();
        s.push(i);
    }

    long long minSum = 0;
    for (int i = 0; i < n; ++i) {
        int leftCount = i - prevMin[i];
        int rightCount = nextMin[i] - i;
        minSum += (long long)nums[i] * leftCount * rightCount;
    }

    // Calculate sum of subarray maximums
    vector<int> prevMax(n), nextMax(n);
    while (!s.empty()) {
        s.pop();
    }

    // Calculate prevMax array
    for (int i = 0; i < n; ++i) {
        while (!s.empty() && nums[s.top()] <= nums[i]) {
            s.pop();
        }
        prevMax[i] = s.empty() ? -1 : s.top();
        s.push(i);
    }

    // Clear the stack to reuse it for nextMax calculation
    while (!s.empty()) {
        s.pop();
    }

    // Calculate nextMax array
    for (int i = n - 1; i >= 0; --i) {
        while (!s.empty() && nums[s.top()] < nums[i]) {
            s.pop();
        }
        nextMax[i] = s.empty() ? n : s.top();
        s.push(i);
    }

    long long maxSum = 0;
    for (int i = 0; i < n; ++i) {
        int leftCount = i - prevMax[i];
        int rightCount = nextMax[i] - i;
        maxSum += (long long)nums[i] * leftCount * rightCount;
    }

    // Final result: sum of max sum - sum of min sum
    return maxSum - minSum;
}
```
