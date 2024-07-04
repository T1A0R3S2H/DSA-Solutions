## Method 1 (brute force)
celeb wali poori row me 0s hone chahiye bas aur celeb wale column me diagonal ko chhodke baaki sab 1
```cpp
class Solution 
{
    public:
    //Function to find if there is a celebrity in the party or not.
    int celebrity(vector<vector<int> >& M, int n) 
    {
        int row=n;
        int col=n;
        
        for (int i=0;i<row;i++) {
            int row_count=0;
            int col_count=0;
            
            //condition 1: check if person i knows nobody, row mw sab 0
            for (int j=0;j<col;j++) {
                if (M[i][j]==0) {
                    row_count++;
                }
            }
            
            //condition 2: check if everyone knows person i, col me sab 1 except the diagonal one
            for (int j=0;j<row;j++) {
                if (M[j][i] == 1 && j != i) {
                    col_count++;
                }
            }
            
            if (row_count==n && col_count==n-1) {
                return i;
            }
        }
        return -1;
    }
};
```

## Method 2 (stacks)
