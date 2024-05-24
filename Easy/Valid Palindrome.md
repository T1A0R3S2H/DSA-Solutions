## Method 1
- Make a function jodo (to concatenate the sub-strings && to make all lower && to allow only alpha-numeric characters).
- Call the jodo function inside the isPalindrome function, and by using two pointers left and right, iterate from both the sides to the string, iff aage se and peeche are same then it is a palindrome.
##
- Time Complexity is O(n), The two-pointer technique iterates through the toCheck string up to half its length, resulting in a time complexity of 
O(m/2)=O(m), where m is the length of the cleaned string toCheck.
Since the length of toCheck can be at most equal to the length of s, the overall time complexity of the isPalindrome function remains 𝑂(𝑛).
- Space Complexity is also O(n) in the case when all the chars in checkPalindrome are alpha-numeric, ie it will go till the size of the strings, n.
##
```
class Solution {
public:


    bool isPalindrome(string s) {
        string toCheck = jodo(s);
        int left=0, right=toCheck.size() - 1;
        while (left<right) {
            if (toCheck[left] != toCheck[right]) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }



    string jodo(const string &s) { 
        string result;
        for (char c:s) {
            if (isalnum(c)) {
                result += tolower(c);
            }
        }
        return result;
    }
};

```
