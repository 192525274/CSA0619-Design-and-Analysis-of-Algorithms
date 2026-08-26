## PROGRAM
``` C
#include <stdio.h>
#include <limits.h>

int sum(int freq[], int i, int j)
{
    int s = 0;

    for (int k = i; k <= j; k++)
        s += freq[k];

    return s;
}

int optimalBST(int keys[], int freq[], int n)
{
    int cost[n][n];

    // Cost of a single key
    for (int i = 0; i < n; i++)
        cost[i][i] = freq[i];

    // Consider chains of length 2 to n
    for (int length = 2; length <= n; length++)
    {
        for (int i = 0; i <= n - length; i++)
        {
            int j = i + length - 1;
            cost[i][j] = INT_MAX;

            int totalFreq = sum(freq, i, j);

            // Try every key as root
            for (int r = i; r <= j; r++)
            {
                int leftCost = (r > i) ? cost[i][r - 1] : 0;
                int rightCost = (r < j) ? cost[r + 1][j] : 0;

                int totalCost = leftCost + rightCost + totalFreq;

                if (totalCost < cost[i][j])
                    cost[i][j] = totalCost;
            }
        }
    }

    return cost[0][n - 1];
}

int main()
{
    int n;

    printf("Enter number of keys: ");
    scanf("%d", &n);

    int keys[n], freq[n];

    printf("Enter keys in sorted order:\n");
    for (int i = 0; i < n; i++)
        scanf("%d", &keys[i]);

    printf("Enter frequencies:\n");
    for (int i = 0; i < n; i++)
        scanf("%d", &freq[i]);

    int result = optimalBST(keys, freq, n);

    printf("\nMinimum Cost = %d\n", result);

    return 0;
}
```
# OUTPUT
<img width="253" height="115" alt="image" src="https://github.com/user-attachments/assets/aa32b12a-dd54-42a4-bc54-9fcf768cbbe5" />

