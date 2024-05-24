# LEETCODE-Array-1255
The provided solution aims to find the maximum score that can be obtained by selecting a subset of words from the given list of words, where each word contributes a score based on the provided letter scores, and the selection is constrained by the available letters.

Let's break down the approach used in the solution step-by-step:

1. **Initialization**:
   - The `maxScoreWords` method initializes a frequency count of the given letters using the `count` array. This array keeps track of how many of each letter are available.

2. **Depth-First Search (DFS) Approach**:
   - The main logic of the solution is implemented using a DFS approach in the `dfs` method. The method explores all possible subsets of words to find the one with the maximum score.
   - The `dfs` method is recursive and processes each word starting from the index `s`. For each word, it checks if the word can be formed using the available letters and calculates the score if the word is used.

3. **Using and Unusing Words**:
   - The `useWord` method checks if a word can be formed with the current letter counts. If it can, it decreases the counts accordingly and returns the score for that word. If the word cannot be formed, it returns -1.
   - The `unuseWord` method restores the counts of the letters after trying to use a word, ensuring the letter counts are correctly maintained for other recursive calls.

4. **Combining Results**:
   - The `dfs` method combines the results by either including or excluding each word and recursively calling itself to explore further combinations. It keeps track of the maximum score found.

Here's the complete class with the methods, as provided:

```java
class Solution {
  public int maxScoreWords(String[] words, char[] letters, int[] score) {
    int[] count = new int[26];
    for (final char c : letters)
      ++count[c - 'a'];
    return dfs(words, 0, count, score);
  }

  // Returns the maximum score you can get from words[s..n).
  private int dfs(String[] words, int s, int[] count, int[] score) {
    int ans = 0;
    for (int i = s; i < words.length; ++i) {
      final int earned = useWord(words, i, count, score);
      if (earned > 0)
        ans = Math.max(ans, earned + dfs(words, i + 1, count, score));
      unuseWord(words, i, count);
    }
    return ans;
  }

  int useWord(String[] words, int i, int[] count, int[] score) {
    boolean isValid = true;
    int earned = 0;
    for (final char c : words[i].toCharArray()) {
      if (--count[c - 'a'] < 0)
        isValid = false;
      earned += score[c - 'a'];
    }
    return isValid ? earned : -1;
  }

  void unuseWord(String[] words, int i, int[] count) {
    for (final char c : words[i].toCharArray())
      ++count[c - 'a'];
  }
}
```

### Key Points:
- **Recursion with Backtracking**: The solution uses recursion to explore all possible subsets of words and backtracks appropriately to restore the state after exploring each option.
- **Validation of Words**: Before considering a word's score, the solution checks if the word can be formed with the current letters using `useWord` and reverts the changes with `unuseWord` if necessary.
- **Maximizing the Score**: The combination of including or excluding each word helps in finding the subset that yields the maximum score.

This solution is efficient given the constraints typically expected for such problems and leverages recursive backtracking to explore all potential combinations.
