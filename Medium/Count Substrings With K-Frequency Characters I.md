### Code
```cpp
class Solution {
public:
    int numberOfSubstrings(string s, int k) {
        int ans=0;
        for(int i=0; i<s.size(); i++) {
            vector<int> vec(26, 0);
            for(int j=i; j<s.size(); j++) {
                vec[s[j]-'a']++;
                if(vec[s[j]-'a'] >= k) {
                    ans+=(s.size()-j);
                    break;
                }
            }
        }
        return ans;
    }
};
```

### Explanation
- This function counts the number of substrings in `s` where at least one character appears at least `k` times.
- We loop over all possible starting indices `i` and check all substrings starting at `i`.
- For each substring, the frequencies of characters are tracked using a vector `vec` of size 26 (one for each lowercase letter).
- As soon as a character in the substring reaches `k` occurrences, we add the count of all valid substrings that extend from this point until the end of the string, using `(s.size() - j)`.
- We then break out of the inner loop since extending the substring further will still produce valid substrings.

### Time Complexity
- Outer loop runs `O(n)` times, where `n` is the length of the string.
- Inner loop also runs `O(n)` times in the worst case.
- The overall time complexity is **O(n²)**.

### Space Complexity
- A fixed-size vector `vec` of size 26 is used to track character frequencies.
- Therefore, the space complexity is **O(1)**, since the space used does not scale with the input size.

### Dry Run
**Input**: s = "abacb", k = 2

1. **i = 0**:
   - j = 0: "a" (frequency of 'a' = 1, not valid).
   - j = 1: "ab" (frequency of 'b' = 1, not valid).
   - j = 2: "aba" (frequency of 'a' = 2, valid → 3 substrings: "aba", "abac", "abacb").
   - Count after this iteration: 3.

2. **i = 1**:
   - j = 1: "b" (frequency of 'b' = 1, not valid).
   - j = 2: "ba" (frequency of 'a' = 1, not valid).
   - j = 3: "bac" (frequency of 'c' = 1, not valid).
   - j = 4: "bacb" (frequency of 'b' = 2, valid → 1 substring: "bacb").
   - Count after this iteration: 4.

3. **i = 2**:
   - No valid substrings found starting at `i = 2`.

4. **i = 3**:
   - No valid substrings found starting at `i = 3`.

5. **i = 4**:
   - No valid substrings found starting at `i = 4`.

**Final Output**: 4 valid substrings.
