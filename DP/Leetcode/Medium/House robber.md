### **Code**:
```cpp
class Solution {
public:
    int tabu(vector<int>& nums){
        int n=nums.size();
        vector<int> dp(n, 0);  //represents the maximum amount
        dp[0]=nums[0];
        if (n>1) dp[1]=max(nums[0], nums[1]);  //to avoid heap buffer overflow
        for(int i=2; i<n; i++){
            int inc=dp[i-2]+nums[i];
            int ex=dp[i-1]+0;
            dp[i]=max(inc,  ex);
        }
        return dp[n-1];
    }
    int rob(vector<int>& nums) {
        return tabu(nums);
    }
};
```
---

### **Explanation**:

This problem asks you to determine the maximum amount of money you can rob from a list of houses, represented by an integer array `nums`, under the constraint that you cannot rob two adjacent houses on the same night. The solution uses dynamic programming to solve this efficiently.

- **Input**: A list `nums` where each element represents the amount of money at each house.
- **Output**: The maximum amount of money you can rob without robbing two consecutive houses.

**Dynamic Programming Approach**:
- Let `dp[i]` represent the maximum amount of money that can be robbed from the first `i` houses.
- For each house `i`, you have two choices:
  1. **Include the current house `i`**: Rob the current house and add its value to `dp[i-2]` (i.e., skip the adjacent house).
  2. **Exclude the current house `i`**: The maximum amount so far will be `dp[i-1]` (i.e., don't rob the current house).
  
- **Recurrence relation**:
  \[
  dp[i] = \max(dp[i-2] + nums[i], dp[i-1])
  \]
  The final result is stored in `dp[n-1]`, which gives the maximum money that can be robbed without breaking the constraint.

---

### **Time Complexity**:

- **Initialization**: Setting `dp[0] = nums[0]` and checking the second house if it exists takes constant time `O(1)`.
- **Loop**: The `for` loop runs from index 2 to `n-1`, so it iterates `n-2` times.
  
Hence, the overall time complexity is **O(n)**, where `n` is the length of the `nums` array.

---

### **Space Complexity**:

- The solution uses an extra array `dp` of size `n` to store the maximum amount of money that can be robbed up to each house.
  
Thus, the space complexity is **O(n)**.

- If space optimization is required, we could reduce it to **O(1)** by only keeping track of the last two states (`dp[i-2]` and `dp[i-1]`), as older states are no longer needed.

---

### **Dry Run**:

Let's walk through an example with the input `nums = [2, 7, 9, 3, 1]`.

1. **Initialization**:
   - `n = 5`
   - `dp = [0, 0, 0, 0, 0]`
   - Set `dp[0] = nums[0] = 2`, so `dp = [2, 0, 0, 0, 0]`
   - Set `dp[1] = max(nums[0], nums[1]) = max(2, 7) = 7`, so `dp = [2, 7, 0, 0, 0]`

2. **Loop**:
   - For `i = 2`:
     - `inc = dp[0] + nums[2] = 2 + 9 = 11`
     - `ex = dp[1] = 7`
     - `dp[2] = max(inc, ex) = max(11, 7) = 11`, so `dp = [2, 7, 11, 0, 0]`
   - For `i = 3`:
     - `inc = dp[1] + nums[3] = 7 + 3 = 10`
     - `ex = dp[2] = 11`
     - `dp[3] = max(inc, ex) = max(10, 11) = 11`, so `dp = [2, 7, 11, 11, 0]`
   - For `i = 4`:
     - `inc = dp[2] + nums[4] = 11 + 1 = 12`
     - `ex = dp[3] = 11`
     - `dp[4] = max(inc, ex) = max(12, 11) = 12`, so `dp = [2, 7, 11, 11, 12]`

3. **Result**: The maximum amount of money that can be robbed is `dp[4] = 12`.

Thus, the output for `nums = [2, 7, 9, 3, 1]` is `12`.
