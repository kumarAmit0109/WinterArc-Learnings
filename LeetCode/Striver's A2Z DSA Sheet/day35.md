# 171. Postfix to Infix Conversion

### Problem Link

[GeeksforGeeks - Postfix to Infix Conversion ](https://www.geeksforgeeks.org/problems/postfix-to-infix-conversion/1)

## Approach: Using Stack for Operand Handling

### Idea/Intuition

To convert a postfix expression to infix notation, we utilize a stack to store operands. By iterating through the postfix expression, we can effectively combine operands with their respective operators to form the correct infix format.

### Algorithm

1. **Initialize a Stack**:

   - Create a stack to hold the operands.

2. **Iterate Over Characters**:

   - Traverse the postfix expression from the first character to the last:
     - If the character is an operand, push it onto the stack.
     - If the character is an operator, pop the top two operands from the stack, combine them with the operator in infix format (operand1 operator operand2), and push the resulting string back onto the stack.

3. **Final Result**:
   - The last remaining item in the stack is the desired infix expression.

### Time Complexity

- **O(n)**: Each character in the postfix expression is processed once.

### Space Complexity

- **O(n)**: Space used by the stack to store operands.

### Code

```cpp
string postToInfix(string exp) {
    stack<string> s;

    for (char ch : exp) {
        // If the character is an operand, push it to the stack
        if (isalnum(ch)) {
            s.push(string(1, ch));
        }
        // If the character is an operator
        else {
            // Pop two operands from the stack
            string op2 = s.top(); s.pop();
            string op1 = s.top(); s.pop();

            // Combine the operands with the operator in infix format
            string infix = "(" + op1 + ch + op2 + ")";
            s.push(infix);
        }
    }

    // The final expression in the stack is the infix expression
    return s.top();
}
```

# 172. Infix to Prefix Conversion

### Article Link

[Convert Infix to Prefix Notation (GeeksforGeeks)](https://www.geeksforgeeks.org/convert-infix-prefix-notation/)

## Approach: Using Stack for Operator Precedence and Associativity with Expression Reversal

### Idea/Intuition

To convert an infix expression to prefix notation, reverse the expression, convert it to postfix notation with modified parentheses, then reverse the postfix result to get the prefix expression.

### Algorithm

1. **Preprocessing**:

   - Remove any whitespace from the expression.

2. **Reverse Expression**:

   - Reverse the infix expression and swap each '(' with ')' and vice versa.

3. **Infix to Postfix Conversion**:

   - Use a stack to convert the modified infix expression to postfix notation.
   - Push operators onto the stack based on precedence and associativity.
   - Handle operands, operators, and parentheses appropriately, popping and appending operators based on precedence.

4. **Reverse Postfix**:
   - Reverse the resulting postfix expression to get the prefix expression.

### Time Complexity

- **O(n)**: Each character in the infix expression is processed multiple times for reversal, transformation, and stack operations.

### Space Complexity

- **O(n)**: Space used by the stack and output strings.

### Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    if (op == '^') return 3;
    return 0;
}

bool isRightAssociative(char op) {
    return op == '^';  // Only '^' is right-associative
}

string infixToPostfix(string exp) {
    // Remove whitespace from the expression
    exp.erase(remove_if(exp.begin(), exp.end(), ::isspace), exp.end());

    string postfix;
    stack<char> operators;

    for (char ch : exp) {
        // If the character is an operand, add it to the result
        if (isalnum(ch)) {
            postfix += ch;
        }
        // If the character is '(', push it to the stack
        else if (ch == '(') {
            operators.push(ch);
        }
        // If the character is ')', pop and output from the stack until '(' is encountered
        else if (ch == ')') {
            while (!operators.empty() && operators.top() != '(') {
                postfix += operators.top();
                operators.pop();
            }
            if (!operators.empty()) {  // Check if there was a matching '('
                operators.pop(); // Pop '('
            }
        }
        // An operator is encountered
        else {
            while (!operators.empty() && operators.top() != '(' &&
                   ((isRightAssociative(ch) && precedence(ch) < precedence(operators.top())) ||
                    (!isRightAssociative(ch) && precedence(ch) <= precedence(operators.top())))) {
                postfix += operators.top();
                operators.pop();
            }
            operators.push(ch);
        }
    }

    // Pop all the operators left in the stack
    while (!operators.empty()) {
        if (operators.top() != '(') {  // Skip any unmatched '(' if present
            postfix += operators.top();
        }
        operators.pop();
    }

    return postfix;
}

string infixToPrefix(string infix) {
    // Remove whitespace from the input
    infix.erase(remove_if(infix.begin(), infix.end(), ::isspace), infix.end());

    // Step 1: Reverse the infix expression
    reverse(infix.begin(), infix.end());

    // Step 2: Replace '(' with ')' and vice versa
    for (char &ch : infix) {
        if (ch == '(') ch = ')';
        else if (ch == ')') ch = '(';
    }

    // Step 3: Convert the modified infix expression to postfix
    string postfix = infixToPostfix(infix);

    // Step 4: Reverse the postfix expression to get the prefix expression
    reverse(postfix.begin(), postfix.end());
    return postfix;
}

int main() {
    string infix = "3^(1+1)";
    cout << "Infix Expression: " << infix << endl;
    string prefix = infixToPrefix(infix);
    cout << "Prefix Expression: " << prefix << endl;

    // Test with whitespace
    string infix2 = "3 ^ (1 + 1)";
    cout << "\nInfix Expression with spaces: " << infix2 << endl;
    string prefix2 = infixToPrefix(infix2);
    cout << "Prefix Expression: " << prefix2 << endl;

    return 0;
}
```

# 173. Next Greater Element I

### Problem Link

[LeetCode - Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/description/)

## Approach 1: Brute Force Method

### Idea/Intuition

In this approach, for each element in `nums1`, we search for the next greater element in `nums2` by iterating through `nums2` until we find the first element that is greater.

### Algorithm

1. **Initialize Result Vector**:

   - Create a vector `ans` to store the results for each element in `nums1`.

2. **Iterate Over Elements of nums1**:

   - For each element `num` in `nums1`:
     - Set a flag `found` to false and a variable `greater` to -1.
     - Traverse through `nums2`:
       - If the current element in `nums2` equals `num`, set `found` to true.
       - If `found` is true and the current element in `nums2` is greater than `num`, set `greater` to the current element and break out of the loop.

3. **Store the Result**:

   - Add the `greater` value to `ans`.

4. **Return Result**:
   - Return the `ans` vector.

### Time Complexity

- **O(n \* m)**: Where `n` is the size of `nums1` and `m` is the size of `nums2`.

### Space Complexity

- **O(1)**: Constant space used for the result vector.

### Code

```cpp
vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
    vector<int> ans;

    for (int i = 0; i < nums1.size(); i++) {
        int found = false;
        int greater = -1;

        // Find nums1[i] in nums2
        for (int j = 0; j < nums2.size(); j++) {
            if (nums2[j] == nums1[i]) {
                found = true;
            }

            // If found, look for the next greater element to the right
            if (found && nums2[j] > nums1[i]) {
                greater = nums2[j];
                break;
            }
        }

        ans.push_back(greater);
    }

    return ans;
}
```

## Approach 2: Using Stack for Next Greater Element Mapping

### Idea/Intuition

This approach utilizes a stack to efficiently find the next greater element for each element in `nums2`. By maintaining a mapping of elements to their next greater elements, we can quickly construct the result for `nums1`.

### Algorithm

1. **Initialize Stack and Map**:

   - Create a stack `s` to store elements and an unordered map `nextGreaterMap` to store the next greater element for each element in `nums2`.

2. **Build the Next Greater Map**:

   - Iterate through each element `num` in `nums2`:
     - While the stack is not empty and the top of the stack is less than `num`, pop from the stack and map the popped element to `num` in `nextGreaterMap`.
     - Push `num` onto the stack.

3. **Handle Remaining Elements**:

   - After processing all elements in `nums2`, pop any remaining elements in the stack and map them to -1, indicating no greater element exists.

4. **Construct Result for nums1**:

   - Create a vector `result` to store the results for `nums1`.
   - For each element in `nums1`, retrieve the next greater element from `nextGreaterMap` and add it to `result`.

5. **Return Result**:
   - Return the `result` vector.

### Time Complexity

- **O(n)**: Each element in `nums2` is processed once.

### Space Complexity

- **O(n)**: Space used by the stack and the map.

### Code

```cpp
vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
    unordered_map<int, int> nextGreaterMap;
    stack<int> s;

    // Build the next greater map for elements in nums2
    for (int num : nums2) {
        while (!s.empty() && s.top() < num) {
            nextGreaterMap[s.top()] = num;
            s.pop();
        }
        s.push(num);
    }

    // Remaining elements in the stack don't have a next greater element
    while (!s.empty()) {
        nextGreaterMap[s.top()] = -1;
        s.pop();
    }

    // Populate the result for nums1 using the next greater map
    vector<int> result;
    for (int num : nums1) {
        result.push_back(nextGreaterMap[num]);
    }

    return result;
}
```

# 174. Next Greater Element II

### Problem Link

[LeetCode - Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/)

## Approach 1: Brute Force Method

### Idea/Intuition

In this approach, for every element in the array, we iterate through the subsequent elements in a circular manner to find the next greater element. If no such element exists, we store -1 in the answer list.

### Algorithm

1. **Initialize Result Vector**:

   - Create a vector `ans` of the same size as `nums`, initialized to -1.

2. **Outer Loop**:

   - Iterate through each element in `nums` using index `i`.

3. **Inner Loop**:

   - For each element at index `i`, iterate through the subsequent elements (from `i+1` to `2*n-1`):
     - Use modulo operation to wrap around the index (`j % n`) to handle the circular nature.
     - Check if `nums[j % n]` is greater than `nums[i]`:
       - If found, store `nums[j % n]` in `ans[i]` and break the inner loop.

4. **Return Result**:
   - Return the `ans` vector.

### Time Complexity

- **O(n^2)**: Each element may lead to another complete pass through the array.

### Space Complexity

- **O(n)**: Space used for the result vector.

### Code

```cpp
vector<int> nextGreaterElements(vector<int>& nums) {
    int n = nums.size();
    vector<int> ans(n, -1); // Initialize result vector with -1

    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < i + n; j++) { // Loop through the next elements
            if (nums[j % n] > nums[i]) { // Check circularly
                ans[i] = nums[j % n]; // Store the next greater element
                break; // Break the inner loop
            }
        }
    }

    return ans;
}
```

## Approach 2: Using Stack

### Idea/Intuition

This approach utilizes a stack to efficiently find the next greater element for each element in a circular array. By iterating through the array twice, we can efficiently determine the next greater element.

### Algorithm

1. **Initialize Stack and Result Vector**:

   - Create a stack `st` and a result vector `ans` initialized with -1.

2. **Iterate Through Elements**:

   - Loop from `2*size - 1` down to `0` to process each element in reverse order:
     - While the stack is not empty and the current element is greater than or equal to the top of the stack, pop elements from the stack.
     - If the stack is not empty and the current index is within the original size, set `ans[i]` to the top of the stack (next greater element).

3. **Push Current Element**:

   - Push the current element (using modulo to wrap around) onto the stack.

4. **Return Result**:
   - Return the `ans` vector.

### Time Complexity

- **O(n)**: Each element is processed a constant number of times.

### Space Complexity

- **O(n)**: Space used by the stack and result vector.

### Code

```cpp
vector<int> nextGreaterElements(vector<int>& nums) {
    int size = nums.size();
    stack<int> st;
    vector<int> ans(size, -1);

    for (int i = 2 * size - 1; i >= 0; i--) {
        while (!st.empty() && nums[i % size] >= st.top()) {
            st.pop(); // Pop elements smaller than current
        }
        if (!st.empty() && i < size) {
            ans[i] = st.top(); // Set next greater element
        }
        st.push(nums[i % size]); // Push current element
    }
    return ans;
}
```

# 175. Next Smaller Element

### Problem Link

[Naukri - Next Smaller Element](https://www.naukri.com/code360/problems/next-smaller-element_1112581)

## Approach 1: Brute Force Method

### Idea/Intuition

In this approach, for each element in the array, we iterate through the rest of the array to find if there exists any element that is just smaller than the current element. If such an element is found, we store it; if no such element is found after traversing the whole array, we store -1.

### Algorithm

1. **Initialize Result Vector**:

   - Create a result vector `ans` initialized with -1.

2. **Iterate Over Elements**:

   - For each element in the array:
     - Check all subsequent elements to find the next smaller element.
     - If found, store it in `ans` and break out of the loop.
     - If not found, `ans[i]` remains -1.

3. **Return Result**:
   - Return the `ans` vector.

### Time Complexity

- **O(n^2)**: Each element may require checking all other elements.

### Space Complexity

- **O(n)**: Space used for the result vector.

### Code

```cpp
vector<int> nextSmallerElement(vector<int> &arr, int n) {
    vector<int> ans(n, -1);
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[i]) {
                ans[i] = arr[j];
                break;
            }
        }
    }
    return ans;
}
```

## Approach 2: Using Stack

### Idea/Intuition

This approach utilizes a stack to efficiently find the next smaller element for each element in the array by traversing it from right to left. The stack helps keep track of the elements for which we haven't found a smaller element yet.

### Algorithm

1. **Initialize Result Vector and Stack**:

   - Create a result vector `ans` initialized with -1.
   - Initialize an empty stack.

2. **Iterate Through Elements in Reverse**:

   - Loop through the array from the last element to the first:
     - While the stack is not empty and the top of the stack is greater than or equal to the current element, pop elements from the stack.
     - If the stack is not empty, set `ans[i]` to the top of the stack (next smaller element).
     - Push the current element onto the stack.

3. **Return Result**:
   - Return the `ans` vector.

### Time Complexity

- **O(n)**: Each element is processed a constant number of times.

### Space Complexity

- **O(n)**: Space used by the stack and result vector.

### Code

```cpp
#include <stack>
vector<int> nextSmallerElement(vector<int> &arr, int n) {
    vector<int> ans(n, -1);
    stack<int> st;
    for (int i = n - 1; i >= 0; i--) {
        while (!st.empty() && st.top() >= arr[i]) {
            st.pop();
        }
        if (!st.empty()) {
            ans[i] = st.top();
        }
        st.push(arr[i]);
    }
    return ans;
}
```
