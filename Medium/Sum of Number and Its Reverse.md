```cpp
class Solution {
public:
    bool sumOfNumberAndReverse(int num) {
        for(int i=num/2;i<=num;i++)
        {
            int rev=helper(i);
            if(rev+i==num)
            {
                return true;
            }
        }
        return false;
    }


    int helper(int n)
    {
        int ans=0;
        int rem;
        while(n>0)
        {
            rem=n%10;
            ans=ans*10+rem;
            n=n/10;
        }
        return ans;
    }
};
```
