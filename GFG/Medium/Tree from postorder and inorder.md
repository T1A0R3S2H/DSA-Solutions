```cpp
class Solution {
  public:
  
    int postIndex;
    
    Node* buildTreeHelper(vector<int>& inorder, vector<int>& postorder, int inorderStart, int inorderEnd) {
        if (inorderStart>inorderEnd) {
            return nullptr;
        }

        Node* root=new Node(postorder[postIndex--]);

        int inorderIndex;
        for (int i=inorderStart; i<=inorderEnd; i++) {
            if (inorder[i]==root->data) {
                inorderIndex=i;
                break;
            }
        }

        // first right then left because in post order, R-L-N
        root->right=buildTreeHelper(inorder, postorder, inorderIndex+1, inorderEnd);
        root->left=buildTreeHelper(inorder, postorder, inorderStart, inorderIndex-1);

        return root;
    }

    // Function to return a tree created from postorder and inoreder traversals.
    Node* buildTree(int n, int in[], int post[]) {
        vector<int> inorder(in, in + n);
        vector<int> postorder(post, post + n);

        postIndex = n - 1;
        return buildTreeHelper(inorder, postorder, 0, n - 1);
    }
};
```
