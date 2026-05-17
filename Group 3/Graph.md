# ***Graph Theory***

- Graph Representation
- Connected Component
- BFS
- DFS
___
- Number of provinces
- Connected Components Problem in Matrix
- Rotten Oranges
- Flood fill algorithm
- Cycle Detection in Undirected Graph (bfs)
- Detect a cycle in an undirected graph
- Distance of nearest cell having one
- Surrounded Regions
- Number of enclaves
- Word ladder I
- Word ladder II
- Number of islands
- Bipartite Graph (DFS)
- Cycle Detection in Directed Graph (DFS)
___
- Topological Sort Algorithm
- Kahn's Algorithm
- Detect a cycle in directed graph
- Course Schedule 1 and 2
- Find eventual safe state
- Alien Dictionary
___
- Shortest path in Directed Acyclic Graph
- Shortest Path in Undirected Graph with unit weights
- Dijkstra Algorithm
- Bellman Ford Algorithm
- Floyd Warshall Algorithm
___
- Shortest Distance in a Binary Maze
- Path with minimum effort
- Cheapest Flight within K stops
- Network Delay Time
- Number of ways to Arrive at Destination
- Minimum Multiplication to Reach End
- Find the City with the smallest number of neighbour at a threshold distance
___
- MST Theory
- Prim's Algorithm
- Disjoint Set | Union by Rank | Union by size | Path Compression
- Number of Operations to Make Network Connected
- Most Stones Removed with Same Row or Column
- Account Merge
- Number of Islands 2
- Making a Large Island
- Swim in Rising Water
___
- Bridges in Graph
- Articulation point in graph
- Kosaraju's Algorithm | Strongly Connected Components
___

## ***Part 1: Learning***

### ***Graph Representation***

- ```cpp
    // ADJACENCY MATRIX
    #include <iostream>
    using namespace std;

    int main()
    {
        int n, m;
        cin >> n >> m;
        // adjacency matrix for undirected graph
        // time complexity: O(n)
        int adj[n+1][n+1];
        for(int i = 0; i < m; i++)
        {
            int u, v;
            cin >> u >> v;
            adj[u][v] = 1;
            adj[v][u] = 1  // this statement will be removed in case of directed graph
        }
        return 0;
    }
    ```

- ```cpp
    // ADJACENCY LIST
    #include <iostream>
    using namespace std;

    int main()
    {
        int n, m;
        cin >> n >> m;
        // adjacency list for undirected graph
        // time complexity: O(2E)
        vector<int> adj[n+1]; // vector< pair <int,int> > adjList[n+1]; If edge weights
        for(int i = 0; i < m; i++)
        {
            int u, v;
            cin >> u >> v;
            adj[u].push_back(v);
            adj[v].push_back(u); // this statement will be removed in case of directed graph. time complexity: O(E)
        }
        return 0;
    }
    ```

### ***Connected Component***

- We say two vertices u and v belong to a same component if there is a path from u to v or v to u. Find the number of connected components in the graph.

- ```cpp
    int countComponents(int V, vector<vector<int>>& edges) {
        
        vector<vector<int>> adj(V);
        for (auto &e : edges) {
            adj[e[0]].push_back(e[1]);
            adj[e[1]].push_back(e[0]);
        }

        vector<int> visited(V, 0);
        int components = 0;

        for (int i = 0; i < V; ++i) {

            if (!visited[i]) {
                components++;

                queue<int> q;
                q.push(i);
                visited[i] = 1;

                while (!q.empty()) {
                    int node = q.front();
                    q.pop();

                    // Visit all unvisited neighbors
                    for (auto &nbr : adj[node]) {
                        if (!visited[nbr]) {
                            visited[nbr] = 1;
                            q.push(nbr);
                        }
                    }
                }
            }
        }
        return components;
    }
    ```

### ***BFS***

- bfs_matrix — neighbor lookup is O(V) per node → overall O(V²).
- bfs_list — neighbor lookup is O(degree) per node → overall O(V+E).

- ```cpp
    #include <bits/stdc++.h>
    using namespace std;

    vector<int> bfs_matrix(int V, vector<vector<int>>& edges, int src) {
        vector<vector<int>> adj(V, vector<int>(V, 0));
        for (auto& e : edges) {
            adj[e[0]][e[1]] = 1;
            adj[e[1]][e[0]] = 1;
        }

        vector<int> vis(V, 0), result;
        queue<int> q;
        q.push(src);
        vis[src] = 1;

        while (!q.empty()) {
            int node = q.front(); q.pop();
            result.push_back(node);
            for (int i = 0; i < V; i++) {
                if (adj[node][i] && !vis[i]) {
                    vis[i] = 1;
                    q.push(i);
                }
            }
        }
        return result;
    }

    vector<int> bfs_list(int V, vector<vector<int>>& edges, int src) {
        vector<vector<int>> adj(V);
        for (auto& e : edges) {
            adj[e[0]].push_back(e[1]);
            adj[e[1]].push_back(e[0]);
        }

        vector<int> vis(V, 0), result;
        queue<int> q;
        q.push(src);
        vis[src] = 1;

        while (!q.empty()) {
            int node = q.front(); q.pop();
            result.push_back(node);
            for (int nbr : adj[node]) {
                if (!vis[nbr]) {
                    vis[nbr] = 1;
                    q.push(nbr);
                }
            }
        }
        return result;
    }
    ```

### ***DFS***

- Same split as BFS — dfs_matrix is O(V²), dfs_list is O(V+E). The recursive solve lambda captures vis and result by reference, starts from src.

- ```cpp
    void dfs_helper_matrix(int node, vector<vector<int>>& adj, vector<int>& vis, vector<int>& result) {
        vis[node] = 1;
        result.push_back(node);
        int V = adj.size();
        for (int i = 0; i < V; i++) {
            if (adj[node][i] && !vis[i]) dfs_helper_matrix(i, adj, vis, result);
        }
    }

    vector<int> dfs_matrix(int V, vector<vector<int>>& edges, int src) {
        vector<vector<int>> adj(V, vector<int>(V, 0));
        for (auto& e : edges) {
            adj[e[0]][e[1]] = 1;
            adj[e[1]][e[0]] = 1;
        }
        vector<int> vis(V, 0), result;
        dfs_helper_matrix(src, adj, vis, result);
        return result;
    }

    void dfs_helper_list(int node, vector<vector<int>>& adj, vector<int>& vis, vector<int>& result) {
        vis[node] = 1;
        result.push_back(node);
        for (int nbr : adj[node]) {
            if (!vis[nbr]) dfs_helper_list(nbr, adj, vis, result);
        }
    }

    vector<int> dfs_list(int V, vector<vector<int>>& edges, int src) {
        vector<vector<int>> adj(V);
        for (auto& e : edges) {
            adj[e[0]].push_back(e[1]);
            adj[e[1]].push_back(e[0]);
        }
        vector<int> vis(V, 0), result;
        dfs_helper_list(src, adj, vis, result);
        return result;
    }
    ```

## ***Part 2: Problems of BFS/DFS

## ***Part 3: Topo Sort and Problems***

### ***Topological Sort Algorithm***

- Applies on Directed Acyclic Graph

- U -> V means U will always come before V in sorting

- **My past memory:** Keep track of outgoing and incoming arrow to the node. AND AND AND after thinking and visualising I got to know, keep track of incoming arrow to a node. Print node which has zero incoming node and then remove arrow which means subtracting incoming arrow count of connected node to this node. Again check for all nodes print which has zero incoming node and so on. What I just thought is solving this by BFS which is also called Kahn's Algorithm. There is DFS solution too.

- ```cpp
    // Using BFS (Kahn's Algorithm)
    vector<int> topologicalSort(int V, vector<int> adj[]) {

        vector<int> indegree(V, 0);
        for (int i = 0; i < V; i++) {
            for (auto it : adj[i]) {
                indegree[it]++;
            }
        }
        
        queue<int> q;
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) { q.push(i); }
        }
        
        vector<int> topo;
        while (!q.empty()) {
            int node = q.front(); q.pop();
            topo.push_back(node);
            
            for (auto it : adj[node]) {
                indegree[it]--;
                if (indegree[it] == 0) { q.push(it); }
            }
        }
        
        return topo;
    }
    ```

- ```cpp
    // Using DFS
    // NEED REVISION - Dry run makes sense but no intuition that why stack holds my answer. It makes sense now. Imagine a DAG like a linkedlist. And see what's happening.
    void dfs(int node, vector<int> adj[], vector<int>& vis, stack<int>& st) {
        vis[node] = 1;
        for (auto it : adj[node]) {
            if (!vis[it]) { dfs(it, adj, vis, st); }
        }
        st.push(node);
    }

    // Function to perform Topological Sort
    vector<int> topoSort(int V, vector<int> adj[]) {
        vector<int> vis(V, 0);

        stack<int> st; // Stack to store vertices in finishing order
        for (int i = 0; i < V; i++) {
            if (!vis[i]) { dfs(i, adj, vis, st); }
        }

        vector<int> ans;
        while (!st.empty()) {
            ans.push_back(st.top());
            st.pop();
        }

        return ans;
    }
    ```

### ***Kahn's Algorithm***

- Topo Sort by BFS as above is Kahn's Algorithm.

### ***Detect a cycle in directed graph***

- Given a Directed Graph with V vertices and E edges, check whether it contains any cycle or not using BFS.

- Obviously here we are discussing using Topo Sort approach. Simple if processed node != total nodes V then cycle exists. How to track count of processed node? Just add a int count = 0 before while q is not empty loop start and do count++ after q.pop done.

### ***Course Schedule 1 and 2***

- There are a total of N tasks, labeled from 0 to N-1. Some tasks may have prerequisites, for example, to do task 0 you have to first complete task 1, which is expressed as a pair: [0, 1]. Given the total number of tasks N and a list of prerequisite pairs P, find if it is possible to finish all tasks.

- There are a total of n tasks you have to pick, labeled from 0 to n-1. Some tasks may have prerequisites tasks, for example, to pick task 0 you have to first finish tasks 1, which is expressed as a pair: [0, 1]. Given the total number of n tasks and a list of prerequisite pairs of size m. Find the order of tasks you should pick to finish all tasks. Note: There may be multiple correct orders, you need to return one of them. If it is impossible to finish all tasks, return an empty array.

- Note: These two questions are linked. The first question asks if it is possible to finish all the tasks and the second question states to return the ordering of the tasks if it is possible to perform all the tasks, otherwise return an empty array.

- **1:** We just have to return true or false possible or not similar to detecting cycle.

- **2:** Is finding actual order if true. So just do usual Kahn Algo. Track the count push your node to answer and if processed node count total node count then return that answer vector. Wait wait wait, you don't have to keep track of count. Just check with ans.size() == total nodes or not.

### ***Find eventual safe state***

- A directed graph of V vertices and E edges is given in the form of an adjacency list adj. Each node of the graph is labeled with a distinct integer in the range 0 to V - 1. A node is a terminal node if there are no outgoing edges. A node is a SAFE NODE if every possible path starting from that node leads to a terminal node. You have to return an array containing all the safe nodes of the graph. The answer should be sorted in ascending order.

- If you look at the quesion, you will realize very easily that all we have to find out is given a DAG graph, which are the nodes which are NOT a part of any cycle as well as NOT have paths that leads to cycle (basically pointing to nodes of cycle.) So how to calculate this ?

- Simple Logic: Terminal Node have outDegree zero. So reverse the edges to align with TopoSort. So now terminal nodes has indegree zero. Make inDegree array and a queue and keep processing nodes like you do in topo sort by decreasing inOrder and pushing to queue if zero inOrder. HERE, intution is that the nodes of cycle will never have inOrder zero and other nodes which were pointing to any node of this cycle will never reach in our queue because now edge will be from nodes in cycle to that node and nodes in cycle are never going to queue because it will never have zero inorder degree.

- ```cpp
    vector<int> eventualSafeNodes(int V, vector<int> adj[]) {
        vector<int> adjRev[V];
        int indegree[V] = {0};

        for (int i = 0; i < V; i++) {
            for (auto it : adj[i]) {
                adjRev[it].push_back(i);
                indegree[i]++;
            }
        }

        queue<int> q;
        vector<int> safeNodes;
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) { q.push(i); }
        }

        while (!q.empty()) {
            int node = q.front(); q.pop();
            safeNodes.push_back(node);

            for (auto it : adjRev[node]) {
                indegree[it]--;
                if (indegree[it] == 0) { q.push(it); }
            }
        }

        sort(safeNodes.begin(), safeNodes.end());
        return safeNodes;
    }
    ```

### ***Alien Dictionary***

- Given array of strings which represent sorted listed of words from alien dictionary. Determine order of character and return it as string. Intuition came that we have to use topological sort but how to make graph from given inputs.

- Input: N = 5, K = 4, dict = {"baa","abcd","abca","cab","cad"}, Output: b d a c, Explanation: We will analyze every consecutive pair to find out the order of the characters. The pair “baa” and “abcd” suggests ‘b’ appears before ‘a’ in the alien dictionary.The pair “abcd” and “abca” suggests ‘d’ appears before ‘a’ in the alien dictionary. The pair “abca” and “cab” suggests ‘a’ appears before ‘c’ in the alien dictionary. The pair “cab” and “cad” suggests ‘b’ appears before ‘d’ in the alien dictionary. So, [‘b’, ‘d’, ‘a’, ‘c’] is a valid ordering.

- Look at int to char conversion also. 

- ```cpp
    vector<int> topoSort(int V, vector<int> adj[]) {
        vector<int> indegree(V, 0);

        for (int i = 0; i < V; i++) {
            for (auto neighbor : adj[i]) { indegree[neighbor]++; }
        }

        queue<int> q;
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) { q.push(i); }
        }

        vector<int> topo; 
        
        while (!q.empty()) {
            int node = q.front(); q.pop();
            topo.push_back(node);

            for (auto neighbor : adj[node]) {
                indegree[neighbor]--;
                if (indegree[neighbor] == 0) { q.push(neighbor); }
            }
        }
        return topo;
    }

    string findOrder(string dict[], int N, int K) {
        vector<int> adj[K];

        // Build graph by comparing adjacent words in dictionary
        for (int i = 0; i < N - 1; i++) {
            string s1 = dict[i]; string s2 = dict[i + 1];
            int len = min(s1.size(), s2.size());

            // Find the first different character and create edge
            for (int ptr = 0; ptr < len; ptr++) {
                if (s1[ptr] != s2[ptr]) {
                    adj[s1[ptr] - 'a'].push_back(s2[ptr] - 'a');
                    break; // only the first mismatch matters
                }
            }
        }

        vector<int> topo = topoSort(K, adj);
        string ans = "";
        for (auto node : topo) { ans += char(node + 'a'); }
        return ans;
    }
    ```


## ***Part 4: Shortest Path Algorithms and Problems***

### ***Shortest path in Directed Acyclic Graph***

- What comes in mind when you visualise DAG? And edges have weight now?

- Finding the shortest path to a vertex is easy if you already know the shortest paths to all the vertices that can precede it. Processing the vertices in topological order ensures that by the time you get to a vertex, you've already processed all the vertices that can precede it which reduces the computation time significantly. In this approach, we traverse the nodes sequentially according to their reachability from the source.

- Dijkstra's algorithm is necessary for graphs that can contain cycles because they can't be topologically sorted. In other cases, the topological sort would work fine as we start from the first node, and then move on to the others in a directed manner.

- ```cpp
    void topoSort(int node, vector<pair<int, int>> adj[], int vis[], stack<int> &st) {
      vis[node] = 1;
      for (auto it : adj[node]) {
        int v = it.first;
        if (!vis[v]) { topoSort(v, adj, vis, st); }
      }
      st.push(node);
    }

    vector<int> shortestPath(int N, int M, vector<vector<int>> &edges) {
      vector<pair<int, int>> adj[N];
      for (int i = 0; i < M; i++) {
        int u = edges[i][0];
        int v = edges[i][1];
        int wt = edges[i][2];
        adj[u].push_back({v, wt});
      }

      int vis[N] = {0};
      stack<int> st; // Stack to store the topological order
      for (int i = 0; i < N; i++) {
        if (!vis[i]) { topoSort(i, adj, vis, st); }
      }

      vector<int> dist(N, 1e9);
      dist[0] = 0;

      while (!st.empty()) { // Process all nodes in topological order
        int node = st.top(); st.pop();

        for (auto it : adj[node]) { // Relax all outgoing edges from the current node
          int v = it.first;
          int wt = it.second;

          if (dist[node] + wt < dist[v]) {
            dist[v] = wt + dist[node];
          }
        }
      }


      for (int i = 0; i < N; i++) {
        if (dist[i] == 1e9) { dist[i] = -1; }
      }
      return dist;
    }
    ```

### ***Shortest Path in Undirected Graph with unit weights***

- If you imagine in mind undirected graph with cycles, you can visualise that using BFS we can find shortest path. Just maintaining a variable array distance. ***BFS because edges are unit weights.*** Focus on fact that queue of BFS is processing node closer to source node naturally. Compare with Dijkstra where we use some heap.

- ```cpp
    vector<int> shortestPath(vector<vector<int>>& edges, int N, int M, int src) {

        vector<int> adj[N]; 
        for (auto it : edges) { adj[it[0]].push_back(it[1]); adj[it[1]].push_back(it[0]); }

        int dist[N];
        for (int i = 0; i < N; i++) { dist[i] = 1e9; }

        dist[src] = 0;

        queue<int> q;
        q.push(src); 

        while (!q.empty()) {
            int node = q.front(); q.pop();
            for (auto it : adj[node]) {
                if (dist[node] + 1 < dist[it]) { // If a shorter path to neighbor is found
                    dist[it] = 1 + dist[node]; 
                    q.push(it); 
                }
            }
        }

        vector<int> ans(N, -1);

        for (int i = 0; i < N; i++) {
            if (dist[i] != 1e9) { ans[i] = dist[i]; }
        }

        return ans; 
    }
    ```

### ***Dijkstra Algorithm***

- Undirected, weighted, connected graph, No negative weight cycle. Find from the source vertex shortest distance to all other nodes.

- ```cpp
    vector<int> dijkstra(int V, vector<vector<int>> adj[], int S) {
        set<pair<int, int>> st; // nodes as {dist,node}. dist: distance from src to node.
        st.insert({0, S}); // set stores the nodes in ascending order of the distances.

        vector<int> dist(V, 1e9); // distance from source to the nodes
        dist[S] = 0;

        while(!st.empty()) {
            auto it = *(st.begin()); // Extract the node with the minimum distance
            int dis = it.first;
            int node = it.second; 
            st.erase(it);

            for(auto it : adj[node]) {
                int adjNode = it[0];
                int edgW = it[1];
                if(dis + edgW < dist[adjNode]) {
                    if(dist[adjNode] != 1e9) { st.erase({dist[adjNode], adjNode}); }
                    dist[adjNode] = dis + edgW; 
                    st.insert({dist[adjNode], adjNode}); 
                }
            }
        }
        return dist; 
    }
    ```

- See the priority queue code. Using a priority queue (min-heap) ensures that we can efficiently pick the node with the smallest current distance, instead of scanning all nodes each time.

- ```cpp
    vector<int> dijkstra(int V, vector<vector<pair<int,int>>>& adj, int src) {
        // Min-heap storing {distance, node}
        priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;
        pq.push({0, src});

        vector<int> dist(V, 1e9);
        dist[src] = 0;       

        while (!pq.empty()) {
            int dis = pq.top().first;
            int node = pq.top().second;
            pq.pop();

            if (dis > dist[node]) continue; // Skip if this distance is outdated

            for (auto it : adj[node]) {
                int adjNode = it.first;
                int edgW = it.second;
                if (dist[node] + edgW < dist[adjNode]) {
                    dist[adjNode] = dist[node] + edgW;
                    pq.push({dist[adjNode], adjNode});
                }
            }
        }
        return dist;
    }
    ```

### ***Bellman Ford Algorithm***

- Given weighted, directed or undirected and connected graph having negative edges. Find shortest distance of all vertices from source vertx S. If negative cycle in graph, return -1. There can be negative edges. Negative cycle is different.

- Dijkstra can loop forever or give incorrect result if negative edge.

- It can also detect negative cycles, a cycle where the total path weight is negative causing the distance to decrease endlessly.

- Treat undirected as two way directed graph. And it works for undirected too.

- Algorithm:
    - Works on the relaxation of edges initution. Why Repeat N-1 Times? Because the shortest path to any point can involve at most one less edge than the total number of points. If taking a certain edge gives a shorter path to a point, update that point with the new shorter distance.

    - After finishing the updates, go through all edges one more time. If any distance can still be reduced, a negative cycle exists. If not, the shortest distances are correct.

    - See that here we are iterating on edges and not on adjacency list or matrix.

    - ```cpp
        vector<int> bellman_ford(int V, vector<vector<int>>& edges, int S) {
            vector<int> dist(V, 1e8);
            dist[S] = 0;

            for (int i = 0; i < V - 1; i++){
                for (auto it : edges){
                    int u = it[0]; int v = it[1]; int wt = it[2];
                    if (dist[u] != 1e8 && dist[u] + wt < dist[v]) {dist[v] = dist[u] + wt;}
                }
            }

            for (auto it : edges){
                int u = it[0]; int v = it[1]; int wt = it[2];
                if (dist[u] != 1e8 && dist[u] + wt < dist[v]) return {-1};
            }
            return dist;
        }
        ```

### ***Floyd Warshall Algorithm***

- Lets see what the heck is this.

- Here we have to find shortest distance between every possible node. It is multisource shortest path problem.

- Ok got it. Here we are finding the shortest path from i to j going via every possible k.

- ```cpp
    void shortest_distance(vector<vector<int>> &matrix){ // Adjacency Matrix
        int n = matrix.size();
        for(int k=0; k<n; k++){ // Intermediate node k
            for(int i=0; i<n; i++){ // {i,j} pair
                for(int j=0; j<n; j++){
                    if(matrix[i][k] == -1 || matrix[k][j] == -1) continue; // Skip if not intermediate node
                    if(matrix[i][j] == -1) matrix[i][j] = matrix[i][k] + matrix[k][j]; // If no direct edge from i to j is present
                    else matrix[i][j] = min(matrix[i][j], matrix[i][k] + matrix[k][j]);
                }
            }
        }
    }
    ```

- How to detect negative cycle ?
    - Just check the matrix[i][i], if it's less than 0, it has negative cycle because this algo will update those value.

- We can apply Dijkstra on every node for this solution only when graph does not have negative cycle, otherwise it will be TLE. And in that case Time Complexity will be V*ElogV. Here it is N^3 in Floyd Warshall.

### ***Shortest Distance in a Binary Maze***

- A maze is there filled with 0 and 1 where 1 means path and 0 means blocked. I just have to find the shortest distance from maze[i][j] to maze[a][b].

- I thought of solution correctly. Treat it like undirected graph with unit weight question and apply bfs to get the answer. up down left right is the graph connection.

- Let's see the code because its implementation will be complex.

- ```cpp
    pair<int, int> source = {0, 1};
    pair<int, int> destination = {2, 2};
    vector<vector<int>> grid = {{1, 1, 1, 1},
                                {1, 1, 0, 1},
                                {1, 1, 1, 1},
                                {1, 1, 0, 0},
                                {1, 0, 0, 1}};
    int res = shortestPath(grid, source, destination);

    int shortestPath(vector<vector<int>> &grid, pair<int, int> source, pair<int, int> destination){
        if (source.first == destination.first && source.second == destination.second) return 0;

        int n = grid.size(); int m = grid[0].size();

        vector<vector<int>> dist(n, vector<int>(m, 1e9));
        dist[source.first][source.second] = 0;

        queue<pair<int, pair<int, int>>> q;
        q.push({0, {source.first, source.second}}); 

        int dr[] = {-1, 0, 1, 0};
        int dc[] = {0, 1, 0, -1};

        while (!q.empty()) {
            auto it = q.front(); q.pop();
            int dis = it.first; int r = it.second.first; int c = it.second.second;

            for (int i = 0; i < 4; i++) {
                int newr = r + dr[i]; int newc = c + dc[i];

                if (newr >= 0 && newr < n && newc >= 0 && newc < m && grid[newr][newc] == 1 && dis + 1 < dist[newr][newc]) {
                    dist[newr][newc] = 1 + dis;
                    if (newr == destination.first && newc == destination.second) return dis + 1;
                    q.push({1 + dis, {newr, newc}});
                }
            }
        }
        return -1;
    }
    ```

### ***Path with minimum effort***

- Similar to above question. But it's not binary maze. It has value which means similar to above implementation but will be using Dijkstra here.

- You are a hiker preparing for an upcoming hike. You are given heights, a 2D array of size rows x columns, where heights[row][col] represents the height of the cell (row, col). You are situated in the top-left cell, (0, 0), and you hope to travel to the bottom-right cell, (rows-1, columns-1) (i.e.,0-indexed). You can move up, down, left, or right, and you wish to find a route that requires the minimum effort. A route's effort is the maximum absolute difference in heights between two consecutive cells of the route.

- Code this one by yourself, because this is too tricky to code.

- ```cpp
    int MinimumEffort(vector<vector<int>> &heights) {

        int n = heights.size();  // Number of rows
        int m = heights[0].size();  // Number of columns

        // Create a priority queue to store the cells and their respective distance from the source
        priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, greater<pair<int, pair<int, int>>>> pq;
        pq.push({0, {0, 0}});  // Push source cell to the priority queue

        // Create a distance matrix, initialized with a large value (unvisited)
        vector<vector<int>> dist(n, vector<int>(m, 1e9));
        dist[0][0] = 0;  // Distance for the source cell (0, 0) is 0

        // Define the possible directions (up, right, down, left)
        int dr[] = {-1, 0, 1, 0};
        int dc[] = {0, 1, 0, -1};

        // Start the Dijkstra algorithm
        while (!pq.empty()) {
            auto it = pq.top(); pq.pop();  // Get the cell with the minimum distance
            
            int diff = it.first;  // The current effort
            int row = it.second.first;
            int col = it.second.second;

            // If we reach the destination cell, return the current effort
            if (row == n - 1 && col == m - 1) return diff;

            // Check all 4 possible adjacent cells
            for (int i = 0; i < 4; i++) {
                int newr = row + dr[i];
                int newc = col + dc[i];

                // Check if the new cell is within bounds
                if (newr >= 0 && newc >= 0 && newr < n && newc < m) {
                    // Calculate the effort required to move to the new cell
                    int newEffort = max(abs(heights[row][col] - heights[newr][newc]), diff);

                    // If the calculated effort is less, update and push to the queue
                    if (newEffort < dist[newr][newc]) {
                        dist[newr][newc] = newEffort;
                        pq.push({newEffort, {newr, newc}});
                    }
                }
            }
        }
        return 0;  // If unreachable (although it should not reach here)
    }
    ```

### ***Cheapest Flight within K stops***

- Given directed graph can be cyclic. Flight[i]=[from,to,price]. Reach from node A to node B (given) as cheap as possible. Edges have weight. You have k stops only.

- First I have to decide whether I can reach Node B from Node A within K stops. So if I do BFS from Node A. Track the level by maintaining a variable which level is being processed. If level become greater than K, return -1. If got to B first return the total edges weight till that point. I am thinking of Dijkstra with that variable count of which level processing. Lets see the solution.

-  ```cpp
    int n = 4, src = 0, dst = 3, K = 1;
    vector<vector<int>> flights = {{0, 1, 100}, {1, 2, 100}, {2, 0, 100}, {1, 3, 600}, {2, 3, 200}};
    int ans = CheapestFLight(n, flights, src, dst, K);

    int CheapestFLight(int n, vector<vector<int>> &flights,int src, int dst, int K){
        vector<pair<int, int>> adj[n];
        for (auto it : flights) adj[it[0]].push_back({it[1], it[2]});


        queue<pair<int, pair<int, int>>> q;
        q.push({0, {src, 0}});

        vector<int> dist(n, 1e9);
        dist[src] = 0;

        while (!q.empty()){
            auto it = q.front(); q.pop();
            int stops = it.first; int node = it.second.first; int cost = it.second.second;
            
            if (stops > K) continue;

            for (auto iter : adj[node]){
                int adjNode = iter.first; int edW = iter.second; 

                if (cost + edW < dist[adjNode] && stops <= K){
                    dist[adjNode] = cost + edW;
                    q.push({stops + 1, {adjNode, cost + edW}});
                }
            }
        }

        if (dist[dst] == 1e9) return -1;
        return dist[dst];
    }
    ```

- Unlike Dijkstra, here normal queue is used. Why ?

- Because this is not Dijkstra — it's a modified BFS where the constraint is stops (levels), not minimum distance. With Dijkstra (min-heap), you process the cheapest path first. But the cheapest path might use more stops. Once you mark a node as visited/settled via the cheapest route, you'll never revisit it through a path with fewer stops — which could be the only valid path under the K-stop limit. A normal queue processes level by level (stops = 0, then 1, then 2...). At each level you relax edges. When stops > K, you stop. This ensures you never exceed K stops, and you still find the cheapest cost within that constraint because you relax dist[] at every level. The stop count acts like BFS levels, so a plain queue naturally processes stop-0 paths, then stop-1 paths, etc. — exactly what's needed.

- Flights: 0→1 (100), 0→2 (500), 1→2 (100), 2→3 (100). src=0, dst=3, K=1 (at most 1 stop, so at most 2 edges). Answer: 300. But that path is 0→1→2→3 = 2 stops, which violates K=1. The valid path is 0→2→3 = 500+100 = 600 with 1 stop. 

### ***Network Delay Time***

- Directed Graph. Weighted Graph. (Node A, Node B, Time A to B). Signal injected at Node K. Signal Propagates one way along directed edges and when reached another node it retransmits to outgoing neighbours. Traversal takes edge weight time. Minimum time for every node to receive the signal. If unreachable then -1.

- Dijkstra Problem it is.

- ```cpp
    vector<vector<int>> times = {{2,1,1},{2,3,1},{3,4,1}};
    int n = 4, k = 2;
    cout << networkDelayTime(times, n, k) << endl;

    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        vector<vector<pair<int, int>>> adj(n + 1);
        for (auto& time : times) { int u = time[0], v = time[1], w = time[2]; adj[u].push_back({v, w}); }


        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
        pq.push({0, k});

        vector<int> dist(n + 1, INT_MAX);
        dist[k] = 0;

        while (!pq.empty()) {
            int time = pq.top().first; int node = pq.top().second; pq.pop();

            for (auto& [nbr, wt] : adj[node]) {
                if (dist[nbr] > time + wt) { dist[nbr] = time + wt; pq.push({dist[nbr], nbr}); }
            }
        }
        int ans = *max_element(dist.begin() + 1, dist.end());
        return ans == INT_MAX ? -1 : ans;
    }
    ```

### ***Number of ways to Arrive at Destination***

- City of N intersections which sounds like node of graph. Bidirectional between some intersections means undirected at some point not all. You can reach from Node A to Node B for sure. At most one road between any two intersections which sounds like graph is connected. No No No its at most not at least so careful. roads[i] = [ui, vi, timei]. Return from Node 0 to Node n-1, how many ways we can travel in shortest amount of time. Return 1e9 + 7 modulo.

- It sounds like Dijkstra but here we have to keep track how many ways in that minimum time. So what is hard. Do normal Dijkstra and just increase count when that distance arrray has same value at that point. But this same value will change right ? So the same value make it dynamic and update as usual in Dijkstra. Lets see the algorithm. Yeah saw the algorithm its same. Now code.

- ```cpp
    int n = 7;
    vector<vector<int>> edges = {{0, 6, 7}, {0, 1, 2}, {1, 2, 3}, {1, 3, 3}, {6, 3, 3}, {3, 5, 1}, {6, 5, 1}, {2, 5, 1}, {0, 4, 5}, {4, 6, 2}};
    int ans = CheapestFLight(n, edges, 0, 3, 1);

    int CheapestFLight(int n, vector<vector<int>> &flights,int src, int dst, int K){
        int mod = (int)(1e9 + 7);

        vector<pair<int, int>> adj[n];
        for (auto it : flights) { adj[it[0]].push_back({it[1], it[2]}); adj[it[1]].push_back({it[0], it[2]}); }

        priority_queue<pair<int, int>,vector<pair<int, int>>, greater<pair<int, int>>> pq;
        pq.push({0, src});

        vector<int> dist(n, INT_MAX), ways(n, 0);
        dist[src] = 0; ways[src] = 1;

        while (!pq.empty()){
            int dis = pq.top().first; int node = pq.top().second; pq.pop();

            for (auto it : adj[node]) {
                int adjNode = it.first; int edW = it.second; 

                if (dis + edW < dist[adjNode]) {
                    dist[adjNode] = dis + edW;
                    pq.push({dis + edW, adjNode});
                    ways[adjNode] = ways[node];
                }

                else if (dis + edW == dist[adjNode]) {
                    ways[adjNode] = (ways[adjNode] + ways[node]) % mod;
                }
            }
        }
        return ways[dst] % mod;
    }
    ```

### ***Minimum Multiplication to Reach End***

- Given start, end, Array of N numbers. Start multiplied by any of N numbers at each step and a modulo is done by 100000 to get new start. Find minimum steps in which the end can be achived starting from start. If not possible then -1.

- Since its a graph question I know I can think in terms of graph. Idk how I will handle this if question would have come without the context of graph.

- What are my nodes ? It can be 0 to 100000.

- What are my edges ? If I reach any node, I can calculate its edge based on my array of values. So I will need a distance array for all my nodes and it will be updated when reached by normal checks.

- But when to stop ? Queue becomes empty or reach the end number. Normal BFS.

- ```cpp
    int start = 3, end = 30;
    vector<int> arr = {2, 5, 7};
    int ans = minimumMultiplications(arr, start, end);

    int minimumMultiplications(vector<int> &arr, int start, int end){
        queue<pair<int, int>> q;
        q.push({start, 0});

        vector<int> dist(100000, 1e9);
        dist[start] = 0;
        int mod = 100000;

        while (!q.empty()){
            int node = q.front().first; int steps = q.front().second; q.pop();

            for (auto it : arr){
                int num = (it * node) % mod;
                if (steps + 1 < dist[num]) {
                    dist[num] = steps + 1;
                    if (num == end) return steps + 1;
                    q.push({num, steps + 1});
                }
            }
        }
        return -1;
    }
    ```

### ***Find the City with the smallest number of neighbour at a threshold distance***

- Given N nodes as cities with weights and integer threshold distance. Find out city which means node with the smallest number of nodes/cities that are reachable through some path and whose distance is at most threshold distance. if multiple node, return greatest node.

- After seeing explanation of question, Find out for each cities which other cities I can travel to within threshold distance and then return the city from where we can go to least number of other cities.

- Implement Floyd Marshal Algorthm and once you get the matrix output from floyd warshall (figure out what it is, you should know), just compare all values of matrix with threshold, maintain a count to track the city and return it.

- ```cpp
    int findCity(int n, int m, vector<vector<int>>& edges, int distanceThreshold) {
        
        vector<vector<int>> dist(n, vector<int> (n, INT_MAX));
        
        for (auto it : edges) {
            dist[it[0]][it[1]] = it[2];
            dist[it[1]][it[0]] = it[2];
        }
        
        for (int i = 0; i < n; i++) dist[i][i] = 0; // Set the diagonal to 0, as the distance from a city to itself is 0

        // Apply Floyd-Warshall Algorithm to find the shortest paths between all pairs of cities
        for (int k = 0; k < n; k++) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    // Update the distance if a shorter path is found
                    if (dist[i][k] == INT_MAX || dist[k][j] == INT_MAX)
                        continue;
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
                }
            }
        }

        // Initialize variables to track the city with the least reachable cities
        int cntCity = n;
        int cityNo = -1;

        // Check each city and count the number of cities within the threshold distance
        for (int city = 0; city < n; city++) {
            int cnt = 0;
            for (int adjCity = 0; adjCity < n; adjCity++) {
                // If the distance to the adjacent city is within the threshold, increment count
                if (dist[city][adjCity] <= distanceThreshold)
                    cnt++;
            }

            // Update the city with the least number of reachable cities
            if (cnt <= cntCity) {
                cntCity = cnt;
                cityNo = city;
            }
        }
        return cityNo;
    }
    ```

## ***Part 5: Minimum Spanning Tree | Disjoint Set Union***

### ***MST Theory***

- A spanning tree is a tree in which we have N nodes and N-1 edges and all are reachable from each other. (All the nodes present in the original graph.)

- A graph may have more than one spanning tree. Basically you remove extra edges to make it spanning tree. Now since edges have weights, removing particular set of edges gives unique sum of edge weights. 

- Among all possible spanning trees of a graph, the minimum spanning tree is the one for which the sum of all the edge weights is the minimum.

- There may exist multiple minimum spanning trees for a graph like a graph may have multiple spanning trees.

- There are a couple of algorithms that help us to find the minimum spanning tree of a graph. One is Prim’s algorithm and the other is Kruskal’s algorithm.

### ***Prim's Algorithm***

- Given a weighted, undirected, and connected graph of V vertices and E edges. The task is to find the sum of weights of the edges of the Minimum Spanning Tree. (Sometimes it may be asked to find the MST as well, where in the MST the edge-informations will be stored in the form {u, v}(u = starting node, v = ending node).)

- Lets first see the algorithm and then understand the code.

- ```cpp
    int V = 5;
    vector<vector<int>> edges = {{0, 1, 2}, {0, 2, 1}, {1, 2, 1}, {2, 3, 2}, {3, 4, 1}, {4, 2, 2}};
    vector<vector<int>> adj[V];
    for (auto it : edges) {
        vector<int> tmp(2);
        tmp[0] = it[1]; tmp[1] = it[2]; adj[it[0]].push_back(tmp);
        tmp[0] = it[0]; tmp[1] = it[2]; adj[it[1]].push_back(tmp);
    }
    int sum = spanningTree(V, adj);

    int spanningTree(int V, vector<vector<int>> adj[]){
        priority_queue<pair<int, int>, vector<pair<int, int> >, greater<pair<int, int>>> pq;
        pq.push({0, 0}); // {wt, node}

        vector<int> vis(V, 0);
        int sum = 0;

        while (!pq.empty()){
            auto it = pq.top(); pq.pop();
            int node = it.second; int wt = it.first;

            if (vis[node] == 1) continue;

            vis[node] = 1; sum += wt;
            for (auto it : adj[node]){
                int adjNode = it[0]; int edW = it[1];
                if (!vis[adjNode]) pq.push({edW, adjNode});
            }
        }
        return sum;
    }
    ```

- Got it what it does. It does similar to what we do in BFS. But here the queue is priority queue based on min heap. So greedily only that edge are being processed which has less weights and its weight is added. But how being greedy is giving right answer ?

### ***Disjoint Set | Union by Rank | Union by size | Path Compression***

- It is kind of data structure when dealing with graph.

- To check if two nodes belong to same component in a graph or not, if can do BFS or DFS but that is brute force O(V+E). But using a Disjoint Set data structure we can solve this same problem in constant time.

- The disjoint Set data structure is generally used for dynamic graphs. 

- Dynamic Graph: Given edges of graph, if we construct a graph adjoint List or Matrix at each step the graph is being updated and is different. And hence our result will be different if we check for two nodes have same component or not. So, after any step, if we try to figure out whether two arbitrary nodes u and v belong to the same component or not, Disjoint Set will be able to answer this query in constant time.

- Functionality of Disjoint Set Data Structure:
    - Finding the parent for a particular node (findPar())
    - Union (in broad terms this method basically adds an edge between two nodes)
        - Union by rank
        - Union by size

- Union by rank:
    - Rank: The rank of a node generally refers to the distance (the number of nodes including the leaf node) between the furthest leaf node and the current node. Basically rank includes all the nodes beneath the current node.
    - Ultimate Parent: The parent of a node generally refers to the node right above that particular node. But the ultimate parent refers to the topmost node or the root node.

- ```cpp
    class DisjointSet {
        vector<int> rank, parent, size;

        DisjointSet(int n) {
            rank.resize(n, 0);
            parent.resize(n);
            size.resize(n);
            for (int i = 0; i < n; i++) parent[i] = i; size[i] = 1;}

        int findUPar(int node) {
            if (node == parent[node]) return node;
            return parent[node] = findUPar(parent[node]);}

        void unionByRank(int u, int v) {
            int ulp_u = findUPar(u); int ulp_v = findUPar(v); if (ulp_u == ulp_v) return;
            if (rank[ulp_u] < rank[ulp_v]) parent[ulp_u] = ulp_v;
            else if (rank[ulp_v] < rank[ulp_u]) parent[ulp_v] = ulp_u;
            else parent[ulp_v] = ulp_u; rank[ulp_u]++;}

        void unionBySize(int u, int v) {
            int ulp_u = findUPar(u); int ulp_v = findUPar(v); if (ulp_u == ulp_v) return;
            if (size[ulp_u] < size[ulp_v]) parent[ulp_u] = ulp_v; size[ulp_v] += size[ulp_u];
            else parent[ulp_v] = ulp_u; size[ulp_u] += size[ulp_v];}};


        DisjointSet ds(7);
        ds.unionBySize(0, 1); ds.unionBySize(1, 2); ds.unionBySize(3, 4); ds.unionBySize(5, 6); ds.unionBySize(4, 5);
        if (ds.findUPar(2) == ds.findUPar(6)) cout << "Same";
        ds.unionBySize(2, 6);
        if (ds.findUPar(2) == ds.findUPar(6)) cout << "Same";
    ```

- From code these are not intutive, so imagine the video, and keep in mind that here it is not any graph, it is a totally different kind of data structure which is stored in just array.

- WHEN doing union by rank, all we have is parent array of size n which is initialized as parent of node = node itself. And rank of every node is zero. FOR each edge, we are doing union by rank where wwe are just attaching parents and to each other and maintaining a rank which is height. IF r(up(u)) <=> r(up(v)), update either parent or rank. Nothing big this algo is. And this is kind of tree we are making.

- Here we are also doing path compression in the line parent[node] = findUPar(parent[node]). Like first time when you are finding parent recursively in array, it updates so that imgine now your tree last edge is cut and directly joined to ultimate parent. So now you do not have to search for logN but constant time to find any parent when asked if two nodes belong to same component or not.

- Also this is rank and not height because when doing path compression we are not reducing height, because height may shrink.

- O(4*alpha) is TC comes from derivation but it is constant.

- Why connect smaller to larger ? Just to maintain the rank/height of node to make kind of efficient search. To keep tree shrinked so that less time to find parent.

- Now comes union by size. After path compresssion, height is distorted so why using rank. We can use size to keep track of size of component. This is just intutive.

### ***Number of Operations to Make Network Connected***

- You are given a graph with n vertices and m edges. You can remove one edge from anywhere and add that edge between any two vertices in one operation. Find the minimum number of operations that will be required to make the graph connected. If it is not possible to make the graph connected, return -1.

- My thinking, If E != V-1, return -1; Simple Spanning Tree concept. If equal move ahead with algo. We have to think of edge cases too what if E>V-1 and we have less component, it should work. So yeah edge cases question but easy, lets see algo.

- ```cpp
    int makeConnected(int n, vector<vector<int>>& connections){
        if ((int)connections.size() < n - 1) return -1;
        DisjointSet ds(n); int components = 0;
        for (auto& edge : connections) ds.unionBySize(edge[0], edge[1]);     
        for (int i = 0; i < n; i++) {if (ds.findUPar(i) == i) components++;}
        return components - 1;}
    ```

### ***Most Stones Removed with Same Row or Column***

- There are n stones at some integer coordinate points on a 2D plane. Each coordinate point may have at most one stone. You need to remove some stones. A stone can be removed if it shares either the same row or the same column as another stone that has not been removed.

- Group the stones like component and from each component remove all except 1.

- Answer is stones.size() - components.size();

- *Defining the component is challenge here in this kind of questions. What is your nodes !*

- ```cpp
    vector<vector<int>> stones = {{0, 0}, {0, 1}, {1, 0}, {1, 2}, {2, 1}, {2, 2}};
    removeStones(stones);

    class DSU {
        unordered_map<int, int> parent;
        int find(int x){
            if (parent.find(x) == parent.end()) parent[x] = x;
            if (x != parent[x]) parent[x] = find(parent[x]);
            return parent[x];}

        void unite(int x, int y){
            parent[find(x)] = find(y);}};

    int removeStones(vector<vector<int>>& stones){
        DSU dsu; unordered_set<int> components;
        for (auto& stone : stones) dsu.unite(stone[0], stone[1] + 10001); // (offset to avoid collision)
        for (auto& stone : stones) components.insert(dsu.find(stone[0]));
        return stones.size() - components.size();}
    ```

- Intution lacking because so short. I may forget.

### *Hard Questions Incoming*

### ***Account Merge***

- Merge accounts that share common emails and return each user’s name followed by their sorted, unique emails.

    ```
    Example 2:
    Input: N = 6
    accounts [ ] =
    [["John","j1@com","j2@com","j3@com"],
    ["John","j4@com"],
    ["Raj",”r1@com”, “r2@com”],
    ["John","j1@com","j5@com"],
    ["Raj",”r2@com”, “r3@com”],
    ["Mary","m1@com"]]
    Output: [["John","j1@com","j2@com","j3@com","j5@com"],
    ["John","j4@com"],
    ["Raj",”r1@com”, “r2@com”,  “r3@com”],
    ["Mary","m1@com"]]
    ```
    
- If I will see this question for the first time, I will definitely think in terms of unordered map. But think that we have to make group based on something, which means DSU Data Structure.

- If you see input and output, you can see that each item at index can act as one node. And you have to figure out which other node should be connected to it. So yes map is needed as I thought.

- ```cpp
    vector<vector<string>> accountsMerge(vector<vector<string>> &details){
        int n = details.size();
        DisjointSet ds(n); // First Implementation
        unordered_map<string, int> mapMailNode; // email -> account index

        for (int i = 0; i < n; i++){
            for (int j = 1; j < details[i].size(); j++){
                string mail = details[i][j];
                if (mapMailNode.find(mail) == mapMailNode.end()) mapMailNode[mail] = i;
                else ds.unionBySize(i, mapMailNode[mail]);}}

        vector<string> mergedMail[n];
        for (auto it : mapMailNode){
            string mail = it.first;
            int node = ds.findUPar(it.second);
            mergedMail[node].push_back(mail);}

        vector<vector<string>> ans;
        for (int i = 0; i < n; i++){
            if (mergedMail[i].empty()) continue;
            sort(mergedMail[i].begin(), mergedMail[i].end());
            vector<string> temp; temp.push_back(details[i][0]);
            for (auto &mail : mergedMail[i]) temp.push_back(mail);
            ans.push_back(temp);}

        sort(ans.begin(), ans.end());
        return ans;}
    ```

### ***Number of Islands 2***

- You are given an n, m which means the row and column of the 2D matrix, and an array of size k denoting the number of operations. Matrix elements are 0 if there is water or 1 if there is land. Originally, the 2D matrix is all 0 which means there is no land in the matrix. The array has k operator(s) and each operator has two integers `A[i][0]`, `A[i][1]` means that you can change the cell `matrix[A[i][0]][A[i][1]]` from sea to island. Return how many islands are there in the matrix after each operation. You need to return an array of size k. Note: An island means a group of 1s such that they share a common side.

- The problem is to count the number of land cells that cannot reach the boundary of the grid. If a land cell is connected (directly or indirectly) to the boundary, then it is not an enclave. So, the strategy is:
    - First, identify all boundary land cells.
    - Mark all land cells connected to them using BFS.
    - The remaining unvisited land cells are enclaves.
- Approach
    - Traverse the boundary of the grid (first row, last row, first col, last col).
    - Whenever a boundary cell is land (1), run BFS from it and mark all reachable land cells as visited.
    - After BFS traversal, iterate over the entire grid and count the number of land cells which are still unvisited → these are enclaves.

- ```cpp
    // Helper function to check if a cell is within bounds
    bool isValid(int adjr, int adjc, int n, int m) {
        return adjr >= 0 && adjr < n && adjc >= 0 && adjc < m;
    }

    // Main function to process all operators and count number of islands
    vector<int> numOfIslands(int n, int m, vector<vector<int>>& operators) {
        DisjointSet ds(n * m); // Disjoint set to manage connected land cells
        int vis[n][m];
        memset(vis, 0, sizeof vis);
        int cnt = 0;
        vector<int> ans;

        for (auto it : operators) {
            int row = it[0], col = it[1];

            // Skip if the cell is already land
            if (vis[row][col] == 1) {
                ans.push_back(cnt);
                continue;
            }

            // Mark cell as land
            vis[row][col] = 1;
            cnt++;

            // Directions to check for adjacent cells (up, right, down, left)
            int dr[] = {-1, 0, 1, 0};
            int dc[] = {0, 1, 0, -1};
            for (int ind = 0; ind < 4; ind++) {
                int adjr = row + dr[ind], adjc = col + dc[ind];
                if (isValid(adjr, adjc, n, m)) {
                    if (vis[adjr][adjc] == 1) {
                        int nodeNo = row * m + col;
                        int adjNodeNo = adjr * m + adjc;
                        if (ds.findUPar(nodeNo) != ds.findUPar(adjNodeNo)) {
                            cnt--;
                            ds.unionBySize(nodeNo, adjNodeNo);
                        }
                    }
                }
            }

            ans.push_back(cnt);
        }
        return ans;
    }
    ```

### ***Making a Large Island***

- Given an n x n binary matrix grid, it is allowed to change at most one 0 to 1. A group of connected 1s forms an island, where two 1s are connected if they share one of their sides. Return the size of the largest island in the grid after applying this operation.

- Understanding:
    - Each time a cell with a 0 is turned into 1, it forms an edge with its four neighboring cells (islands) if they exist. This makes the graph dynamic in nature.
    - We can efficiently manage these dynamic edges using the Disjoint Set data structure (Union-Find), which helps with merging islands and tracking their sizes.

- How to store cells as nodes in the Disjoint Set?
    - The cells can be numbered sequentially. For a cell at coordinates (i, j), the node number is given by Node number = i * n + j, where n is the number of columns in the grid.

- Edge Cases:
    - If all cells in the grid are 1, only one island will be formed, and the size of the island will be the answer.
    - If a 0 is turned to 1, we need to check its neighbors to find the size of the island formed. But to avoid double counting an island, we store the ultimate parents of neighboring islands in a set to ensure each island is counted once.

- Approach:
    - Create a Disjoint Set data structure to manage connected components (islands) in the grid, where each cell is a separate component initially.
    - Traverse through the grid to identify initial islands (connected 1s) and perform union operations to merge adjacent land cells into the same component using the union by size method.
    - Traverse the grid again to consider each 0 cell, and determine the potential island size if this 0 is changed to 1.
    - For each 0 cell, check all its neighboring cells to find unique components (using ultimate parents) and calculate the combined size of these components.
    - Keep track of the maximum island size encountered during the above calculations.
    - Handle edge cases, like if there are no 0 cells in the grid, by checking the sizes of existing islands.
    - Return the size of the largest possible island after changing at most one 0 to 1.

- ```cpp
    // DelRow and delCol for neighbors
    vector<int> delRow = {-1, 0, 1, 0};
    vector<int> delCol = {0, 1, 0, -1};
    
    bool isValid(int &i, int &j, int &n) {
        if(i < 0 || i >= n) return false;
        if(j < 0 || j >= n) return false;
        return true;
    }
    
    void addInitialIslands(vector<vector<int>> grid, DisjointSet &ds, int n) {
        for (int row = 0; row < n ; row++) {
            for (int col = 0; col < n ; col++) {
                if (grid[row][col] == 0) continue;
                for (int ind = 0; ind < 4; ind++) {
                    int newRow = row + delRow[ind];
                    int newCol = col + delCol[ind];
                    
                    if (isValid(newRow, newCol, n) && grid[newRow][newCol] == 1) {
                        int nodeNo = row * n + col; // Get the number for node
                        int adjNodeNo = newRow * n + newCol; // Get the number for neighbor
                        ds.unionBySize(nodeNo, adjNodeNo); /* Take union of both nodes as they are part of the same island */
                    }
                }
            }
        }
    }

    int largestIsland(vector<vector<int>>& grid) {
        int n = grid.size();
        DisjointSet ds(n*n);
        addInitialIslands(grid, ds, n);
        
        int ans = 0;
        for (int row = 0; row < n; row++) {
            for (int col = 0; col < n; col++) {
                if (grid[row][col] == 1) continue;
                set<int> components; /* Set to store the ultimate parents of neighboring islands */
                for (int ind = 0; ind < 4; ind++) {
                    int newRow = row + delRow[ind];
                    int newCol = col + delCol[ind];
                    if (isValid(newRow, newCol, n) && grid[newRow][newCol] == 1) {
                        /* Perform union and store ultimate parent in the set */
                        int nodeNumber = newRow * n + newCol;
                        components.insert(ds.findUPar(nodeNumber));
                    }
                }

                int sizeTotal = 0; // To store the size of current largest island
                for (auto it : components) { // Iterate on all the neighboring ultimate parents
                    sizeTotal += ds.size[it]; // Update the size
                }                
                ans = max(ans, sizeTotal + 1); // Store the maximum size of island
            }
        }
        
        for (int cellNo = 0; cellNo < n * n; cellNo++) { // Edge case
            ans = max(ans, ds.size[ds.findUPar(cellNo)]); // Keep the answer updated
        }
        return ans; // Return the answer
    }
    ```

### ***Swim in Rising Water***

- You are given an n × n matrix grid where grid[i][j] is the unique elevation of cell (i, j).
Rain starts falling at time t = 0. At any later time t ≥ 0, every cell is covered by water to depth t. You may move 4-directionally (up, down, left, right) between adjacent cells instantaneously iff the elevations of both cells are ≤ t. Starting from the top-left cell (0, 0), return the minimum time t at which you can reach the bottom-right cell (n − 1, n − 1).

- We want to reach the bottom-right cell while the water level keeps rising. At any moment, we can only step into a cell if the water has risen above that cell’s elevation. Therefore, instead of waiting at each step, we check, what’s the lowest elevation we can move to next.

- To always move through the least risky path, we simulate the rising water using a min-heap, where we always expand the lowest elevation cell available to us. The maximum elevation on the chosen path determines the earliest time we can reach the end.

- ```cpp
    int swimInWater(vector<vector<int>>& grid) {
        int n = grid.size();

        // Create a min-heap for cells based on elevation
        priority_queue<vector<int>, vector<vector<int>>, greater<vector<int>>> minHeap;
        minHeap.push({grid[0][0], 0, 0}); // Push starting cell to heap

        // Create visited matrix
        vector<vector<int>> visited(n, vector<int>(n, 0));
        visited[0][0] = 1;

        vector<pair<int, int>> dirs = {{0,1}, {1,0}, {0,-1}, {-1,0}}; // Direction vectors for movement

        // Process heap until destination is reached
        while (!minHeap.empty()) {
            auto curr = minHeap.top(); minHeap.pop(); // Extract cell with least elevation
            int elevation = curr[0], r = curr[1], c = curr[2];

            if (r == n - 1 && c == n - 1) return elevation; // If destination reached, return elevation

            for (auto& dir : dirs) {
                int nr = r + dir.first;
                int nc = c + dir.second;

                // Check bounds and if not visited
                if (nr >= 0 && nc >= 0 && nr < n && nc < n && !visited[nr][nc]) {
                    visited[nr][nc] = 1;
                    minHeap.push({max(elevation, grid[nr][nc]), nr, nc});
                }
            }
        }
        return -1;
    }
    ```


## ***Part 6: Other Important Algorithms (All as DFS)***

### ***Bridges in Graph***

- There are n servers numbered from 0 to n - 1 connected by undirected server-to-server connections forming a network where connections[i] = [ai, bi] represents a connection between servers ai and bi. Any server can reach other servers directly or indirectly through the network. A critical connection is a connection that, if removed, will make some servers unable to reach some other servers. Return all critical connections in the network in any order.

- Simple DFS with a timer counter is solving this !!!

- ```cpp
    int timer = 1; // global

    void dfs(int node, int parent, vector<int> &vis, vector<int> adj[], int tin[], int low[], vector<vector<int>> &bridges){
        vis[node] = 1;
        tin[node] = low[node] = timer;
        timer++;

        for (auto it : adj[node]){
            if (it == parent) continue;
            if (vis[it] == 0){
                dfs(it, node, vis, adj, tin, low, bridges);
                low[node] = min(low[node], low[it]);
                if (low[it] > tin[node]) bridges.push_back({it, node});}
            else low[node] = min(low[node], low[it]);}}

    vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections){
        vector<int> adj[n];
        for (auto it : connections) { int u = it[0], v = it[1]; adj[u].push_back(v); adj[v].push_back(u); }

        vector<int> vis(n, 0);
        vector<vector<int>> bridges;

        int tin[n]; // Discovery time
        int low[n]; // Lowest reachable time

        dfs(0, -1, vis, adj, tin, low, bridges); // (assuming the graph contains a single component otherwise, we will call DFS for every component) with parent -1.
        return bridges;}
    ```

### ***Articulation point in graph***

- Find all articulation points (cut vertices) in an undirected graph whose removal increases the number of connected components.

- Kind of same as bridges. Here its node that we have to find. Similar DFS Solution.

- ```cpp
    int timer = 1;

    // DFS to find articulation points
    void dfs(int node, int parent, vector<int> &vis, int tin[], int low[],
             vector<int> &mark, vector<int> adj[]) {

        vis[node] = 1;
        tin[node] = low[node] = timer++; // set discovery and low time
        int child = 0;

        for (auto it : adj[node]) {
            if (it == parent) continue; // skip the edge to parent

            if (!vis[it]) {
                dfs(it, node, vis, tin, low, mark, adj); // recursive DFS
                low[node] = min(low[node], low[it]);     // update low time

                // Check articulation condition (excluding root)
                if (low[it] >= tin[node] && parent != -1) {
                    mark[node] = 1;
                }
                child++;
            }
            else {
                // back edge case
                low[node] = min(low[node], tin[it]);
            }
        }

        // If root node has more than one child
        if (parent == -1 && child > 1) {
            mark[node] = 1;
        }
    }

    vector<int> articulationPoints(int n, vector<int> adj[]) {
        vector<int> vis(n, 0), mark(n, 0);
        int tin[n], low[n];

        for (int i = 0; i < n; i++) {
            if (!vis[i]) {
                dfs(i, -1, vis, tin, low, mark, adj);
            }
        }

        vector<int> ans;
        for (int i = 0; i < n; i++) {
            if (mark[i]) ans.push_back(i);
        }
        return ans.empty() ? vector<int>{-1} : ans;
    }
    ```

### ***Kosaraju's Algorithm | Strongly Connected Components***

- Given a Directed Graph with V vertices (Numbered from 0 to V-1) and E edges, Find the number of strongly connected components in the graph. 

- In a directed graph, strongly connected components (SCCs) are subsets of nodes where every node is reachable from every other node within the same subset. 

- Again dfs based solution.

- In a directed graph, strongly connected components (SCCs) are subsets of nodes where every node is reachable from every other node within the same subset. Kosaraju’s Algorithm efficiently finds these SCCs by leveraging the concept of graph transposition and finishing times in DFS. The key idea is: if we perform a DFS on the graph and record the finishing times of nodes, then by reversing the graph and doing DFS in the order of decreasing finishing times, we can group nodes into SCCs.

- Approach: Perform a DFS traversal on the graph to fill nodes into a stack according to their finishing times (nodes that finish later are pushed later). Reverse (transpose) the graph by reversing the direction of every edge. Perform DFS again on the transposed graph, in the order defined by the stack. Each DFS traversal gives one strongly connected component. Count the number of DFS calls on the transposed graph, which equals the number of SCCs.

- ```cpp
    void dfs(int node, vector<int> &vis, vector<int> adj[], stack<int> &st) {
        vis[node] = 1;
        for (auto it : adj[node]) {
            if (!vis[it]) {
                dfs(it, vis, adj, st);
            }
        }
        // Push the node into stack after visiting all neighbors
        st.push(node);
    }

    // Step 2: Perform DFS on transposed graph
    void dfs3(int node, vector<int> &vis, vector<int> adjT[]) {
        vis[node] = 1;
        for (auto it : adjT[node]) {
            if (!vis[it]) {
                dfs3(it, vis, adjT);
            }
        }
    }

    // Function to find number of strongly connected components
    int kosaraju(int V, vector<int> adj[]) {
        vector<int> vis(V, 0);
        stack<int> st;

        // Step 1: Do DFS to fill stack by finishing times
        for (int i = 0; i < V; i++) {
            if (!vis[i]) {
                dfs(i, vis, adj, st);
            }
        }

        // Step 2: Build the transpose graph
        vector<int> adjT[V];
        for (int i = 0; i < V; i++) {
            vis[i] = 0; // reset visited
            for (auto it : adj[i]) {
                adjT[it].push_back(i); // reverse edge
            }
        }

        // Step 3: Process stack to count SCCs
        int scc = 0;
        while (!st.empty()) {
            int node = st.top();
            st.pop();
            if (!vis[node]) {
                scc++;
                dfs3(node, vis, adjT);
            }
        }
        return scc;
    }
    ```


