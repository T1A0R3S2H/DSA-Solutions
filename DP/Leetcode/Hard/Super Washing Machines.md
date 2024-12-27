### Code:
```cpp
class Solution {
public:
    int findMinMoves(vector<int>& machines) {
        int totalDresses = accumulate(machines.begin(), machines.end(), 0);
        int n = machines.size();

        // Base case: If total dresses cannot be equally distributed
        if (totalDresses % n != 0) return -1;

        int target = totalDresses / n; // Each machine should have `target` dresses
        int currImbalance = 0, maxMoves = 0;

        for (int dresses : machines) {
            // Calculate current imbalance
            currImbalance += dresses - target;
            // Maximum moves required is the maximum of:
            // 1. Absolute imbalance at this point (to balance flow)
            // 2. Excess/deficit at this machine (direct moves needed here)
            maxMoves = max(maxMoves, max(abs(currImbalance), dresses - target));
        }

        return maxMoves;
    }
};
```

### Explanation:
1. **Base Case**: 
   - Pehle hum total dresses ko calculate karte hain. Agar total dresses machines ke beech equally distribute nahi kiya jaa sakta, matlab agar `totalDresses % n != 0` hai, toh return `-1` because evenly distribution possible nahi hai.
   
2. **Target Calculation**: 
   - Agar total dresses evenly distribute ho sakte hain, toh har machine ko `target` dresses chahiye, jo ki `totalDresses / n` hote hain.

3. **Imbalance Calculation**: 
   - `currImbalance` ek running total hai jo har machine ka excess ya deficit track karta hai. Agar koi machine target se zyada dresses rakhti hai, toh woh dresses next machine ko dena padta hai, aur agar kam dresses hain, toh woh receive karegi.
   
4. **Max Moves Calculation**: 
   - Har iteration mein hum calculate karte hain maximum moves jo required ho sakte hain:
     - **Imbalance flow**: `abs(currImbalance)` ko track karte hain, jo current imbalance ka absolute value hai.
     - **Excess/Deficit**: `dresses - target` ka value, jo har machine ke liye excess ya deficit represent karta hai.
   - Hum `max` ko use karte hain taaki jo bhi zyada required ho, woh move properly consider ho sake.

5. **Result**: 
   - Final mein, `maxMoves` return karte hain jo minimum number of moves hain jo dresses ko balance karne ke liye required hain.

### Time Complexity:
- **O(n)**: Hum ek hi baar array ko traverse karte hain, har machine ke dresses ko evaluate karte hain, jo ki O(n) time complexity ke andar hota hai.

### Space Complexity:
- **O(1)**: Hum sirf kuch variables use kar rahe hain (`totalDresses`, `target`, `currImbalance`, `maxMoves`), toh space complexity constant hai.

### Dry Run:
**Example 1: machines = [1, 0, 5]**

1. **Total Dresses** = 6, **Target** = 6 / 3 = 2.
2. **First Machine**:
   - `currImbalance = 1 - 2 = -1`
   - `maxMoves = max(0, abs(-1), 1 - 2) = 1`
3. **Second Machine**:
   - `currImbalance = -1 + (0 - 2) = -3`
   - `maxMoves = max(1, abs(-3), 0 - 2) = 3`
4. **Third Machine**:
   - `currImbalance = -3 + (5 - 2) = 0`
   - `maxMoves = max(3, abs(0), 5 - 2) = 3`
   
Result: **3**

**Example 2: machines = [0, 3, 0]**

1. **Total Dresses** = 3, **Target** = 3 / 3 = 1.
2. **First Machine**:
   - `currImbalance = 0 - 1 = -1`
   - `maxMoves = max(0, abs(-1), 0 - 1) = 1`
3. **Second Machine**:
   - `currImbalance = -1 + (3 - 1) = 1`
   - `maxMoves = max(1, abs(1), 3 - 1) = 2`
4. **Third Machine**:
   - `currImbalance = 1 + (0 - 1) = 0`
   - `maxMoves = max(2, abs(0), 0 - 1) = 2`
   
Result: **2**

**Example 3: machines = [0, 2, 0]**

1. **Total Dresses** = 2, **Target** = 2 / 3 = 0 (not divisible, so return -1).

Result: **-1**

### Conclusion:
- Yeh solution efficiently calculate karta hai minimum moves ko `max` ka use karke, jisme imbalance aur excess/deficit dono ko handle kiya jaata hai. Maximum imbalance ko consider karke, solution sabse zyada required move ko track karta hai jo balance karne mein madad karta hai.
