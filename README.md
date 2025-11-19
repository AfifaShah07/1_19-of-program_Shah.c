#include <stdio.h>
#include <string.h>

#define MAX 100

int graph[MAX][MAX];
int visited[MAX], disc[MAX], low[MAX], parent[MAX];
int timeDFS = 0;
int articulation_found = 0;
int n; // number of vertices

void DFS(int u) {
    visited[u] = 1;
    disc[u] = low[u] = ++timeDFS;

    int children = 0;

    for (int v = 0; v < n; v++) {
        if (graph[u][v]) {

            if (!visited[v]) {
                parent[v] = u;
                children++;

                DFS(v);

                low[u] = (low[u] < low[v]) ? low[u] : low[v];

                // articulation point check
                if ((parent[u] == -1 && children > 1) ||
                    (parent[u] != -1 && low[v] >= disc[u])) {
                    articulation_found = 1;
                }
            }
            else if (v != parent[u]) {
                low[u] = (low[u] < disc[v]) ? low[u] : disc[v];
            }
        }
    }
}

int main() {
    printf("Enter number of vertices: ");
    scanf("%d", &n);

    printf("Enter adjacency matrix:\n");
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            scanf("%d", &graph[i][j]);

    // Initialize
    memset(visited, 0, sizeof(visited));
    memset(parent, -1, sizeof(parent));

    // Run DFS from node 0
    DFS(0);

    // Check if fully connected
    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            printf("Graph is NOT Bi-Connected (Not fully connected)\n");
            return 0;
        }
    }

    // Check articulation point
    if (articulation_found)
        printf("Graph is NOT Bi-Connected (Articulation point exists)\n");
    else
        printf("Graph IS Bi-Connected\n");

    return 0;
}# 1_19-of-program_Shah.c
Detecting whether graph is bi-connected or not
