## Method 1
Can be done using one loop only, first sort the array and check if 
```bash
arr[i]==arr[i+1]
```
Here, the time complexity is O(n^2) and the space complexity is O(1)


## Method 2
It is the brute force approach, which involves solving using nested loops.

## Method 3
It involves the use of Hash Set, if an element is encountered, ***put it in Hash Set*** and check if the element is alreaddy there in the set afterwards. If yes, return true.

```bash
unordered_set <int> seen;
for(int num:nums){
  if (seen.count(num)>0){
    return true;
  }
seen.insert(num);
}
```

## Method 4
First iterate through the array and store each element as the key of the ***Hash Map*** (whihch represents the number of occurrences of the element). If we encounter an element which already exists with count >=1, return true.
```bash
unordered_map <int, int> seem;
for (int num:nums){
  if (seem[num]>=1){
    return true;
    seem[num]++;
  }
return false;
}
