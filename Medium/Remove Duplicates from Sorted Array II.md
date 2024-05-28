## Method 1 (Hash map)
Explanation:

- Base Case: If the array has 2 or fewer elements, no modifications are needed (it already meets the "at most twice" condition).

- Hash Map (ginti): This map stores the frequency of each unique element encountered.

- Index i: This is used to track the position where we'll place the next valid element (i.e., elements with frequency 1 or 2).

- Iteration:

1. Loop through the nums array.
2. For each num:
3. Increment its frequency in the ginti map.
4. If the frequency is less than or equal to 2, it's a valid element:
5. Place it at nums[i] (overwriting any previous invalid elements).
6. Increment i.
7. Return Value: After the loop, the value of i represents the length of the modified array with duplicates removed according to the rules.

Key Differences from Your Code:

In-Place Modification: Instead of just counting frequencies, this code directly modifies the nums array in-place.
Single Pass: It does this in a single pass through the array, making it efficient.
Frequency Check: The if (ginti[num] <= 2) condition ensures we only include elements with valid frequencies.



```bash
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n=nums.size();
        if (n<=2) { 
            return n;
        }

        unordered_map<int, int> ginti;
        int i=0;
        for (int num:nums) {
            ginti[num]++; //count the freq

            if (ginti[num] <= 2) { //atmost freq should be 2 of a particular element
                nums[i]=num;
                i++; 
            }
        }
        
        return i;
    }
};

```
