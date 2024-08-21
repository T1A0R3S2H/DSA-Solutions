```cpp
class Solution
{
    public:
    //Function to merge k sorted arrays.
    vector<int> mergeKArrays(vector<vector<int>> arr, int K){
        vector<int> vec;
        priority_queue<int, vector<int>, greater<int>> mini;
    
        // Insert all elements of all arrays into the min-heap
        for (int i=0; i<K; i++) {
            for (int j=0; j<arr[i].size(); j++) {
                mini.push(arr[i][j]);
            }
        }
    
        // Extract elements from the min-heap and store them in the result vector
        while (!mini.empty()) {
            int s=mini.top();
            mini.pop();
            vec.push_back(s);
        }
    
        return vec;
        }
    
};
```
