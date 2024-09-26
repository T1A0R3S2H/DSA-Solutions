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

## C++ Code
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
## Code walkthrough
Let's walk through the `gameOfLife` function, explaining each step of the code:

```cpp
void gameOfLife(vector<vector<int>>& board) {
    int m = board.size();
    int n = board[0].size();
```
1. The function starts by getting the dimensions of the board:
   - `m` is the number of rows
   - `n` is the number of columns

```cpp
    vector<vector<int>> nextState = board;
```
2. A copy of the board is created to store the next state. This is necessary because we need to update all cells simultaneously without affecting the calculations for neighboring cells.

```cpp
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
```
3. Two nested loops iterate through each cell on the board:
   - `i` represents the current row
   - `j` represents the current column

```cpp
            int neighborCase = getNeighborCase(board, i, j, m, n);
```
4. For each cell, we call `getNeighborCase` to determine the number of live neighbors:
   - This function counts the live neighbors and returns a value from 0 to 6
   - We'll examine `getNeighborCase` in detail later

```cpp
            int currentState = board[i][j];
```
5. We store the current state of the cell (0 for dead, 1 for alive)

```cpp
            switch (neighborCase) {
                case 0: // No live neighbors
                case 1: // One live neighbor
                    nextState[i][j] = 0; // Dies in both cases (rule 1)
                    break;
```
6. The switch statement begins. For 0 or 1 live neighbors:
   - The cell dies (or stays dead) due to underpopulation (rule 1)

```cpp
                case 2: // Two live neighbors
                    nextState[i][j] = currentState; // Stays the same (rule 2)
                    break;
```
7. For 2 live neighbors:
   - The cell maintains its current state (rule 2)

```cpp
                case 3: // Three live neighbors
                    nextState[i][j] = 1; // Lives or becomes alive (rules 2 and 4)
                    break;
```
8. For 3 live neighbors:
   - The cell becomes or stays alive (rules 2 and 4)

```cpp
                case 4: // Four live neighbors
                case 5: // Five live neighbors
                default: // Six or more live neighbors
                    nextState[i][j] = 0; // Dies due to overpopulation (rule 3)
                    break;
```
9. For 4 or more live neighbors:
   - The cell dies due to overpopulation (rule 3)

```cpp
            }
        }
    }
```
10. The switch statement and loops end after processing all cells

```cpp
    board = nextState;
}
```
11. Finally, the original board is updated with the new state

Now, let's examine the `getNeighborCase` function:

```cpp
int getNeighborCase(const vector<vector<int>>& board, int row, int col, int m, int n) {
    int liveNeighbors = 0;
```
1. Initialize a counter for live neighbors

```cpp
    for (int i = -1; i <= 1; i++) {
        for (int j = -1; j <= 1; j++) {
            if (i == 0 && j == 0) continue;
```
2. Nested loops check all 8 surrounding cells:
   - Skip the cell itself (when i == 0 and j == 0)

```cpp
            int newRow = row + i;
            int newCol = col + j;
            if (newRow >= 0 && newRow < m && newCol >= 0 && newCol < n) {
                liveNeighbors += board[newRow][newCol];
            }
```
3. For each valid neighbor (within board boundaries):
   - Add its value (0 or 1) to the `liveNeighbors` count

```cpp
        }
    }
    return min(liveNeighbors, 6);
}
```
4. Return the count of live neighbors, capped at 6 (since 6+ neighbors all result in the same outcome)

This walkthrough explains each step of the code, showing how the Game of Life rules are implemented using the switch case for different neighbor scenarios.

## Conclusion

This implementation efficiently solves the Game of Life problem while adhering to the specified requirements of using a switch case to handle different neighbor scenarios. It correctly applies the rules of the game and updates the board state accordingly.



