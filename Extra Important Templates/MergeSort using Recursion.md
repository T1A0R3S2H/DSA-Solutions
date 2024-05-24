## Sort left array, sort right array then merge both the sorted arraya baar baar recursively
- [Video Explanation](https://www.youtube.com/watch?v=cdHEpbBVjRM&list=PLDzeHZWIZsTryvtXdMr6rPh4IDexB5NIA&index=38&pp=iAQB)

```
class Solution {
public:
    void merge(int arr[], int l, int m, int r) {
        int n1 = m - l + 1;
        int n2 = r - m;

        int i = l;
        int j = m + 1;
        int k = l;

        std::vector<int> temp(r - l + 1);

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

        while (i <= m) {
            temp[k - l] = arr[i];
            i++;
            k++;
        }

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
