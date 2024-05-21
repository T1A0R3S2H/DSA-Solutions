## Method 1
Can be done using Hash Map, if lengths are unequal, return false, if equal:- create a Hash Map (or set) to store the freq. of one string. Iterate in another string, if a character matches, decrement the freq. If no match or freq becomes -ve, return false.
Time Complexity is O(n) and the S.C. is O(1) because the unordered map takes constant time (and at max 26 elements, a-z will be stored in the map in case of worst case).

## Method 2 
Another method is to first sort both the strings and check the equality, if both the strings are equal, return true, else false.
