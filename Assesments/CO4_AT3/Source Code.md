## PROGRAM
``` C
#include <stdio.h>

#define N 6

typedef struct {
    int start;
    int finish;
    int id;
} Activity;

/* Sort activities by finish time */
void sortActivities(Activity a[]) {
    for (int i = 0; i < N - 1; i++) {
        for (int j = i + 1; j < N; j++) {
            if (a[i].finish > a[j].finish) {
                Activity temp = a[i];
                a[i] = a[j];
                a[j] = temp;
            }
        }
    }
}

/* Greedy Approach */
void greedy(Activity a[]) {
    int count = 0;
    int lastFinish = -1;

    printf("\nGreedy Approach:\n");
    printf("Selected Activities: ");

    for (int i = 0; i < N; i++) {
        if (a[i].start >= lastFinish) {
            printf("A%d ", a[i].id);
            lastFinish = a[i].finish;
            count++;
        }
    }

    printf("\nMaximum Activities = %d\n", count);
}

/* Dynamic Programming Approach */
void dynamicProgramming(Activity a[]) {
    int dp[N];
    int parent[N];

    for (int i = 0; i < N; i++) {
        dp[i] = 1;
        parent[i] = -1;

        for (int j = 0; j < i; j++) {
            if (a[j].finish <= a[i].start &&
                dp[j] + 1 > dp[i]) {
                dp[i] = dp[j] + 1;
                parent[i] = j;
            }
        }
    }

    int max = 0;
    int last = 0;

    for (int i = 0; i < N; i++) {
        if (dp[i] > max) {
            max = dp[i];
            last = i;
        }
    }

    int selected[N];
    int k = 0;

    while (last != -1) {
        selected[k++] = last;
        last = parent[last];
    }

    printf("\nDynamic Programming Approach:\n");
    printf("Selected Activities: ");

    for (int i = k - 1; i >= 0; i--) {
        printf("A%d ", a[selected[i]].id);
    }

    printf("\nMaximum Activities = %d\n", max);
}

int main() {
    Activity activities[N] = {
        {1, 2, 1},
        {3, 4, 2},
        {0, 6, 3},
        {5, 7, 4},
        {8, 9, 5},
        {5, 9, 6}
    };

    sortActivities(activities);

    greedy(activities);
    dynamicProgramming(activities);

    return 0;
}
```
# OUTPUT
<img width="337" height="208" alt="image" src="https://github.com/user-attachments/assets/92e5cb91-d06e-41f7-aa53-46c65a3c6941" />
