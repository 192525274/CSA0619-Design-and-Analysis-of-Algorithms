## PROGRAM
``` C
#include <stdio.h>
#include <string.h>

#define N 6

char guests[N][20] = {
    "Alice", "Bob", "Carol", "David", "Eve", "Frank"
};

int seated[N] = {0};
int arrangement[N];
int count = 0;

/* Check whether two guests must sit together */
int mustSitTogether(int a, int b) {
    if ((a == 0 && b == 1) || (a == 1 && b == 0))
        return 1;

    if ((a == 2 && b == 3) || (a == 3 && b == 2))
        return 1;

    return 0;
}

/* Check whether two guests cannot sit together */
int cannotSitTogether(int a, int b) {
    if ((a == 4 && b == 5) || (a == 5 && b == 4))
        return 1;

    if ((a == 1 && b == 4) || (a == 4 && b == 1))
        return 1;

    return 0;
}

/* Check whether the guest can be placed */
int isSafe(int guest, int position) {
    int left;

    if (position == 0)
        return 1;

    left = arrangement[position - 1];

    /* Forbidden adjacency */
    if (cannotSitTogether(guest, left))
        return 0;

    return 1;
}

/* Check all constraints after completing the arrangement */
int checkFinalArrangement() {
    int i, left, right;

    /* Check circular adjacency */
    for (i = 0; i < N; i++) {
        left = arrangement[i];
        right = arrangement[(i + 1) % N];

        if (cannotSitTogether(left, right))
            return 0;
    }

    /* Required adjacency: Alice-Bob */
    int aliceBob = 0;
    for (i = 0; i < N; i++) {
        left = arrangement[i];
        right = arrangement[(i + 1) % N];

        if (mustSitTogether(left, right)) {
            if ((left == 0 && right == 1) ||
                (left == 1 && right == 0))
                aliceBob = 1;
        }
    }

    /* Required adjacency: Carol-David */
    int carolDavid = 0;
    for (i = 0; i < N; i++) {
        left = arrangement[i];
        right = arrangement[(i + 1) % N];

        if ((left == 2 && right == 3) ||
            (left == 3 && right == 2))
            carolDavid = 1;
    }

    if (!aliceBob || !carolDavid)
        return 0;

    return 1;
}

/* Backtracking function */
void backtrack(int position) {
    int guest, i;

    if (position == N) {
        if (checkFinalArrangement()) {
            count++;

            printf("\nValid Seating Plan %d:\n", count);

            for (i = 0; i < N; i++) {
                printf("%s", guests[arrangement[i]]);

                if (i != N - 1)
                    printf(" -> ");
            }

            printf(" -> %s", guests[arrangement[0]]);
            printf("\n");
        }

        return;
    }

    for (guest = 0; guest < N; guest++) {

        /* Guest should not already be seated */
        if (seated[guest] == 0) {

            /* Check constraints */
            if (isSafe(guest, position)) {

                arrangement[position] = guest;
                seated[guest] = 1;

                backtrack(position + 1);

                /* Backtrack */
                seated[guest] = 0;
            }
        }
    }
}

int main() {

    /*
       Fix Alice at the first position.
       This removes duplicate rotations of the same
       circular arrangement.
    */
    arrangement[0] = 0;
    seated[0] = 1;

    printf("SEATING ARRANGEMENT USING BACKTRACKING\n");
    printf("---------------------------------------\n");

    backtrack(1);

    printf("\nTotal Valid Seating Plans = %d\n", count);

    return 0;
}
```
# OUTPUT
<img width="408" height="233" alt="image" src="https://github.com/user-attachments/assets/8573a4e1-8743-431c-b255-f94804895469" />
