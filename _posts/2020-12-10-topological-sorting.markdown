---
layout: post
title:  "Topological Sorting"
tags: graph
usemathjax: true
---

[Topological sorting](https://en.wikipedia.org/wiki/Topological_sorting): In computer science, a topological sort or topological ordering of a directed graph is a linear ordering of its vertices such that for every directed edge `uv` from vertex `u` to vertex `v`, `u` comes before `v` in the ordering.

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

[Parallel Courses III][parallel-courses-iii]

{% highlight java %}
public int minimumTime(int n, int[][] relations, int[] time) {
    int[] indegree = new int[n + 1];
    List<Integer>[] graph = new List[n + 1];
    for (int i = 0; i <= n; i++) {
        graph[i] = new ArrayList<>();
    }
    for (int[] r : relations) {
        graph[r[0]].add(r[1]);
        indegree[r[1]]++;
    }

    // minimum time to reach this node
    int[] cost = new int[n + 1];
    Queue<Integer> q = new LinkedList<>();
    for (int i = 1; i <= n; i++) {
        if (indegree[i] == 0) {
            q.offer(i);
            cost[i] = time[i - 1];
        }
    }

    while (!q.isEmpty()) {
        int node = q.poll();
        for (int neighbor : graph[node]) {
            cost[neighbor] = Math.max(cost[neighbor], cost[node] + time[neighbor - 1]);
            if (--indegree[neighbor] == 0) {
                q.offer(neighbor);
            }
        }
    }
    return Arrays.stream(cost).max().getAsInt();
}
{% endhighlight %}

[Largest Color Value in a Directed Graph][largest-color-value-in-a-directed-graph]

{% highlight java %}
public int largestPathValue(String colors, int[][] edges) {
    int n = colors.length();
    List<Integer>[] graph = new List[n];
    for (int i = 0; i < n; i++) {
        graph[i] = new ArrayList<>();
    }

    int[] indegree = new int[n];
    for (int[] e : edges) {
        graph[e[0]].add(e[1]);
        indegree[e[1]]++;
    }

    // dp[i][j]: max count of i-th node, j-th color
    int[][] dp = new int[n][26];

    // zero indegree
    Queue<Integer> q = new LinkedList<>();
    for (int i = 0; i < n; i++) {
        if (indegree[i] == 0) {
            q.offer(i);
            dp[i][colors.charAt(i) - 'a'] = 1;
        }
    }

    int count = 0, max = 0;
    while (!q.isEmpty()) {
        int node = q.poll();
        count++;

        // if max is updated at this node
        // then the color of this node must be the most frequent
        max = Math.max(max, dp[node][colors.charAt(node) - 'a']);

        for (int child : graph[node]) {
            // updates dp of child node
            for (int i = 0; i < 26; i++) {
                dp[child][i] = Math.max(dp[child][i], dp[node][i] + (colors.charAt(child) - 'a' == i ? 1 : 0));
            }

            if (--indegree[child] == 0) {
                q.offer(child);
            }
        }
    }
    return count == n ? max : -1;
}
{% endhighlight %}

# DFS

[Sort Items by Groups Respecting Dependencies][sort-items-by-groups-respecting-dependencies]

{% highlight java %}
private List<Integer>[] graph;
private int[] indegree;
private int n;

public int[] sortItems(int n, int m, int[] group, List<List<Integer>> beforeItems) {
    this.n = n;

    // items belong to the same group are virtually wrapped together
    // n <= group node index <= m
    //
    // if k < n
    //   if k is in a group
    //     - graph[k] is inner-group after items of item k
    //     - indegree[k] is inner-group indegree of item k
    //   else if k is not in any group
    //     - graph[k] is after items of item k
    //     - indegree[k] is indegree of item k
    // otherwise
    //   - graph[k] is members of group (k - n)
    //   - indegree[k] is indegree of group (k - n)
    this.graph = new ArrayList[n + m];
    this.indegree = new int[n + m];

    for (int i = 0; i < graph.length; i++) {
        graph[i] = new ArrayList<>();
    }

    for (int i = 0; i < n; i++) {
        // wraps nodes that belong to the same group
        // the new index becomes: n + group[i]
        if (group[i] >= 0) {
            graph[n + group[i]].add(i);
            // marks indegree as 1
            // so when we scan to find the 0-indegree node
            // if index < n, it guarantees to be a node that doesn't belong to a group
            indegree[i]++;
        }
    }

    for (int i = 0; i < n; i++) {
        int ig = map(group, i);
        for (int b : beforeItems.get(i)) {
            int bg = map(group, b);

            // current item and its before item are in the same group
            if (bg == ig) {
                graph[b].add(i);
                // remember, inner-group indegrees start from 1
                indegree[i]++;
            } else {
                // either i is not in a group
                // or i and b are in different groups
                // this is for "external" links
                graph[bg].add(ig);
                indegree[ig]++;
            }
        }
    }

    List<Integer> list = new ArrayList<>();
    for (int i = 0; i < indegree.length; i++) {
        // when indegree[i] == 0
        // if i < n, the node i doesn't belong to any group (see comments above)
        // if i >= n, the dfs topologically sorts the members in the group
        if (indegree[i] == 0) {
            dfs(i, list);
        }
    }

    return list.size() == n ? list.stream().mapToInt(i -> i).toArray() : new int[0];
}

// maps the item to its group node index it it belongs to a group
// otherwise returns its item index
private int map(int[] group, int item) {
    return group[item] < 0 ? item : group.length + group[item];
}

private void dfs(int curr, List<Integer> list) {
    if (curr < n) {
        list.add(curr);
    }

    // marks it as visited (-1)
    // so the start condition of the dfs indegree[node] == 0 won't be met again
    indegree[curr]--;

    // if curr < n, and
    // - curr doesn't belong to any group, then graph[curr] is its after items
    // - curr belongs to a group, then group[curr] is the after items of the same groups
    // otherwise, graph[curr] are the members of the group, and only members with degree 1 will be picked
    for (int child : graph[curr]) {
        // remember, inner-group indegrees are based on 1
        if (--indegree[child] == 0) {
            dfs(child, list);
        }
    }
}
{% endhighlight %}

# Two-level Topological Sort

[Sort Items by Groups Respecting Dependencies][sort-items-by-groups-respecting-dependencies]

{% highlight java %}
private List<Integer>[] groupGraph, itemGraph;
private int[] groupsIndegree, itemsIndegree;

public int[] sortItems(int n, int m, int[] group, List<List<Integer>> beforeItems) {
    // maps items with -1 group to new isolated groups
    // and updates the group count
    for (int i = 0; i < group.length; i++) {
        if (group[i] < 0) {
            group[i] = m++;
        }
    }

    this.itemGraph = new ArrayList[n];
    this.groupGraph = new ArrayList[m];

    for (int i = 0; i < n; i++) {
        itemGraph[i] = new ArrayList<>();
    }
    for (int i = 0; i < m; i++) {
        groupGraph[i] = new ArrayList<>();
    }

    this.itemsIndegree = new int[n];
    this.groupsIndegree = new int[m];

    // builds items
    for (int i = 0; i < n; i++) {
        for (int item : beforeItems.get(i)) {
            itemGraph[item].add(i);
            itemsIndegree[i]++;
        }
    }

    // builds group
    for (int i = 0; i < group.length; i++) {
        int toGroup = group[i];
        for (int fromItem : beforeItems.get(i)) {
            int fromGroup = group[fromItem];
            if (fromGroup != toGroup) {
                groupGraph[fromGroup].add(toGroup);
                groupsIndegree[toGroup]++;
            }
        }
    }

    // topological sort
    List<Integer> itemsList = sort(itemGraph, itemsIndegree, n);
    List<Integer> groupsList = sort(groupGraph, groupsIndegree, m);

    // detects if there are any cycles
    if (groupsList.isEmpty() || itemsList.isEmpty()) {
        return new int[0];
    }

    // maps items to their group
    List<Integer>[] membersInGroup = new ArrayList[m];
    for (int i = 0; i < m; i++) {
        membersInGroup[i] = new ArrayList<>();
    }

    for (int item : itemsList) {
        membersInGroup[group[item]].add(item);
    }

    int[] result = new int[n];
    int index = 0;
    for (int g : groupsList) {
        List <Integer> items = membersInGroup[g];
        for (int item : items) {
            result[index++] = item;
        }
    }
    return result;
}

private List<Integer> sort(List<Integer>[] graph, int[] indegree, int count) {
    List <Integer> list = new ArrayList<>();
    Queue <Integer> q = new LinkedList();
    for (int i = 0; i < graph.length; i++) {
        if (indegree[i] == 0) {
            q.offer(i);
        }
    }

    while (!q.isEmpty()) {
        int node = q.poll();
        count--;
        list.add(node);
        for (int neighbor : graph[node]) {
            indegree[neighbor]--;
            if (indegree[neighbor] == 0) {
                q.offer(neighbor);
            }
        }
    }
    return count == 0 ? list : Collections.EMPTY_LIST;
}
{% endhighlight %}

# Cycle Detection

A topological ordering is possible iff the graph has no directed cycles, that is, iff it is a directed acyclic graph (DAG).

**White-Gray-Black DFS**

[Course Schedule II][course-schedule-ii]

{% highlight java %}
private Map<Integer, List<Integer>> graph;
private Color[] color;
private List<Integer> order;

private enum Color {
    WHITE,  // node is not processed yet
    GRAY,  // node is being processed
    BLACK  // node and all its descendants are processed
}

public int[] findOrder(int numCourses, int[][] prerequisites) {        
    color = new Color[numCourses];
    order = new ArrayList<>();

    // by default all nodes are WHITE
    Arrays.fill(color, Color.WHITE);

    graph = new HashMap<>();
    for (int[] p : prerequisites) {
        graph.computeIfAbsent(p[1], k -> new ArrayList<>()).add(p[0]);
    }

    // dfs unprocessed node
    for (int i = 0; i < numCourses; i++) {
        if (color[i] == Color.WHITE) {
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
    color[node] = Color.GRAY;

    if (graph.containsKey(node)) {
        for (int neighbor : graph.get(node)) {
            // detects cycle; stops
            if ((color[neighbor] == Color.WHITE && !dfs(neighbor)) || color[neighbor] == Color.GRAY) {
                return false;
            }
        }
    }

    // finishes the recursion
    color[node] = Color.BLACK;
    order.add(node);
    return true;
}
{% endhighlight %}

[Detect Cycles in 2D Grid][detect-cycles-in-2d-grid]

Don't visit the last cell.

## Cycle Length

[Maximum Employees to Be Invited to a Meeting][maximum-employees-to-be-invited-to-a-meeting]

{% highlight java %}
public int maximumInvitations(int[] favorite) {
    int n = favorite.length;
    // for every index i, there is a directed edge from i to favorite[i]
    // topological sort picks out acyclic part
    int[] indegrees = new int[n];
    for (int i = 0; i < n; i++) {
        indegrees[favorite[i]]++;
    }

    // enqueues leaves
    boolean[] removed = new boolean[n];
    Queue<Integer> q = new LinkedList<>();
    for (int i = 0; i < n; ++i) {
        if (indegrees[i] == 0) {
            removed[i] = true;
            q.offer(i);
        }
    }

    // dp[i]: longest path from leaves to i exclusively
    int[] dp = new int[n]; 
    while (!q.isEmpty()) {
        int node = q.poll(), to = favorite[node];
        dp[to] = Math.max(dp[to], dp[node] + 1);

        if (--indegrees[to] == 0) {
            removed[to] = true;
            q.offer(to);
        }
    }

    // now remaining nodes are all cycles
    // checks each cycle's length.
    // case 1: cycle length == 2. i.e. two nodes like each other
    // finds the longest arm on each side
    int count1 = 0;
    // case 2: cycle length > 2
    // finds the longest cycle
    int count2 = 0;
    for (int i = 0; i < n; i++) {
        if (!removed[i]) {
            // gets the cycle length
            int length = 0;
            for (int j = i; !removed[j]; j = favorite[j]) {
                removed[j] = true;
                length++;
            }

            if (length == 2) {
                // there can be multiple isolated connected components of case 1
                // adds them all up
                count1 += 2 + dp[i] + dp[favorite[i]];
            } else {
                count2 = Math.max(count2, length);
            }
        }
    }
    return Math.max(count1, count2);
}
{% endhighlight %}

[Critical Connections in a Network][critical-connections-in-a-network]

{% highlight java %}
private List<List<Integer>> graph;
private Set<List<Integer>> set;

public List<List<Integer>> criticalConnections(int n, List<List<Integer>> connections) {
    this.graph = new ArrayList<>(n);
    for (int i = 0; i < n; i++) {
        graph.add(new ArrayList<>());
    }

    for (List<Integer> e : connections) {
        int s1 = e.get(0), s2 = e.get(1);
        graph.get(s1).add(s2);
        graph.get(s2).add(s1);
    }

    this.set = new HashSet<>(connections);

    // the depth of a node during DFS
    int[] rank = new int[n];
    Arrays.fill(rank, -2);

    dfs(rank, 0, 0);
    return new ArrayList<>(set);
}

// returns min depth of all nodes in the subtree
private int dfs(int[] rank, int node, int depth) {
    if (rank[node] >= 0) {
        return rank[node];
    }

    rank[node] = depth;

    int min = depth;
    for (int neighbor: graph.get(node)) {
        // skips parent
        // here's the reason why rank is initialized with -2, rather than -1:
        // if depth == 0, then its child node will be skipped by mistake
        if (rank[neighbor] == depth - 1) {
            continue;
        }

        int minSub = dfs(rank, neighbor, depth + 1);
        min = Math.min(min, minSub);

        // cycle detected
        if (minSub <= depth) {
            // discards the edge (node, neighbor) in the cycle
            set.remove(Arrays.asList(node, neighbor));
            set.remove(Arrays.asList(neighbor, node));
        }
    }
    return min;
}
{% endhighlight %}

This algorith  is very similar to Tarjan's Algorithm. The meaning of `rank` and `dfn/low` is slightly different, but in essence they are closely related.

![Example](https://assets.leetcode.com/uploads/2019/09/03/1537_ex1_2.png)

```
node: 0
rank: 0,-2,-2,-2

node: 1
rank: 0,1,-2,-2

node: 2
rank: 0,1,2,-2

neighbor: 0
depth: 2
minSub: 0
rank: 0,1,2,-2

neighbor: 2
depth: 1
minSub: 0
rank: 0,1,2,-2

node: 3
rank: 0,1,2,2

neighbor: 3
depth: 1
minSub: 2
rank: 0,1,2,2

neighbor: 1
depth: 0
minSub: 0
rank: 0,1,2,2

neighbor: 2
depth: 0
minSub: 2
rank: 0,1,2,2
```

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

            // when the neighbor has 1 edge, there are two possibilities
            // - after this for-loop, the neighbor will be the only remaining node.
            //   that means the node is the only centroid.
            //   adding it to the newLeaves list won't affect the result,
            //   because the while-loop condition n > 2 will not be met.
            //   the centroid is added to the newLeaves list even though the for-loop is not complete.
            // - after this for-loop, more than 1 node will be left.
            //   this occurs at any round of 2-centroid case, or any round but the last of 1-centroid case.
            //   the node is added to the newLeaves list at the last iteration of the for-loop
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

[Count Subtrees With Max Distance Between Cities][count-subtrees-with-max-distance-between-cities]

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

# Longest Path

Longest path in a DAG can be solved by topological sorting.

[Longest Increasing Path in a Matrix][longest-increasing-path-in-a-matrix]

Another solution is DFS + memoization

# Count

[Count Ways to Build Rooms in an Ant Colony][count-ways-to-build-rooms-in-an-ant-colony]

$$ \frac{n!}{\prod{s_i}} $$

where \\(s_i\\) is the size of the subtree at the i-th node.

[count-subtrees-with-max-distance-between-cities]: https://leetcode.com/problems/count-subtrees-with-max-distance-between-cities/
[count-ways-to-build-rooms-in-an-ant-colony]: https://leetcode.com/problems/count-ways-to-build-rooms-in-an-ant-colony/
[course-schedule-ii]: https://leetcode.com/problems/course-schedule-ii/
[critical-connections-in-a-network]: https://leetcode.com/problems/critical-connections-in-a-network/
[detect-cycles-in-2d-grid]: https://leetcode.com/problems/detect-cycles-in-2d-grid/
[largest-color-value-in-a-directed-graph]: https://leetcode.com/problems/largest-color-value-in-a-directed-graph/
[longest-increasing-path-in-a-matrix]: https://leetcode.com/problems/longest-increasing-path-in-a-matrix/
[maximum-employees-to-be-invited-to-a-meeting]: https://leetcode.com/problems/maximum-employees-to-be-invited-to-a-meeting/
[minimum-height-trees]: https://leetcode.com/problems/minimum-height-trees/
[parallel-courses-iii]: https://leetcode.com/problems/parallel-courses-iii/
[sequence-reconstruction]: https://leetcode.com/problems/sequence-reconstruction/
[sort-items-by-groups-respecting-dependencies]: https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/
[tree-diameter]: https://leetcode.com/problems/tree-diameter/
