```cpp
class Solution {
public:
    void merge(int arr[], int l, int m, int r) {
        int n1 = m - l + 1; //left ka size
        int n2 = r - m; //right ka size

        int i = l; //start ka pointer
        int j = m + 1; //middle+1 ka pointer
        int k = l; //end ka pointer

        vector<int> temp(r - l + 1);

        while (i <= m && j <= r) {
            if (arr[i] <= arr[j]) {
                temp[k - l] = arr[i];
                i++;
            } else {
                temp[k - l] = arr[j];
                j++;
            }
            k++;
        }
        
        
        //copy in the left sorted array
        while (i <= m) {
            temp[k - l] = arr[i];
            i++;
            k++;
        }

        
        //copy in the right sorted array
        while (j <= r) {
            temp[k - l] = arr[j];
            j++;
            k++;
        }

        for (int i = l; i <= r; ++i) {
            arr[i] = temp[i - l];
        }
    }

    void mergeSort(int arr[], int l, int r) {
        if (l < r) {
            int m = l + (r - l) / 2;

            // Sort first and second halves
            mergeSort(arr, l, m);
            mergeSort(arr, m + 1, r);

            // Merge the sorted halves
            merge(arr, l, m, r);
        }
    }
};
```
