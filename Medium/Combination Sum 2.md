## Method 1
The `combinationSum2` function solves the problem of finding all unique combinations of candidate numbers that sum up to a given target. It starts by sorting the `candidates` vector to group duplicates together. Then, it calls the `backtrack` helper function with an empty `combination` vector, the `result` vector (to store valid combinations), the sorted `candidates` vector, the target value, and the starting index `0`. 

The `backtrack` function uses recursion to explore all possible combinations. It first checks if the remaining target is negative (invalid combination) or zero (valid combination, add to `result`). If the target is positive, it iterates through the remaining candidates, skipping duplicates. For each unique candidate, it adds it to the current `combination`, recursively calls `backtrack` with the updated `combination`, remaining target, and next starting index, and then backtracks by removing the last element from `combination`. This process continues until all valid combinations are found. The sorting of candidates and skipping of duplicates during backtracking ensures that only unique combinations are generated. The time complexity is O(2^n * k), where n is the number of candidates and k is the maximum combination length, and the space complexity is O(k * x), where x is the number of valid combinations.

```bash
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<vector<int>> result;
        vector<int> combination;
        sort(candidates.begin(), candidates.end()); // Sort the candidates to avoid duplicates
        backtrack(result, combination, candidates, target, 0);
        return result;
    }

private:
    void backtrack(vector<vector<int>>& result, vector<int>& combination, vector<int>& candidates, int target, int start) {
        if (target < 0) {
            return; // If the remaining target becomes negative, return
        } 
        
        else if (target == 0) {
            result.push_back(combination);
            return;
        }

        for (int i = start; i < candidates.size(); i++) {
            if (i > start && candidates[i] == candidates[i - 1]) {
                continue; // Skip duplicates
            }
            combination.push_back(candidates[i]);
            backtrack(result, combination, candidates, target - candidates[i], i + 1); // recurse with remaining
            combination.pop_back(); // Backtrack
        }
    }
};
```
1. We define the combinationSum2 function that takes a vector of candidates and a target integer as input.
We create an empty vector result to store all the valid combinations.
We create an empty vector combination to store the current combination being built.
We sort the candidates vector in ascending order. This step is crucial for avoiding duplicates later in the backtracking process.
We call the backtrack helper function, passing the result, combination, candidates, target, and the starting index 0.
Finally, we return the result vector containing all valid combinations.

2. The function takes five arguments:

`result`: A reference to the vector where all valid combinations will be stored.
`combination`: A reference to the current combination being built.
`candidates`: A reference to the vector of candidate numbers.
`target`: The remaining target value to be achieved.
`start`: The starting index for the current recursion level.
