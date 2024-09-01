### Code:
```cpp
#include <bits/stdc++.h>
using namespace std;
 
int main() {
    int t;
    cin>>t;
    while (t--) {
        int n, k;
        cin>>n>>k;
        vector<int> a(n);
        for (int i=0; i<n; i++) {
            cin>>a[i];
        }
        if (k==1 && !is_sorted(a.begin(), a.end())){
            cout<<"NO"<<endl;
        }
        else{
            cout<<"YES"<<endl;
        }
    }
    return 0;
}
```
### Idea
- The main idea is that when `k=1`, which means, the o/p will YES iff the array is sorted (because k=1 means no reversals, the elements will be in place).
- If `k>1`, which means for any n and k>1, we can reverse and get the sorted output.
