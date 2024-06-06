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
There can be 3 types of problems:
![image](https://github.com/T1A0R3S2H/Leetcode-Progess/assets/123285559/b5dc499b-adc2-4eab-82b9-fe3b09a8bf05)
1. Finding the element at a particular position
```bash

#include <bits/stdc++.h>
using namespace std;

int nCr(int n, int r) {
    long long res = 1;

    // calculating nCr:
    for (int i = 0; i < r; i++) {
        res = res * (n - i);
        res = res / (i + 1);
    }
    return res;
}

int pascalTriangle(int r, int c) {
    int element = nCr(r - 1, c - 1);
    return element;
}

int main()
{
    int r = 5; // row number
    int c = 3; // col number
    int element = pascalTriangle(r, c);
    cout << "The element at position (r,c) is: "
            << element << "n";
    return 0;
}
        
        
```
2. Given the row number n. Print the n-th row of Pascal’s triangle
```bash

#include <bits/stdc++.h>
using namespace std;

int nCr(int n, int r) {
    long long res = 1;

    // calculating nCr:
    for (int i = 0; i < r; i++) {
        res = res * (n - i);
        res = res / (i + 1);
    }
    return res;
}

void pascalTriangle(int n) {
    // printing the entire row n:
    for (int c = 1; c <= n; c++) {
        cout << nCr(n - 1, c - 1) << " ";
    }
    cout << "n";
}

int main()
{
    int n = 5;
    pascalTriangle(n);
    return 0;
}

```
3. Printing the  entire triangle when n is given
```bash
#include <bits/stdc++.h>
using namespace std;

void pascalTriangle(int n) {
    long long ans = 1;
    cout << ans << " "; // printing 1st element

    //Printing the rest of the part:
    for (int i = 1; i < n; i++) {
        ans = ans * (n - i);
        ans = ans / i;
        cout << ans << " ";
    }
    cout << endl;
}

int main()
{
    int n = 5;
    pascalTriangle(n);
    return 0;
}
```


##
[Link of the article](https://takeuforward.org/data-structure/program-to-generate-pascals-triangle/)
