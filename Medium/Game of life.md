# Method 1 (switch case)

## Overview

This document explains the implementation of Conway's Game of Life in C++, following specific guidelines for using switch cases to handle different neighbor scenarios.

## Class Structure

The solution is implemented as a class named `Solution` with two main components:

1. `gameOfLife`: The primary function that implements the Game of Life rules.
2. `getNeighborCase`: A helper function that determines the number of live neighbors for a cell.

## The `gameOfLife` Function

### Purpose
This function applies the rules of the Game of Life to update the state of the board.

### Process
1. Initialize variables for board dimensions and create a copy of the board for the next state.
2. Iterate through each cell on the board.
3. For each cell:
   - Determine the number of live neighbors using `getNeighborCase`.
   - Apply the Game of Life rules using a switch statement based on the neighbor case.
4. Update the original board with the new state.

### Switch Case Implementation
The switch statement handles 6 cases based on the number of live neighbors:
- Cases 0 and 1: Cell dies due to underpopulation.
- Case 2: Cell remains in its current state.
- Case 3: Cell becomes or stays alive.
- Cases 4, 5, and default (6 or more): Cell dies due to overpopulation.

This implementation directly addresses the four rules of the Game of Life:
1. Any live cell with fewer than two live neighbors dies.
2. Any live cell with two or three live neighbors lives on.
3. Any live cell with more than three live neighbors dies.
4. Any dead cell with exactly three live neighbors becomes a live cell.

## The `getNeighborCase` Function

### Purpose
This helper function counts the number of live neighbors for a given cell.

### Process
1. Initialize a counter for live neighbors.
2. Check all 8 surrounding cells (horizontal, vertical, diagonal).
3. For each valid neighbor cell (within board boundaries), increment the counter if the neighbor is alive.
4. Return the count, capped at 6 (since 6+ neighbors all result in the same outcome).

## Time and Space Complexity

- Time Complexity: O(m * n), where m and n are the dimensions of the board.
- Space Complexity: O(m * n) for the temporary next state board.

## Code
```cpp
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        int m = board.size();
        int n = board[0].size();
        vector<vector<int>> nextState = board;

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int neighborCase = getNeighborCase(board, i, j, m, n);
                int currentState = board[i][j];

                switch (neighborCase) {
                    case 0: // No live neighbors
                    case 1: // One live neighbor
                        nextState[i][j] = 0; // Dies in both cases (rule 1)
                        break;
                    case 2: // Two live neighbors
                        nextState[i][j] = currentState; // Stays the same (rule 2)
                        break;
                    case 3: // Three live neighbors
                        nextState[i][j] = 1; // Lives or becomes alive (rules 2 and 4)
                        break;
                    case 4: // Four live neighbors
                    case 5: // Five live neighbors
                    default: // Six or more live neighbors
                        nextState[i][j] = 0; // Dies due to overpopulation (rule 3)
                        break;
                }
            }
        }
        board = nextState;
    }

private:
    int getNeighborCase(const vector<vector<int>>& board, int row, int col, int m, int n) {
        int liveNeighbors = 0;
        for (int i = -1; i <= 1; i++) {
            for (int j = -1; j <= 1; j++) {
                if (i == 0 && j == 0) continue;
                int newRow = row + i;
                int newCol = col + j;
                if (newRow >= 0 && newRow < m && newCol >= 0 && newCol < n) {
                    liveNeighbors += board[newRow][newCol];
                }
            }
        }
        return min(liveNeighbors, 6); // Cap at 6 for the switch case
    }
};
```

## Conclusion

This implementation efficiently solves the Game of Life problem while adhering to the specified requirements of using a switch case to handle different neighbor scenarios. It correctly applies the rules of the game and updates the board state accordingly.
