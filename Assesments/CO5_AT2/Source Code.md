## PROGRAM
``` c
#include <stdio.h>

#define N 9

// Print the Sudoku grid
void printGrid(int grid[N][N])
{
    int i, j;

    for (i = 0; i < N; i++)
    {
        for (j = 0; j < N; j++)
        {
            printf("%d ", grid[i][j]);
        }
        printf("\n");
    }
}

// Check if placing num is valid
int isSafe(int grid[N][N], int row, int col, int num)
{
    int i, j;

    // Check row
    for (i = 0; i < N; i++)
    {
        if (grid[row][i] == num)
            return 0;
    }

    // Check column
    for (i = 0; i < N; i++)
    {
        if (grid[i][col] == num)
            return 0;
    }

    // Find starting position of 3x3 box
    int startRow = row - row % 3;
    int startCol = col - col % 3;

    // Check 3x3 box
    for (i = 0; i < 3; i++)
    {
        for (j = 0; j < 3; j++)
        {
            if (grid[startRow + i][startCol + j] == num)
                return 0;
        }
    }

    return 1;
}

// Find an empty cell
int findEmptyCell(int grid[N][N], int *row, int *col)
{
    int i, j;

    for (i = 0; i < N; i++)
    {
        for (j = 0; j < N; j++)
        {
            if (grid[i][j] == 0)
            {
                *row = i;
                *col = j;
                return 1;
            }
        }
    }

    return 0;
}

// Solve Sudoku using Backtracking
int solveSudoku(int grid[N][N])
{
    int row, col;
    int num;

    // If there is no empty cell, Sudoku is solved
    if (!findEmptyCell(grid, &row, &col))
        return 1;

    // Try numbers 1 to 9
    for (num = 1; num <= 9; num++)
    {
        // Check whether number can be placed
        if (isSafe(grid, row, col, num))
        {
            // Place number
            grid[row][col] = num;

            // Recursively solve remaining cells
            if (solveSudoku(grid))
                return 1;

            // Backtrack
            grid[row][col] = 0;
        }
    }

    // No number can be placed
    return 0;
}

int main()
{
    int grid[N][N] =
    {
        {5, 3, 0, 0, 7, 0, 0, 0, 0},
        {6, 0, 0, 1, 9, 5, 0, 0, 0},
        {0, 9, 8, 0, 0, 0, 0, 6, 0},
        {8, 0, 0, 0, 6, 0, 0, 0, 3},
        {4, 0, 0, 8, 0, 3, 0, 0, 1},
        {7, 0, 0, 0, 2, 0, 0, 0, 6},
        {0, 6, 0, 0, 0, 0, 2, 8, 0},
        {0, 0, 0, 4, 1, 9, 0, 0, 5},
        {0, 0, 0, 0, 8, 0, 0, 7, 9}
    };

    printf("Original Sudoku:\n\n");
    printGrid(grid);

    if (solveSudoku(grid))
    {
        printf("\nCompleted Sudoku:\n\n");
        printGrid(grid);
    }
    else
    {
        printf("\nNo solution exists.\n");
    }

    return 0;
}
```
# OUTPUT
<img width="419" height="315" alt="Screenshot 2026-08-31 173731" src="https://github.com/user-attachments/assets/6f488bbc-2ef7-4bee-9b96-08d842953285" />
