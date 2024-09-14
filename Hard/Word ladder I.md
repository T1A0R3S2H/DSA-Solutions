### Code:
```cpp
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        // Creating a queue ds of type {word, transitions to reach ‘word’}.
        queue<pair<string, int>> q;

        // BFS traversal with pushing values in queue 
        // when after a transformation, a word is found in wordList.
        q.push({beginWord, 1});

        // Push all values of wordList into a set
        // to make deletion from it easier and in less time complexity.
        unordered_set<string> st(wordList.begin(), wordList.end());
        st.erase(beginWord);
        while (!q.empty())
        {
            string word = q.front().first;
            int steps = q.front().second;
            q.pop();

            // we return the steps as soon as
            // the first occurence of targetWord is found.
            if (word == endWord)
                return steps;

            for (int i = 0; i < word.size(); i++)
            {
                // Now, replace each character of ‘word’ with char
                // from a-z then check if ‘word’ exists in wordList.
                char original = word[i];
                for (char ch = 'a'; ch <= 'z'; ch++)
                {
                    word[i] = ch;
                    // check if it exists in the set and push it in the queue.
                    if (st.find(word) != st.end())
                    {
                        st.erase(word);
                        q.push({word, steps + 1});
                    }
                }
                word[i] = original;
            }
        }
        // If there is no transformation sequence possible
        return 0;
    }
};
```


### Explanation:
The problem asks for the shortest transformation sequence from a `beginWord` to an `endWord` using a list of words (`wordList`), where each transformation changes only one character at a time. The transformation sequence must exist in the wordList and must differ by exactly one character between adjacent words.

The approach uses **Breadth-First Search (BFS)** to explore the shortest path from `beginWord` to `endWord`. A queue is used to track the current word and the number of transformations taken to reach that word. For each word, the algorithm tries replacing each letter of the word with every possible letter ('a' to 'z') and checks if the transformed word exists in the `wordList`.

- **Queue**: It stores pairs of `{word, steps}` where `word` is the current word, and `steps` is the number of transformations taken to reach it.
- **Set**: A set is used to store words from the wordList for O(1) lookup and efficient deletion of words already processed.
- **Termination**: As soon as `endWord` is found, the algorithm returns the number of steps required. If the queue is exhausted and no transformation to `endWord` is possible, it returns 0.

### Time Complexity:
- The BFS runs while there are words in the queue. For each word, the algorithm generates all possible transformations by changing each character (26 possible transformations per character).
- There are at most `n` words in the word list, and for each word, we attempt `O(L*26)` transformations, where `L` is the length of the word.

Thus, the time complexity is approximately **O(n * L * 26)**, which simplifies to **O(n * L)**, where:
- `n` is the number of words in the `wordList`,
- `L` is the length of each word.

### Space Complexity:
- The space complexity is **O(n)** for the queue and set, where `n` is the number of words in the wordList.
- Each word transformation is stored in the queue, and the set is used to store the remaining words.

Thus, the space complexity is **O(n)**.

### Dry Run:
Let's take an example to understand how the algorithm works:
- **Input**: `beginWord = "hit"`, `endWord = "cog"`, `wordList = ["hot", "dot", "dog", "lot", "log", "cog"]`.
  
1. **Initialization**: 
   - Queue: `[{ "hit", 1 }]` (start with `hit`, step count is 1).
   - Set: `{"hot", "dot", "dog", "lot", "log", "cog"}`.

2. **First iteration**:
   - Word: `"hit"`, Steps: `1`.
   - Try replacing each character in `"hit"`:
     - Change 'h' -> 'a' to 'z'. Only `"hot"` is valid and in the set.
     - Queue: `[{ "hot", 2 }]`.
     - Remove `"hot"` from the set.

3. **Second iteration**:
   - Word: `"hot"`, Steps: `2`.
   - Try replacing each character in `"hot"`:
     - Only `"dot"` and `"lot"` are valid transformations.
     - Queue: `[{ "dot", 3 }, { "lot", 3 }]`.
     - Remove `"dot"` and `"lot"` from the set.

4. **Third iteration**:
   - Word: `"dot"`, Steps: `3`.
   - Try replacing each character in `"dot"`:
     - Only `"dog"` is valid.
     - Queue: `[{ "lot", 3 }, { "dog", 4 }]`.
     - Remove `"dog"` from the set.

5. **Fourth iteration**:
   - Word: `"lot"`, Steps: `3`.
   - Try replacing each character in `"lot"`:
     - Only `"log"` is valid.
     - Queue: `[{ "dog", 4 }, { "log", 4 }]`.
     - Remove `"log"` from the set.

6. **Fifth iteration**:
   - Word: `"dog"`, Steps: `4`.
   - Try replacing each character in `"dog"`:
     - Only `"cog"` is valid.
     - Queue: `[{ "log", 4 }, { "cog", 5 }]`.
     - Remove `"cog"` from the set.

7. **Sixth iteration**:
   - Word: `"cog"`, Steps: `5`.
   - We have found the `endWord`, so we return `5` as the number of transformations.

### Final Output: `5`.
