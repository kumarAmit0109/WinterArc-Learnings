# Circular Sentence

**Problem Link**: [Circular Sentence](https://leetcode.com/problems/circular-sentence)

## Approach 1: Split and Check Word Connections

### Approach

1. **Split Sentence into Words**:

   - Use a loop to split the sentence by spaces and store each word in a vector `words`.

2. **Check Circular Condition**:

   - For each word `i`, check if the last character of `words[i]` matches the first character of the next word `words[(i + 1) % n]`.
   - If any consecutive words do not satisfy this condition, return `false`.

3. **Return Result**:
   - If all consecutive words are connected, return `true`.

### Time Complexity

- **O(n)**, where `n` is the number of characters in the sentence, as we parse each character once.

### Space Complexity

- **O(k)**, where `k` is the number of words in the sentence, as we store each word in a vector.

### Code Implementation

```cpp
class Solution {
public:
    bool isCircularSentence(string sentence) {
        vector<string> words;
        string word = "";

        // Split the sentence into words
        for (char ch : sentence) {
            if (ch == ' ') {
                words.push_back(word);
                word = "";
            } else {
                word += ch;
            }
        }
        words.push_back(word); // Add the last word

        int n = words.size();

        // Check the circular condition for all words
        for (int i = 0; i < n; ++i) {
            char lastChar = words[i].back();
            char nextFirstChar = words[(i + 1) % n].front();
            if (lastChar != nextFirstChar) {
                return false;
            }
        }

        return true;
    }
};
```

## Approach 2: Simple Iterative Check

### Approach

1. **Check Start and End Characters**:

   - First, check if the first and last characters of the sentence are the same. If not, the sentence cannot be circular.

2. **Iterate Through the Sentence**:

   - Traverse the sentence character by character, particularly focusing on the spaces between words.
   - When a space is encountered, ensure that the character before the space matches the character after the space.

3. **Return Result**:

   - If all checks pass, return `true`; otherwise, return `false`.

### Time Complexity

- **O(n)**, where `n` is the length of the sentence, as we traverse the string once.

### Space Complexity

- **O(1)**, as no additional data structures are used that grow with input size.

### Code Implementation

```cpp
class Solution {
public:
    bool isCircularSentence(string sentence) {
        int n = sentence.size();

        // Check if the sentence is circular by verifying start and end characters
        if (sentence.front() != sentence.back()) {
            return false;
        }

        // Iterate through the sentence, checking boundaries between words
        for (int i = 1; i < n; ++i) {
            if (sentence[i] == ' ' && sentence[i - 1] != sentence[i + 1]) {
                return false;
            }
        }

        return true;
    }
};
```
