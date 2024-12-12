# 😪 Method 1 (But, TLE!) 
### Code

```cpp
class Solution {
public:
    int dfs(vector<int>& price, vector<vector<int>>& special, vector<int>& needs) {
        int n = price.size();
        int minCost = 0;
        
        // Direct price calculate karo bina kisi special offer ke
        for (int i = 0; i < n; i++) {
            minCost += needs[i] * price[i];
        }

        // Har special offer ko try karo
        for (const auto& offer : special) {
            vector<int> newNeeds = needs;
            bool validOffer = true;

            // Check karo agar ye offer valid hai
            for (int i = 0; i < n; i++) {
                if (newNeeds[i] < offer[i]) {
                    validOffer = false; // Agar items kam hain toh invalid offer
                    break;
                }
                newNeeds[i] -= offer[i];
            }

            // Agar offer valid hai toh cost ko update karo
            if (validOffer) {
                minCost = min(minCost, offer[n] + dfs(price, special, newNeeds));
            }
        }

        return minCost;
    }

    int shoppingOffers(vector<int>& price, vector<vector<int>>& special, vector<int>& needs) {
        return dfs(price, special, needs);
    }
};
```

---

### **ETSD**

**Explanation**:
1. **Recursive DFS**:
   - Har possibility explore karenge:
     - Ya toh special offer lagayenge aur baaki items ke liye cost calculate karenge.
     - Ya bina kisi offer ke saari items kharidenge (base case).
2. **Base Cost**: Sabhi items ko unke regular price pe kharidne ka cost calculate karenge.
3. **Valid Offer Check**: Har special offer ke liye check karenge ki items `needs` se zyada toh nahi hain.

---

**Time Complexity**:
- Worst-case: \(O((\text{max needs})^n \times \text{special.size})\), jaha \(n\) items ka count hai.
  - Recursive calls ke branching aur har offer ko check karne ki wajah se exponential complexity.

**Space Complexity**:
- \(O(n)\): Recursion stack ka depth items ke count ke barabar hoga.

---

**Dry Run**:
1. **Input**: `price = [2,5]`, `special = [[3,0,5],[1,2,10]]`, `needs = [3,2]`
2. **Steps**:
   - Direct cost bina kisi offer ke = \(3 \times 2 + 2 \times 5 = 14\).
   - Special offer `[3,0,5]` use karo: \(5 + dfs([0,2])\):
     - Direct cost `[0,2]` ke liye: \(2 \times 5 = 10\).
   - Special offer `[1,2,10]` use karo: \(10 + dfs([2,0])\):
     - Direct cost `[2,0]` ke liye: \(2 \times 2 = 4\).
   - Minimum = 14 (special `[1,2,10]` ek baar aur baaki items direct).

**Output**: **14**



---

# 🔥 Method 2 (Recursion + Memoisation)
### Explanation
This code solves the problem of finding the minimum cost to satisfy a list of `needs` given the individual item prices and special offers using a memoized recursive approach. 

#### Key Components:
1. **`solveMem` Function**:
   - Recursive function to compute the minimum cost to fulfill the `needs`.
   - Uses memoization to avoid recomputation for the same `needs` state.

2. **Direct Cost Calculation**:
   - Calculates the cost of fulfilling the `needs` without using any special offers.

3. **Special Offers Validation**:
   - Iterates through each offer, checks if it can be applied to the current `needs`.
   - If valid, updates `newNeeds` and recursively calculates the cost.

4. **Memoization**:
   - Results are stored in a hash map (`dp`) with the current `needs` vector as the key to avoid redundant calculations.

5. **Custom Hash Function**:
   - A custom hash function is used for `unordered_map` to handle `vector<int>` as a key efficiently.

---

### Time Complexity
- **Recursive Calls**:  
  For each unique state of `needs`, the function is called once. If `n` is the number of items and each item's quantity can range from `0` to `q`, the total number of unique states is approximately \( q^n \).
  
- **Per Function Call**:
  - Direct cost computation: \( O(n) \).
  - Iterating through `special`: \( O(s \times n) \), where \( s \) is the number of special offers.
  
- **Overall Complexity**:
  \( O(q^n \times s \times n) \).

---

### Space Complexity
- **Memoization Table**:  
  Stores at most \( q^n \) states of `needs`, where each state has size \( n \).  
  \( O(q^n \times n) \).

- **Call Stack**:  
  Depth of recursion is \( n \), so stack space is \( O(n) \).

- **Total**:  
  \( O(q^n \times n + n) \), dominated by memoization.

---

### Dry Run
#### Input:
- `price = [2, 3]`  
- `special = [[1, 1, 4], [2, 2, 7]]`  
- `needs = [3, 2]`

#### Execution:
1. Initial call: `solveMem(price, special, needs = [3, 2], dp = {})`
   - **Direct cost**: \( 3 \times 2 + 2 \times 3 = 12 \).  
   - Initialize `minCost = 12`.

2. **First Special Offer `[1, 1, 4]`:**
   - Valid. Update `newNeeds = [2, 1]`.
   - Recursive call: `solveMem(price, special, needs = [2, 1], dp = {})`.

3. **Second Special Offer `[2, 2, 7]`:**
   - Valid. Update `newNeeds = [1, 0]`.
   - Recursive call: `solveMem(price, special, needs = [1, 0], dp = {})`.

4. Base case reached:
   - Compute cost for `needs = [1, 0]`: Direct cost = 2.  
   - Memoize and backtrack.

5. Backtrack through recursive calls, computing `minCost` at each step:
   - `needs = [2, 1]`: Cost = 7 (offer `[2, 2, 7]`) + memoized cost for `[1, 0] = 2` (but this offer is invalid).
   - `needs = [3, 2]`: Cost = 11.

#### Result:
The minimum cost is **11**.
