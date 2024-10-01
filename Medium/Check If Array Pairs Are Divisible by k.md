# Optimised Approach | Power of modulo🔥
### Prerequisites:
![image](https://github.com/user-attachments/assets/71c3c3fd-69ff-4ed5-a12b-8f766bfd5c21)

![image](https://github.com/user-attachments/assets/6a2b148c-1492-4747-807c-8c5260f24ca2)

### Code
```cpp
class Solution {
public:
    bool canArrange(vector<int>& arr, int k) {
        vector<int> count(k, 0);
        for (int num:arr) {
            int remainder=((num % k) + k) % k;
            count[remainder]++;
        }
        // remainder zero can only occur even times (to form pairs)
        if (count[0]%2!=0) return false;
        for (int i=1; i<=k/2; i++) {

            // modulo ki condition
            if (count[i]!=count[k-i]) return false;
        }
        return true;
    }
};
```
### Explanation

The problem requires checking whether an array of integers can be divided into pairs such that the sum of each pair is divisible by a given integer \( k \). The key steps in solving this problem are:

1. 
![image](https://github.com/user-attachments/assets/4ce763cc-0dc4-47f8-a06c-b9ca2af4a649)


2. **Count of Remainders**:
   - Use an array `count` of size \( k \) to keep track of how many integers yield each possible remainder when divided by \( k \).
   - Iterate through the input array to calculate the remainder for each integer and update the corresponding index in the `count` array.

3. **Pairing Logic**:
   - The pairing can be checked using the following rules:
     - For remainder \( 0 \): There must be an even count of elements giving this remainder since they can pair with themselves.
     - For each remainder \( i \) from \( 1 \) to \( k/2 \), the count of elements giving a remainder \( i \) must equal the count of elements giving the remainder \( k - i \). This ensures that each element can be paired correctly. i.e the complementary wali chheze.
![image](https://github.com/user-attachments/assets/89b3701c-df3b-4eea-8662-b8bb363f3fd4)

4. **Final Decision**:
   - If all conditions are satisfied, return `true`; otherwise, return `false`.

### Time Complexity

- **Remainder Counting**:
  - The algorithm iterates through the array once to compute the remainders, which takes \( O(n) \), where \( n \) is the length of the array.
  
- **Pairing Check**:
  - The algorithm then checks up to \( k/2 \) to compare counts, which takes \( O(k) \).

Overall, the time complexity is:
\[
O(n + k)
\]
Given that \( k \) can be at most \( 10^5 \) and \( n \) can be at most \( 10^5 \), this complexity is efficient for the input constraints.

### Space Complexity

- The space complexity is determined by the `count` array, which requires \( O(k) \) space. Therefore, the space complexity is:
\[
O(k)
\]
In this case, the additional space used is minimal compared to the input size.

### Dry Run

Let’s perform a dry run with the input:
```plaintext
arr = [39, 5, 30, -8, 46, 1, -10, 10, 8, -6, -5, 10]
k = 40
```

1. **Initialize Count Array**:
   \[
   \text{count} = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
   \]

2. **Count Remainders**:
   - \( 39 \mod 40 = 39 \) → `count[39]++` → `count[39] = 1`
   - \( 5 \mod 40 = 5 \) → `count[5]++` → `count[5] = 1`
   - \( 30 \mod 40 = 30 \) → `count[30]++` → `count[30] = 1`
   - \( -8 \mod 40 = 32 \) → `count[32]++` → `count[32] = 1`
   - \( 46 \mod 40 = 6 \) → `count[6]++` → `count[6] = 1`
   - \( 1 \mod 40 = 1 \) → `count[1]++` → `count[1] = 1`
   - \( -10 \mod 40 = 30 \) → `count[30]++` → `count[30] = 2`
   - \( 10 \mod 40 = 10 \) → `count[10]++` → `count[10] = 1`
   - \( 8 \mod 40 = 8 \) → `count[8]++` → `count[8] = 1`
   - \( -6 \mod 40 = 34 \) → `count[34]++` → `count[34] = 1`
   - \( -5 \mod 40 = 35 \) → `count[35]++` → `count[35] = 1`
   - \( 10 \mod 40 = 10 \) → `count[10]++` → `count[10] = 2`

3. **Final Count Array**:
   \[
   \text{count} = [0, 1, 0, 0, 0, 1, 1, 0, 1, 0, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 0, 0, 0, 0, 0, 0, 1, 1]
   \]

4. **Check Conditions**:
   - **For remainder 0**: `count[0] % 2 == 0` → \( 0 \) (satisfied)
   - **For \( i = 1 \)**: `count[1] == count[39]` → \( 1 != 0 \) (not satisfied)

Since the condition for \( i = 1 \) is not satisfied, the function returns `false`.

### Conclusion

The output for the input array is:
```plaintext
Output = false
```

This means that it is not possible to arrange the given array into pairs whose sums are all divisible by \( k \).


---

# Brute force approach | O(n^2)


```cpp
class Solution {
public:
    bool canArrange(vector<int>& arr, int k) {
        int n = arr.size();
        vector<bool> used(n, false);
        
        for (int i = 0; i < n; i++) {
            if (used[i]) continue;
            
            bool found_pair = false;
            for (int j = i + 1; j < n; j++) {
                if (!used[j] && (((arr[i] % k + k) % k + (arr[j] % k + k) % k) % k == 0)) {
                    used[i] = used[j] = true;
                    found_pair = true;
                    break;
                }
            }
            
            if (!found_pair) return false;
        }
        
        return true;
    }
};

```

Let's go through the changes made to your original approach:

1. We still iterate through all pairs of elements in the array, but now we consider all possible pairs, not just those between the first and second halves.

2. We use a `used` vector to keep track of which elements have already been paired.

3. The condition for a valid pair has been updated to handle negative numbers correctly:
   `(((arr[i] % k + k) % k + (arr[j] % k + k) % k) % k == 0)`
   This ensures that negative numbers are properly handled when checking for divisibility by k.

4. We mark elements as used once they've been paired.

5. If we can't find a pair for any element, we return false.

6. If we successfully pair all elements, we return true.

### Explanation of how to handle the mdoulo of the -ve part
```cpp
(((arr[i] % k + k) % k + (arr[j] % k + k) % k) % k == 0)
```
This part of the code is checking if the sum of two numbers (arr[i] and arr[j]) is divisible by k. However, it's doing so in a way that handles both positive and negative numbers correctly. Let's break it down step by step:

1. `(arr[i] % k + k) % k`:
   - First, we take arr[i] modulo k.
   - Then we add k to this result.
   - Finally, we take the modulo k again.
   
   This sequence of operations ensures that we get a positive number between 0 and k-1, even if arr[i] is negative.

2. `(arr[j] % k + k) % k`:
   - This does the same thing for arr[j].

3. We add these two results together.

4. We take the modulo k of this sum.

5. We check if the final result is equal to 0.

If the final result is 0, it means that the sum of arr[i] and arr[j] is divisible by k.

Here's why this works:

- For positive numbers, `(x % k + k) % k` is the same as `x % k`.
- For negative numbers, `(x % k + k) % k` gives the positive remainder when x is divided by k.

By using this formula, we're effectively finding `(arr[i] + arr[j]) % k == 0`, but in a way that works correctly for both positive and negative numbers.

For example, let's say k = 5:
- If arr[i] = 7 and arr[j] = 3, we get ((2 + 5) % 5 + (3 + 5) % 5) % 5 = (2 + 3) % 5 = 0
- If arr[i] = -7 and arr[j] = 2, we get ((3 + 5) % 5 + (2 + 5) % 5) % 5 = (3 + 2) % 5 = 0

In both cases, the sum (7 + 3 = 10, and -7 + 2 = -5) is divisible by 5, and our formula correctly identifies this.

This approach is particularly useful because it avoids potential integer overflow issues that could occur if we simply added arr[i] and arr[j] before taking the modulo.
The time complexity of this solution is O(n^2), where n is the length of the array. While this might not be the most efficient solution for very large inputs, it should work correctly for the given constraints and follows your intended brute force approach.
