```cpp
class Solution {
  public:
    void dfs(vector<vector<int>>& image, int i, int j, int newColor, int startColor) {
        // Check for out-of-bounds or already processed pixel or different color pixel
        if (i < 0 || i >= image.size() || j < 0 || j >= image[0].size() || image[i][j] == newColor || image[i][j] != startColor) {
            return;
        }

        // Change the pixel color
        image[i][j] = newColor;

        // Recursively apply DFS in four directions
        dfs(image, i - 1, j, newColor, startColor); // Up
        dfs(image, i + 1, j, newColor, startColor); // Down
        dfs(image, i, j - 1, newColor, startColor); // Left
        dfs(image, i, j + 1, newColor, startColor); // Right
    }

    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        int startColor = image[sr][sc];
        
        // If the starting pixel's color is already the new color, no need to process further
        if (startColor != newColor) {
            dfs(image, sr, sc, newColor, startColor);
        }
        
        return image;
    }
};
```
