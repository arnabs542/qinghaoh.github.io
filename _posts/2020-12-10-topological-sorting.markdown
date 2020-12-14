---
layout: post
title:  "Topological Sorting"
tags: graph
---

[Topological sorting](https://en.wikipedia.org/wiki/Topological_sorting)

# Kahn's Algorithm

[Course Schedule II][course-schedule-ii]

{% highlight java %}
// Kahn's algorithm
public int[] findOrder(int numCourses, int[][] prerequisites) {
    Map<Integer, List<Integer>> graph = new HashMap<>();
    int[] indegree = new int[numCourses];

    for (int[] p : prerequisites) {
        graph.computeIfAbsent(p[1], k -> new ArrayList<>()).add(p[0]);
        indegree[p[0]]++;
    }

    // zero indegree
    Queue<Integer> q = new LinkedList<>();
    for (int i = 0; i < numCourses; i++) {
        if (indegree[i] == 0) {
            q.offer(i);
        }
    }

    int[] order = new int[numCourses];
    int count = 0;
    while (!q.isEmpty()) {
        int course = q.poll();
        if (graph.containsKey(course)) {
            for (int neighbor : graph.get(course)) {
                if (--indegree[neighbor] == 0) {
                    q.offer(neighbor);
                }
            }
        }
        order[count++] = course;
    }

    return count == numCourses ? order : new int[0];
}
{% endhighlight %}

# DFS

A topological ordering is possible if and only if the graph has no directed cycles, that is, if it is a directed acyclic graph (DAG)

White-Gray-Black DFS

[Course Schedule II][course-schedule-ii]

{% highlight java %}
private final int WHITE = 1;  // node is not processed yet
private final int GRAY = 2;  // node is being processed
private final int BLACK = 3;  // node and all its descendants are processed
private Map<Integer, List<Integer>> graph;
private int[] color;
private List<Integer> order;

public int[] findOrder(int numCourses, int[][] prerequisites) {        
    color = new int[numCourses];
    order = new ArrayList<>();

    // By default all nodes are WHITE
    Arrays.fill(color, WHITE);

    graph = new HashMap<>();
    for (int[] p : prerequisites) {
        graph.computeIfAbsent(p[1], k -> new ArrayList<>()).add(p[0]);
    }

    // dfs unprocessed node
    for (int i = 0; i < numCourses; i++) {
        if (color[i] == WHITE) {
            if (!dfs(i)) {
                return new int[0];
            }
        }
    }

    Collections.reverse(order);
    return order.stream().mapToInt(Integer::valueOf).toArray();
}

private boolean dfs(int node) {
    // starts the recursion
    color[node] = GRAY;

    boolean result = true;
    if (graph.containsKey(node)) {
        for (int neighbor : graph.get(node)) {
            if (color[neighbor] == WHITE) {
                result = result && dfs(neighbor);
            } else if (color[neighbor] == GRAY) {
                // detects cycle; stops
                return false;
            }
        }
    }

    // finishes the recursion
    color[node] = BLACK;
    order.add(node);
    return result;
}
{% endhighlight %}

# Centroids

[Minimum Height Trees][minimum-height-trees]

A tree is an undirected graph in which any two vertices are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.

The number of centroids of a tree is no more than 2.

{% highlight java %}
public List<Integer> findMinHeightTrees(int n, int[][] edges) {
    if (n == 1) {
        return Collections.singletonList(0);
    }

    List<Set<Integer>> tree = new ArrayList<>();
    for (int i = 0; i < n; i++) {
        tree.add(new HashSet<>());
    }
    for (int[] e : edges) {
        tree.get(e[0]).add(e[1]);
        tree.get(e[1]).add(e[0]);
    }

    // finds the leaves
    List<Integer> leaves = new ArrayList<>();
    for (int i = 0; i < n; i++) {
        if (tree.get(i).size() == 1) {
            leaves.add(i);
        }
    } 

    // trims the leaves until centroids
    while (n > 2) {
        n -= leaves.size();
        List<Integer> newLeaves = new ArrayList<>();
        for (int leaf : leaves) {
            int neighbor = tree.get(leaf).iterator().next();
            tree.get(neighbor).remove(leaf);
            if (tree.get(neighbor).size() == 1) {
                newLeaves.add(neighbor);
            }
        }
        leaves = newLeaves;
    }
    return leaves;
}
{% endhighlight %}

[Tree Diameter][tree-diameter]

# Uniqueness

[Uniqueness](https://en.wikipedia.org/wiki/Topological_sorting#Uniqueness)

[Hamiltonian path](https://en.wikipedia.org/wiki/Hamiltonian_path)

In the mathematical field of graph theory, a Hamiltonian path (or traceable path) is a path in an undirected or directed graph that visits each vertex exactly once.

If a topological sort has the property that all pairs of consecutive vertices in the sorted order are connected by edges, then these edges form a directed Hamiltonian path in the DAG.

If a Hamiltonian path exists, the topological sort order is unique; no other order respects the edges of the path. Conversely, if a topological sort does not form a Hamiltonian path, the DAG will have two or more valid topological orderings, for in this case it is always possible to form a second valid ordering by swapping two consecutive vertices that are not connected by an edge to each other.

[Sequence Reconstruction][sequence-reconstruction]

{% highlight java %}
public boolean sequenceReconstruction(int[] org, List<List<Integer>> seqs) {
    if (seqs.size() == 0){
        return false;
    }

    int n = org.length;
    int[] index = new int[n + 1];  // index of element in org
    boolean[] pairs = new boolean[n];  // pairs[i]: org[i] and org[i + 1] make a pair

    for (int i = 0; i < n; i++) {
        index[org[i]] = i;
    }

    for(List<Integer> s : seqs) {
        for (int i = 0; i < s.size(); i++) {
            if (s.get(i) > n || s.get(i) < 0) {
                return false;
            }

            // all sequences in seqs should be a subsequence of org
            if (i > 0 && index[s.get(i - 1)] >= index[s.get(i)]) {
                return false;
            }

            // all pairs of consecutive elements in org should be in some sequence in seqs
            if (i > 0 && index[s.get(i - 1)] + 1 == index[s.get(i)]) {
                pairs[index[s.get(i - 1)]] = true;
            }
        }
    }

    for (int i = 0; i < n - 1; i++) {
        if (!pairs[i]) {
            return false;
        }
    }

    return true;
}
{% endhighlight %}

[course-schedule-ii]: https://leetcode.com/problems/course-schedule-ii/
[minimum-height-trees]: https://leetcode.com/problems/minimum-height-trees/
[sequence-reconstruction]: https://leetcode.com/problems/sequence-reconstruction/
[tree-diameter]: https://leetcode.com/problems/tree-diameter/
