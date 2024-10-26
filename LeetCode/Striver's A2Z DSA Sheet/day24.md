# 116. String to Integer (atoi)

### Problem Link

[LeetCode - String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/description/)

## Approach: Recursive Method

### Algorithm:

1. **Skip Leading Whitespaces:**  
   Traverse the string until all leading spaces are skipped.

2. **Handle Optional Sign:**  
   Check if the character at the current index is either `+` or `-`.  
   Set a boolean flag `isNeg` to `true` if the character is `-` to signify a negative number.

3. **Recursive Conversion of Digits:**  
   Define a helper function `myAtoiRecursive` that processes each character one by one:

   - **Base Case:** If the current character is not a digit or the index is out of bounds, return the accumulated result.
   - **Overflow Check:** Before adding each digit, check if adding it would cause overflow.
   - **Digit Accumulation:** Multiply the accumulated value by 10, add the current digit, and move to the next character.

4. **Return Result with Sign:**  
   The final result is returned with the appropriate sign based on `isNeg`.

### Time Complexity:

- **O(n):** We process each character once.

### Space Complexity:

- **O(n):** The recursion stack may reach a depth of `n` in the worst case.

### Code:

```cpp
int myAtoiRecursive(string& s, int i, bool isNeg, long no) {
    // Base case: If index `i` is out of bounds or the character is not a digit, return the result
    if (i >= s.length() || s[i] < '0' || s[i] > '9') {
        return isNeg ? -no : no;
    }
    int digit = s[i] - '0';
    // Check for overflow
    if (!isNeg && no > (INT_MAX - digit) / 10) {
        return INT_MAX;
    }
    if (isNeg && no > (INT_MAX - digit) / 10) {
        return INT_MIN;
    }
    // Accumulate the digit in `no` and move to the next character
    no = no * 10 + digit;
    return myAtoiRecursive(s, i + 1, isNeg, no);
}

int myAtoi(string s) {
    int i = 0;
    int n = s.length();

    // Step 1: Skip leading whitespaces
    while (i < n && s[i] == ' ') {
        i++;
    }

    // Step 2: Handle optional sign
    bool isNeg = false;
    if (i < n && (s[i] == '+' || s[i] == '-')) {
        if (s[i] == '-') {
            isNeg = true;
        }
        i++;
    }

    // Step 3: Begin recursion to process digits
    return myAtoiRecursive(s, i, isNeg, 0);
}
```

# 117. Pow(x, n)

### Problem Link

[LeetCode - Pow(x, n)](https://leetcode.com/problems/powx-n/description/)

## Approach 1: Simple Recursive Method

### Algorithm:

1. **Define Base Case:**  
   In the helper function, if `power` is `0`, return `1` since any number to the power of `0` is `1`.

2. **Recursive Step:**  
   Multiply the number `x` with the result of `myPowRecursive(x, power - 1)` to gradually reduce the power by `1` in each recursive call.

3. **Handle Negative Power:**  
   In the main function:
   - If `n` is negative, calculate the result for `-n` using the recursive helper and then return `1 / result`.
   - If `n` is positive, return the result directly from the recursive helper function.

### Time Complexity:

- **O(n):** The recursion depth is `n`, with each call multiplying once.

### Space Complexity:

- **O(n):** The recursion stack depth reaches `n`.

### Code:

```cpp
double myPowRecursive(double x, int power) {
    // Base case: If power is 0, return 1
    if (power == 0) {
        return 1;
    }
    // Recursive step: Multiply x and call with power - 1
    return x * myPowRecursive(x, power - 1);
}

double myPow(double x, int n) {
    // If n is negative, compute the reciprocal
    if (n < 0) {
        return 1 / myPowRecursive(x, -n);
    }
    // If n is positive, return the result of the recursive helper function
    return myPowRecursive(x, n);
}
```

## Approach 2: Recursive Halving Method

### Algorithm:

1. **Convert Negative Exponent to Positive:**  
   If `n` is negative, convert it to positive for ease of computation. The final result will be the reciprocal of the answer.

2. **Recursive Halving:**

   - If `n` is even, calculate `myPowRecursive(x * x, n / 2)`, which squares `x` and halves `n`.
   - If `n` is odd, calculate `x * myPowRecursive(x, n - 1)` by multiplying `x` and decrementing `n` by 1.

3. **Apply Reciprocal if Needed:**  
   If `n` was initially negative, return `1 / result`, otherwise return the result as-is.

### Time Complexity:

- **O(log n):** Recursive calls reduce `n` by half each time.

### Space Complexity:

- **O(log n):** Stack depth is `log n` due to recursive calls.

### Code:

```cpp
double myPowRecursive(double x, long long n) {
    // Base case: If power is 0, return 1
    if (n == 0) {
        return 1.0;
    }

    // Recursive halving: If n is even, square x and halve n
    if (n % 2 == 0) {
        return myPowRecursive(x * x, n / 2);
    }
    // If n is odd, multiply x and reduce n by 1
    else {
        return x * myPowRecursive(x, n - 1);
    }
}

double myPow(double x, int n) {
    long long nn = n; // Convert int to long long to handle negative overflow

    // Convert negative exponent to positive and apply reciprocal later
    if (nn < 0) {
        nn = -nn;
        return 1.0 / myPowRecursive(x, nn);
    }

    // Call recursive helper function for positive nn
    return myPowRecursive(x, nn);
}
```

# 118. Count Good Numbers

### Problem Link

[LeetCode - Count Good Numbers](https://leetcode.com/problems/count-good-numbers/description/)

## Approach: Mathematical Calculation Using Modular Exponentiation

### Algorithm

1. **Counting Positions**:

   - The number of even positions can be calculated as \((n + 1) / 2\).
   - The number of odd positions is \( n / 2\).

2. **Calculating Combinations**:

   - For each even position, there are \( 5 \) possible digits (0, 2, 4, 6, 8).
   - For each odd position, there are \( 4 \) possible digits (1, 3, 5, 7, 9).
   - Thus, the total combinations can be expressed as:
     \[
     \text{Total Good Numbers} = 5^{\text{evenPositions}} \times 4^{\text{oddPositions}}
     \]

3. **Modular Arithmetic**:
   - To handle large numbers, we will return the result modulo \( 10^9 + 7 \).

### Time Complexity

- **O(log n)**: Exponentiation is performed in logarithmic time due to the fast exponentiation method.

### Space Complexity

- **O(1)**: Only a constant amount of extra space is used.

### Code

```cpp
const int MOD = 1e9 + 7;

long long myPow(long long base, long long exp) {
    long long result = 1;
    while (exp > 0) {
        if (exp % 2 == 1) {
            result = (result * base) % MOD; // If exp is odd, multiply the base
        }
        base = (base * base) % MOD; // Square the base
        exp /= 2; // Divide the exponent by 2
    }
    return result;
}

int countGoodNumbers(long long n) {
    // Calculate the number of even and odd positions
    long long evenPositions = (n + 1) / 2;
    long long oddPositions = n / 2;

    long long evenCount = myPow(5, evenPositions); // 5^evenPositions % MOD
    long long oddCount = myPow(4, oddPositions); // 4^oddPositions % MOD

    return (evenCount * oddCount) % MOD; // (5^evenPositions * 4^oddPositions) % MOD
}
```

# 119. Sort a Stack

### Problem Link

[Naukri - Sort a Stack](https://www.naukri.com/code360/problems/sort-a-stack_985275)

## Approach: Recursive Sort and Insert

### Algorithm:

1. **Remove Elements Temporarily:**

   - Recursively pop elements from the stack until it becomes empty. This helps to remove all elements from the stack, so they can be inserted back in sorted order.

2. **Insert in Sorted Order:**

   - For each popped element, use another recursive function to insert it back into the stack in sorted order.
   - The base condition for insertion is when the stack is empty or the top element is less than the current element.

3. **Recursive Rebuilding:**
   - After the recursive call for sorting is complete, each element is re-inserted back to maintain sorted order in the stack.

### Time Complexity:

- **O(n^2):** Each element requires an insertion that may involve multiple recursive calls.

### Space Complexity:

- **O(n):** Stack depth is `n` due to recursive calls.

### Code:

```cpp
void sortedInsert(stack<int>& stack, int num) {
    // Base case: If stack is empty or top element is smaller than num
    if (stack.empty() || stack.top() < num) {
        stack.push(num);
        return;
    }

    // Remove the top element and insert num in sorted order
    int temp = stack.top();
    stack.pop();
    sortedInsert(stack, num);

    // Push the stored element back into the stack
    stack.push(temp);
}

void sortStack(stack<int>& stack) {
    // Base case: If stack is empty, return
    if (stack.empty()) {
        return;
    }

    // Step 1: Remove the top element
    int num = stack.top();
    stack.pop();

    // Step 2: Sort the remaining stack recursively
    sortStack(stack);

    // Step 3: Insert the removed element back in sorted order
    sortedInsert(stack, num);
}
```

# 120. Reverse a Stack using Recursion

### Problem Link

[Naukri - Reverse a Stack using Recursion](https://www.naukri.com/code360/problems/reverse-stack-using-recursion_631875)

## Approach: Recursive Reversal with Helper Function

### Algorithm

1. **Base Case (reverseStack)**: If the stack is empty, return immediately.
2. **Recursive Step**:
   - Save the top element and pop it.
   - Recursively reverse the remaining stack.
   - Use a helper function `insertAtBottom` to insert the saved element at the bottom of the reversed stack.
3. **InsertAtBottom Function**:
   - Inserts the element at the bottom by recursively removing all elements, inserting the new element, and re-pushing the removed elements back on top.

### Time Complexity

- **O(n²)**: Each recursive `reverseStack` call has an `insertAtBottom` call, which may operate over all elements.

### Space Complexity

- **O(n)**: Due to recursive call stack.

### Code

```cpp
void insertAtBottom(stack<int>& stack, int element) {
    // Base case for inserting at bottom
    if (stack.empty()) {
        stack.push(element);
        return;
    }

    // Save the top element and pop it to reach the bottom
    int topElement = stack.top();
    stack.pop();

    // Recursive call to insert `element` at the bottom of the stack
    insertAtBottom(stack, element);

    // Push the saved element back onto the stack
    stack.push(topElement);
}

void reverseStack(stack<int>& stack) {
    // Base case: If stack is empty, return
    if (stack.empty()) return;

    // Save the current top element and pop it
    int topElement = stack.top();
    stack.pop();

    // Recursive call to reverse the remaining stack
    reverseStack(stack);

    // Insert the saved element at the bottom of the reversed stack
    insertAtBottom(stack, topElement);
}
```
