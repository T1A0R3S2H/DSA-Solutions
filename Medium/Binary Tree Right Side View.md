```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> res;
        if(root == NULL) return res;
        dfs(root, 0, res);
        return res;
    }
    
    void dfs(TreeNode* node, int level, vector<int>& res) {
        if(node == NULL) return;
        if(res.size() == level) res.push_back(node->val);
        dfs(node->right, level+1, res);
        dfs(node->left, level+1, res);
    }
};
```
### Explanation

The goal of the `rightSideView` function is to return the values of the nodes you can see when looking at the binary tree from the right side.

#### rightSideView Function

1. **Initialization**: 
    - We initialize an empty vector `res` to store the result.
    - We check if the `root` is NULL. If it is, we return the empty vector `res`.

2. **DFS Call**:
    - If the `root` is not NULL, we initiate a Depth-First Search (DFS) by calling the `dfs` function with the root node, level 0, and the result vector `res`.

#### dfs Function

The `dfs` function is a recursive function that performs the DFS traversal of the tree:

1. **Base Case**:
    - If the current node (`node`) is NULL, we return immediately, as there is nothing to process.

2. **Processing the Current Node**:
    - We check if the size of the result vector `res` is equal to the current level. This condition means that we are visiting this level for the first time.
    - If the condition is true, we add the current node's value to the result vector `res`.

3. **Recursive Calls**:
    - We first recursively call the `dfs` function on the right child of the current node with the level incremented by 1.
    - After that, we recursively call the `dfs` function on the left child of the current node with the level incremented by 1.

By prioritizing the right child over the left child during the DFS, we ensure that the rightmost node at each level is added to the result vector `res`.

### Time Complexity

The time complexity of this solution is \( O(N) \), where \( N \) is the number of nodes in the tree. This is because we visit each node exactly once during the DFS traversal.

### Space Complexity

The space complexity of this solution is \( O(N) \). This accounts for:
1. The space used by the result vector `res`, which in the worst case can store \( O(N) \) node values (one for each level of a skewed tree).
2. The space used by the call stack during the DFS traversal. In the worst case, the depth of the recursion tree can be \( O(N) \) (again, in the case of a skewed tree).

### Dry Run

Let's perform a dry run on the following binary tree:

```
       1
     /   \
    2     3
     \   / \
      5 4   6
         \
          7
```

1. **Initial Call**: `rightSideView(root)`
    - `root` is not NULL, so we initialize `res` as an empty vector and call `dfs(root, 0, res)`.

2. **First Call**: `dfs(node: 1, level: 0, res: [])`
    - `node` is not NULL.
    - `res.size()` (0) is equal to `level` (0), so we add `1` to `res`. Now `res` is `[1]`.
    - We call `dfs(3, 1, res)`.

3. **Second Call**: `dfs(node: 3, level: 1, res: [1])`
    - `node` is not NULL.
    - `res.size()` (1) is equal to `level` (1), so we add `3` to `res`. Now `res` is `[1, 3]`.
    - We call `dfs(6, 2, res)`.

4. **Third Call**: `dfs(node: 6, level: 2, res: [1, 3])`
    - `node` is not NULL.
    - `res.size()` (2) is equal to `level` (2), so we add `6` to `res`. Now `res` is `[1, 3, 6]`.
    - We call `dfs(NULL, 3, res)`.

5. **Fourth Call**: `dfs(node: NULL, level: 3, res: [1, 3, 6])`
    - `node` is NULL, so we return immediately.

6. **Back to Third Call**:
    - We call `dfs(NULL, 3, res)`.

7. **Fifth Call**: `dfs(node: NULL, level: 3, res: [1, 3, 6])`
    - `node` is NULL, so we return immediately.

8. **Back to Second Call**:
    - We call `dfs(4, 2, res)`.

9. **Sixth Call**: `dfs(node: 4, level: 2, res: [1, 3, 6])`
    - `node` is not NULL.
    - `res.size()` (3) is not equal to `level` (2), so we do not add `4` to `res`.
    - We call `dfs(7, 3, res)`.

10. **Seventh Call**: `dfs(node: 7, level: 3, res: [1, 3, 6])`
    - `node` is not NULL.
    - `res.size()` (3) is equal to `level` (3), so we add `7` to `res`. Now `res` is `[1, 3, 6, 7]`.
    - We call `dfs(NULL, 4, res)`.

11. **Eighth Call**: `dfs(node: NULL, level: 4, res: [1, 3, 6, 7])`
    - `node` is NULL, so we return immediately.

12. **Back to Seventh Call**:
    - We call `dfs(NULL, 4, res)`.

13. **Ninth Call**: `dfs(node: NULL, level: 4, res: [1, 3, 6, 7])`
    - `node` is NULL, so we return immediately.

14. **Back to Sixth Call**:
    - We call `dfs(NULL, 3, res)`.

15. **Tenth Call**: `dfs(node: NULL, level: 3, res: [1, 3, 6, 7])`
    - `node` is NULL, so we return immediately.

16. **Back to First Call**:
    - We call `dfs(2, 1, res)`.

17. **Eleventh Call**: `dfs(node: 2, level: 1, res: [1, 3, 6, 7])`
    - `node` is not NULL.
    - `res.size()` (4) is not equal to `level` (1), so we do not add `2` to `res`.
    - We call `dfs(NULL, 2, res)`.

18. **Twelfth Call**: `dfs(node: NULL, level: 2, res: [1, 3, 6, 7])`
    - `node` is NULL, so we return immediately.

19. **Back to Eleventh Call**:
    - We call `dfs(5, 2, res)`.

20. **Thirteenth Call**: `dfs(node: 5, level: 2, res: [1, 3, 6, 7])`
    - `node` is not NULL.
    - `res.size()` (4) is not equal to `level` (2), so we do not add `5` to `res`.
    - We call `dfs(NULL, 3, res)`.

21. **Fourteenth Call**: `dfs(node: NULL, level: 3, res: [1, 3, 6, 7])`
    - `node` is NULL, so we return immediately.

22. **Back to Thirteenth Call**:
    - We call `dfs(NULL, 3, res)`.

23. **Fifteenth Call**: `dfs(node: NULL, level: 3, res: [1, 3, 6, 7])`
    - `node` is NULL, so we return immediately.

The final result vector `res` is `[1, 3, 6, 7]`, which represents the values of the nodes visible from the right side of the tree.
