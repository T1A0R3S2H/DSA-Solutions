## Method 1 (my approach)
```bash
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n=nums.size();
        int maxi=nums[0];
        int maxim=nums[0];
        for (int i=1;i<n;i++){
            maxim=max(nums[i], maxim+nums[i]);
            maxi=max(maxim, maxi);
        }
        return maxi;
    }
};
```
##

The Kadane's algorithm approach for finding the maximum subarray sum is based on dynamically updating the maximum sum ending at each index of the array. We initialize two variables, `maxim` and `maxi`, with the first element of the array. We iterate through the array starting from the second element. At each step, we update `maxim` to be the maximum of the current element and the sum of the current element and the maximum sum ending at the previous index. Then, we update `maxi` to be the maximum of `maxi` and `maxim`, ensuring it captures the maximum sum encountered so far. By continuing this process for each element of the array, we eventually obtain the maximum sum of a subarray. This approach has a time complexity of O(n), where n is the size of the array, and a space complexity of O(1), as we only use a constant amount of extra space for storing the variables `maxim` and `maxi`.

## Method 2 (Striver's Optimal Approach, Kadane's Algorithm)
```bash

#include <bits/stdc++.h>
using namespace std;

long long maxSubarraySum(int arr[], int n) {
    long long maxi = LONG_MIN; // maximum sum
    long long sum = 0;

    for (int i = 0; i < n; i++) {

        sum += arr[i];

        if (sum > maxi) {
            maxi = sum;
        }

        // If sum < 0: discard the sum calculated
        if (sum < 0) {
            sum = 0;
        }
    }

    // To consider the sum of the empty subarray
    // uncomment the following check:

    //if (maxi < 0) maxi = 0;

    return maxi;
}

int main()
{
    int arr[] = { -2, 1, -3, 4, -1, 2, 1, -5, 4};
    int n = sizeof(arr) / sizeof(arr[0]);
    long long maxSum = maxSubarraySum(arr, n);
    cout << "The maximum subarray sum is: " << maxSum << endl;
    return 0;
}

```
##
If in a follow up question, we are also told to find one of the subarrays which gives maximum sum:
```bash
#include <bits/stdc++.h>
using namespace std;

vector<int> maxSubarraySum(int arr[], int n) {
    long long maxSum = INT_MIN; // maximum sum
    long long currSum = 0;
    vector<int> maxSubarray; // to store the maximum subarray
    vector<int> currSubarray; // to store the current subarray

    for (int i = 0; i < n; i++) {
        currSubarray.push_back(arr[i]); // add the current element to the current subarray
        currSum += arr[i];

        if (currSum > maxSum) {
            maxSum = currSum;
            maxSubarray = currSubarray; // update the maximum subarray
        }

        // If currSum < 0: discard the current subarray
        if (currSum < 0) {
            currSum = 0;
            currSubarray.clear(); // clear the current subarray
        }
    }

    return maxSubarray;
}

int main() {
    int arr[] = {-2, 1, -3 ,4 ,-1 ,2 , 1, -5, 4};
    int n = sizeof(arr) / sizeof(arr[0]);
    vector<int> maxSubarray = maxSubarraySum(arr, n);

    cout << "The maximum subarray sum is: " << accumulate(maxSubarray.begin(), maxSubarray.end(), 0LL) << endl;
    cout << "The maximum subarray is: ";
    for (int num : maxSubarray) {
        cout << num << " ";
    }
    cout << endl;

    return 0;
}
```
##
In this modified code, we use two vectors: currSubarray to store the current subarray, and maxSubarray to store the maximum subarray found so far. We update currSubarray by adding the current element arr[i] to it. If the current sum currSum becomes greater than the maximum sum maxSum, we update maxSum and copy currSubarray to maxSubarray.
If currSum becomes negative, we reset currSum to 0 and clear currSubarray because any subarray ending with a negative sum cannot contribute to the maximum subarray sum.
In the end, we print the maximum subarray sum using accumulate from the numeric header, and then we print the elements of the maxSubarray vector.
