---
layout: post
title:  "Shortest Path"
tags: graph
---
# Dijkstra

[Dijkstra's algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm)

Dijkstra's Shortest Path First algorithm (SPF algorithm) is an algorithm for finding the shortest paths between nodes in a graph. A ***single*** node as the "source" node and finds shortest paths from the source to all other nodes in the graph, producing a shortest-path tree.

```
function Dijkstra(Graph, source):

    create vertex set Q

    for each vertex v in Graph:
        dist[v] ← INFINITY
        prev[v] ← UNDEFINED
        add v to Q
    dist[source] ← 0

    while Q is not empty:
        u ← vertex in Q with min dist[u]

        remove u from Q

        for each neighbor v of u:           // only v that are still in Q
            alt ← dist[u] + length(u, v)
            if alt < dist[v]:
                dist[v] ← alt
                prev[v] ← u

    return dist[], prev[]
```

Comparison with BFS:

|       | BFS  | Dijkstra |
|-------| ------------- | ------------- |
| graph | unweighted  | weighted (non-negative) |
| queue | queue  | priority queue  |
| time complexity | O(V + E) | O(V + Elog(V)) |

[Path with Maximum Probability][path-with-maximum-probability]

{% highlight java %}
public double maxProbability(int n, int[][] edges, double[] succProb, int start, int end) {
    // builds graph
    Map<Integer, List<int[]>> graph = new HashMap<>();  // node : (neighbor : prob index)
    for (int i = 0; i < edges.length; i++) {
        int[] edge = edges[i];
        graph.computeIfAbsent(edge[0], k -> new ArrayList<>()).add(new int[]{edge[1], i});
        graph.computeIfAbsent(edge[1], k -> new ArrayList<>()).add(new int[]{edge[0], i});
    }

    // Dijkstra
    double[] p = new double[n];
    p[start] = 1;

    // max heap
    Queue<Integer> pq = new PriorityQueue<>(Comparator.comparingDouble(i -> -p[i]));
    int node = start;
    pq.offer(node);

    while (!pq.isEmpty() && node != end) {
        node = pq.poll();

        if (!graph.containsKey(node)) {
            continue;
        }

        for (int[] pair : graph.get(node)) {
            int neighbor = pair[0], index = pair[1];
            if (p[node] * succProb[index] > p[neighbor]) {
                p[neighbor] = p[node] * succProb[index];
                pq.offer(neighbor);
            }
        }
    }

    return p[end];
}
{% endhighlight %}

DFS will underflow or LTE.

[Number of Restricted Paths From First to Last Node][number-of-restricted-paths-from-first-to-last-node]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;
private List<int[]>[] graph;
private int[] dist, memo;
private int n;

public int countRestrictedPaths(int n, int[][] edges) {
    this.n = n;
    this.graph = new List[n + 1];
    for (int i = 1; i <= n; i++) {
        graph[i] = new ArrayList<>();
    }

    // u : {v, weight}
    for (int[] e : edges) {
        graph[e[0]].add(new int[]{e[1], e[2]});
        graph[e[1]].add(new int[]{e[0], e[2]});
    }

    this.dist = new int[n + 1];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[n] = 0;

    Queue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[1]));
    int[] node = new int[]{n, 0};
    pq.offer(node);

    while (!pq.isEmpty()) {
        node = pq.poll();

        // there's a node in the queue that's processed and more preferred
        // so this node can be discarded
        if (dist[node[0]] < node[1]) {
            continue;
        }

        for (var e : graph[node[0]]) {
            int neighbor = e[0];
            if (dist[node[0]] + e[1] < dist[neighbor]) {
                dist[neighbor] = dist[node[0]] + e[1];
                pq.offer(new int[]{neighbor, dist[neighbor]});
            }
        }
    }

    this.memo = new int[n + 1];
    Arrays.fill(memo, -1);

    return dfs(1);
}

private int dfs(int node) {
    if (node == n) {
        return 1;
    }

    if (memo[node] >= 0) {
        return memo[node];
    }

    int count = 0;
    for (var e : graph[node]) {
        int neighbor = e[0];
        // on the restricted path, dist is monotonically increasing
        // so there's no cycle
        if (dist[neighbor] < dist[node]) {
            count = (count + dfs(neighbor)) % MOD;
        }
    }

    return memo[node] = count;
}
{% endhighlight %}

[The Maze III][the-maze-iii]

{% highlight java %}
{% raw %}
private static final int[][] DIRECTIONS = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
{% endraw %}
private static final char[] INSTRUCTIONS = {'d', 'r', 'u', 'l'};

class Point implements Comparable<Point> {
    int row, col, distance;
    String instruction;

    Point(int row, int col, int distance, String instruction) {
        this.row = row;
        this.col = col;
        this.distance = distance;
        this.instruction = instruction;
    }

    @Override
    public int compareTo(Point o) {
        return this.distance == o.distance ? this.instruction.compareTo(o.instruction) : this.distance - o.distance;
    }
}

public String findShortestWay(int[][] maze, int[] ball, int[] hole) {
    int m = maze.length, n = maze[0].length;
    boolean[][] visited = new boolean[m][n];

    Queue<Point> pq = new PriorityQueue<>();
    pq.offer(new Point(ball[0], ball[1], 0, ""));

    while (!pq.isEmpty()) {
        Point p = pq.poll();
        if (p.row == hole[0] && p.col == hole[1]) {
            return p.instruction;
        }

        visited[p.row][p.col] = true;

        for (int i = 0; i < DIRECTIONS.length; i++) {
            int[] d = DIRECTIONS[i];
            int r = p.row + d[0], c = p.col + d[1];
            int distance = p.distance;

            // keeps rolling until hitting a wall or the hole
            while (r >= 0 && r < m && c >= 0 && c < n && maze[r][c] == 0) {
                distance++;
                if (r == hole[0] && c == hole[1]) {
                    r += d[0];
                    c += d[1];
                    break;
                }
                r += d[0];
                c += d[1];
            }
            r -= d[0];
            c -= d[1];

            if (!visited[r][c]) {
                pq.offer(new Point(r, c, distance, p.instruction + INSTRUCTIONS[i]));
            }
        }
    }
    return "impossible";
}
{% endhighlight %}

## Within K Steps

[Cheapest Flights Within K Stops][cheapest-flights-within-k-stops]

{% highlight java %}
public int findCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
    // buils graph
    Map<Integer, List<int[]>> graph = new HashMap<>();
    for (int[] f : flights) {
        graph.computeIfAbsent(f[0], k -> new ArrayList<>()).add(new int[]{f[1], f[2]});
    }

    // Dijkstra
    int[] node = {src, 0, K};  // city, price, stops
    Queue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[1]));
    pq.add(node);

    while (!pq.isEmpty()) {
        node = pq.poll();

        if (node[0] == dst) {
            return node[1];
        }

        if (node[2] >= 0) {
            if (!graph.containsKey(node[0])) {
                continue;
            }

            for (int[] pair : graph.get(node[0])) {
                // some nodes may have the same city due to different paths
                pq.offer(new int[]{pair[0], node[1] + pair[1], node[2] - 1});
            }
        }
    }

    return -1;
}
{% endhighlight %}

## Variations

Cost function is monotonically increasing/decreasing.

[Path with Minimum Effort][path-with-minimum-effort]

{% highlight java %}
{% raw %}
private static final int[][] DIRECTIONS = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
{% endraw %}

public int minimumEffortPath(int[][] heights) {
    int m = heights.length, n = heights[0].length;
    Set<Integer> visited = new HashSet<>();

    // i, j, effort
    Queue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[2]));
    pq.offer(new int[]{0, 0, 0});

    while (!pq.isEmpty()) {
        int[] node = pq.poll();
        int i = node[0], j = node[1], e = node[2];
        if (i == m - 1 && j == n - 1) {
            return e;
        }

        if (visited.add(i * n + j)) {
            for (int[] d : DIRECTIONS) {
                int r = i + d[0], c = j + d[1];
                if (r >= 0 && r < m && c >= 0 && c < n && !visited.contains(r * n + c)) {
                    pq.offer(new int[]{r, c, Math.max(Math.abs(heights[r][c] - heights[i][j]), e)});
                }
            }
        }
    }
    return -1;
}
{% endhighlight %}

[Path With Maximum Minimum Value][path-with-maximum-minimum-value]

[Swim in Rising Water][swim-in-rising-water]

[Campus Bikes II][campus-bikes-ii]

{% highlight java %}
public int assignBikes(int[][] workers, int[][] bikes) {
    // worker, bike mask, distance
    Queue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[2]));
    pq.offer(new int[]{0, 0, 0});

    Set<String> visited = new HashSet<>();
    while (!pq.isEmpty()){
        int[] node = pq.poll();
        int worker = node[0], mask = node[1], d = node[2];

        if (visited.add(worker + "#" + mask)) {
            if (worker == workers.length) {
                return d;
            }

            for (int i = 0; i < bikes.length; i++) {
                // i-th bike is available
                if ((mask & (1 << i)) == 0) {
                    pq.offer(new int[]{worker + 1, mask | (1 << i), d + distance(workers[worker], bikes[i])});
                }
            }
        }

    }
    return -1;
}

private int distance(int[] p1, int[] p2) {
    return Math.abs(p1[0] - p2[0]) + Math.abs(p1[1] - p2[1]);
}
{% endhighlight %}

# Floyd-Warshall Algorithm

[Floyd-Warshall algorithm](https://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall_algorithm) is an algorithm for finding shortest paths in a directed weighted graph with positive or negative edge weights (but with no negative cycles). A single execution of the algorithm will find the lengths (summed weights) of shortest paths between ***all*** pairs of vertices.

[Course Schedule IV][course-schedule-iv]

{% highlight java %}
// Floyd–Warshall Algorithm
public List<Boolean> checkIfPrerequisite(int numCourses, int[][] prerequisites, int[][] queries) {
    boolean[][] graph = new boolean[numCourses][numCourses];
    for (int[] p : prerequisites) {
        graph[p[0]][p[1]] = true;
    }

    for (int k = 0; k < numCourses; k++) {
        for (int i = 0; i < numCourses; i++) {
            for (int j = 0; j < numCourses; j++) {
                graph[i][j] = graph[i][j] || (graph[i][k] && graph[k][j]);
            }
        }
    }

    List<Boolean> answer = new ArrayList<>();
    for (int[] q : queries) {
        answer.add(graph[q[0]][q[1]]);
    }
    return answer;
}
{% endhighlight %}

[campus-bikes-ii]: https://leetcode.com/problems/campus-bikes-ii/
[course-schedule-iv]: https://leetcode.com/problems/course-schedule-iv/
[cheapest-flights-within-k-stops]: https://leetcode.com/problems/cheapest-flights-within-k-stops/
[number-of-restricted-paths-from-first-to-last-node]: https://leetcode.com/problems/number-of-restricted-paths-from-first-to-last-node/
[path-with-maximum-minimum-value]: https://leetcode.com/problems/path-with-maximum-minimum-value/
[path-with-maximum-probability]: https://leetcode.com/problems/path-with-maximum-probability/
[path-with-minimum-effort]: https://leetcode.com/problems/path-with-minimum-effort/
[swim-in-rising-water]: https://leetcode.com/problems/swim-in-rising-water/
[the-maze-iii]: https://leetcode.com/problems/the-maze-iii/
