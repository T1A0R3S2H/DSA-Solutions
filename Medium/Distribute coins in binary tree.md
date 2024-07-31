### Approach Explanation

The problem requires distributing coins in a binary tree such that each node has exactly one coin, using the minimum number of moves. To achieve this, the solution involves a post-order traversal of the tree, which processes the children of a node before the node itself. This traversal ensures that we account for all coin movements needed in the subtrees before adjusting the current node.

### Key Concepts

1. **Post-Order Traversal**:
   - Visit the left subtree.
   - Visit the right subtree.
   - Process the current node.
   
   This traversal is essential because we need to know the coin balance of the children before we can determine the necessary moves for the current node.

2. **Balance Calculation**:
   - For each node, calculate the balance of coins in its left and right subtrees.
   - Use these balances to determine the number of moves required to make the current node have exactly one coin.

3. **Moves Calculation**:
   - The number of moves required to balance a node is the sum of the absolute values of the balances of its left and right subtrees.
   - The balance of a node is calculated by summing its value and the balances of its children, then subtracting one (since each node should have exactly one coin).

### Detailed Step-by-Step Explanation

```cpp
class Solution {
public:
    int distributeCoins(TreeNode* root) {
        int moves = 0;  // Initialize the number of moves to 0
        balanceCoins(root, moves);  // Start balancing the coins from the root
        return moves;  // Return the total number of moves required
    }
    
    // Helper function to balance the coins in the tree
    int balanceCoins(TreeNode* node, int& moves) {
        if (!node) return 0;  // Base case: if the node is null, return 0
        
        // Recursively calculate the balance of coins for the left and right subtrees
        int leftBalance = balanceCoins(node->left, moves);
        int rightBalance = balanceCoins(node->right, moves);
        
        // Total moves needed for the current node is the sum of absolute balances of its subtrees
        moves += abs(leftBalance) + abs(rightBalance);
        
        // Return the balance of the current node
        // Balance is node's value plus left and right balances minus 1 (as each node needs 1 coin)
        return node->val + leftBalance + rightBalance - 1;
    }
};
```

### Example Dry Run

Let's dry run the example `[3,0,0]`.

#### Tree Structure
```
    3
   / \
  0   0
```

1. **Initial Call**: `distributeCoins(root)`
   - `moves = 0`
   - Call `balanceCoins(root, moves)`

2. **First Call**: `balanceCoins(3, moves)`
   - Call `balanceCoins(0 (left child of 3), moves)`
   
3. **Second Call**: `balanceCoins(0 (left child of 3), moves)`
   - `node` is not `nullptr`
   - Call `balanceCoins(nullptr, moves)` returns 0 (left child of 0)
   - Call `balanceCoins(nullptr, moves)` returns 0 (right child of 0)
   - `leftBalance = 0`
   - `rightBalance = 0`
   - `moves += abs(0) + abs(0) = 0`
   - Return `0 + 0 + 0 - 1 = -1`
   
4. **Back to First Call**: `balanceCoins(3, moves)`
   - `leftBalance = -1`
   - Call `balanceCoins(0 (right child of 3), moves)`
   
5. **Third Call**: `balanceCoins(0 (right child of 3), moves)`
   - `node` is not `nullptr`
   - Call `balanceCoins(nullptr, moves)` returns 0 (left child of 0)
   - Call `balanceCoins(nullptr, moves)` returns 0 (right child of 0)
   - `leftBalance = 0`
   - `rightBalance = 0`
   - `moves += abs(0) + abs(0) = 0`
   - Return `0 + 0 + 0 - 1 = -1`
   
6. **Back to First Call**: `balanceCoins(3, moves)`
   - `rightBalance = -1`
   - `moves += abs(-1) + abs(-1) = 2`
   - Return `3 + (-1) + (-1) - 1 = 0`

7. **Final Result**: `moves = 2`

So, the minimum number of moves required is 2.

### Time Complexity (TC)
The time complexity is \(O(n)\), where \(n\) is the number of nodes in the tree. This is because each node is visited exactly once during the post-order traversal.

### Space Complexity (SC)
The space complexity is \(O(h)\), where \(h\) is the height of the tree. This is due to the recursive call stack, which in the worst case (for a skewed tree) could be equal to the number of nodes, but in a balanced tree, it will be \(O(\log n)\).
