### Code:
```cpp
class Solution {
  public:
    vector <int> bottomView(Node *root) {
		vector<int> ans;
		if(root == NULL) return ans;
		map<int,int>mp; // col, val;
		queue<pair<Node*, int>>Q; // node, horizontal distance values
		Q.push({root, 0});
		
		while(!Q.empty()){
			auto temp=Q.front();
			Q.pop();
			Node* node=temp.first;
			int col=temp.second;
			// even if we got a node at a particular col, we will overrwrite it, 
			// becuase we want bottom view
            mp[col]=node->data;
			if(node->left) Q.push({node->left, col-1});
			if(node->right) Q.push({node->right, col+1});
		}
		
		for(auto x:mp){
			ans.push_back(x.second);
		}
		return ans;
    }
};
```
### Explanation:

The `bottomView` function is designed to return the bottom view of a binary tree. The bottom view of a binary tree is the set of nodes visible when the tree is viewed from the bottom. For each horizontal distance from the root, the bottom-most node encountered is part of the bottom view.

1. **Initialization**:
   - A vector `ans` is used to store the result.
   - A map `mp` is used to store the node's data at each horizontal distance. The key is the horizontal distance (`col`), and the value is the node's data.
   - A queue `Q` is used for level-order traversal. It stores pairs of nodes and their corresponding horizontal distances.

2. **Traversal**:
   - The root node is pushed into the queue with a horizontal distance of `0`.
   - The function enters a while-loop that continues until the queue is empty.
   - For each node, its data is stored in the map, overwriting any previous value at that horizontal distance. This ensures that the last (bottom-most) node encountered at each horizontal distance is stored in the map.

3. **Output**:
   - The map is iterated in order of horizontal distances (from left to right), and the stored values are pushed into the result vector `ans`.

### Time Complexity Analysis:

- **Insertion and Lookup in Map**: Both operations in a map are O(log N). Here, N refers to the number of nodes in the tree.
- **Traversal**: Each node is processed once, so the time complexity for traversal is O(N).
- **Overall Time Complexity**: Since each node is processed once, and insertion and lookup in the map are O(log N), the overall time complexity is O(N log N).

### Space Complexity Analysis:

- **Map**: The map stores at most `N` entries, one for each horizontal distance. Hence, space complexity for the map is O(N).
- **Queue**: The queue can hold at most `N` nodes, leading to a space complexity of O(N).
- **Overall Space Complexity**: O(N), due to the map and queue.

### Dry Run:

Let's perform a dry run on a simple example:

```
       1
     /   \
    2     3
   / \   / \
  4   5 6   7
```

- **Initialization**:
  - `ans = []`
  - `mp = {}`
  - `Q = [(1, 0)]` (root node with horizontal distance 0)

1. **First Iteration**:
   - `temp = (1, 0)`, so `node = 1` and `col = 0`.
   - `mp = {0: 1}` (insert 1 at horizontal distance 0).
   - Push left child `(2, -1)` and right child `(3, 1)` into the queue.
   - `Q = [(2, -1), (3, 1)]`.

2. **Second Iteration**:
   - `temp = (2, -1)`, so `node = 2` and `col = -1`.
   - `mp = {0: 1, -1: 2}` (insert 2 at horizontal distance -1).
   - Push left child `(4, -2)` and right child `(5, 0)` into the queue.
   - `Q = [(3, 1), (4, -2), (5, 0)]`.

3. **Third Iteration**:
   - `temp = (3, 1)`, so `node = 3` and `col = 1`.
   - `mp = {0: 1, -1: 2, 1: 3}` (insert 3 at horizontal distance 1).
   - Push left child `(6, 0)` and right child `(7, 2)` into the queue.
   - `Q = [(4, -2), (5, 0), (6, 0), (7, 2)]`.

4. **Fourth Iteration**:
   - `temp = (4, -2)`, so `node = 4` and `col = -2`.
   - `mp = {0: 1, -1: 2, 1: 3, -2: 4}` (insert 4 at horizontal distance -2).
   - Node 4 has no children, so no further nodes are added to the queue.
   - `Q = [(5, 0), (6, 0), (7, 2)]`.

5. **Fifth Iteration**:
   - `temp = (5, 0)`, so `node = 5` and `col = 0`.
   - `mp = {0: 5, -1: 2, 1: 3, -2: 4}` (overwrite 1 with 5 at horizontal distance 0).
   - Node 5 has no children, so no further nodes are added to the queue.
   - `Q = [(6, 0), (7, 2)]`.

6. **Sixth Iteration**:
   - `temp = (6, 0)`, so `node = 6` and `col = 0`.
   - `mp = {0: 6, -1: 2, 1: 3, -2: 4}` (overwrite 5 with 6 at horizontal distance 0).
   - Node 6 has no children, so no further nodes are added to the queue.
   - `Q = [(7, 2)]`.

7. **Seventh Iteration**:
   - `temp = (7, 2)`, so `node = 7` and `col = 2`.
   - `mp = {0: 6, -1: 2, 1: 3, -2: 4, 2: 7}` (insert 7 at horizontal distance 2).
   - Node 7 has no children, so no further nodes are added to the queue.
   - `Q = []`.

8. **Queue is Empty**: Exit the loop.

9. **Construct Result**:
   - Iterate over `mp` and populate `ans`:
     - For `col = -2`, value is 4.
     - For `col = -1`, value is 2.
     - For `col = 0`, value is 6.
     - For `col = 1`, value is 3.
     - For `col = 2`, value is 7.
   - `ans = [4, 2, 6, 3, 7]`.

### Conclusion:

- The `bottomView` function correctly computes the bottom view of the binary tree by maintaining the last node seen at each horizontal distance.
- The key difference from the `topView` function is that the `bottomView` function allows overwriting the values in the map for each horizontal distance, ensuring that the last (bottom-most) node at each horizontal distance is recorded. This ensures that nodes that are lower in the tree overwrite those above them at the same horizontal distance, making the bottom view visible.
