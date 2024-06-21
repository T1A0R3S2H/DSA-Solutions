## Method 1 (using recursion)
In the provided code, the subset sums are not directly returned from the functions. Instead, the subset sums are accumulated in the `ans` vector, which is then returned by the `subsetSums` function.

Here's the breakdown of how the subset sums are returned:

1. The `subsetSumsHelper` function is a recursive helper function that calculates the subset sums. It takes the following parameters:
   - `ind`: The current index in the input array `arr`.
   - `arr`: The input array of integers.
   - `n`: The size of the input array `arr`.
   - `ans`: The reference to the vector where the subset sums will be stored.
   - `sum`: The current sum of the subset.

2. The `subsetSumsHelper` function has two main cases:
   - **Element is picked**: The function recursively calls itself with `ind + 1` and updates the `sum` by adding the current element `arr[ind]`.
   - **Element is not picked**: The function recursively calls itself with `ind + 1` but does not update the `sum`.

3. The base case of the recursion is when `ind == n`, which means the function has reached the end of the input array. At this point, the current `sum` is added to the `ans` vector, and the function returns.

4. The `subsetSums` function is the main entry point. It initializes an empty `ans` vector and then calls the `subsetSumsHelper` function with the initial parameters (`0`, `arr`, `n`, `ans`, `0`).

5. After the `subsetSumsHelper` function has finished executing and populating the `ans` vector, the `subsetSums` function sorts the `ans` vector in ascending order and returns it.

So, the subset sums are not directly returned from the functions, but rather accumulated in the `ans` vector, which is then returned by the `subsetSums` function. This allows the function to efficiently generate and store all possible subset sums without the need to return them explicitly.
```bash
public:
    void subsetSumsHelper(int ind, vector < int > & arr, int n, vector < int > & ans, int sum) {
      if (ind == n) {
        ans.push_back(sum);
        return;
      }
      //element is picked
      subsetSumsHelper(ind + 1, arr, n, ans, sum + arr[ind]);
      //element is not picked
      subsetSumsHelper(ind + 1, arr, n, ans, sum);
    }
  vector < int > subsetSums(vector < int > arr, int n) {
    vector < int > ans;
    subsetSumsHelper(0, arr, n, ans, 0);
    sort(ans.begin(), ans.end());
    return ans;
  }
```

Okay, let's go through an example to better understand how the subset sums are accumulated in the `ans` vector.

Let's consider the input array `arr = [1, 2, 3]`.

1. The `subsetSumsHelper` function is first called with the following parameters:
   - `ind = 0`
   - `arr = [1, 2, 3]`
   - `n = 3`
   - `ans = empty vector`
   - `sum = 0`

2. The function then explores the two cases:
   - **Element is picked**: The function recursively calls itself with `ind = 1` and `sum = 0 + 1 = 1`: `subsetSumsHelper(1, [1, 2, 3], 3, ans, 1)`.
   - **Element is not picked**: The function recursively calls itself with `ind = 1` and `sum = 0`: `subsetSumsHelper(1, [1, 2, 3], 3, ans, 0)`.

3. The recursive calls continue:
   - For the case where the element is picked:
     - The function is called with `ind = 1`, `sum = 1`.
     - It again explores the two cases:
       - **Element is picked**: The function recursively calls itself with `ind = 2` and `sum = 1 + 2 = 3`: `subsetSumsHelper(2, [1, 2, 3], 3, ans, 3)`.
       - **Element is not picked**: The function recursively calls itself with `ind = 2` and `sum = 1`: `subsetSumsHelper(2, [1, 2, 3], 3, ans, 1)`.
   - For the case where the element is not picked:
     - The function is called with `ind = 1`, `sum = 0`.
     - It again explores the two cases:
       - **Element is picked**: The function recursively calls itself with `ind = 2` and `sum = 0 + 2 = 2`: `subsetSumsHelper(2, [1, 2, 3], 3, ans, 2)`.
       - **Element is not picked**: The function recursively calls itself with `ind = 2` and `sum = 0`: `subsetSumsHelper(2, [1, 2, 3], 3, ans, 0)`.

4. The recursive calls continue until the base case is reached (`ind == n`, i.e., `ind == 3`). At this point, the current `sum` is added to the `ans` vector:
   - For `sum = 0`, `ans = [0]`.
   - For `sum = 1`, `ans = [0, 1]`.
   - For `sum = 2`, `ans = [0, 1, 2]`.
   - For `sum = 3`, `ans = [0, 1, 2, 3]`.

5. As the recursion unwinds, the function returns to the previous recursive calls, and the process continues for the other possible combinations of picked and not-picked elements.

By the time the recursion is complete, the `ans` vector will contain all the unique subset sums: `[0, 1, 2, 3, 4, 5, 6, 7]`.

The key point is that the subset sums are not directly added to the `ans` vector during the recursive calls. Instead, the `sum` variable is updated, and the final subset sums are added to the `ans` vector when the base case is reached (`ind == n`).
