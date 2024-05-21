## Method 1
Brute force approach, ie using two loops
```bash
for (int i=0;i<(n-1);i++){
  for (int j=i+1;j<n;j++){
    if (nums[i]+nums[i+1]==target){
      return true;
    }
  }
}
```


## Method 2
Involves the use of Hash Map, idea is to find the complement of each element **(target-nums[i])** and check if it exists in the Hash Map (where elements were stored previously).
```bash
unordered_map <int, int> numMap;
int n=nums.size();
for (int i=0;i<n;i++){
  int complement=(target-nums[i]);
  if (numMap.count(complement)){
    return {numMap[complement], i};
  }
  numMap[nums[i]]=i;
}
return {};
```
Time Complexity is O(n) and Space Complexity is O(n)
