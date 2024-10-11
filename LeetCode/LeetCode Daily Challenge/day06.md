# Problem: Sentence Similarity III

[LeetCode Problem Link](https://leetcode.com/problems/sentence-similarity-iii)

## Approach

To determine if two sentences are similar, we can identify a common prefix and suffix between them and check if the remaining words can be ignored. The steps are as follows:

1. **Split the Sentences into Words**: Break both sentences into individual words.
2. **Find Common Prefix**: Identify the longest sequence of matching words from the beginning of both sentences.
3. **Find Common Suffix**: Identify the longest sequence of matching words from the end of both sentences.
4. **Check the Leftover Part**:
   - Calculate the number of words that are left in each sentence after removing the common prefix and suffix.
   - If either sentence has no leftover words, they are similar.

### Time Complexity

- The time complexity is \(O(N)\), where \(N\) is the number of words in the longer sentence, due to the traversal of both sentences to find the prefix and suffix.

### Space Complexity

- The space complexity is \(O(M)\), where \(M\) is the number of words stored in the vectors for the split sentences.

## C++ Code Implementation

```cpp
class Solution {
public:
    bool areSentencesSimilar(string sentence1, string sentence2) {
        vector<string> words1, words2;
        stringstream ss1(sentence1), ss2(sentence2);
        string word;

        // Split sentence1 into words
        while (ss1 >> word) {
            words1.push_back(word);
        }

        // Split sentence2 into words
        while (ss2 >> word) {
            words2.push_back(word);
        }

        int len1 = words1.size();
        int len2 = words2.size();
        int prefixCount = 0, suffixCount = 0;

        // Find common prefix
        while (prefixCount < len1 && prefixCount < len2 && words1[prefixCount] == words2[prefixCount]) {
            prefixCount++;
        }

        // Find common suffix
        while (suffixCount < len1 - prefixCount && suffixCount < len2 - prefixCount &&
               words1[len1 - 1 - suffixCount] == words2[len2 - 1 - suffixCount]) {
            suffixCount++;
        }

        // Check the leftover part
        return (len1 - prefixCount - suffixCount <= 0 || len2 - prefixCount - suffixCount <= 0);
    }
};
```
