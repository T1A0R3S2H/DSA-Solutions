```bash
#include <vector>
#include <unordered_set>
using namespace std;

class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        if (matrix.empty()) return;

        int rows = matrix.size();
        int cols = matrix[0].size();
        unordered_set<int> zeroRows;
        unordered_set<int> zeroCols;

        // Step 1: Identify all rows and columns that should be zeroed
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                if (matrix[i][j] == 0) {
                    zeroRows.insert(i);
                    zeroCols.insert(j);
                }
            }
        }

        // Step 2: Set identified rows to zero
        for (int row : zeroRows) {
            for (int j = 0; j < cols; ++j) {
                matrix[row][j] = 0;
            }
        }

        // Step 3: Set identified columns to zero
        for (int col : zeroCols) {
            for (int i = 0; i < rows; ++i) {
                matrix[i][col] = 0;
            }
        }
    }
};

```
