#### Code:
```cpp
class Solution {
public:
    int solveMem(int current, int clipboard, int n, vector<vector<int>>& dp) {
        if (current > n) return INT_MAX / 2; // Agar current n se bada ho jaye to invalid state
        if (current == n) return 0; // Agar current target ke barabar ho to no more operations needed

        if (dp[current][clipboard] != -1) return dp[current][clipboard]; // Agar already calculate ho chuka ho

        // Copy All operation
        int copyAll = INT_MAX / 2;
        if (clipboard != current) { // Agar clipboard me abhi tak current nahi hai to copy kar sakte hain
            copyAll = 1 + solveMem(current, current, n, dp);
        }

        // Paste operation
        int paste = INT_MAX / 2;
        if (clipboard > 0) { // Agar clipboard me kuch characters hain tabhi paste kar sakte hain
            paste = 1 + solveMem(current + clipboard, clipboard, n, dp);
        }

        return dp[current][clipboard] = min(copyAll, paste); // Minimum steps ka result store karke return karein
    }

    int minSteps(int n) {
        if (n == 1) return 0; // Agar n = 1 ho to koi operation ki zarurat nahi hai

        vector<vector<int>> dp(n + 1, vector<int>(n + 1, -1)); // DP array ko initialize karte hain
        return solveMem(1, 0, n, dp); // Initial state: current = 1, clipboard = 0
    }
};
```

---

#### Explanation:
1. **Recursive Idea**:
   - Hum ek `solveMem` function likhte hain jo screen ke current characters (`current`), clipboard ke characters (`clipboard`), aur target (`n`) ke basis par minimum steps return kare.
   - Do operations hain:
     - **Copy All**: Agar `clipboard` me current characters nahi hain, to copy karenge aur clipboard ko update karenge.
     - **Paste**: Agar clipboard me kuch characters hain, to unhe paste karenge aur screen ke characters badhenge.
   - Har operation me ek step count hota hai, aur dono operations ka minimum result return karte hain.

2. **Base Cases**:
   - Agar `current == n`, iska matlab target achieve ho gaya, to 0 return karo.
   - Agar `current > n`, iska matlab invalid state hai, to `INT_MAX / 2` return karo (taaki yeh result consider na ho).

3. **Memoization**:
   - DP array (`dp[current][clipboard]`) use karte hain taaki har state ko ek hi baar calculate karein aur redundant calculations avoid ho.

4. **Initial Call**:
   - `minSteps` function se hum recursion start karte hain `current = 1` aur `clipboard = 0` ke saath.

---

#### Time Complexity:
- **States**: `O(n^2)` (screen ke har `current` value ke liye `clipboard` ka ek state hoga).
- **Operations per State**: `O(1)` (do operations: Copy All aur Paste).
- **Total Complexity**: **O(n^2)**.

#### Space Complexity:
- **DP Array**: `O(n^2)` (2D array `dp` use hoti hai).
- **Recursive Stack**: `O(n)` (maximum recursion depth).
- **Total Complexity**: **O(n^2)**.

---

#### Dry Run:
**Input**: `n = 3`  
**Initial State**: `current = 1`, `clipboard = 0`  

---

1. **First Call**:  
   - `solveMem(1, 0, 3, dp)`:
     - Copy All: `1 + solveMem(1, 1, 3, dp)`
     - Paste: Not possible (`clipboard == 0`).

---

2. **Second Call**:
   - `solveMem(1, 1, 3, dp)`:
     - Copy All: Not possible (`clipboard == current`).
     - Paste: `1 + solveMem(2, 1, 3, dp)`.

---

3. **Third Call**:
   - `solveMem(2, 1, 3, dp)`:
     - Copy All: `1 + solveMem(2, 2, 3, dp)`.
     - Paste: `1 + solveMem(3, 1, 3, dp)`.

---

4. **Fourth Call**:
   - `solveMem(3, 1, 3, dp)`:
     - Target achieved (`current == n`), so return `0`.

---

5. **Backtracking**:
   - `solveMem(2, 1, 3, dp)`:
     - Paste operation result = `1 + 0 = 1`.
     - Minimum steps = `1`.

---

6. **Back to Second Call**:
   - `solveMem(1, 1, 3, dp)`:
     - Paste operation result = `1 + 1 = 2`.
     - Minimum steps = `2`.

---

7. **Back to First Call**:
   - `solveMem(1, 0, 3, dp)`:
     - Copy All operation result = `1 + 2 = 3`.
     - Minimum steps = `3`.

**Output**: `3` steps.  
Operations:
1. Copy All (`A` in clipboard).
2. Paste (`AA` on screen).
3. Paste (`AAA` on screen).
