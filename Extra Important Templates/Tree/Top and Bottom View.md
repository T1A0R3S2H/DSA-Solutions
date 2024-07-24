## Explanation of Top View and Bottom View Codes

### Top View Code
The top view of a binary tree is the set of nodes visible when the tree is viewed from the top. Here's the detailed explanation of how the top view code works:

```cpp
vector<int> topView(Node *root) {
    vector<int> ans;
    if (root == NULL) return ans; // If the tree is empty, return an empty vector

    map<int, int> mp; // Map to store the first node's value at each horizontal distance (hd)
    queue<pair<Node*, int>> Q; // Queue to store nodes along with their horizontal distance
    Q.push({root, 0}); // Start with the root node at horizontal distance 0

    while (!Q.empty()) {
        auto temp = Q.front(); // Get the front node in the queue
        Q.pop();
        Node* node = temp.first;
        int hd = temp.second;

        // If no node is recorded at this horizontal distance, record the current node's value
        if (mp.find(hd) == mp.end()) {
            mp[hd] = node->data;
        }

        // Add the left child to the queue with hd - 1
        if (node->left) Q.push({node->left, hd - 1});

        // Add the right child to the queue with hd + 1
        if (node->right) Q.push({node->right, hd + 1});
    }

    // Extract the values from the map and add them to the answer vector
    for (auto x : mp) {
        ans.push_back(x.second);
    }

    return ans;
}
```

### Bottom View Code
The bottom view of a binary tree is the set of nodes visible when the tree is viewed from the bottom. Here's the detailed explanation of how the bottom view code works:

```cpp
vector<int> bottomView(Node *root) {
    vector<int> ans;
    if (root == NULL) return ans; // If the tree is empty, return an empty vector

    map<int, int> mp; // Map to store the last node's value at each horizontal distance (hd)
    queue<pair<Node*, int>> Q; // Queue to store nodes along with their horizontal distance
    Q.push({root, 0}); // Start with the root node at horizontal distance 0

    while (!Q.empty()) {
        auto temp = Q.front(); // Get the front node in the queue
        Q.pop();
        Node* node = temp.first;
        int hd = temp.second;

        // Record the current node's value (will overwrite previous values at the same hd)
        mp[hd] = node->data;

        // Add the left child to the queue with hd - 1
        if (node->left) Q.push({node->left, hd - 1});

        // Add the right child to the queue with hd + 1
        if (node->right) Q.push({node->right, hd + 1});
    }

    // Extract the values from the map and add them to the answer vector
    for (auto x : mp) {
        ans.push_back(x.second);
    }

    return ans;
}
```

### Key Differences

1. **Condition for Updating Map**:
    - **Top View**: Updates the map only if the horizontal distance (`hd`) is not already in the map. This ensures only the first encountered node at each `hd` is recorded.
    - **Bottom View**: Updates the map unconditionally, ensuring the last encountered node at each `hd` is recorded.

2. **Common Logic**:
    - Both use a queue to perform level order traversal.
    - Both use a map to store nodes based on their horizontal distances.
    - The final result is derived from the map, sorted by horizontal distances.

### Time Complexity
- **Time Complexity**: Both solutions have a time complexity of \(O(n \log n)\), where \(n\) is the number of nodes in the binary tree. This is because:
  - The level order traversal visits all nodes once, which is \(O(n)\).
  - Inserting into the map and extracting the elements from the map both take \(O(\log n)\) operations, resulting in \(O(n \log n)\) overall.

### Space Complexity
- **Space Complexity**: Both solutions have a space complexity of \(O(n)\), which includes:
  - The queue storing up to \(O(n)\) nodes in the worst case.
  - The map storing up to \(O(n)\) horizontal distances.

# DRY RUN
Let's perform a dry run of both the top view and bottom view algorithms using the same example binary tree.

### Example Binary Tree
```
          1
         / \
        2   3
       / \   \
      4   5   6
           \
            7
```

### Top View Dry Run
**Initial State:**
- `root = 1`
- `mp = {}`
- `Q = [(1, 0)]` (node, horizontal distance)

**Steps:**

1. **First iteration:**
   - `temp = (1, 0)`
   - `node = 1`, `hd = 0`
   - `mp = {0: 1}`
   - `Q = [(2, -1), (3, 1)]`

2. **Second iteration:**
   - `temp = (2, -1)`
   - `node = 2`, `hd = -1`
   - `mp = {0: 1, -1: 2}`
   - `Q = [(3, 1), (4, -2), (5, 0)]`

3. **Third iteration:**
   - `temp = (3, 1)`
   - `node = 3`, `hd = 1`
   - `mp = {0: 1, -1: 2, 1: 3}`
   - `Q = [(4, -2), (5, 0), (6, 2)]`

4. **Fourth iteration:**
   - `temp = (4, -2)`
   - `node = 4`, `hd = -2`
   - `mp = {0: 1, -1: 2, 1: 3, -2: 4}`
   - `Q = [(5, 0), (6, 2)]`

5. **Fifth iteration:**
   - `temp = (5, 0)`
   - `node = 5`, `hd = 0`
   - `mp` remains unchanged as `hd = 0` is already in `mp`
   - `Q = [(6, 2), (7, 1)]`

6. **Sixth iteration:**
   - `temp = (6, 2)`
   - `node = 6`, `hd = 2`
   - `mp = {0: 1, -1: 2, 1: 3, -2: 4, 2: 6}`
   - `Q = [(7, 1)]`

7. **Seventh iteration:**
   - `temp = (7, 1)`
   - `node = 7`, `hd = 1`
   - `mp` remains unchanged as `hd = 1` is already in `mp`
   - `Q = []` (empty, end of while loop)

**Final State:**
- `mp = {0: 1, -1: 2, 1: 3, -2: 4, 2: 6}`
- Result: `[-2, -1, 0, 1, 2] => [4, 2, 1, 3, 6]`

**Top View Output:**
```cpp
[4, 2, 1, 3, 6]
```

### Bottom View Dry Run
**Initial State:**
- `root = 1`
- `mp = {}`
- `Q = [(1, 0)]` (node, horizontal distance)

**Steps:**

1. **First iteration:**
   - `temp = (1, 0)`
   - `node = 1`, `hd = 0`
   - `mp = {0: 1}`
   - `Q = [(2, -1), (3, 1)]`

2. **Second iteration:**
   - `temp = (2, -1)`
   - `node = 2`, `hd = -1`
   - `mp = {0: 1, -1: 2}`
   - `Q = [(3, 1), (4, -2), (5, 0)]`

3. **Third iteration:**
   - `temp = (3, 1)`
   - `node = 3`, `hd = 1`
   - `mp = {0: 1, -1: 2, 1: 3}`
   - `Q = [(4, -2), (5, 0), (6, 2)]`

4. **Fourth iteration:**
   - `temp = (4, -2)`
   - `node = 4`, `hd = -2`
   - `mp = {0: 1, -1: 2, 1: 3, -2: 4}`
   - `Q = [(5, 0), (6, 2)]`

5. **Fifth iteration:**
   - `temp = (5, 0)`
   - `node = 5`, `hd = 0`
   - `mp = {0: 5, -1: 2, 1: 3, -2: 4}` (updates `hd = 0` with node 5)
   - `Q = [(6, 2), (7, 1)]`

6. **Sixth iteration:**
   - `temp = (6, 2)`
   - `node = 6`, `hd = 2`
   - `mp = {0: 5, -1: 2, 1: 3, -2: 4, 2: 6}`
   - `Q = [(7, 1)]`

7. **Seventh iteration:**
   - `temp = (7, 1)`
   - `node = 7`, `hd = 1`
   - `mp = {0: 5, -1: 2, 1: 7, -2: 4, 2: 6}` (updates `hd = 1` with node 7)
   - `Q = []` (empty, end of while loop)

**Final State:**
- `mp = {0: 5, -1: 2, 1: 7, -2: 4, 2: 6}`
- Result: `[-2, -1, 0, 1, 2] => [4, 2, 5, 7, 6]`

**Bottom View Output:**
```cpp
[4, 2, 5, 7, 6]
```

### Summary
- **Top View Output:** `[4, 2, 1, 3, 6]`
- **Bottom View Output:** `[4, 2, 5, 7, 6]`

The top view shows the nodes visible from the top of the tree, while the bottom view shows the nodes visible from the bottom, as we dry-ran through the same example binary tree.
