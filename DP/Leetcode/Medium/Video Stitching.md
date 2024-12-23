### Problem:
Humein kuch video clips diye gaye hain jo ek sporting event ko represent karte hain. Har clip ka ek start aur end time hota hai. Humara goal hai ki minimum clips use kar ke poore time range `[0, time]` ko cover karen. Agar possible nahi hai, toh `-1` return karen.

---
# Method 1 (DP)
### Code  
```cpp
class Solution {
public:
    unordered_map<int, int> dp;
    
    int solveMem(int current_end, int time, vector<vector<int>>& clips) {
        // Base case: agar time poora cover ho gaya
        if (current_end >= time) {
            return 0;
        }
        
        // Agar result already memoized hai
        if (dp.count(current_end)) {
            return dp[current_end];
        }
        
        int ans = INT_MAX - 1;
        
        // Har clip ko check karte hain jo current_end ko extend kar sakti hai
        for (auto& clip : clips) {
            if (clip[0] <= current_end) { // Clip ka start current_end ke pehle hai
                if (clip[1] > current_end) { // Clip ka end current_end ke aage hai
                    int next = solveMem(clip[1], time, clips);
                    
                    // Agar valid solution mila toh minimum clips update karo
                    if (next != INT_MAX - 1) {
                        ans = min(ans, 1 + next);
                    }
                }
            } else if (clip[0] > current_end) { // Clips ko sorted hone ke karan, aage wale sab useless hain
                break;
            }
        }
        
        // Result memoize karke return karo
        return dp[current_end] = ans;
    }

    int videoStitching(vector<vector<int>>& clips, int time) {
        // Clips ko unke start time ke basis pe sort karo
        sort(clips.begin(), clips.end());
        
        // Memoization map ko clear karo
        dp.clear();
        
        // Solve shuru karo 0 se
        int result = solveMem(0, time, clips);
        
        // Agar result valid hai toh return karo, nahi toh -1
        if (result >= INT_MAX - 1) {
            return -1;
        } 
        else {
            return result;
        }
    }
};
```

---

### Explanation  
- **Problem**: Hume `time` seconds takka poora sporting event cover karna hai, aur minimum clips find karni hain jo yeh kaam karein.
- **Approach**:
  1. **Sorting**: Clips ko start time ke basis pe sort karte hain.
  2. **Recursive + Memoization Solution**:
     - `solveMem(current_end, time, clips)`: Yeh function check karta hai ki kaun si clips ko use karke `current_end` takka coverage `time` tak extend karna hai.
     - Har clip jo current coverage ko extend kar sakti hai, usko consider karte hain.
  3. **Memoization**: Already computed states ko memoize karte hain taaki redundant calculations avoid ho.
  4. **Base Case**: Agar `current_end >= time` ho gaya, iska matlab poora coverage mil chuki hai, toh return `0`.

---

### Time Complexity  
1. **Sorting**: \(O(n \log n)\), jahan \(n\) = total clips.
2. **Recursion**: Har state \(O(n)\) clips ko check karti hai. Total \(O(time)\) states.  
   Total complexity: \(O(n \cdot time)\).

---

### Space Complexity  
1. **Memoization Map**: \(O(time)\).
2. **Recursion Stack**: \(O(time)\).  
   Total space: \(O(time)\).

---

### Dry Run  
**Input**:  
`clips = [[0,2],[4,6],[8,10],[1,9],[1,5],[5,9]], time = 10`  

**Steps**:  
1. Clips ko sort karte hain: `[[0,2],[1,5],[1,9],[4,6],[5,9],[8,10]]`.
2. \(current\_end = 0\):
   - Clip `[0,2]` ko lete hain. Call `solveMem(2, 10, clips)`.
3. \(current\_end = 2\):
   - Clip `[1,9]` ko lete hain. Call `solveMem(9, 10, clips)`.
4. \(current\_end = 9\):
   - Clip `[8,10]` ko lete hain. Call `solveMem(10, 10, clips)`.
5. \(current\_end = 10\): Base case, return `0`.  
**Result**: Minimum clips = `3` (`[0,2], [1,9], [8,10]`).

---

### Additional Points  
- **Why `dp.count` Used**: `dp.count(current_end)` ka use isliye kiya gaya hai kyunki `unordered_map` ka default value `0` hota hai. Isse explicit initialization ki zarurat nahi padti.  
- **INT_MAX - 1**: Overflow avoid karne ke liye.

---



# Method 2 (without DP)
### Code:

```cpp
class Solution {
public:
    int videoStitching(vector<vector<int>>& clips, int time) {
        // Step 1: Clips ko start time ke hisaab se sort karte hain
        sort(clips.begin(), clips.end());

        int count = 0, current_end = 0, last_end = 0;
        int i = 0;

        while (current_end < time) {
            // Step 2: Wo clip dhoondhna jo current range ko sabse zyada extend kare
            while (i < clips.size() && clips[i][0] <= current_end) {
                last_end = max(last_end, clips[i][1]);
                i++;
            }

            // Step 3: Agar koi clip nahi milti jo coverage ko extend kare, return -1
            if (last_end <= current_end) {
                return -1;
            }

            // Step 4: Coverage ko extend karna, aur count badhana
            current_end = last_end;
            count++;
        }

        return count;
    }
};
```

### Explanation (ETSD):

**Explanation**:
- **Step 1 (Sorting)**: Sabse pehle hum clips ko unke `start` time ke hisaab se sort karte hain. Isse humein pata chal jata hai ki kaun sa clip hum pehle use karenge. Agar clips ko sort na kiya, toh hum properly valid clips nahi select kar paate.
  
- **Step 2 (Greedy Selection)**: Ab hum har step pe wo clip select karte hain jo `current_end` ko sabse zyada extend kare. Matlab agar abhi tak humne `current_end` tak coverage paa li hai, toh hum wo clip choose karenge jiska `end` sabse zyada ho.
  
- **Step 3 (Checking for Unreachable Coverage)**: Agar hum aise clip dhoondte hain jo `current_end` ko extend na kare, toh iska matlab hai ki poora range cover karna impossible hai, aur hum `-1` return karte hain.

- **Step 4 (Update Coverage)**: Jab hum ek clip select kar lete hain, hum `current_end` ko us clip ke `end` se update karte hain aur clip count ko badhate hain. Yeh process tab tak chalti hai jab tak hum poora time range `[0, time]` cover nahi kar lete.

### Time Complexity:
- **Sorting**: `O(n log n)` jahan `n` clips ki length hai.
- **Main Loop**: Clip ko ek baar iterate karte hain, isliye time complexity `O(n)` hai.
  
Total time complexity: **O(n log n)** because sorting dominates the time complexity.

### Space Complexity:
- **Space Complexity**: `O(1)` kyunki hum sirf kuch extra variables (`count`, `current_end`, `last_end`, `i`) use kar rahe hain, aur koi additional space nahi use kar rahe hain.

### Dry Run (Dry Run Example):

**Input**: `clips = [[0,2],[4,6],[8,10],[1,9],[1,5],[5,9]]`, `time = 10`

1. **Sorting the clips**:
   After sorting: `clips = [[0, 2], [1, 5], [1, 9], [4, 6], [5, 9], [8, 10]]`

2. **Step 1**: Start with `current_end = 0`, `last_end = 0`, and `count = 0`.

3. **Step 2**: Hum `current_end = 0` pe hain. Ab humein wo clip chahiye jo `0` tak ya usse pehle start hota ho. Bas clip `[0, 2]` valid hai, toh hum use karte hain. Ab `current_end = 2`, aur `count = 1`.

4. **Step 3**: Ab `current_end = 2` ho gaya. Hum dekhte hain wo clips jo `2` tak ya usse pehle start hote hain. Valid clips hain `[1, 5]` aur `[1, 9]`.
   - Inmein se clip `[1, 9]` ka `end = 9` sabse zyada hai, toh hum usse select karte hain.
   - Ab `current_end = 9`, aur `count = 2`.

5. **Step 4**: Ab `current_end = 9` ho gaya. Hum dekhte hain wo clips jo `9` tak ya usse pehle start hote hain. Valid clips hain `[8, 10]` aur `[5, 9]`.
   - Inmein se clip `[8, 10]` ka `end = 10` hai, jo range ko poora cover karne ke liye sahi hai. Hum use karte hain.
   - Ab `current_end = 10`, aur `count = 3`.

6. **Result**: Humne range `[0, 10]` ko 3 clips se cover kar liya: `[0, 2]`, `[1, 9]`, aur `[8, 10]`.

**Output**: `3`

---

### Summary:

- **Approach**: Hum greedy approach use karte hain, jisme hum hamesha wo clip choose karte hain jo current coverage ko sabse zyada extend kare.
- **Sorting** se clips ko ek order mein arrange karte hain taaki valid clip select karna asaan ho jaye.
- **Time Complexity**: `O(n log n)` due to sorting aur loop.
- **Space Complexity**: `O(1)` kyunki hum sirf kuch variables use karte hain.

Agar kuch aur confusion ho toh batao, main help karne ke liye hoon! 😊
