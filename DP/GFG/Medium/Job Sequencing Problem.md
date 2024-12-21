### **Code:**

```cpp
class Solution {
  public:
    vector<int> JobSequencing(vector<int> &id, vector<int> &deadline,
                              vector<int> &profit) {
        int n = id.size();
        vector<pair<int, int>> jobs; // {profit, deadline}
        
        // Step 1: Creating a jobs array containing {profit, deadline} pairs
        for (int i = 0; i < n; ++i) {
            jobs.push_back({profit[i], deadline[i]});
        }
        
        // Step 2: Sorting jobs by profit in descending order
        sort(jobs.rbegin(), jobs.rend());

        int maxDeadline = 0;
        // Step 3: Find the maximum deadline
        for (int i = 0; i < n; ++i) {
            maxDeadline = max(maxDeadline, jobs[i].second);
        }

        // Step 4: Create a slot array to track available time slots
        vector<int> slots(maxDeadline + 1, -1);
        int maxi = 0, jobCount = 0;

        // Step 5: Assign jobs to available slots
        for (int i = 0; i < n; ++i) {
            int profit = jobs[i].first;
            int deadline = jobs[i].second;

            // Find a slot for this job (from deadline to 1)
            for (int j = deadline; j > 0; --j) {
                if (slots[j] == -1) {
                    // Assign the job to this slot
                    slots[j] = i;
                    maxi += profit;
                    ++jobCount;
                    break;
                }
            }
        }

        // Return the maximum number of jobs and total profit
        return {jobCount, maxi};
    }
};
```

---

### **Explanation:**

1. **Input:**
   - `id[]`: Job IDs (not directly used in the calculation).
   - `deadline[]`: Each job's deadline.
   - `profit[]`: Profit for each job.
   
2. **Job Array Creation:**
   - Sabse pehle, hum ek `jobs` vector bana rahe hain jo har job ka **profit** aur **deadline** store karega.
   - Humne yeh `{profit[i], deadline[i]}` pairs me store kiya hai.
   
   ```cpp
   jobs.push_back({profit[i], deadline[i]});
   ```
   - Ab `jobs[]` me har job ka profit aur deadline pair ki tarah hoga.

3. **Sorting the Jobs:**
   - Jobs ko **descending order** mein **profit** ke basis pe sort kar rahe hain:
   
   ```cpp
   sort(jobs.rbegin(), jobs.rend());
   ```
   - Matlab, sabse zyada profit wali jobs sabse pehle aayengi.

4. **Find Maximum Deadline:**
   - Ab hum find karte hain ki **maximum deadline** kya hai. Yeh hume yeh batata hai ki **kitne slots** ki zaroorat hogi.
   
   ```cpp
   maxDeadline = max(maxDeadline, jobs[i].second);
   ```

5. **Slot Array Creation:**
   - **Slots array** banate hain jo track karega ki kaunsa time slot available hai ya filled hai. Size hoga `maxDeadline + 1` (1-based index ke liye):
   
   ```cpp
   vector<int> slots(maxDeadline + 1, -1);
   ```
   - Agar slot available hai, toh `-1` rahega, agar filled hai toh index par job ka ID store ho jayega.

6. **Job Assignment to Slots:**
   - Ab hum har job ke liye available slot dhoondhne ki koshish karte hain, **deadline se start karke** (pehle time slot ko check karte hain aur phir move karte hain).
   
   ```cpp
   for (int j = deadline; j > 0; --j) {
       if (slots[j] == -1) {
           slots[j] = i; // Job assign to slot
           maxi += profit; // Add profit
           ++jobCount; // Increment job count
           break; // No need to check further slots
       }
   }
   ```

7. **Return Result:**
   - Final result me hum return karte hain:
     - `jobCount`: Kitni jobs complete hui.
     - `maxProfit`: Total profit jo un jobs se mila.
   
   ```cpp
   return {jobCount, maxi};
   ```

---

### **Time Complexity:**

1. **Sorting jobs** takes `O(n log n)` where `n` is the number of jobs.
2. **Assigning jobs to slots** involves iterating over each job, and for each job, we check up to `maxDeadline` slots.
   - In the worst case, checking slots for all jobs takes `O(n * maxDeadline)` time.

   **Total Time Complexity** = `O(n log n + n * maxDeadline)`.

---

### **Space Complexity:**

1. **Slots Array**: We use a `slots[]` array of size `maxDeadline + 1`, so the space complexity is `O(maxDeadline)`.
2. **Jobs Array**: The `jobs[]` array has `n` jobs, so `O(n)` space.

   **Total Space Complexity** = `O(maxDeadline + n)`.

---

### **Dry Run Example:**

**Input:**
```cpp
id = [1, 2, 3, 4]
deadline = [4, 1, 1, 1]
profit = [20, 1, 40, 30]
```

**Steps:**

1. **Create jobs array**:
   ```cpp
   jobs = [(20, 4), (1, 1), (40, 1), (30, 1)]
   ```

2. **Sort jobs by profit in descending order**:
   ```cpp
   jobs = [(40, 1), (30, 1), (20, 4), (1, 1)]
   ```

3. **Find max deadline**:
   - `maxDeadline = 4` (from job `(20, 4)`).

4. **Create slots array**:
   ```cpp
   slots = [-1, -1, -1, -1, -1]
   ```

5. **Assign jobs to slots**:
   - **Job 1 (profit 40, deadline 1)**:
     - Slot 1 is available → Assign to slot 1.
     - `slots = [-1, 0, -1, -1, -1]`
     - `maxProfit = 40`, `jobCount = 1`

   - **Job 2 (profit 30, deadline 1)**:
     - Slot 1 is filled → No available slot.
     - Skip this job.

   - **Job 3 (profit 20, deadline 4)**:
     - Slot 4 is available → Assign to slot 4.
     - `slots = [-1, 0, -1, -1, 2]`
     - `maxProfit = 60`, `jobCount = 2`

   - **Job 4 (profit 1, deadline 1)**:
     - Slot 1 is filled → No available slot.
     - Skip this job.

6. **Return**:
   ```cpp
   return {jobCount = 2, maxProfit = 60};
   ```

**Output:**
```cpp
{2, 60}
```

---

### **Conclusion:**

- Humne jobs ko descending profit order mein sort kiya aur fir available slots mein assign kiya taki **maximum profit** mile.
- **Time complexity** ko optimize karte hue humne sorting aur slot assignment ki strategy apnayi hai.
- Ye approach greedy hai, jisme hum hamesha highest profit wali jobs ko prioritize karte hain.
