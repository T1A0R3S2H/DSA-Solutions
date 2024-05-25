## Method 1
The approach to solve this problem using two pointers is as follows: We initialize two pointers, `left` and `right`, at the start and end of the input array `height`, respectively. Then, we enter a loop that continues until `left` and `right` pointers meet. In each iteration, we calculate the width of the potential container as the distance between the `left` and `right` pointers. We then find the minimum height between `height[left]` and `height[right]`, which represents the maximum height of water that the container can hold. We calculate the area of the potential container by multiplying the width and the minimum height. We update the `maxWater` variable with the maximum value between the current `maxWater` and the calculated area. After this, we move the pointer pointing to the shorter line inwards, as the water level is determined by the shorter line, and the area between the two lines cannot contribute to the maximum area. We repeat this process until the `left` and `right` pointers meet, at which point we have explored all possible containers, and the `maxWater` variable holds the maximum area of water that can be contained. The time complexity of this solution is O(n), where n is the length of the input array `height`, and the space complexity is O(1), as we only use a few additional variables to store the pointers and temporary values.
- The time complexity of this approach is O(n), where n is the length of the input array height. This is because we iterate through the array once with the two pointers, left and right, moving them towards each other.
- The space complexity of this approach is O(1), as we only use a few additional variables to store the pointers (left and right), maxWater, and temporary values like width and minHeight. The additional space used is constant and does not depend on the size of the input array.
##
```
int maxArea(vector<int>& height) {
    int n = height.size();
    int left = 0, right = n - 1;
    int maxWater = 0;

    while (left < right) {
        int width = right - left;
        int minHeight = min(height[left], height[right]);
        int area = width * minHeight;
        maxWater = max(maxWater, area);

        // Move the pointer pointing to the shorter line
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }

    return maxWater;
}
```

##

## Method 2 (Somewhat Efficient)
This approach is based on the observation that the maximum area of water that can be contained is limited by the height of the shorter vertical line. We can start with the maximum possible area by considering the area between the two outermost lines and then gradually reduce the area by moving the pointer pointing to the shorter line towards the other end.
- The time complexity of the efficient approach is O(n), where n is the length of the input array height. This is because the approach uses a single loop that iterates through the array once, moving the left and right pointers towards each other. In each iteration, a constant number of operations are performed, including finding the minimum height, calculating the area, updating maxWater, and moving one of the pointers. Since the loop runs for n iterations, and each iteration takes constant time, the overall time complexity is O(n).
- The space complexity of the efficient approach is O(1), which means it uses a constant amount of additional space, regardless of the size of the input array height. The approach only uses a few additional variables to store the pointers (left and right), maxWater, and temporary values like minHeight. These variables take up a constant amount of space, independent of the input size. Therefore, the space complexity is O(1), which is optimal.
  ##

```
int maxArea(vector<int>& height) {
    int n = height.size();
    int left = 0, right = n - 1;
    int maxWater = 0;

    while (left < right) {
        int minHeight = min(height[left], height[right]);
        maxWater = max(maxWater, minHeight * (right - left));

        // Move the pointer pointing to the shorter line
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }

    return maxWater;
}
```
