# Method 1 (brute force)
```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        vector<int> left(n), right(n);
        
        // Compute left boundaries
        left[0] = 0;
        for (int i = 1; i < n; i++) {
            int p = i - 1;
            while (p >= 0 && heights[p] >= heights[i]) {
                p = left[p] - 1;
            }
            left[i] = p + 1;
        }
        
        // Compute right boundaries
        right[n-1] = n - 1;
        for (int i = n - 2; i >= 0; i--) {
            int p = i + 1;
            while (p < n && heights[p] >= heights[i]) {
                p = right[p] + 1;
            }
            right[i] = p - 1;
        }
        
        // Compute max area
        int maxArea = 0;
        for (int i = 0; i < n; i++) {
            int area = heights[i] * (right[i] - left[i] + 1);
            if (area > maxArea) {
                maxArea = area;
            }
        }
        
        return maxArea;
    }
};
```


# Method 2 (stack)
```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        stack<int> s;
        int maxArea = 0;
        
        for (int i = 0; i <= n; i++) {
            int h;
            if (i == n) {
                h = 0;
            } else {
                h = heights[i];
            }
            
            while (!s.empty() && h < heights[s.top()]) {
                int height = heights[s.top()];
                s.pop();
                
                int width;
                if (s.empty()) {
                    width = i;
                } else {
                    width = i - s.top() - 1;
                }
                
                int area = height * width;
                if (area > maxArea) {
                    maxArea = area;
                }
            }
            
            s.push(i);
        }
        
        return maxArea;
    }
};
```
