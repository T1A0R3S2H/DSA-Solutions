```cpp
long maximumSumSubarray(int K, vector<int> &Arr , int N){
        int i=0;
        int j=0;
        long sum=0;
        long maxi=INT_MIN;
        while(j<N) {
            sum+=Arr[j];
            if(j-i+1<K) {
              j++;
            } 
            else if(j-i+1==K){
                maxi=max(maxi,sum);
                sum=sum-Arr[i];
                j++;    
                i++;
            }
        }
        return maxi;
    }
```
- Window ka size hoga `j-i+1`, to `i` ko as a start pointer le rahe hai and `j` ko end. And then we are moving `j` till we reach our desired size ki window and when it has reached, then we perform the calculations.
- Till we do not reach the desired size, ie `k`, we increment `j`
- While incrementing `j`, we are also calculating the sum and as we reach our desired size, and then we have to go for the next window, we have to remove the element from the starting ie `i++` and also remove this element from the sum, ie `sum=sum-Arr[i]`
