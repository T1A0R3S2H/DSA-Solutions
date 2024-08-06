```cpp
class Solution{
public:
    int isStackPermutation(int N,vector<int> &A,vector<int> &B){
        stack<int> result;
        int j=0;  // Index for B array
        
        for (int x:A) {
            result.push(x);
            while (!result.empty() && j < B.size() && result.top()==B[j]) {
                result.pop();
                j++;
            }
        }
        
        return j==B.size();
    }
};
```
