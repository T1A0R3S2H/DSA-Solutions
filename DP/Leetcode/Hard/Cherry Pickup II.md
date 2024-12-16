### **Code**  
```cpp
class Solution {
public:
    int solveMem(vector<vector<int>>& grid, int row, int c1, int c2, vector<vector<vector<int>>>& dp) {
        int cols = grid[0].size();
        int rows = grid.size();
        
        // Out of bounds
        if (c1 < 0 || c1 >= cols || c2 < 0 || c2 >= cols) {
            return INT_MIN;
        }
        
        // Base case: last row
        if (row == rows - 1) {
            if (c1 == c2) {
                return grid[row][c1]; // Same cell
            } else {
                return grid[row][c1] + grid[row][c2];
            }
        }
        
        // Memoization
        if (dp[row][c1][c2] != -1) return dp[row][c1][c2];
        
        // Current cherries
        int cherries = 0;
        if (c1 == c2) {
            cherries = grid[row][c1]; // Both robots on the same cell
        } else {
            cherries = grid[row][c1] + grid[row][c2]; // Different cells
        }
        
        // Explore all possible moves
        int maxCherries = INT_MIN;
        for (int dc1 = -1; dc1 <= 1; ++dc1) { // Robot 1's moves
            for (int dc2 = -1; dc2 <= 1; ++dc2) { // Robot 2's moves
                int nextCherries = solveMem(grid, row + 1, c1 + dc1, c2 + dc2, dp);
                if (nextCherries > maxCherries) {
                    maxCherries = nextCherries;
                }
            }
        }
        
        // Add current cherries to the result of the best move
        dp[row][c1][c2] = cherries + maxCherries;
        return dp[row][c1][c2];
    }
    
    int cherryPickup(vector<vector<int>>& grid) {
        int rows = grid.size();
        int cols = grid[0].size();
        
        // Memoization table
        vector<vector<vector<int>>> dp(rows, vector<vector<int>>(cols, vector<int>(cols, -1)));
        
        // Start the recursion
        return solveMem(grid, 0, 0, cols - 1, dp);
    }
};
```

---

### **Explanation**  

#### Problem Ka Overview:
- Do robots hain: Robot 1 `(c1 = 0)` aur Robot 2 `(c2 = cols-1)`.  
- Dono robots top row se bottom row tak chalenge aur har cell se cherries utha lenge.  
- Dono robots ka movement 3 options hai:
  - Left diagonal: `(i+1, j-1)`  
  - Down: `(i+1, j)`  
  - Right diagonal: `(i+1, j+1)`  
- Agar dono same cell par aate hain, to ek hi cherry utha payega.

##### 1. **Last Row Me Same Cell Check**  
Last row par alag se same cell check karne ki zarurat isliye hoti hai kyunki uske baad koi aur move possible nahi hai. Agar dono robots same cell par hain, to sirf ek robot cherry utha payega. Isliye hum directly `grid[row][c1]` return karte hain. Agar dono alag cells par hain, to hum dono cells ki cherries ka sum return karte hain (`grid[row][c1] + grid[row][c2]`). Yeh condition recursion ke andar nahi aati kyunki last row ke baad koi recursion nahi chalti.

---

##### 2. **`row+1` To Hamesha Ho Rahi Hai**  
Recursion hamesha `row+1` kar raha hai kyunki hum grid ko row-wise traverse kar rahe hain, top to bottom. `dc1` aur `dc2` ka kaam sirf columns ko adjust karna hai:  
- `dc1 = -1` → Left diagonal (`i+1, j-1`).  
- `dc1 = 0` → Down (`i+1, j`).  
- `dc1 = 1` → Right diagonal (`i+1, j+1`).  
Yeh movement robot ke current column (`c1` ya `c2`) ke basis par decide hoti hai, aur row automatic next row (i.e., `row+1`) hoti hai.
---

#### Recursive Approach Ka Flow:
1. **Base Case**:
   - Agar robot out-of-bounds ja raha hai (`c1 < 0`, `c1 >= cols`, `c2 < 0`, `c2 >= cols`), return `INT_MIN`.  
   - Agar `row == rows-1` hai (last row par), to agar dono same cell par hain, return `grid[row][c1]`. Warna, dono cells ki cherries ka sum return karo.  

2. **Recursive Case**:
   - Har cell ka cherries collect karo (`grid[row][c1]` + `grid[row][c2]`). Agar same cell hai, ek cherry hi count hoti hai.  
   - Next row par jao aur har robot ke liye 3-3 moves explore karo (total 9 combinations).  

3. **Memoization**:
   - Agar `dp[row][c1][c2] != -1` hai, iska matlab ye state pehle solve ho chuki hai. Direct result return kar do.  
   - Current cherries + next best move ka result `dp[row][c1][c2]` me store kar lo.  

4. **Start Point**:
   - Recursion `(row=0, c1=0, c2=cols-1)` se start hoti hai.  

---

### **Time Complexity**  
- Har cell ke liye 9 moves explore karte hain.  
- Total states = `rows * cols * cols`.  
- TC: **O(rows * cols^2 * 9) = O(rows * cols^2)**.  

---

### **Space Complexity**  
- Memoization table size = `rows * cols * cols`.  
- SC: **O(rows * cols^2)**.  

---

### **Dry Run**

#### Input:  
`grid = [[3, 1, 1], [2, 5, 1], [1, 5, 5], [2, 1, 1]]`  

#### Memoization Table (`dp`):  
Initially, all values = `-1`.

#### Flow:  

1. **Start at `(row=0, c1=0, c2=2)`**:
   - Cherries = `grid[0][0] + grid[0][2] = 3 + 1 = 4`.
   - Explore all 9 combinations for `c1` and `c2` movements.

2. **Recursive Call `(row=1, c1=0, c2=1)`**:
   - Cherries = `grid[1][0] + grid[1][1] = 2 + 5 = 7`.  
   - Explore all moves for next row.

3. **Recursive Call `(row=2, c1=0, c2=1)`**:
   - Cherries = `grid[2][0] + grid[2][1] = 1 + 5 = 6`.  
   - Explore next moves.

4. **Recursive Call `(row=3, c1=0, c2=1)`**:
   - Cherries = `grid[3][0] + grid[3][1] = 2 + 1 = 3`.  
   - Base case reached, return `3`.

---

#### Result Calculation:
- **Bottom-Up Memoization**:
  - `(row=3)` → `(row=2)` → `(row=1)` → `(row=0)`.  
  - Final answer = Maximum cherries collected by both robots.  

#### Final Output:
`24`
