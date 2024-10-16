```cpp
class Solution { public:

int solveMem(int n, vector<int>& days, vector<int>& costs, int

index, vector<int>& dp){

if (index>=n) { return 0;

}

if (dp[index] != -1) {

return dp[index];

}

// 1-day pass

int opt1=costs [0]+solveMem(n, days, costs, index+1, dp);

// 7-day pass

int i;

for (i=index; i<n && days [i] <days [index]+7; i++);

int opt2=costs [1]+solveMem(n, days, costs, i, dp);

// 30-day pass

for (i=index; i<n && days [i]<days [index]+30; i++); int opt3=costs [2]+solveMem(n, days, costs, i, dp);

dp[index]=min(opt1, min(opt2, opt3));

return dp[index];

}

// int solveTab(vector<int>& days, vector<int>& costs){

// }

int mincostTickets (vector<int>& days, vector<int>& costs) {

int n=days.size();

int index=0;

vector<int> dp(n, -1);

return solveMem(n, days, costs, 0, dp);

// return solveTab(days, costs);

}
}
```