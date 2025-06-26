# s
# Daa
1A #include <stdio.h>
#include <string.h>

void search(char* pat, char* txt)
{
    int M = strlen(pat);
    int N = strlen(txt);

    for (int i = 0; i <= N - M; i++) {
        int j;
        for (j = 0; j < M; j++)
            if (txt[i + j] != pat[j])
                break;

        if (j == M) // if pat[0...M-1] = txt[i, i+1, ...i+M-1]
            printf("Pattern found at index %d \n", i);
    }
}

int main()
{
    char txt[] = "AABAACAADAABAAABAA";
    char pat[] = "AABA";
    search(pat, txt);
    return 0;
}



1B   #include <stdio.h>

int binarySearch(int arr[], int left, int right, int key) {
    while (left <= right) {
        int mid = left + (right - left) / 2;

        if (arr[mid] == key)
            return mid;

        if (arr[mid] < key)
            left = mid + 1;
        else
            right = mid - 1;
    }
    return -1;
}

int main() {
    int arr[] = {2, 5, 8, 12, 16, 23, 38, 56, 72, 91};
    int size = sizeof(arr) / sizeof(arr[0]);
    int key = 23;

    int result = binarySearch(arr, 0, size - 1, key);
    if (result == -1)
        printf("Element is not present in array.\n");
    else
        printf("Element is present at index %d.\n", result);

    return 0;
}



1Bb    #include <stdio.h>

// Recursive Binary Search Function
int binarySearch(int arr[], int left, int right, int key) {
    if (right >= left) {
        int mid = left + (right - left) / 2;

        if (arr[mid] == key)
            return mid;

        if (arr[mid] > key)
            return binarySearch(arr, left, mid - 1, key);

        return binarySearch(arr, mid + 1, right, key);
    }
    return -1;
}

int main(void) {
    int arr[] = {2, 5, 8, 12, 16, 23, 38, 56, 72, 91};
    int size = sizeof(arr) / sizeof(arr[0]);
    int key = 23;

    int index = binarySearch(arr, 0, size - 1, key);
    if (index == -1) {
        printf("Element is not present in array.\n");
    } else {
        printf("Element is present at index %d.\n", index);
    }

    return 0;
}



2 merge sort
#include <stdio.h>
#include <stdlib.h>

int count = 0;

void merge(int a[10], int left, int mid, int right) {
    int i = left, j = mid + 1, k = left;
    int b[10];

    while (i <= mid && j <= right) {
        count++;
        if (a[i] < a[j]) {
            b[k++] = a[i++];
        } else {
            b[k++] = a[j++];
        }
    }

    while (i <= mid) {
        b[k++] = a[i++];
    }

    while (j <= right) {
        b[k++] = a[j++];
    }

    for (i = left; i <= right; i++) {
        a[i] = b[i];
    }
}

void mergesort(int a[10], int left, int right) {
    int mid;
    if (left < right) {
        mid = (left + right) / 2;
        mergesort(a, left, mid);
        mergesort(a, mid + 1, right);
        merge(a, left, mid, right);
    }
}

int main() {
    int a[10], n, i;

    printf("Enter number of elements (max 10): ");
    scanf("%d", &n);

    if (n > 10 || n < 1) {
        printf("Invalid number of elements.\n");
        return 1;
    }

    printf("Enter %d elements:\n", n);
    for (i = 0; i < n; i++) {
        scanf("%d", &a[i]);
    }

    mergesort(a, 0, n - 1);

    printf("Sorted array: ");
    for (i = 0; i < n; i++) {
        printf("%d ", a[i]);
    }

    printf("\nNumber of comparisons: %d\n", count);

    return 0;
}





3 quick
#include <stdio.h>
#include <stdlib.h>

int count = 0;

int partition(int a[], int low, int high) {
    int i = low + 1, j = high, pivot = a[low], temp;

    while (1) {
        while (i <= high && pivot >= a[i]) {
            i++;
            count++;
        }
        count++;
        while (j >= low && pivot < a[j]) {
            j--;
            count++;
        }
        count++;
        if (i < j) {
            temp = a[i];
            a[i] = a[j];
            a[j] = temp;
        } else {
            temp = a[low];
            a[low] = a[j];
            a[j] = temp;
            return j;
        }
    }
}

void quicksort(int a[], int low, int high) {
    if (low < high) {
        int j = partition(a, low, high);
        quicksort(a, low, j - 1);
        quicksort(a, j + 1, high);
    }
}

int main() {
    int a[10], i, n;
    printf("Enter number of elements: ");
    scanf("%d", &n);

    printf("Enter elements:\n");
    for (i = 0; i < n; i++) {
        scanf("%d", &a[i]);
    }

    quicksort(a, 0, n - 1);

    printf("Sorted elements:\n");
    for (i = 0; i < n; i++) {
        printf("%d\t", a[i]);
    }

    printf("\nNumber of comparisons: %d\n", count);
    return 0;
}





4th dfs
#include <stdio.h>
#include <stdbool.h>

#define MAX_NODES 100

void dfs(int graph[MAX_NODES][MAX_NODES], bool visited[], int node, int num_nodes) {
    visited[node] = true;
    for (int i = 0; i < num_nodes; i++) {
        if (graph[node][i] == 1 && !visited[i]) {
            dfs(graph, visited, i, num_nodes);
        }
    }
}

bool is_connected(int graph[MAX_NODES][MAX_NODES], int num_nodes) {
    bool visited[MAX_NODES] = {false};

    dfs(graph, visited, 0, num_nodes);

    for (int i = 0; i < num_nodes; i++) {
        if (!visited[i]) {
            return false;
        }
    }

    return true;
}

int main() {
    int num_nodes, num_edges;
    printf("Enter number of nodes: ");
    scanf("%d", &num_nodes);
    printf("Enter number of edges: ");
    scanf("%d", &num_edges);

    if (num_nodes > MAX_NODES) {
        printf("Too many nodes. Maximum allowed is %d.\n", MAX_NODES);
        return 1;
    }

    int graph[MAX_NODES][MAX_NODES] = {0};

    printf("Enter the edges (node1 node2):\n");
    for (int i = 0; i < num_edges; i++) {
        int node1, node2;
        scanf("%d %d", &node1, &node2);
        if (node1 >= num_nodes || node2 >= num_nodes || node1 < 0 || node2 < 0) {
            printf("Invalid edge (%d, %d). Node values must be between 0 and %d.\n", node1, node2, num_nodes - 1);
            i--; // reattempt this edge
            continue;
        }
        graph[node1][node2] = 1;
        graph[node2][node1] = 1; // For undirected graph
    }

    if (is_connected(graph, num_nodes)) {
        printf("The graph is connected.\n");
    } else {
        printf("The graph is not connected.\n");
    }

    return 0;
}






5th topological
#include <stdio.h>
#define MAX 10

int adj[MAX][MAX];
int visited[MAX];
int stack[MAX];
int top = -1;

void push(int v) {
    stack[++top] = v;
}

void dfs(int v, int n) {
    visited[v] = 1;
    for (int i = 0; i < n; i++) {
        if (adj[v][i] == 1 && visited[i] == 0) {
            dfs(i, n);
        }
    }
    push(v);
}

void topologicalSort(int n) {
    for (int i = 0; i < n; i++) {
        visited[i] = 0;
    }

    for (int i = 0; i < n; i++) {
        if (visited[i] == 0) {
            dfs(i, n);
        }
    }

    printf("Topological Order: ");
    while (top >= 0) {
        printf("%d ", stack[top--]);
    }
    printf("\n");
}

int main() {
    int n, edges, u, v;

    printf("Enter the number of vertices: ");
    scanf("%d", &n);

    printf("Enter the number of edges: ");
    scanf("%d", &edges);

    // Initialize adjacency matrix
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            adj[i][j] = 0;
        }
    }

    printf("Enter edges (format: src dest):\n");
    for (int i = 0; i < edges; i++) {
        scanf("%d %d", &u, &v);
        adj[u][v] = 1; // Directed edge from u to v
    }

    topologicalSort(n);
    return 0;
}




6th warshall
#include <stdio.h>

void warsh(int p[][10], int n) {
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                p[i][j] = p[i][j] || (p[i][k] && p[k][j]);
            }
        }
    }
}

int main() {
    int a[10][10], n;

    printf("Enter the number of vertices: ");
    scanf("%d", &n);

    printf("Enter the adjacency matrix:\n");
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            scanf("%d", &a[i][j]);
        }
    }

    warsh(a, n);

    printf("Resultant transitive closure matrix:\n");
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            printf("%d ", a[i][j]);
        }
        printf("\n");
    }

    return 0;
}



7 horspool
#include <stdio.h>
#include <string.h>
#define MAX 256

void createShiftTable(char *pattern, int patternLength, int shiftTable[]) {
    for (int i = 0; i < MAX; i++) {
        shiftTable[i] = patternLength;
    }
    for (int i = 0; i < patternLength - 1; i++) {
        shiftTable[(unsigned char)pattern[i]] = patternLength - 1 - i;
    }
}

int horspoolMatching(char *text, char *pattern) {
    int textLength = strlen(text);
    int patternLength = strlen(pattern);
    int shiftTable[MAX];

    createShiftTable(pattern, patternLength, shiftTable);

    int i = patternLength - 1;

    while (i < textLength) {
        int k = 0;
        while (k < patternLength && pattern[patternLength - 1 - k] == text[i - k]) {
            k++;
        }
        if (k == patternLength) {
            return i - patternLength + 1;
        } else {
            i += shiftTable[(unsigned char)text[i]];
        }
    }
    return -1; // Should be outside the while loop
}

int main() {
    char text[100], pattern[50];

    printf("Enter the text: ");
    fgets(text, sizeof(text), stdin);
    text[strcspn(text, "\n")] = '\0';

    printf("Enter the pattern: ");
    fgets(pattern, sizeof(pattern), stdin);
    pattern[strcspn(pattern, "\n")] = '\0';

    int position = horspoolMatching(text, pattern);
    if (position != -1) {
        printf("Pattern found at position: %d\n", position);
    } else {
        printf("Pattern not found in the text.\n");
    }
    return 0;
}



8 floyds
#include <stdio.h>
#define INF 999
#define MAX 10

int min(int a, int b) {
    return (a < b) ? a : b;
}

void floydWarshall(int n, int d[MAX][MAX]) {
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
            }
        }
    }
}

int main() {
    int n, a[MAX][MAX];

    printf("Enter the number of nodes: ");
    scanf("%d", &n);

    printf("Enter the adjacency matrix (use %d for INF):\n", INF);
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            scanf("%d", &a[i][j]);
        }
    }

    floydWarshall(n, a);

    printf("\nAll pairs shortest path matrix:\n");
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (a[i][j] == INF)
                printf("INF ");
            else
                printf("%3d ", a[i][j]);
        }
        printf("\n");
    }

    return 0;
}



9 knapsack
#include <stdio.h>

int w[10], p[10], n;

int max(int a, int b) {
    return a > b ? a : b;
}

int knap(int i, int m) {
    if (i == n) return 0;
    if (w[i] > m)
        return knap(i + 1, m);
    return max(knap(i + 1, m), knap(i + 1, m - w[i]) + p[i]);
}

int main() {
    int m, i, max_profit;
    printf("Enter the number of objects: ");
    scanf("%d", &n);
    printf("Enter the knapsack capacity: ");
    scanf("%d", &m);
    printf("Enter profit followed by weight:\n");
    for (i = 0; i < n; i++)
        scanf("%d %d", &p[i], &w[i]);
    max_profit = knap(0, m);
    printf("Max profit = %d\n", max_profit);
    return 0;
}




10 kruskals
#include <stdio.h>
#define INFINITY 999
#define MAX 20

struct EDGE {
    int x, y, wt;
} e[MAX * MAX];

int parent[MAX], cost[MAX][MAX], t[MAX][2];
int nedges = 0;

void sort_edges() {
    struct EDGE temp;
    for (int i = 0; i < nedges - 1; i++) {
        for (int j = 0; j < nedges - 1 - i; j++) {
            if (e[j].wt > e[j + 1].wt) {
                temp = e[j];
                e[j] = e[j + 1];
                e[j + 1] = temp;
            }
        }
    }
}

int get_parent(int v) {
    while (parent[v])
        v = parent[v];
    return v;
}

void join(int i, int j) {
    parent[j] = i;
}

void kruskal(int n) {
    int i, j, k = 0, sum = 0;
    for (int idx = 0; idx < nedges; idx++) {
        int u = e[idx].x;
        int v = e[idx].y;
        int pu = get_parent(u);
        int pv = get_parent(v);
        if (pu != pv) {
            join(pu, pv);
            t[k][0] = u;
            t[k][1] = v;
            sum += e[idx].wt;
            k++;
            if (k == n - 1) break;  // Stop if MST is complete
        }
    }

    printf("\nCost of the spanning tree is: %d\n", sum);
    printf("The edges of the spanning tree are:\n");
    for (i = 0; i < n - 1; i++)
        printf("%d -> %d\n", t[i][0], t[i][1]);
}

int main() {
    int i, j, n;
    printf("Enter the number of vertices: ");
    scanf("%d", &n);

    for (i = 0; i < n; i++)
        parent[i] = 0;

    printf("Enter the cost adjacency matrix (0=self loop, 999=no edge):\n");
    for (i = 0; i < n; i++) {
        for (j = 0; j < n; j++) {
            scanf("%d", &cost[i][j]);
            if (i < j && cost[i][j] != 0 && cost[i][j] != INFINITY) {
                e[nedges].x = i;
                e[nedges].y = j;
                e[nedges].wt = cost[i][j];
                nedges++;
            }
        }
    }

    sort_edges();
    kruskal(n);
    return 0;
}




10b prims
#include <stdio.h>
#define MAX 10
#define INF 999

void prims(int cost[MAX][MAX], int n) {
    int visited[MAX] = {0}, mincost = 0;
    int i, j, u = 0, v = 0, min, a, b, ne = 1;

    visited[1] = 1; // Start from node 1

    printf("\nThe edges of the Minimum Spanning Tree are:\n");

    while (ne < n) {
        min = INF;

        for (i = 1; i <= n; i++) {
            if (visited[i]) {
                for (j = 1; j <= n; j++) {
                    if (!visited[j] && cost[i][j] < min) {
                        min = cost[i][j];
                        a = u = i;
                        b = v = j;
                    }
                }
            }
        }

        if (!visited[v]) {
            printf("%d) Edge (%d,%d) = %d\n", ne++, a, b, min);
            mincost += min;
            visited[v] = 1;
        }

        cost[a][b] = cost[b][a] = INF;
    }

    printf("\nMinimum total cost = %d\n", mincost);
}

int main() {
    int cost[MAX][MAX], n, i, j;

    printf("Enter number of nodes: ");
    scanf("%d", &n);

    printf("Enter cost adjacency matrix:\n");
    for (i = 1; i <= n; i++)
        for (j = 1; j <= n; j++) {
            scanf("%d", &cost[i][j]);
            if (cost[i][j] == 0)
                cost[i][j] = INF;
        }

    prims(cost, n);
    return 0;
}
   





11  Dijkstra
#include <stdio.h>
#define INF 999
#define MAX 10

void dijkstra(int cost[MAX][MAX], int n, int source, int v[MAX], int d[MAX]) {
    int i, j, u, min;

    v[source] = 1;

    for (int count = 1; count < n; count++) {
        min = INF;
        for (i = 1; i <= n; i++) {
            if (!v[i] && d[i] < min) {
                min = d[i];
                u = i;
            }
        }

        v[u] = 1;

        for (j = 1; j <= n; j++) {
            if (!v[j] && d[u] + cost[u][j] < d[j])
                d[j] = d[u] + cost[u][j];
        }
    }
}

int main() {
    int cost[MAX][MAX], d[MAX], v[MAX];
    int i, j, n, sourceNode;

    printf("Enter number of vertices: ");
    scanf("%d", &n);

    printf("Enter Cost Adjacency matrix:\n");
    for (i = 1; i <= n; i++)
        for (j = 1; j <= n; j++) {
            scanf("%d", &cost[i][j]);
            if (cost[i][j] == 0 && i != j)
                cost[i][j] = INF;
        }

    printf("Enter the source (1 to %d): ", n);
    scanf("%d", &sourceNode);

    for (i = 1; i <= n; i++) {
        d[i] = cost[sourceNode][i];
        v[i] = 0;
    }
    d[sourceNode] = 0;
    v[sourceNode] = 1;

    dijkstra(cost, n, sourceNode, v, d);

    printf("\nShortest distances from node %d:\n", sourceNode);
    for (i = 1; i <= n; i++) {
        if (i != sourceNode)
            printf("%d -> %d = %d\n", sourceNode, i, d[i]);
    }

    return 0;
}





12 n queens
#include <stdio.h>
#include <stdlib.h>

int x[10];
static int c = 1;

void printBoard(int n) {
    int i, j;
    printf("Solution %d:\n", c++);
    for (i = 1; i <= n; i++) {
        for (j = 1; j <= n; j++) {
            if (x[i] == j)
                printf("Q\t");
            else
                printf("-\t");
        }
        printf("\n");
    }
}

int place(int k, int i) {
    int j;
    for (j = 1; j < k; j++) {
        if ((x[j] == i) || abs(x[j] - i) == abs(j - k))
            return 0;  // Not safe to place
    }
    return 1; // Safe to place
}

void nQueens(int k, int n) {
    int i;
    for (i = 1; i <= n; i++) {
        if (place(k, i)) {
            x[k] = i;
            if (k == n) {
                printf("\n");
                printBoard(n);
            } else {
                nQueens(k + 1, n);
            }
        }
    }
}

int main() {
    int n;
    printf("Enter the number of queens:\n");
    scanf("%d", &n);
    nQueens(1, n);
    if (c == 1)
        printf("No solutions!\n");
    return 0;
}
