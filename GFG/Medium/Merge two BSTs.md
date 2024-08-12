```cpp
class Solution {
public:
    vector<int> ans;

    void inorder(Node* root, vector<int>& vec) {
        if (root) {
            inorder(root->left, vec);
            vec.push_back(root->data);
            inorder(root->right, vec);
        }
    }
    
    vector<int> mergeSortedArrays(vector<int>& v1, vector<int>& v2) {
        vector<int> merged;
        int i = 0, j = 0;
        while (i < v1.size() && j < v2.size()) {
            if (v1[i] < v2[j]) {
                merged.push_back(v1[i++]);
            } else {
                merged.push_back(v2[j++]);
            }
        }
        // Add remaining elements
        while (i < v1.size()) {
            merged.push_back(v1[i++]);
        }
        while (j < v2.size()) {
            merged.push_back(v2[j++]);
        }
        return merged;
    }

    vector<int> merge(Node *root1, Node *root2) {
        vector<int> v1, v2;
        inorder(root1, v1);
        inorder(root2, v2);
        return mergeSortedArrays(v1, v2);
    }
    
};
```
