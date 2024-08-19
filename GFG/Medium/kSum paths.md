# Code:
```cpp
class Solution {
public:
    void solve(Node *root, int k, int &count, vector<int> &path) {
        // base case
        if (root == NULL) return;

        path.push_back(root->data);

        // left path
        solve(root->left, k, count, path);
        // right path
        solve(root->right, k, count, path);

        // check ksum
        int n = path.size();
        int sum = 0;
        for (int i = n - 1; i >= 0; i--) {
            sum += path[i];
            if (sum == k) count++;
        }

        // backtrack: remove the current node from the path
        path.pop_back();
    }

    int sumK(Node *root, int k) {
        vector<int> path;
        int count = 0;
        solve(root, k, count, path);
        return count;
    }
};
```
### Explanation
The given code counts the number of paths in a binary tree that sum to a given value `k`. The approach uses Depth-First Search (DFS) and backtracking.

1. **Function `solve(Node *root, int k, int &count, vector<int> &path)`:**
   - **Base Case:** If the current node (`root`) is `NULL`, return immediately.
   - **Add Current Node to Path:** Append the current node’s value to the `path`.
   - **Recur for Left and Right Subtrees:** Call `solve` recursively for the left and right children of the current node.
   - **Check All Paths Ending at Current Node:** After exploring both children, check all sub-paths ending at the current node to see if their sum equals `k`. If so, increment the `count`.
   - **Backtracking:** Remove the current node from the `path` to backtrack and explore other paths correctly.

2. **Function `int sumK(Node *root, int k)`:**
   - **Initialize Path and Count:** Create an empty `path` vector and initialize `count` to `0`.
   - **Call Solve:** Invoke `solve` with the root node, target sum `k`, `count`, and `path`.
   - **Return Result:** Return the total count of valid paths.

### Time Complexity (TC)
- Each node is visited once, and for each node, a path sum calculation is performed, which takes O(n) time in the worst case (where n is the depth of the tree). Thus, the overall time complexity is O(n^2) in the worst case.

### Space Complexity (SC)
- The space complexity is O(n) due to the `path` vector storing the nodes in the current path. Additionally, the recursion stack can go as deep as the height of the tree, which is also O(n) in the worst case.

### Dry Run
Let's do a dry run with a simple example.

**Input Tree:**
```
      1
     / \
    2   3
```
**Target Sum (k):** 3

**Expected Output:** 2 (Paths: [1, 2] and [3])

**Step-by-Step Execution:**

1. **Initial Call:** `sumK(root, 3)`
   - `path = []`
   - `count = 0`

2. **First Call to `solve`:** `solve(root, 3, count, path)`
   - `root = 1`
   - `path = [1]`

3. **Recur Left:**
   - `solve(root->left, 3, count, path)`
   - `root = 2`
   - `path = [1, 2]`

4. **Base Case Check for Left of 2:** `solve(NULL, 3, count, path)` (returns immediately)
5. **Base Case Check for Right of 2:** `solve(NULL, 3, count, path)` (returns immediately)

6. **Check Paths Ending at 2:**
   - `path = [1, 2]`
   - Sum from rightmost end:
     - 2: sum = 2
     - 1 + 2: sum = 3 (count incremented to 1)

7. **Backtrack:** `path = [1]`

8. **Recur Right:**
   - `solve(root->right, 3, count, path)`
   - `root = 3`
   - `path = [1, 3]`

9. **Base Case Check for Left of 3:** `solve(NULL, 3, count, path)` (returns immediately)
10. **Base Case Check for Right of 3:** `solve(NULL, 3, count, path)` (returns immediately)

11. **Check Paths Ending at 3:**
    - `path = [1, 3]`
    - Sum from rightmost end:
      - 3: sum = 3 (count incremented to 2)
      - 1 + 3: sum = 4

12. **Backtrack:** `path = [1]`
13. **Check Paths Ending at 1:**
    - `path = [1]`
    - Sum from rightmost end:
      - 1: sum = 1

14. **Final Count:** `count = 2`

15. **Return Result:** `sumK` returns `2`.

The dry run confirms the correct path sums and backtracking. The expected output matches the obtained result.
