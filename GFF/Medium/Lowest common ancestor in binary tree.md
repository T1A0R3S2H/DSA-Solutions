```cpp
class Solution
{
    public:
    //Function to return the lowest common ancestor in a Binary Tree.
    Node* lca(Node* root ,int n1 ,int n2 ) {

        if(root==nullptr){
            return root;
        }
        
        if(root->data==n1 || root->data==n2){
            return root;
        }
        
        Node* LEFTSIDE  =lca(root->left,n1,n2);
        Node* RIGHTSIDE =lca(root->right,n1,n2);
        
        if(LEFTSIDE!=nullptr && RIGHTSIDE!=nullptr){
            return root;
        } 
        
        else if(LEFTSIDE!=nullptr){
            return LEFTSIDE;
        }
        
        else{
            return RIGHTSIDE;
        }
        
}
};
```

Here's your explanation converted into markdown code:

```markdown
The question is simply asking to find the subtree's root which will contain both `n1` and `n2`, right?! So we do the same,

Well, if there is no root, you gotta return null/root.
```cpp
if (root == nullptr) { 
    return root; 
} 
```
If your root data contains either `n1` or `n2`, you return that root. **WHY??** Because if the root itself is one of the two nodes, then it itself must be the LCA. Because the other one, no matter where, will have the LCA as the other node, which is now the root.

Now save your left answer of the left side of the tree in a variable. This variable will either have a `null` or a valid node depending on recursive calls if it finds the required node. Do the same for the right side: make a variable and save the answer.

Now, we have two different variables, namely `left` and `right`. Both contain answers, either `null` or a valid node. Now we check three conditions.
1. If the left answer is not `null` and the right answer is also not `null`, then the answer is the root itself. Since left and right are subtrees of the root, and if both are not `null`, it clearly means the values `n1` and `n2` lie in the right as well as the left side. For example, Example test case 1 is the perfect example for this case.

2. If the left answer is not `null` but the right answer is `null`, then it means both `n1` and `n2` lie on the left side of the tree inside the subtree with root as the left answer. Return the left answer. For example, Example test case 2 is the perfect example for this case.

3. If the right answer is not `null` but the left answer is `null`, then it means both `n1` and `n2` lie on the right side of the tree inside the subtree with root as the right answer. Return the right answer.
