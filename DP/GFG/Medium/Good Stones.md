### Code

```cpp
class Solution{
public:

    bool solveRec(int index, vector<int> &arr, vector<int> &visited) {
        // Base cases
        if (index<0 || index>=arr.size()) {
            return true; // Out of bounds means safe
        }
        if (visited[index]==1) {
            return false; // Already visited, means a loop
        }
        if (visited[index]==2) {
            return true; // Already processed as good stone
        }

        // Mark current stone as visited
        visited[index]=1;

        // Recursive call for the next stone
        int nextIndex=index+arr[index];
        bool result=solveRec(nextIndex, arr, visited);

        // Mark current stone as processed
        if (result) visited[index]=2;
        else visited[index]=1;
        
        return result;
    }
    
    
    bool solveMem(int index, vector<int> &arr, vector<int> &dp) {
        // Base cases
        if (index < 0 || index >= arr.size()) {
            return true; // Out of bounds means safe
        }
        if (dp[index] != -1) {
            if (dp[index] == 1) {
                return true;
            } 
            else {
                return false;
            }
        }

        // Mark current stone as visiting
        dp[index] = 0;

        // Recursive call for the next stone
        int nextIndex = index + arr[index];
        bool result = solveMem(nextIndex, arr, dp);

        // Memoize the result
        if (result) {
            dp[index] = 1;
        } 
        else {
            dp[index] = 0;
        }

        return result;
    }

    int goodStones(int n, vector<int> &arr){
        vector<int> dp(n, -1); // -1: not visited, 0: bad stone, 1: good stone
        int count = 0;

        for (int i = 0; i < n; i++) {
            if (solveMem(i, arr, dp)) {
                count++;
            }
        }

        return count;
    }  
};
```

---

### Explanation (E)
Yeh problem recursion aur memoization ke mix ka use karti hai. Hum har stone ko traverse karte hain aur yeh check karte hain ki woh stone safe hai ya loop mein phasega. Do tarike hain:
1. **Recursion (solveRec):** Har stone ko depth-first search ki tarah check karo aur yeh ensure karo ki stone safe hai ya nahi.
2. **Memoization (solveMem):** Result ko store karte hain jisse hum ek hi stone ko dobara calculate na karein.

**Steps:**
- Agar koi stone index ke baahar chala jaye toh woh safe hai (good stone).
- Agar ek stone pe pahle visit kiya ho aur woh cycle mein ho, toh woh bad stone hai.
- Memoization ka use karke hum redundant calculations avoid karte hain aur time complexity optimize karte hain.

---

### Time Complexity (T)
1. Har stone ko ek baar visit karte hain. Is wajah se overall complexity \(O(N)\) hai.
2. Recursive aur memoization approach dono efficient hain.

**Final Time Complexity:** \(O(N)\)

---

### Space Complexity (S)
1. Recursion ke liye stack space lagta hai, jo maximum \(O(N)\) ho sakta hai.
2. Memoization ke liye ek DP array lagti hai, jo bhi \(O(N)\) space use karta hai.

**Final Space Complexity:** \(O(N)\)

---

### Dry Run (D)
**Input:** `n = 7, arr = [2, 3, -1, 2, -2, 4, 1]`

**Process:**
1. Stone 0: 
   - Jump to index 2. 
   - Stone 2 ke through ek loop detect hoti hai, toh Stone 0 bad stone.
2. Stone 3: 
   - Out of bounds chala jata hai, safe hai (good stone).
3. Stone 5: 
   - Directly out of bounds, good stone.
4. Stone 6: 
   - Directly out of bounds, good stone.

**Output:** `3` (Good stones: Index 3, 5, 6)
