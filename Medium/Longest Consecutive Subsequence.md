## Method 1
# Done in O(n^2) although was asked to do in O(n)
## Step 1: Convert Input Array to Hash Set
Hash Set Creation: The input array nums is converted into an unordered_set, named ```numSet```. An unordered_set is a container that stores unique elements in no particular order. It allows for fast insertion, deletion, and lookup operations, which are crucial for optimizing the solution.
## Step 2: Initialize Variables
```maxStreak```: This variable keeps track of the maximum length of a consecutive sequence found so far. It is initialized to 0.
## Step 3: Iterate Through Each Number in the Set
The code iterates through each number in ```numSet``` using a range-based for loop. This is possible because ```numSet``` is iterable like an array.
## Step 4: Check for Consecutive Sequences Starting from Each Number
Consecutive Check: For each number, it checks if the number minus one (num - 1) exists in the set. This is done using the ```count()``` function of the unordered_set, which returns true if the element exists and false otherwise. If the number minus one does not exist, it means this number could be the start of a new consecutive sequence.
***The line if (!numSet.count(num - 1)) checks if the number that is one less than the current number (num - 1) exists in the numSet. If it does not exist, meaning there is no consecutive number immediately preceding the current number, then the loop begins.***
##
**Existence Check**: The numSet.count(num - 1) expression checks if the number num - 1 is present in the numSet. The count() function returns the number of occurrences of num - 1 in the set. If num - 1 is not in the set, count() returns 0, and the negation operator `` turns this into true.
##
**Starting a New Sequence**: If the condition evaluates to true, it indicates that the current number (num) cannot be extended to the left (i.e., there is no num - 1 in the set). This suggests that num could be the start of a new consecutive sequence. Thus, the algorithm proceeds to extend the sequence to the right by incrementing num and checking for the existence of num + 1 in the set.
##
**Loop Execution**: The loop continues to add to the streak (currentStreak) as long as num + 1 exists in the set, indicating a valid consecutive sequence. Once a number is found that does not have a successor in the set, the loop stops, and the algorithm moves on to the next number in the set.
##
## Step 5: Extend Streak Until Non-Consecutive Number Is Found
Streak Extension: If the condition in step 4 is met, it enters a while loop where it increments the current number ```(currentNum)``` and extends the streak ```(currentStreak)``` by 1 for each consecutive number found in the set. This loop continues as long as the incremented number plus one ```(currentNum + 1)``` exists in the set, indicating a valid consecutive sequence.
## Step 6: Update Maximum Streak
After extending the streak for a potential consecutive sequence, it compares the current streak length ```(currentStreak)``` with the maximum streak found so far ```(maxStreak)```. If the current streak is longer, ```maxStreak``` is updated to reflect the new maximum.
## Step 7: Return the Maximum Streak
Finally, after iterating through all numbers in the set, the function returns the value of ```maxStreak```, which represents the length of the longest consecutive elements sequence found in the input array.
Efficiency and Time Complexity
##
### This approach efficiently handles the problem by leveraging the properties of unordered_set for fast lookups and a single pass through the input data, achieving the desired O(n) time complexity. The space complexity is also optimized since it uses a hash set to store unique numbers from the input array, avoiding duplication and unnecessary storage.
### This method is particularly effective for solving problems involving sequences or patterns in arrays, where identifying unique elements and their relationships is key to finding the solution efficiently.
##

```bash
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> numSet(nums.begin(), nums.end());
        int maxStreak = 0;
        
        for (int num : numSet) {
            if (!numSet.count(num - 1)) {
                //left me nahi mila to right me dhundo
                int currentNum = num;
                int currentStreak = 1;
                
                while (numSet.count(currentNum + 1)) { 
                    currentNum += 1;
                    currentStreak += 1;
                }
                
                maxStreak = max(maxStreak, currentStreak);
            }
        }
        
        return maxStreak;
    }
};

```


## Method 2 (O(n))
```bash
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if (nums.empty()) return 0;
        
        sort(nums.begin(), nums.end());
        
        int longestStreak = 1;
        int currentStreak = 1;
        
        for (int i = 1; i < nums.size(); ++i) {
            if (nums[i] != nums[i - 1]) { // Skip duplicates
                if (nums[i] == nums[i - 1] + 1) {
                    currentStreak++;
                } else {
                    longestStreak = max(longestStreak, currentStreak);
                    currentStreak = 1;
                }
            }
        }
        
        return max(longestStreak, currentStreak);
    }
};
```
