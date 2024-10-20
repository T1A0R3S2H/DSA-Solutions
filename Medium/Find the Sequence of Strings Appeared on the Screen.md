### Code
```cpp
class Solution {
public:
    vector<string> stringSequence(string target) {
        vector<string> answer;
        string prev="";
        for(int i=0; i<target.size(); i++) {
            prev+=target[i];
            for(char ch='a' ; ch<=target[i]; ch++) {
                prev.pop_back();
                answer.push_back(prev+ch);
                prev+=target[i];
            }
        }
        return answer;
    }
};
```

### Explanation
- `answer`: A vector to store the sequence of strings that appear on the screen.
- `prev`: Tracks the current string on the screen as each character from the `target` string is appended.
- For each character in `target`, the loop:
  1. Appends the character to `prev`.
  2. A nested loop from 'a' to the current character in `target` pops the last character of `prev`, appends a new character, and stores the result in `answer`.
  3. Re-appends the original character from `target` to `prev` after each iteration.

### Time Complexity
- **O(n * 26)** where `n` is the length of `target`. For each character in `target`, we loop over a maximum of 26 characters ('a' to 'z').

### Space Complexity
- **O(n)** where `n` is the length of `target`, as the space used to store the result scales linearly with the size of the target string.

### Dry Run (Line-by-Line)

Let's take `target = "abc"` and go through the code step by step.

#### Initial Setup
```cpp
vector<string> answer;   // Initializes an empty vector to store the results.
string prev = "";        // Starts with an empty string on the screen.
```

#### First Iteration (`i = 0`, `target[0] = 'a'`)
```cpp
prev += target[i];       // prev = "a" (appending the first character 'a').
```
Now, the inner loop:
```cpp
for (char ch = 'a'; ch <= target[0]; ch++) {
    prev.pop_back();         // prev becomes "" (removing last character).
    answer.push_back(prev + ch); // answer = ["a"] ("" + 'a').
    prev += target[i];       // prev = "a" (re-adding 'a').
}
```
- After this iteration, `answer = ["a"]`.

#### Second Iteration (`i = 1`, `target[1] = 'b'`)
```cpp
prev += target[i];       // prev = "ab" (appending 'b' to "a").
```
Now, the inner loop:
```cpp
for (char ch = 'a'; ch <= target[1]; ch++) {
    prev.pop_back();         // prev becomes "a" (removing last character).
    answer.push_back(prev + ch); // answer = ["a", "aa"] (prev + 'a').
    prev += target[i];       // prev = "ab" (re-adding 'b').
}
```
- For `ch = 'b'`:
```cpp
prev.pop_back();         // prev becomes "a" (removing last character).
answer.push_back(prev + ch); // answer = ["a", "aa", "ab"] (prev + 'b').
prev += target[i];       // prev = "ab" (re-adding 'b').
```
- After this iteration, `answer = ["a", "aa", "ab"]`.

#### Third Iteration (`i = 2`, `target[2] = 'c'`)
```cpp
prev += target[i];       // prev = "abc" (appending 'c' to "ab").
```
Now, the inner loop:
```cpp
for (char ch = 'a'; ch <= target[2]; ch++) {
    prev.pop_back();         // prev becomes "ab" (removing last character).
    answer.push_back(prev + ch); // answer = ["a", "aa", "ab", "aba"] (prev + 'a').
    prev += target[i];       // prev = "abc" (re-adding 'c').
}
```
- For `ch = 'b'`:
```cpp
prev.pop_back();         // prev becomes "ab" (removing last character).
answer.push_back(prev + ch); // answer = ["a", "aa", "ab", "aba", "abb"] (prev + 'b').
prev += target[i];       // prev = "abc" (re-adding 'c').
```
- For `ch = 'c'`:
```cpp
prev.pop_back();         // prev becomes "ab" (removing last character).
answer.push_back(prev + ch); // answer = ["a", "aa", "ab", "aba", "abb", "abc"] (prev + 'c').
prev += target[i];       // prev = "abc" (re-adding 'c').
```
- After this iteration, `answer = ["a", "aa", "ab", "aba", "abb", "abc"]`.

Finally, the function returns the `answer`.
