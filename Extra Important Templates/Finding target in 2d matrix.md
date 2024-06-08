## Linear Search
```bash
#include <bits/stdc++.h>
using namespace std;
vector<int> linearSearch(vector<vector<int>> arr, int target)
{
for (int i = 0; i < arr.size(); i++) {
	for (int j = 0; j < arr[i].size(); j++) {
	if (arr[i][j] == target) {
		return {i, j};
	}
	}
}
return {-1, -1};
}

// Driver code 
int main()
{

vector<vector<int>> arr =
{ { 3, 12, 9 },
{ 5, 2, 89 },
{ 90, 45, 22 } };


int target = 89;
vector<int> ans = linearSearch(arr, target);
cout << "Element found at index: [" << ans[0] << " " <<ans[1] <<"]";

return 0;
}

```

## Binary Search (more efficient)
```bash


class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int rows = matrix.size();
        int cols = matrix[0].size();

        int start = 0;
        int end = rows*cols - 1;

        while(start <= end){
            int mid = start + (end-start)/2;

            int row_index = mid/cols;
            int col_index = mid%cols;

            // binary search
            if(matrix[row_index][col_index] < target) start = mid + 1;
            else if(matrix[row_index][col_index] == target) return true;
            else end = mid - 1;
        }
        return false;
    }
};
```
