# Method 1 (BUT TLE)
```cpp
class Solution{
  public:
    //Function to return the maximum sum of non-adjacent nodes.
    int getMaxSum(Node* root) {
    if (root == nullptr) {
        return 0;
    }

    // Sum including the current node
    int sumIncluding = root->data;
    if (root->left) {
        sumIncluding += getMaxSum(root->left->left) + getMaxSum(root->left->right);
    }
    if (root->right) {
        sumIncluding += getMaxSum(root->right->left) + getMaxSum(root->right->right);
    }

    // Sum excluding the current node
    int sumExcluding = getMaxSum(root->left) + getMaxSum(root->right);

    // Return the maximum of including and excluding sums
    return max(sumIncluding, sumExcluding);
}
};
```

1. For each node, we calculate two sums:
   - `sumIncluding`: The sum including the current node and its grandchildren (skipping its immediate children).
   - `sumExcluding`: The sum excluding the current node (including its children).

2. We then return the maximum of these two sums.



# Method 2 (Pair, Include/Exclude)
```cpp
class Solution{
  public:
    //Function to return the maximum sum of non-adjacent nodes.
    pair<int,int> solve(Node* root) {
        //base case
        if(root == NULL) {
            pair<int,int> p = make_pair(0,0);
            return p;
        }
        
        pair<int,int> left = solve(root->left);
        pair<int,int> right = solve(root->right);
        pair<int,int> res;
        // include the current node
        res.first = root->data + left.second + right.second;
        // exclude the current node
        res.second = max(left.first, left.second) + max(right.first, right.second);
        return res;
        
    }
    int getMaxSum(Node *root) 
    {
        pair<int,int> ans = solve(root);
        return max(ans.first, ans.second);
    }
};
```
