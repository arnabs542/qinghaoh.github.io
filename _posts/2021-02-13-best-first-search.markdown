---
layout: post
title:  "Best First Search"
tags: graph
---
# A\* Search

[A\* search algorithm](https://en.wikipedia.org/wiki/A*_search_algorithm) selects the path that minimizes

```f(n) = g(n) + h(n)```

where 
* `n` is the next node on the path
* `g(n)` is the cost of the path from the start node to `n`
* `h(n)` is a heuristic function that estimates the cost of the cheapest path from `n` to the goal
  * problem-specific
  * admissible: it never overestimates the cost of reaching the goal - A* is guaranteed to return a least-cost path
  * monotone/consistent: its estimate is always less than or equal to the estimated distance from any neighbouring vertex to the goal, plus the cost of reaching that neighbour - A* is guaranteed to find an optimal path without processing any node more than once and A* is equivalent to running Dijkstra's algorithm with the reduced cost `d'(x, y) = d(x, y) + h(y) − h(x)`

[Shortest Path in Binary Matrix][shortest-path-in-binary-matrix]

{% highlight java %}
{% raw %}
private static final int[][] DIRECTIONS = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}, {1, 1}, {-1, 1}, {-1, -1}, {1, -1}};
{% endraw %}
private static final int MAX = 200;

public int shortestPathBinaryMatrix(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    if (grid[m - 1][n - 1] == 1) {
        return -1;
    }

    // x, y, g, f
    Queue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[3]));
    pq.offer(new int[]{0, 0, 1, 1 + Math.max(m, n)});

    Map<Integer, Integer> minDist = new HashMap<>();

    while (!pq.isEmpty()) {
        int[] node = pq.poll();
        int r = node[0], c = node[1];
        if (r < 0 || r == m || c < 0 || c == n || grid[r][c] == 1) {
            continue;
        }
        if (r == m - 1 && c == n - 1) {
            return node[2];
        }

        if (minDist.getOrDefault(r * n + c, MAX) <= node[2]) {
            continue;
        }

        minDist.put(r * n + c, node[2]);

        for (int[] d : DIRECTIONS) {
            int g = node[2] + 1;

            // h(n) is a heuristic function that 
            // estimates the cost of the cheapest path from node to the goal
            // here h(n) = diagonal distance (max norm)
            int h = Math.max(Math.abs(m - 1 - r), Math.abs(n - 1 - c));

            pq.offer(new int[]{r + d[0], c + d[1], g, g + h});
        }
    }

    return -1;
}
{% endhighlight %}

[shortest-path-in-binary-matrix]: https://leetcode.com/problems/shortest-path-in-binary-matrix/
