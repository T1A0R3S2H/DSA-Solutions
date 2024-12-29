### C++ Code:

```cpp
class Solution {
public:
    int n; // Grid ka size
    vector<vector<vector<int>>> dp; // Memoization table

    // Recursive function to calculate length in ek direction
    int solveMem(int i, int j, int dir, vector<vector<int>>& grid) {
        // Base case: Agar cell out of bounds ho ya 0 ho
        if (i < 0 || i >= n || j < 0 || j >= n || grid[i][j] == 0) {
            return 0;
        }

        // Agar value pehle se compute hai toh directly return karte hain
        if (dp[i][j][dir] != -1) {
            return dp[i][j][dir];
        }

        // Har direction ke liye length calculate karo
        if (dir == 0) { // Up direction
            dp[i][j][dir] = 1 + solveMem(i - 1, j, dir, grid);
        } else if (dir == 1) { // Down direction
            dp[i][j][dir] = 1 + solveMem(i + 1, j, dir, grid);
        } else if (dir == 2) { // Left direction
            dp[i][j][dir] = 1 + solveMem(i, j - 1, dir, grid);
        } else { // Right direction
            dp[i][j][dir] = 1 + solveMem(i, j + 1, dir, grid);
        }

        return dp[i][j][dir];
    }

    int orderOfLargestPlusSign(int size, vector<vector<int>>& mines) {
        n = size;

        // Pehle grid ko 1 se initialize karte hain aur mines ko 0 set karte hain
        vector<vector<int>> grid(n, vector<int>(n, 1));
        for (auto& mine : mines) {
            grid[mine[0]][mine[1]] = 0;
        }

        // Memoization table ko initialize karo
        dp = vector<vector<vector<int>>>(n, vector<vector<int>>(n, vector<int>(4, -1)));

        int maxOrder = 0; // Plus sign ka maximum order track karte hain
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    // 4 directions ka minimum stretch calculate karo
                    int up = solveMem(i, j, 0, grid);
                    int down = solveMem(i, j, 1, grid);
                    int left = solveMem(i, j, 2, grid);
                    int right = solveMem(i, j, 3, grid);

                    // Plus sign ka order determine karo
                    int order = min({up, down, left, right});
                    maxOrder = max(maxOrder, order);
                }
            }
        }

        return maxOrder;
    }
};
```

---

### Explanation:
1. **Input Parsing**:
   - Ek grid diya gaya hai jo \(n \times n\) ka hai, jisme initially sab cells 1 hain.
   - Mines array se un cells ko 0 set karte hain jo mine hain.

2. **Recursion + Memoization**:
   - Har cell ke liye 4 directions ka maximum stretch calculate karte hain:
     - Up, Down, Left, Right.
   - Agar calculation pehle hi ho chuki hai, toh directly DP table se value return karte hain.

3. **Order Calculation**:
   - Har cell ke liye 4 directions ka minimum value nikalte hain.
   - Plus sign ka order woh minimum value hota hai.
   - Maximum order track karte hain across sab cells.

4. **Output**:
   - Maximum order of the plus sign return karte hain.

---

### Time Complexity:
- **Grid Traversal**: \(O(n^2)\).
- **Recursive Calculation**: \(O(1)\) per cell per direction due to memoization.
- Total: \(O(n^2)\).

### Space Complexity:
- **DP Table**: \(O(n^2)\) for storing results for 4 directions.
- Total: \(O(n^2)\).

---

### Dry Run:
#### Input:
```plaintext
n = 5
mines = [[4, 2]]
```

#### Grid:
```plaintext
1 1 1 1 1
1 1 1 1 1
1 1 1 1 1
1 1 1 1 1
1 1 0 1 1
```

#### Step-by-Step:
1. Cell \((2, 2)\):
   - Up = 3, Down = 2, Left = 2, Right = 2 → Min = 2.
2. Update maxOrder = 2.

#### Final Output:
```plaintext
2
```

---

Yeh solution recursion aur memoization ka efficient use karke problem ko optimize karta hai aur kisi bhi unnecessary computation ko avoid karta hai.
