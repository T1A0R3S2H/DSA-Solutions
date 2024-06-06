## Method 1
```bash
#include <vector>
using namespace std;

class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> matrix(numRows);

        for (int i = 0; i < numRows; ++i) {
            matrix[i].resize(i + 1, 1);  // Initialize row with 1s

            for (int j = 1; j < i; ++j) {  // Fill in the middle elements
                matrix[i][j] = matrix[i - 1][j - 1] + matrix[i - 1][j];
            }
        }

        return matrix;
    }
};

```
##
The approach to generating Pascal's Triangle starts by initializing a 2D vector to hold the triangle with `numRows` rows. Each row `i` is initialized to have `i + 1` elements, all set to 1, ensuring the first and last elements of each row are 1, which is a characteristic of Pascal's Triangle. For the elements between the first and last in each row, we calculate their values by summing the two elements directly above them from the previous row. Specifically, the element at position `j` in row `i` is computed as the sum of the elements at positions `j - 1` and `j` from row `i - 1`. This process is repeated for each row until all `numRows` rows are constructed, resulting in the complete Pascal's Triangle.

![image](https://github.com/T1A0R3S2H/Leetcode-Progess/assets/123285559/4f3eb66d-4379-4740-9444-3295e13ccee5)

## Method 2 (Striver's)
[Link of the article](https://takeuforward.org/data-structure/program-to-generate-pascals-triangle/)
