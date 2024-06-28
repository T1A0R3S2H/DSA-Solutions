**Method 1 (brute force)**
================================================

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        if (haystack.length() < needle.length()) {
            return -1;
        }
        
        for (int i = 0; i <= haystack.length() - needle.length(); i++) {
            if (haystack.substr(i, needle.length()) == needle) {
                return i;
            }
        }
        
        return -1;        
    }
};
```



### Problem Statement

Given two strings, `haystack` and `needle`, find the starting index of `needle` in `haystack`. If `needle` is not found in `haystack`, return -1.

### Solution

The solution uses a simple string matching algorithm, also known as the "brute force" approach. It iterates through the `haystack` string and checks if the current substring matches the `needle` string. If a match is found, it returns the starting index of the match. If no match is found after checking all substrings, it returns -1.

### Key Concepts

* **String Substring**: A substring is a contiguous sequence of characters within a string.
* **String Matching**: The process of finding a specific string (the `needle`) within another string (the `haystack`).

### Algorithm Steps

1. Check if the length of `haystack` is less than the length of `needle`. If true, return -1 (since `needle` cannot be found in `haystack`).
2. Iterate through the `haystack` string using a loop variable `i`.
3. For each iteration, extract a substring from `haystack` starting at index `i` and with a length equal to the length of `needle`.
4. Compare the extracted substring with `needle`. If they match, return the starting index `i`.
5. If no match is found after checking all substrings, return -1.

### Time Complexity

The time complexity of this algorithm is O(n*m), where n is the length of `haystack` and m is the length of `needle`. This is because we are iterating through the `haystack` string and comparing each substring with `needle`.

### Space Complexity

The space complexity of this algorithm is O(1), since we are only using a constant amount of space to store the loop variables and the substring.
