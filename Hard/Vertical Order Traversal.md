```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
        vector<vector<int>> verticalTraversal(TreeNode* root) {
        // Use a map to store nodes by their horizontal distance and level
        map<int, map<int, multiset<int>>> nodes;
        // Use a queue to perform level order traversal
        queue<pair<TreeNode*, pair<int, int>>> q;
        vector<vector<int>> ans;
        
        if (root == nullptr) return ans; // Return empty vector if root is null
        
        // Start with the root node at horizontal distance 0 and level 0
        q.push(make_pair(root, make_pair(0, 0)));
        
        while (!q.empty()) {
            pair<TreeNode*, pair<int, int>> temp = q.front();
            q.pop();
            TreeNode* front = temp.first;
            int hd = temp.second.first;
            int lvl = temp.second.second;
            
            // Store the node's value in the map using a multiset to keep it sorted
            nodes[hd][lvl].insert(front->val);
            
            // Push left and right children with updated horizontal distance and level
            if (front->left) {
                q.push(make_pair(front->left, make_pair(hd - 1, lvl + 1)));
            }
            if (front->right) {
                q.push(make_pair(front->right, make_pair(hd + 1, lvl + 1)));
            }
        }
        
        // Traverse the map and collect the results
        for (auto i : nodes) {
            vector<int> column;
            for (auto j : i.second) {
                column.insert(column.end(), j.second.begin(), j.second.end());
            }
            ans.push_back(column);
        }
        
        return ans;
    }
};
```


### Explanation

**Time Complexity (TC):**

- **Traversal and Insertion:** The BFS traversal takes \(O(N)\) time, where \(N\) is the number of nodes in the tree. For each node, inserting its value into the `multiset` takes \(O(\log k)\) time on average, where \(k\) is the number of elements at that horizontal distance and level.
- **Total Time Complexity:** The overall time complexity is \(O(N \log k)\). Since \(k\) can be approximated to be less than \(N\), the time complexity can be considered \(O(N \log N)\) in the worst case.

**Space Complexity (SC):**

- **Map and Multiset Storage:** Storing nodes in the `map<int, map<int, multiset<int>>>` structure takes \(O(N)\) space.
- **Queue Storage:** The queue used for BFS traversal also takes \(O(N)\) space in the worst case.
- **Total Space Complexity:** The overall space complexity is \(O(N)\).

### Dry Run

**Input:**
```cpp
root = [1, 2, 3, 4, 5, 6, 7]
```

**Tree Structure:**
```
       1
      / \
     2   3
    / \ / \
   4  5 6  7
```

**Step-by-Step Dry Run:**

1. **Initialization:**
   - `nodes`: `{}` (empty map)
   - `q`: `[(1, (0, 0))]` (queue initialized with the root node)

2. **First Iteration:**
   - `temp`: `(1, (0, 0))`
   - `front`: `1`
   - `hd`: `0`, `lvl`: `0`
   - Insert `1` into `nodes[0][0]`
   - `nodes`: `{0: {0: {1}}}`
   - Add left child `(2, (-1, 1))` and right child `(3, (1, 1))` to the queue
   - `q`: `[(2, (-1, 1)), (3, (1, 1))]`

3. **Second Iteration:**
   - `temp`: `(2, (-1, 1))`
   - `front`: `2`
   - `hd`: `-1`, `lvl`: `1`
   - Insert `2` into `nodes[-1][1]`
   - `nodes`: `{-1: {1: {2}}, 0: {0: {1}}}`
   - Add left child `(4, (-2, 2))` and right child `(5, (0, 2))` to the queue
   - `q`: `[(3, (1, 1)), (4, (-2, 2)), (5, (0, 2))]`

4. **Third Iteration:**
   - `temp`: `(3, (1, 1))`
   - `front`: `3`
   - `hd`: `1`, `lvl`: `1`
   - Insert `3` into `nodes[1][1]`
   - `nodes`: `{-1: {1: {2}}, 0: {0: {1}}, 1: {1: {3}}}`
   - Add left child `(6, (0, 2))` and right child `(7, (2, 2))` to the queue
   - `q`: `[(4, (-2, 2)), (5, (0, 2)), (6, (0, 2)), (7, (2, 2))]`

5. **Fourth Iteration:**
   - `temp`: `(4, (-2, 2))`
   - `front`: `4`
   - `hd`: `-2`, `lvl`: `2`
   - Insert `4` into `nodes[-2][2]`
   - `nodes`: `{-2: {2: {4}}, -1: {1: {2}}, 0: {0: {1}}, 1: {1: {3}}}`
   - `q`: `[(5, (0, 2)), (6, (0, 2)), (7, (2, 2))]`

6. **Fifth Iteration:**
   - `temp`: `(5, (0, 2))`
   - `front`: `5`
   - `hd`: `0`, `lvl`: `2`
   - Insert `5` into `nodes[0][2]`
   - `nodes`: `{-2: {2: {4}}, -1: {1: {2}}, 0: {0: {1}, 2: {5}}, 1: {1: {3}}}`
   - `q`: `[(6, (0, 2)), (7, (2, 2))]`

7. **Sixth Iteration:**
   - `temp`: `(6, (0, 2))`
   - `front`: `6`
   - `hd`: `0`, `lvl`: `2`
   - Insert `6` into `nodes[0][2]`
   - `nodes`: `{-2: {2: {4}}, -1: {1: {2}}, 0: {0: {1}, 2: {5, 6}}, 1: {1: {3}}}`
   - `q`: `[(7, (2, 2))]`

8. **Seventh Iteration:**
   - `temp`: `(7, (2, 2))`
   - `front`: `7`
   - `hd`: `2`, `lvl`: `2`
   - Insert `7` into `nodes[2][2]`
   - `nodes`: `{-2: {2: {4}}, -1: {1: {2}}, 0: {0: {1}, 2: {5, 6}}, 1: {1: {3}}, 2: {2: {7}}}`
   - `q`: `[]` (queue is empty, end of BFS)

9. **Collecting Results:**
   - Traverse the `nodes` map to collect results:
     - Column -2: `nodes[-2][2] = {4}` -> `[4]`
     - Column -1: `nodes[-1][1] = {2}` -> `[2]`
     - Column 0: `nodes[0][0] = {1}`, `nodes[0][2] = {5, 6}` -> `[1, 5, 6]`
     - Column 1: `nodes[1][1] = {3}` -> `[3]`
     - Column 2: `nodes[2][2] = {7}` -> `[7]`

**Output:**
```cpp
[[4], [2], [1, 5, 6], [3], [7]]
```

This matches the expected output, confirming the correctness of the function.
