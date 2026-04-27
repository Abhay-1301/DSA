# ***Graph Theory***

## ***Part 1: Learning***

- **Adjacency Matrix:** How to write in C++. It's just a matrix if you visualise. If you can visualise, you can write code.

- **Adjacency List:** How to write in C++. It is 

- **Connected Component:** No need of notes. Also it can be find simply how many bfs was called on non visited item in initial loop.

- **BFS:** 

- **DFS:**
    ```
    int V = 5; Given
    vector<int> adj[V];
    Populate adjacency list. adj[i] = array or vector of nodes connected to i. adj[2] = {1,3}. It will be pair if weights are there.
    vector<int> visited(V, 0);
    vector<int> result;
    dfs(0, adj, visited, result)
    dfs (int v, vector<int> adj[], vector<int>& visited, vector<int>& result)
    visited[v] = 1;
    result.push_back(v);
    for (int u : adj[v]) { if (!visited[u]) { dfs(u, adj, visited, result) }}
    ```

## ***Part 2: Problems of BFS/DFS***

## ***Part 3: Topo Sort and Problems***

- ### Topological Sort Algorithm
    - Applies on Directed Acyclic Graph
    - U -> V means U will always come before V in sorting
    - **My past memory:** Keep track of outgoing and incoming arrow to the node. AND AND AND after thinking and visualising I got to know, keep track of incoming arrow to a node. Print node which has zero incoming node and then remove arrow which means subtracting incoming arrow count of connected node to this node. Again check for all nodes print which has zero incoming node and so on.
    - Handle edge cases. Will see what I missed in code.
    - What I just thought is solving this by BFS which is also called Kahn's Algorithm. There is DFS solution too.
    - Algorithm: (Using BFS)
        ```
        int V = 6, E = 6;
        vector<int> adj[V]; adj[I].push_back(j); for all i and j as per question
        ans = topologicalSort(V, adj);
        vector<int> topologicalSort(int V, vector<int> adj[])
        vector<int> indegree(V, 0); iteration i from 0 to V; auto it : adj[I]; indegree[it]++;
        queue<int> q; iteration i from 0 to V; if indegree[i]=0 -> q.push(i)
        Vector<int> ans;
        While q is not empty -> node = q.front, q.pop, ans.push(node); auto it: adj[node] -> indegree[it]— and if indegree[it] = 0 -> q.push(it) [again loop back to while is not empty]
        Return ans
        ```

    - Algorithm: (Using DFS)
        *NEED REVISION - Dry run makes sense but no intuition that why stack holds my answer.*
        ```
        int V = 6, E = 6;
        vector<int> adj[V]; adj[I].push_back(j); for all i and j as per question
        ans = topologicalSort(V, adj);
        vector<int> topoSort(int V, vector<int> adj[])
        vector<int> vis(V, 0);
        stack<int> st;
        Iteration i from 0 to V if not visited i then call dfs(i,adj,vis,st)
        void dfs(int node, vector<int> adj[], vector<int>& vis, stack<int>& st)
        vis[node] = 1;
        For auto it: adj[node] if not visited it then dfs(it,adj,vis,st)
        St.push(node)
        vector<int> ans;
        While stack is not empty -> ans.push_back(st.top) then st.pop
        Return ans
        ```

- ### Kahn's Algorithm
    - Topo Sort by BFS as above

- ### Detect a cycle in directed graph
    - Obviously here we are discussing using Topo Sort approach. Simple if processed node != total nodes V then cycle exists. How to track count of processed node? Just add a int count = 0 before while q is not empty loop start and do count++ after q.pop done.

- ### Course Schedule 1 and 2
    - **1:** We just have to return true or false possible or not similar to detecting cycle.
    - **2:** Is finding actual order if true. So just do usual Kahn Algo. Track the count push your node to answer and if processed node count total node count then return that answer vector. Wait wait wait, you don't have to keep track of count. Just check with ans.size() == total nodes or not

- ### Find eventual safe state
    - Ss

- ### Alien Dictionary
    - Given array of strings which represent sorted listed of words from alien dictionary. Determine order of character. Intuition came that we have to use topological sort but how to make graph from given inputs.

## ***Part 4: Shortest Path Algorithms and Problems***

- ### Shortest Path in Undirected Graph with unit weights
    - If you imagine in mind undirected graph with cycles, you can visualise that using BFS we can find shortest path. Just maintaining a variable array distance. ***BFS because edges are unit weights.*** Focus on fact that queue of BFS is processing node closer to source node naturally. Compare with Dijkstra where we use some heap.

        ```
        Int N, Int M, edges array, source node = 0
        Ans = shortestPath(edges, N, M, 0)
        vector<int> shortestPath(vector<vector<int>>& edges, int N, int M, int src)
        vector<int> adj[N]; 
        For auto it: edges -> adj[it[0]].push_back(it[1]) and adj[it[1]].push_back(it[0]);
        Vector<int> dist(n,1e9) and dist[src] = 0
        Queue<int>q and q .push(src)
        While q is not empty
        node =q.front and q.pop
        for auto it: adj[node]
        if dist[node]+1 < dist[it]
        dist[it] = 1 + dist[node] and q.push(it)
        ans(n,-1) ans[I] = dist[I] if dist I not equal to 1e9 return ans
        ```

- ### Shortest path in Directed Acyclic Graph
    - What comes in mind when you visualise DAG? And edges have weight now?

- ### Dijkstra Algorithm
    - Undirected, weighted, connected graph, No negative weight cycle. Find from the source vertex shortest distance to all other nodes.

        ```
        int V = 3, E = 3, S = 2
        vector<vector<int>> adj[V]; and populate it using edges
        vector<int> res = dijkstra(V, adj, S);
        vector<int> dijkstra(int V, vector<vector<int>> adj[], int S)
        set<pair<int, int>> st; 
        vector<int> dist(V, 1e9); 
        st.insert({0, S}); 
        dist[S] = 0;
        While set is not empty
        Auto it = *(st.begin()) and node = it.second, dis = it.first and st.erase(it)
        For auto it: adj[node]
        adjNode = it[0], edge = it[1]
        If dis + edgW < dist[adjNode]
        If dist[adjNode] is not 1e9 then st.erase(dist[adj[node],adjNode)
        dist[adjNode] = dis+edgW and st.insert(){dist[adjNode], adjNode}
        Return dist
        ```

    - See the priority queue code. Using a priority queue (min-heap) ensures that we can efficiently pick the node with the smallest current distance, instead of scanning all nodes each time.

- ### Shortest Distance in a Binary Maze
    - A maze is there filled with 0 and 1 where 1 means path and 0 means blocked. I just have to find the shortest distance from maze[i][j] to maze[a][b].
    - I thought of solution correctly. Treat it like undirected graph with unit weight question and apply bfs to get the answer. up down left right is the graph connection.
    - Let's see the code because its implementation will be complex.
        ```cpp
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

            int n = grid.size(); int m = grid[0].size(); queue<pair<int, pair<int, int>>> q; vector<vector<int>> dist(n, vector<int>(m, 1e9)); dist[source.first][source.second] = 0; q.push({0, {source.first, source.second}}); 

            int dr[] = {-1, 0, 1, 0}; int dc[] = {0, 1, 0, -1};

            while (!q.empty()) {
                auto it = q.front(); q.pop(); int dis = it.first; int r = it.second.first; int c = it.second.second;
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

- ### Path with minimum effort
    - Similar to above question. But it's not binary maze. It has value which means similar to above implementation but will be using Dijkstra here.
    - Code this one by yourself, because this is too tricky to code.

- ### Cheapest Flight within K stops
    - Given directed graph can be cyclic. Flight[i]=[from,to,price]. Reach from node A to node B (given) as cheap as possible. Edges have weight. You have k stops only.
    - First I have to decide whether I can reach Node B from Node A within K stops. So if I do BFS from Node A. Track the level by maintaining a variable which level is being processed. If level become greater than K, return -1. If got to B first return the total edges weight till that point. I am thinking of Dijkstra with that variable count of which level processing. Lets see the solution.
    - Algorithm:
        ```cpp
        int n = 4, src = 0, dst = 3, K = 1; vector<vector<int>> flights = {{0, 1, 100}, {1, 2, 100}, {2, 0, 100}, {1, 3, 600}, {2, 3, 200}}; int ans = CheapestFLight(n, flights, src, dst, K);

        int CheapestFLight(int n, vector<vector<int>> &flights,int src, int dst, int K){

            vector<pair<int, int>> adj[n]; for (auto it : flights) adj[it[0]].push_back({it[1], it[2]});
            queue<pair<int, pair<int, int>>> q; q.push({0, {src, 0}});
            vector<int> dist(n, 1e9); dist[src] = 0;

            while (!q.empty()){
                auto it = q.front(); q.pop(); int stops = it.first; int node = it.second.first; int cost = it.second.second; if (stops > K) continue;

                for (auto iter : adj[node]){
                    int adjNode = iter.first; int edW = iter.second; 
                    if (cost + edW < dist[adjNode] && stops <= K) dist[adjNode] = cost + edW; q.push({stops + 1, {adjNode, cost + edW}});
                }
            }
            if (dist[dst] == 1e9) return -1;
            return dist[dst];
        }
        ```
    - Unlike Dijkstra, here normal queue is used. Why ?

- ### Network Delay Time
    - Directed Graph. Weighted Graph. (Node A, Node B, Time A to B). Signal injected at Node K. Signal Propagates one way along directed edges and when reached another node it retransmits to outgoing neighbours. Traversal takes edge weight time. Minimum time for every node to receive the signal. If unreachable then -1.
    - Dijkstra Problem it is.
    - Algorithm:
        ```cpp
        vector<vector<int>> times = {{2,1,1},{2,3,1},{3,4,1}}; int n = 4, k = 2; cout << networkDelayTime(times, n, k) << endl;

        int networkDelayTime(vector<vector<int>>& times, int n, int k) {
            vector<vector<pair<int, int>>> adj(n + 1);
            for (auto& time : times) int u = time[0], v = time[1], w = time[2]; adj[u].push_back({v, w});
            priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq; pq.push({0, k});
            vector<int> dist(n + 1, INT_MAX); dist[k] = 0;

            while (!pq.empty()) {
                int time = pq.top().first; int node = pq.top().second; pq.pop();

                for (auto& [nbr, wt] : adj[node]) {
                    if (dist[nbr] > time + wt) dist[nbr] = time + wt; pq.push({dist[nbr], nbr});
                }

            }
            int ans = *max_element(dist.begin() + 1, dist.end()); return ans == INT_MAX ? -1 : ans;
        }
        ```

- ### Number of ways to Arrive at Destination
    - City of N intersections which sounds like node of graph. Bidirectional between some intersections means undirected at some point not all. You can reach from Node A to Node B for sure. At most one road between any two intersections which sounds like graph is connected. No No No its at most not at least so careful. roads[i] = [ui, vi, timei]. Return from Node 0 to Node n-1, how many ways we can travel in shortest amount of time. Return 1e9 + 7 modulo.
    - It sounds like Dijkstra but here we have to keep track how many ways in that minimum time. So what is hard. Do normal Dijkstra and just increase count when that distance arrray has same value at that point. But this same value will change right ? So the same value make it dynamic and update as usual in Dijkstra. Lets see the algorithm. Yeah saw the algorithm its same. Now code.
    - Algorithm:
        ```cpp
        int n = 7; vector<vector<int>> edges = {{0, 6, 7}, {0, 1, 2}, {1, 2, 3}, {1, 3, 3}, {6, 3, 3}, {3, 5, 1}, {6, 5, 1}, {2, 5, 1}, {0, 4, 5}, {4, 6, 2}}; int ans = CheapestFLight(n, edges, 0, 3, 1);

        int CheapestFLight(int n, vector<vector<int>> &flights,int src, int dst, int K){
            int mod = (int)(1e9 + 7);
            vector<pair<int, int>> adj[n]; for (auto it : flights) adj[it[0]].push_back({it[1], it[2]}); adj[it[1]].push_back({it[0], it[2]});
            priority_queue<pair<int, int>,vector<pair<int, int>>, greater<pair<int, int>>> pq; pq.push({0, src});
            vector<int> dist(n, INT_MAX), ways(n, 0); dist[src] = 0; ways[src] = 1;

            while (!pq.empty()){
                int dis = pq.top().first; int node = pq.top().second; pq.pop();

                for (auto it : adj[node]) {
                    int adjNode = it.first; int edW = it.second; 

                    if (dis + edW < dist[adjNode]) dist[adjNode] = dis + edW; pq.push({dis + edW, adjNode}); ways[adjNode] = ways[node];

                    else if (dis + edW == dist[adjNode]) ways[adjNode] = (ways[adjNode] + ways[node]) % mod;
                }
            }
            return ways[dst] % mod;
        }
        ```

- ### Minimum Multiplication to Reach End
    - Given start, end, Array of N numbers. Start multiplied by any of N numbers at each step and a modulo is done by 100000 to get new start. Find minimum steps in which the end can be achived starting from start. If not possible then -1.
    - Since its a graph question I know I can think in terms of graph. Idk how I will handle this if question would have come without the context of graph.
    - What are my nodes ? It can be 0 to 100000.
    - What are my edges ? If I reach any node, I can calculate its edge based on my array of values. So I will need a distance array for all my nodes and it will be updated when reached by normal checks.
    - But when to stop ? Queue becomes empty or reach the end number. Normal BFS.
    - Algorithm:
        ```cpp
        int start = 3, end = 30; vector<int> arr = {2, 5, 7}; int ans = minimumMultiplications(arr, start, end);

        int minimumMultiplications(vector<int> &arr, int start, int end){
            queue<pair<int, int>> q; q.push({start, 0});
            vector<int> dist(100000, 1e9); dist[start] = 0; int mod = 100000;

            while (!q.empty()){
                int node = q.front().first; int steps = q.front().second; q.pop();

                for (auto it : arr){
                    int num = (it * node) % mod;
                    if (steps + 1 < dist[num]) dist[num] = steps + 1; {if (num == end) return steps + 1;} q.push({num, steps + 1});
                }
            }
            return -1;
        }
        ```

- ### Bellman Ford Algorithm
    - Given weighted, directed or undirected and connected graph having negative edges. Find shortest distance of all vertices from source vertx S. If negative cycle in graph, return -1. There can be negative edges. Negative cycle is different.
    - Dijkstra can loop forever or give incorrect result if negative edge.
    - It can also detect negative cycles, a cycle where the total path weight is negative causing the distance to decrease endlessly.
    - Treat undirected as two way directed graph. And it works for undirected too.
    - Algorithm:
        - Works on the relaxation of edges initution. Why Repeat N-1 Times? Because the shortest path to any point can involve at most one less edge than the total number of points. If taking a certain edge gives a shorter path to a point, update that point with the new shorter distance.
        - After finishing the updates, go through all edges one more time. If any distance can still be reduced, a negative cycle exists. If not, the shortest distances are correct.
        - See that here we are iterating on edges and not on adjacency list or matrix.
        ```cpp
        vector<int> bellman_ford(int V, vector<vector<int>>& edges, int S) {
            vector<int> dist(V, 1e8); dist[S] = 0;
            for (int i = 0; i < V - 1; i++){
                for (auto it : edges){
                    int u = it[0]; int v = it[1]; int wt = it[2];
                    if (dist[u] != 1e8 && dist[u] + wt < dist[v]) dist[v] = dist[u] + wt;
                }
            }
            for (auto it : edges){
                int u = it[0]; int v = it[1]; int wt = it[2];
                if (dist[u] != 1e8 && dist[u] + wt < dist[v]) return {-1};
            }
            return dist;
        }
        ```

- ### Floyd Warshall Algorithm
    - Lets see what the heck is this.
    - Here we have to find shortest distance between every possible node. It is multisource shortest path problem.
    - Ok got it. Here we are finding the shortest path from i to j going via every possible k.
    - Algorithm:
        ```cpp
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

- ### Find the City with the smallest number of neighbour at a threshold distance
    - Given N nodes as cities with weights and integer threshold distance. Find out city which means node with the smallest number of nodes/cities that are reachable through some path and whose distance is at most threshold distance. if multiple node, return greatest node.
    - After seeing explanation of question, Find out for each cities which other cities I can travel to within threshold distance and then return the city from where we can go to least number of other cities.
    - Implement Floyd Marshal Algorthm and once you get the matrix output from floyd warshall (figure out what it is, you should know), just compare all values of matrix with threshold, maintain a count to track the city and return it.

## ***Part 5: Minimum Spanning Tree | Disjoint Set Union***

- ### MST Theory
    - A spanning tree is a tree in which we have N nodes and N-1 edges and all are reachable from each other. (All the nodes present in the original graph.)
    - A graph may have more than one spanning tree. Basically you remove extra edges to make it spanning tree. Now since edges have weights, removing particular set of edges gives unique sum of edge weights. 
    - Among all possible spanning trees of a graph, the minimum spanning tree is the one for which the sum of all the edge weights is the minimum.
    - There may exist multiple minimum spanning trees for a graph like a graph may have multiple spanning trees.
    - There are a couple of algorithms that help us to find the minimum spanning tree of a graph. One is Prim’s algorithm and the other is Kruskal’s algorithm.

- ### Prim's Algorithm
    - Given a weighted, undirected, and connected graph of V vertices and E edges. The task is to find the sum of weights of the edges of the Minimum Spanning Tree. (Sometimes it may be asked to find the MST as well, where in the MST the edge-informations will be stored in the form {u, v}(u = starting node, v = ending node).)
    - Lets first see the algorithm and then understand the code.
    - Algorithm
        ```cpp
        int V = 5; vector<vector<int>> edges = {{0, 1, 2}, {0, 2, 1}, {1, 2, 1}, {2, 3, 2}, {3, 4, 1}, {4, 2, 2}};
        vector<vector<int>> adj[V];
        for (auto it : edges) {
            vector<int> tmp(2);
            tmp[0] = it[1]; tmp[1] = it[2]; adj[it[0]].push_back(tmp);
            tmp[0] = it[0]; tmp[1] = it[2]; adj[it[1]].push_back(tmp);
        }
        int sum = spanningTree(V, adj);

        int spanningTree(int V, vector<vector<int>> adj[]){
            priority_queue<pair<int, int>, vector<pair<int, int> >, greater<pair<int, int>>> pq; pq.push({0, 0}); // {wt, node}
            vector<int> vis(V, 0); int sum = 0;

            while (!pq.empty()){
                auto it = pq.top(); pq.pop(); int node = it.second; int wt = it.first;
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

- ### Disjoint Set | Union by Rank | Union by size | Path Compression
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
    - Algorithms:
        ```cpp
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

- ### Number of Operations to Make Network Connected
    - You are given a graph with n vertices and m edges. You can remove one edge from anywhere and add that edge between any two vertices in one operation. Find the minimum number of operations that will be required to make the graph connected. If it is not possible to make the graph connected, return -1.
    - My thinking, If E != V-1, return -1; Simple Spanning Tree concept. If equal move ahead with algo. We have to think of edge cases too what if E>V-1 and we have less component, it should work. So yeah edge cases question but easy, lets see algo.
    - Similar to above code.
        ```cpp
        int makeConnected(int n, vector<vector<int>>& connections){
            if ((int)connections.size() < n - 1) return -1;
            DisjointSet ds(n); int components = 0;
            for (auto& edge : connections) ds.unionBySize(edge[0], edge[1]);     
            for (int i = 0; i < n; i++) {if (ds.findUPar(i) == i) components++;}
            return components - 1;}
        ```

- ### Most Stones Removed with Same Row or Column
    - There are n stones at some integer coordinate points on a 2D plane. Each coordinate point may have at most one stone. You need to remove some stones. A stone can be removed if it shares either the same row or the same column as another stone that has not been removed.
    - Group the stones like component and from each component remove all except 1.
    - Answer is stones.size() - components.size();
    - *Defining the component is challenge here in this kind of questions. What is your nodes !*
    - Algorithm:

        ```cpp
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
    - Intution lacking becausw so short. I may forget.

- ### *Hard Questions Incoming*

- ### Account Merge
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
    - Let's see the algorithm.
        ```cpp
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

- ### Number of Islands 2

- ### Making a Large Island

- ### Swim in Rising Water

## ***Other Important Algorithms (All as DFS)***

- ### Bridges in Graph
    - There are n servers numbered from 0 to n - 1 connected by undirected server-to-server connections forming a network where connections[i] = [ai, bi] represents a connection between servers ai and bi. Any server can reach other servers directly or indirectly through the network.A critical connection is a connection that, if removed, will make some servers unable to reach some other servers. Return all critical connections in the network in any order.
    - Simple DFS with a timer counter is solving this !!!
    - Algo:
        ```cpp
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
            for (auto it : connections) int u = it[0], v = it[1]; adj[u].push_back(v); adj[v].push_back(u);

            vector<int> vis(n, 0); int tin[n]; // Discovery time
            vector<vector<int>> bridges; int low[n]; // Lowest reachable time

            dfs(0, -1, vis, adj, tin, low, bridges); // (assuming the graph contains a single component otherwise, we will call DFS for every component) with parent -1.
            return bridges;}
        ```

- ### Articulation point in graph
    - Find all articulation points (cut vertices) in an undirected graph whose removal increases the number of connected components.
    - Kind of same as bridges. Here its node that we hhave to find. Similar DFS Solution.

- ### Kosaraju's Algorithm | Strongly Connected Components
    - Given a Directed Graph with V vertices (Numbered from 0 to V-1) and E edges, Find the number of strongly connected components in the graph. 
    - In a directed graph, strongly connected components (SCCs) are subsets of nodes where every node is reachable from every other node within the same subset. 
    - Again dfs based solution.
