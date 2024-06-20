## Method 1 (Pick/ Not pick)
[Reference Video](https://www.youtube.com/watch?v=OyZFFqQtu98&list=PLgUwDviBIf0p4ozDR_kJJkONnb1wdx2Ma&index=50)
```bash
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> result;
        vector<int> combination;
        sort(candidates.begin(), candidates.end()); // Sort for avoiding duplicates
        backtrack(result, combination, candidates, target, 0);
        return result;
    }

private:
    void backtrack(vector<vector<int>>& result, vector<int>& combination, vector<int>& candidates, int target, int start) {
        if (target < 0) {
            return; // If target becomes negative, we have overshot, so return
        } 
        else if (target == 0) {
            result.push_back(combination); // If target becomes 0, we have found a valid combination
        } 
        else {
            for (int i = start; i < candidates.size(); i++) {
                combination.push_back(candidates[i]); // Add the current candidate to the combination
                backtrack(result, combination, candidates, target - candidates[i], i); // Recurse with the remaining target
                combination.pop_back(); // Backtrack by removing the last added candidate
            }
        }
    }
};
```
