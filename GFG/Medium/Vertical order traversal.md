![Screenshot 2024-07-24 085128](https://github.com/user-attachments/assets/d1309801-2752-44bb-9702-6d7ee35d63ff)
### Code:
```cpp
class Solution {
public:
    // Function to find the vertical order traversal of Binary Tree.
    vector<int> verticalOrder(Node *root) {
        // Using a map to store nodes based on their horizontal distance and level.
        map<int, map<int, vector<int>>> nodes;
        
        // Queue to perform level order traversal with each element containing
        // the node, its horizontal distance (hd), and its level.
        queue<pair<Node*, pair<int, int>>> q;
        
        vector<int> ans; // Vector to store the final vertical order traversal.
        
        if (root == NULL)
            return ans; // If root is NULL, return an empty vector.
            
        // Pushing the root node into the queue with hd = 0 and level = 0.
        q.push(make_pair(root, make_pair(0, 0)));
        
        // Performing level order traversal using the queue.
        while (!q.empty()) {
            // Extracting the front element from the queue.
            pair<Node*, pair<int, int>> temp = q.front();
            q.pop();
            
            Node* frontNode = temp.first; // Extracting the node.
            int hd = temp.second.first; // Extracting the horizontal distance.
            int lvl = temp.second.second; // Extracting the level.
            
            // Storing the node's data in the map at the appropriate hd and lvl.
            nodes[hd][lvl].push_back(frontNode->data);
            
            // Pushing left child into the queue with updated hd and level.
            if (frontNode->left)
                q.push(make_pair(frontNode->left, make_pair(hd - 1, lvl + 1)));
                
            // Pushing right child into the queue with updated hd and level.
            if (frontNode->right)
                q.push(make_pair(frontNode->right, make_pair(hd + 1, lvl + 1)));
        }
        
        // Extracting nodes from the map in vertical order.
        for (auto i : nodes) { // Iterate over horizontal distances.
            for (auto j : i.second) { // Iterate over levels.
                for (auto k : j.second) { // Iterate over nodes at each level.
                    ans.push_back(k); // Push node data into the result vector.
                }
            }
        }
        
        return ans; // Returning the final vertical order traversal vector.
    }
};
```

### Explanation:

1. **Data Structures Used**:
   - `map<int, map<int, vector<int>>> nodes`: This is a nested map where:
     - The outer map (`map<int, map<int, vector<int>>>`) uses `int` as keys representing horizontal distances (`hd`).
     - The inner map (`map<int, vector<int>>`) uses `int` as keys representing levels (`lvl`).
     - `vector<int>` stores node values at each horizontal distance and level.
   - `queue<pair<Node*, pair<int, int>>> q`: This queue stores pairs:
     - `Node*` represents the node itself.
     - `pair<int, int>` represents horizontal distance (`hd`) and level (`lvl`) of the node.

2. **Algorithm**:
   - The algorithm performs a level order traversal (BFS) starting from the root node.
   - For each node processed from the queue (`q`):
     - Its data (`frontNode->data`) is stored in `nodes` at the respective `hd` and `lvl`.
     - Left and right children of the node are pushed into the queue with updated `hd` and `lvl`.
   
3. **Final Result**:
   - After processing all nodes, the nested loops (`for` loops) iterate through `nodes` to collect node values in vertical order.
   - Values are pushed into the `ans` vector which is returned as the final result.

4. **Edge Cases**:
   - The function checks if `root` is `NULL` and returns an empty vector `ans` if true, handling the case of an empty tree.

### Example Usage:

- Construct a binary tree (not shown in the provided code).
- Create an instance of `Solution`.
- Call `verticalOrder` method with the root of the tree to get the vertical order traversal.

This approach ensures that nodes are stored and retrieved in vertical order based on their horizontal distance and level using maps and a queue for efficient traversal and storage.


Let's perform a dry run of the given C++ code with the provided binary tree example:

**Input Binary Tree:**
```
           1
         /   \
       2       3
     /   \   /   \
   4      5 6      7
              \      \
               8      9
```

**Steps:**

1. **Initialization:**
   - Start with an empty `ans` vector to store the final vertical order traversal.
   - `q` is a queue to facilitate level-order traversal.
   - `nodes` is a nested map to store nodes based on their horizontal distance (`hd`) and level.

2. **Processing the Root:**
   - Start with the root node `1`:
     - Push `1` with `hd = 0` and `level = 0` into the queue.
     - `nodes[0][0].push_back(1)` adds `1` to `nodes` at `hd = 0` and `level = 0`.

3. **Level Order Traversal:**
   - Dequeue `1` from the queue:
     - Process its left child `2`:
       - Push `2` with `hd = -1` and `level = 1` into the queue.
       - `nodes[-1][1].push_back(2)` adds `2` to `nodes` at `hd = -1` and `level = 1`.
     - Process its right child `3`:
       - Push `3` with `hd = 1` and `level = 1` into the queue.
       - `nodes[1][1].push_back(3)` adds `3` to `nodes` at `hd = 1` and `level = 1`.

4. **Continue Traversal:**
   - Dequeue `2` from the queue:
     - Process its left child `4`:
       - Push `4` with `hd = -2` and `level = 2` into the queue.
       - `nodes[-2][2].push_back(4)` adds `4` to `nodes` at `hd = -2` and `level = 2`.
     - Process its right child `5`:
       - Push `5` with `hd = 0` and `level = 2` into the queue.
       - `nodes[0][2].push_back(5)` adds `5` to `nodes` at `hd = 0` and `level = 2`.

   - Continue similarly for other nodes (`3`, `4`, `5`, `6`, `7`, `8`, `9`) until the queue is empty.

5. **Building the Final Output:**
   - After all nodes are processed:
     - Iterate through `nodes` to collect values in vertical order.
     - Flatten and collect values from `nodes` into `ans` vector.

6. **Final Output:**
   - Print the values in `ans`, which should be `4 2 1 5 6 3 8 7 9`.

**Dry Run Conclusion:**
The vertical order traversal output is `4 2 1 5 6 3 8 7 9`, which matches the expected output for the given binary tree example. This demonstrates that the C++ code effectively performs vertical order traversal using a combination of maps and queues.
