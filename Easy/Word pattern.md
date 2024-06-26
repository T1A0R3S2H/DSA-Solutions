# Word Pattern Matching Algorithm

## Problem Statement

Given a `pattern` and a string `s`, find if `s` follows the same pattern.

- **Follow** means a full match, such that there is a bijection between a letter in `pattern` and a **non-empty** word in `s`.

## Examples

1. Input: pattern = "abba", s = "dog cat cat dog"
   Output: true

2. Input: pattern = "abba", s = "dog cat cat fish"
   Output: false

3. Input: pattern = "aaaa", s = "dog cat cat dog"
   Output: false

## Constraints

- 1 <= pattern.length <= 300
- `pattern` contains only lower-case English letters.
- 1 <= s.length <= 3000
- `s` contains only lowercase English letters and spaces ' '.
- `s` does not contain any leading or trailing spaces.
- All the words in `s` are separated by a **single space**.

## Algorithm

1. Create two unordered maps:
   - `cp`: maps characters from pattern to words in s
   - `pc`: maps words in s to characters in pattern

2. Split the string `s` into words:
   - Use `istringstream` to split the string

3. Check if the number of words matches the pattern length:
   - If not, return false

4. Iterate through the pattern and words simultaneously:
   - For each character `c` in pattern and word `wd`:
     - Check if `c` is already mapped to a word:
       - If yes and it's not `wd`, return false
     - Check if `wd` is already mapped to a character:
       - If yes and it's not `c`, return false
     - Update both mappings: `cp[c] = wd` and `pc[wd] = c`

5. If we complete the iteration without returning false, return true

## Code
```cpp
class Solution {
public:
    bool wordPattern(string p, string s) {
        unordered_map<char, string> cp;
        unordered_map<string, char> pc;
        vector<string> w;
        
        // Split s into words
        istringstream iss(s);
        string word;
        while (iss >> word) {
            w.push_back(word);
        }
        
        // Check if the number of words matches the pattern length
        if (p.length() != w.size()) {
            return false;
        }
        
        // Check the pattern
        for (int i = 0; i < p.length(); i++) {
            char c = p[i];
            string wd = w[i];
            
            // Check char to word mapping
            if (cp.count(c) && cp[c] != wd) return false;
            cp[c] = wd;
            
            // Check word to char mapping
            if (pc.count(wd) && pc[wd] != c) return false;
            pc[wd] = c;
        }
        
        return true;
    }
};




```

## Time Complexity

O(n), where n is the length of the pattern (or number of words in s)

## Space Complexity

O(n) for the maps and vector of words
