### Code:
```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n=heights.size();
        vector<int> left(n), right(n);
        
        // Compute left boundaries
        left[0]=0;
        for (int i=1; i<n; i++) {
            int p=i-1;
            while (p>=0 && heights[p]>=heights[i]) {
                p=left[p]-1;
            }
            left[i]=p+1;
        }
        
        // Compute right boundaries
        right[n-1]=n-1;
        for (int i=n-2; i>=0; i--) {
            int p=i+1;
            while (p<n && heights[p]>=heights[i]) {
                p=right[p]+1;
            }
            right[i]=p-1;
        }
        
        // Compute max area
        int maxArea=0;
        for (int i=0; i<n; i++) {
            int area=heights[i]*(right[i]-left[i]+1);
            if (area>maxArea) {
                maxArea=area;
            }
        }
        
        return maxArea;
    }

    int maximalRectangle(vector<vector<char>>& matrix) {
        int maxi=INT_MIN;
        vector<int>histogram(matrix[0].size(), 0);
        for(int i=0; i<matrix.size(); i++){
            // create histogram array
            for(int j=0; j<matrix[0].size(); j++){
                if(matrix[i][j]=='1') histogram[j]++;
                else histogram[j]=0;
            }
            maxi=max(maxi, largestRectangleArea(histogram));
        }
        return maxi;
    }
};
```

### Explanation:

### Explanation of `largestRectangleArea` Function
The `largestRectangleArea` function calculates the maximum rectangular area that can be formed in a histogram represented by the `heights` array. Each element in `heights` corresponds to the height of a bar. The function finds the width of the largest rectangle that can be formed using each bar as the shortest bar in a sub-array. This is achieved by calculating two auxiliary arrays, `left` and `right`, where:
- **`left[i]`** stores the index of the left boundary where a height smaller than `heights[i]` appears, or the start if no such boundary exists. This determines how far left we can extend the rectangle without encountering a smaller height.
- **`right[i]`** stores the index of the right boundary where a height smaller than `heights[i]` appears, or the end if no such boundary exists.
Using these boundaries, the function computes the area of the rectangle that each bar could form, with its height equal to `heights[i]` and width equal to `(right[i] - left[i] + 1)`. Finally, it returns the largest rectangle area found.

### Explanation of `maximalRectangle` Function and Its Use of `largestRectangleArea`
The `maximalRectangle` function applies `largestRectangleArea` to find the maximal rectangle in a 2D binary matrix filled with '0's and '1's. It treats each row in the matrix as if it were a new histogram, building a cumulative `histogram` array where each element represents the height of consecutive '1's in a column up to the current row. This histogram effectively represents the heights of rectangles of consecutive '1's that end at each row. For each updated row histogram, the function calls `largestRectangleArea` to calculate the maximal rectangle area for that row's histogram. This process repeats for every row in the matrix, and the function keeps track of the largest area encountered. Thus, `largestRectangleArea` enables `maximalRectangle` to compute the maximal rectangle by interpreting each row as a histogram, treating the matrix as a set of stacked histograms.

1. **`largestRectangleArea` function**:
   - This function computes the maximum rectangular area for a given histogram (array of heights).
   - The `left` and `right` arrays store boundaries for each bar in the histogram to calculate the width of the rectangle centered at each bar.

2. **`maximalRectangle` function**:
   - This function iterates through each row of the matrix to create a histogram that represents heights of consecutive '1's in columns up to the current row.
   - For each row (updated histogram), it calculates the maximal rectangle area using `largestRectangleArea` and keeps track of the maximum.

### Time Complexity:

- **`largestRectangleArea`**: \(O(n)\) for calculating the max rectangle area in a histogram, where \(n\) is the number of columns.
- **`maximalRectangle`**: \(O(m \times n)\), where \(m\) is the number of rows and \(n\) is the number of columns, as it calls `largestRectangleArea` for each row.

**Total Time Complexity**: \(O(m \times n)\)

### Space Complexity:

- **`largestRectangleArea`**: \(O(n)\) for the `left` and `right` vectors.
- **`maximalRectangle`**: \(O(n)\) for the histogram vector.

**Total Space Complexity**: \(O(n)\)

### Dry Run:

For `matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]`:

1. **First row histogram**: `[1, 0, 1, 0, 0]`
   - Maximum rectangle area from `largestRectangleArea`: 1

2. **Second row histogram**: `[2, 0, 2, 1, 1]`
   - Maximum rectangle area from `largestRectangleArea`: 3

3. **Third row histogram**: `[3, 1, 3, 2, 2]`
   - Maximum rectangle area from `largestRectangleArea`: 6

4. **Fourth row histogram**: `[4, 0, 0, 3, 0]`
   - Maximum rectangle area from `largestRectangleArea`: 4

Final maximum rectangle area is **6**.
