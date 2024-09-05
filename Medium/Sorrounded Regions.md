### Code
```cpp
class Solution {
public:
    void DFS(vector<vector<char>>& board, int i, int j, int m, int n) {
        if(i<0 || j<0 || i>=m || j>=n || board[i][j]!='O') return;
        board[i][j]='#';
        DFS(board, i-1, j, m, n);
        DFS(board, i+1, j, m, n);
        DFS(board, i, j-1, m, n);
        DFS(board, i, j+1, m, n);
    }
    
    void solve(vector<vector<char>>& board){
        int m=board.size();
        if(m==0) return;  
        int n=board[0].size();

        //Moving over firts and last column   
        for(int i=0; i<m; i++) {
            if(board[i][0]=='O') DFS(board, i, 0, m, n);
            if(board[i][n-1]=='O') DFS(board, i, n-1, m, n);
        }
         
        //Moving over first and last row   
        for(int j=0; j<n; j++) {
            if(board[0][j]=='O') DFS(board, 0, j, m, n);
            if(board[m-1][j]=='O') DFS(board, m-1, j, m, n);
        }
        
        // after DFS
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){

                // '0' after DFS, which means can be converted, so convert '0' to 'X'
                if(board[i][j]=='O') board[i][j]='X';

                // '#' after DFS, which means, on boundary, so change back to '0'
                if(board[i][j]=='#') board[i][j]='O';
            }
        }
    }
};
```

### Explanation:

The problem "Surrounded Regions" asks us to identify regions of 'O's on a 2D board that are completely surrounded by 'X's. These regions need to be captured by changing all the 'O's in the region to 'X's. A region is not surrounded if it touches the boundary of the board, meaning it can't be captured.

The approach is to use **DFS (Depth First Search)** to mark all 'O's that are connected to the boundary, as they can't be surrounded. After this marking, we can flip all remaining 'O's (surrounded regions) to 'X' and revert the boundary-connected 'O's back to their original state.

### Steps:
1. **Mark boundary-connected 'O's:**
   - Traverse the boundary rows and columns. For any 'O' found on the boundary, perform DFS to mark all connected 'O's as `'#'` (a temporary marker to indicate these 'O's are part of a boundary region and should not be converted to 'X').
   
2. **Convert surrounded regions:**
   - After marking, traverse the entire board again. Convert any remaining 'O' (not connected to the boundary) to 'X', since these 'O's are part of a surrounded region.
   
3. **Revert the marked regions:**
   - Finally, convert all `'#'` markers back to 'O', as these 'O's were part of regions that could not be surrounded.

### Time Complexity:
- **DFS traversal:** Each DFS call processes an 'O' once, and we perform DFS from each boundary 'O'. In the worst case, all cells could be traversed by DFS.
- **Final traversal:** We iterate through all cells of the board to make the final conversions.
- **Overall:** The time complexity is \(O(m \times n)\), where \(m\) and \(n\) are the dimensions of the board.

### Space Complexity:
- **DFS recursion:** The space complexity of the recursion stack in DFS is \(O(m \times n)\) in the worst case, when all cells are 'O' and connected.
- **Overall space complexity:** \(O(m \times n)\) due to the DFS recursion.

### Dry Run:

Let's walk through **Example 1**:

```cpp
Input: board = [["X","X","X","X"],
                ["X","O","O","X"],
                ["X","X","O","X"],
                ["X","O","X","X"]]
```

1. **Initial state:**

```
X X X X
X O O X
X X O X
X O X X
```

2. **Step 1:** DFS from boundary:
   - Perform DFS on boundary 'O's. The only boundary 'O' is at (3, 1).
   
   After DFS from (3, 1):

```
X X X X
X O O X
X X O X
X # X X
```

3. **Step 2:** Convert surrounded 'O's to 'X':
   - Remaining 'O's are surrounded, so they are converted to 'X'.

```
X X X X
X X X X
X X X X
X # X X
```

4. **Step 3:** Revert boundary-connected 'O's (marked with `#`) back to 'O':

```
X X X X
X X X X
X X X X
X O X X
```

### Output:

```cpp
[["X","X","X","X"],
 ["X","X","X","X"],
 ["X","X","X","X"],
 ["X","O","X","X"]]
```
