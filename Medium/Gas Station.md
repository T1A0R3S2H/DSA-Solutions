```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int total_petrol=0;
        int curr_petrol=0;
        int start_index=0;
        for(int i=0;i<gas.size();i++){
            total_petrol+=gas[i]-cost[i];
            curr_petrol+=gas[i]-cost[i];
            if(curr_petrol<0){
                start_index=i+1;
                curr_petrol=0;
            }
        }
        if(total_petrol>=0){
            return start_index;
        }
        else{
            return -1;
        }
    }
};




```
