### TL;DR:
The solution maximizes the number of content children by sorting both the greed factors (`g`) and the cookie sizes (`s`). It uses a two-pointer approach to assign cookies to children where each child can get at most one cookie, and the cookie size must be greater than or equal to the child's greed factor.

### CETSD:

**Code:**
```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        int total = 0;
        if (s.size() == 0) return 0;
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int i = 0, j = 0;
        while (i < g.size() && j < s.size()) {
            if (g[i] <= s[j]) {
                total++;
                i++; 
                j++;
            } else {
                j++;
            }
        }
        return total;
    }
};
```

**Explanation:**
1. **Sorting:** Both the `g` (greed factors) and `s` (cookie sizes) arrays are sorted in ascending order.
2. **Two-pointer approach:** 
   - Pointer `i` traverses the `g` array (children), and pointer `j` traverses the `s` array (cookies).
   - If the current cookie size (`s[j]`) is greater than or equal to the current child’s greed factor (`g[i]`), then assign this cookie to the child, increment both pointers, and increase the `total` content children count.
   - If the cookie size is smaller than the child’s greed factor, move to the next cookie (increment `j`).

**Time Complexity:**  
- Sorting both `g` and `s` takes O(n log n) and O(m log m), respectively, where `n` is the number of children and `m` is the number of cookies.
- The two-pointer traversal takes O(n + m).  
Thus, the overall time complexity is **O(n log n + m log m)**.

**Space Complexity:**  
- The space complexity is **O(1)** because we are only using a few extra variables for the traversal (constant space).

**Dry Run Example:**

For `g = [1, 2, 3]` and `s = [1, 1]`:
- Sort `g`: `[1, 2, 3]`
- Sort `s`: `[1, 1]`
- Start with `i = 0` and `j = 0`:
  - `g[0] = 1`, `s[0] = 1`: `g[0] <= s[0]`, so assign this cookie. Increment `total` to 1, `i = 1`, `j = 1`.
  - `g[1] = 2`, `s[1] = 1`: `g[1] > s[1]`, move to the next cookie, `j = 2`.
- End of loop. The result is 1.

Output: **1**
