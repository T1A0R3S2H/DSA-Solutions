## Logic (condsidering 3, the middle element as the pivot):
![image](https://github.com/T1A0R3S2H/Leetcode-Progess/assets/123285559/34a9c6eb-f8fc-40cc-bcaf-2469f67961d5)
##
1. Take a pivot
2. Count all elements<pivot: count
3. Place pivot on the (start+count)th index
4. Now, satisfy the condition that the elements on the left of the pivot should be < pivot and right ones should be > pivot
5. i aur j pointers lene for start and end respectively
6. If arr[i]>arr[j] then swap the elements and i++ and j--
7. To ye karne se left wala part less than pivot ho gaya aur right wala part greater than pivot
8. Ab separately left aur right parts ko sort kardo
9. Isse sort ho jayega array
10. Fir baaki elements kko bhi unki sahi jagah pohocha do

```bash
#include<iostream>
using namespace std;


int partition( int arr[], int s, int e) {

    int pivot = arr[s];

    int cnt = 0;
    for(int i = s+1; i<=e; i++) {
        if(arr[i] <=pivot) {
            cnt++;
        }
    }

    //place pivot at right position
    int pivotIndex = s + cnt;
    swap(arr[pivotIndex], arr[s]);

    //left and right wala part smbhal lete h 
    int i = s, j = e;

    while(i < pivotIndex && j > pivotIndex) {

        while(arr[i] <= pivot) 
        {
            i++;
        }

        while(arr[j] > pivot) {
            j--;
        }

        if(i < pivotIndex && j > pivotIndex) {
            swap(arr[i++], arr[j--]);
        }

    }

    return pivotIndex;

}

void quickSort(int arr[], int s, int e) {

    //base case
    if(s >= e) 
        return ;

    //partitioon karenfe
    int p = partition(arr, s, e);

    //left part sort karo
    quickSort(arr, s, p-1);

    //right wala part sort karo
    quickSort(arr, p+1, e);

}

int main() {

    int arr[10] = {2,4,1,6,9 ,9,9,9,9,9};
    int n = 10;

    quickSort(arr, 0, n-1);

    for(int i=0; i<n; i++) 
    {
        cout << arr[i] << " ";
    } cout << endl;


    return 0;
}
```

##
Reference video: [Quick Sort using Recursion](https://www.youtube.com/watch?v=sNaHN4tZmRk&list=PLDzeHZWIZsTryvtXdMr6rPh4IDexB5NIA&index=40)
