## PROGRAM
``` C
#include <stdio.h>
#include <limits.h>

#define V 4

int minDistance(int dist[], int visited[])
{
    int min = INT_MAX, min_index = -1;

    for (int v = 0; v < V; v++)
    {
        if (!visited[v] && dist[v] < min)
        {
            min = dist[v];
            min_index = v;
        }
    }

    return min_index;
}

void dijkstra(int graph[V][V], int src)
{
    int dist[V];
    int visited[V];

    for (int i = 0; i < V; i++)
    {
        dist[i] = INT_MAX;
        visited[i] = 0;
    }

    dist[src] = 0;

    for (int count = 0; count < V - 1; count++)
    {
        int u = minDistance(dist, visited);

        visited[u] = 1;

        for (int v = 0; v < V; v++)
        {
            if (!visited[v] && graph[u][v] &&
                dist[u] != INT_MAX &&
                dist[u] + graph[u][v] < dist[v])
            {
                dist[v] = dist[u] + graph[u][v];
            }
        }
    }

    printf("Shortest distances from source A:\n");

    for (int i = 0; i < V; i++)
    {
        printf("A -> %c = %d\n", 'A' + i, dist[i]);
    }
}

int main()
{
    int graph[V][V] = {
        {0, 4, 2, 0},
        {4, 0, 0, 5},
        {2, 0, 0, 1},
        {0, 5, 1, 0}
    };

    dijkstra(graph, 0);

    return 0;
}
```
# OUTPUT
<img width="284" height="152" alt="image" src="https://github.com/user-attachments/assets/9fcafa71-d548-4a7f-a3e8-0b152c48369e" />

