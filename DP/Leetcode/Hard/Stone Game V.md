### Code

```cpp
class Solution {
public:
    int solveMem(int left, int right, vector<int>& stoneValue, vector<vector<int>>& dp) {
        // Base case: agar sirf ek stone bacha ho
        if (left == right) {
            return 0;
        }
        if (dp[left][right] != -1) return dp[left][right];

        int maxScore = 0;
        int leftSum = 0;
        int rightSum = accumulate(stoneValue.begin() + left, stoneValue.begin() + right + 1, 0);
        
        // Har possible cut ke liye check karte hain
        for (int cut = left; cut < right; ++cut) {
            leftSum += stoneValue[cut];
            rightSum -= stoneValue[cut];
            
            if (leftSum > rightSum) {
                // Left part zyada hai, Bob left hata dega
                maxScore = max(maxScore, rightSum + solveMem(cut + 1, right, stoneValue, dp));
            } 
            else if (leftSum < rightSum) {
                // Right part zyada hai, Bob right hata dega
                maxScore = max(maxScore, leftSum + solveMem(left, cut, stoneValue, dp));
            } 
            else {
                // Agar sums barabar hain, dono cases check karenge
                maxScore = max(maxScore, leftSum + solveMem(left, cut, stoneValue, dp));
                maxScore = max(maxScore, rightSum + solveMem(cut + 1, right, stoneValue, dp));
            }
        }

        dp[left][right] = maxScore;
        return maxScore;
    }

    int stoneGameV(vector<int>& stoneValue) {
        int n = stoneValue.size();
        vector<vector<int>> dp(n, vector<int>(n, -1));
        return solveMem(0, n - 1, stoneValue, dp);
    }
};
```

---

### Explanation (E)

#### Problem Samajhte Hain:
1. **Alice aur Bob ka khel**:
   - Alice ek row ko do parts mein todti hai (left aur right).
   - Bob dono parts ka sum nikalta hai aur **zyada value wale part ko hata deta hai**.
   - **Kam value wala part Alice ke score mein add hota hai.**
   - Agar dono parts ka sum barabar hai, to Alice decide karti hai ki kaunsa part hataana hai.

2. **Goal**:
   - Alice ka maximum possible score return karna hai.

#### Approach:
1. **Recursive Function**:
   - Function `solveMem(left, right, stoneValue, dp)` banayi jo subarray `[left, right]` ke liye maximum score calculate kare.
   - Har possible cut ke liye:
     - Left aur Right ka sum calculate karte hain.
     - Decision lete hain ki kaunsa part hataana hai (`leftSum` aur `rightSum` ke basis pe).
   - Recursion call karte hain bache hue part ke liye.

2. **Memoization**:
   - DP array (`dp`) banayi jisme har subarray `[left, right]` ka result store hota hai taaki redundant calculations avoid ho.

3. **Iterative Cuts**:
   - Har position `cut` pe array todte hain aur left aur right ka sum update karte hain.
   - Conditions ke basis pe `maxScore` update karte hain:
     - Agar `leftSum > rightSum`, to Bob **left part hata deta hai**, aur Alice ka score `rightSum` mein add hota hai.
     - Agar `leftSum < rightSum`, to Bob **right part hata deta hai**, aur Alice ka score `leftSum` mein add hota hai.
     - Agar `leftSum == rightSum`, to dono possibilities consider karte hain aur maximum choose karte hain.

4. **Base Case**:
   - Agar sirf ek hi stone bacha hai (`left == right`), to score `0` hoga kyunki aur todna possible nahi.


#### leftSum aur rightSum ka khel:

1. **Initial Setup**:
   - Jab hum `left` aur `right` ke beech me subarray ko split kar rahe hote hain (cut ke through), tab hum **leftSum** aur **rightSum** track kar rahe hain.
   - **leftSum** wo sum hai jo left part me stones ki value ka hai.
   - **rightSum** wo sum hai jo right part me stones ki value ka hai.

2. **First Line: `leftSum += stoneValue[cut];`**
   - Yeh line **leftSum** ko update kar rahi hai.
   - **leftSum** ko abhi tak jitna value add kiya gaya tha, usme current `cut` ke position ka stone value add kar rahe hain.
   - Jaise-jaise hum cut ko aage badhate hain, hum left part me stones ko include karte jaayenge, isliye **leftSum** ko increment kar rahe hain.

   **Example**:  
   Agar cut 1 pe ho, toh `leftSum` ko stoneValue[cut] (yaani stoneValue[1]) ke value ko add kiya jaayega.

3. **Second Line: `rightSum -= stoneValue[cut];`**
   - Yeh line **rightSum** ko update kar rahi hai.
   - **rightSum** ko jo value tha, usme se `cut` position ka stone value minus kar rahe hain.
   - Jab hum `cut` position par move karte hain, toh left part me stone ko add karte hain, isliye right part me wo stone ab nahi hoga, isliye hum **rightSum** me se us stone ki value subtract karte hain.

   **Example**:  
   Agar cut 1 pe ho, toh hum `rightSum` se stoneValue[1] ko subtract karenge, kyunki ab right part me wo stone nahi hoga.

---

#### Samjhaane ke liye ek example lete hain:

##### StoneValue: [6, 2, 3, 4, 5, 5]

- **Initial LeftSum = 0**, **RightSum = 25** (poora sum)

##### Cut at 0:
- **leftSum**:  
   - Pehle **leftSum = 0**
   - Phir **stoneValue[0] = 6** ko add kiya, toh **leftSum = 6**

- **rightSum**:
   - Pehle **rightSum = 25**
   - Phir **stoneValue[0] = 6** ko subtract kiya, toh **rightSum = 25 - 6 = 19**

---

##### Cut at 1:
- **leftSum**:  
   - Pehle **leftSum = 6** (previous value)
   - Phir **stoneValue[1] = 2** ko add kiya, toh **leftSum = 6 + 2 = 8**

- **rightSum**:
   - Pehle **rightSum = 19** (updated value from previous cut)
   - Phir **stoneValue[1] = 2** ko subtract kiya, toh **rightSum = 19 - 2 = 17**

---

Yeh process har cut ke liye repeat hota hai. Har baar hum left aur right part ke sums ko update karte hain, aur phir decide karte hain ki Bob kaunsa part hataayega, aur Alice ka score kya hoga.

---

#### Summary:
- **`leftSum += stoneValue[cut]`**: Left part mein ek aur stone add kar rahe hain.
- **`rightSum -= stoneValue[cut]`**: Right part se ek stone hata rahe hain.

##### Isse hum dynamically track kar pa rahe hain ki har cut ke baad left aur right parts ka sum kya hai, aur accordingly Alice ka score update kar pa rahe hain.
---

### Time Complexity (T)

- **Recursion Depth**: DP table ka size `n x n` hai aur har cut pe ek baar calculation hoti hai.
- **Cut Iterations**: Har subarray ke liye `(right - left)` cuts possible hain.
- **Total Complexity**: \( O(n^3) \).

---

### Space Complexity (S)

1. **DP Table**: \( O(n^2) \) space chahiye to store results for subarrays.
2. **Recursive Stack**: Maximum recursion depth \( O(n) \).
3. **Total Space**: \( O(n^2) \).

---

### Dry Run (D)

#### Input:
`stoneValue = [6, 2, 3, 4, 5, 5]`

