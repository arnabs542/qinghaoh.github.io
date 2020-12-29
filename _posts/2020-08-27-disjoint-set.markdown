---
layout: post
title:  "Disjoint Set"
tags: union
---

Time Complexity:

* find: `O(α(n))`
* union: `O(α(n))`

where α(n) is the extremely slow-growing [inverse Ackermann function](https://en.wikipedia.org/wiki/Ackermann_function#Inverse).

[Redundante Connection][redundant-connection]

{% highlight java %}
private int[] parent;

public int[] findRedundantConnection(int[][] edges) {
    parent = new int[edges.length + 1];

    for (int[] edge : edges) {
        if (!union(edge[0], edge[1])) {
            return edge;
        }
    }

    return null;
}

private int find(int u) {
    return parent[u] == 0 ? u : find(parent[u]);
}

private boolean union(int u, int v) {
    int uset = find(u);
    int vset = find(v);

    if (uset == vset) {
        return false;
    }

    parent[uset] = vset;
    return true;
}
{% endhighlight %}

[Evaluate Division][evaluate-division]

{% highlight java %}
Map<String, String> parent = new HashMap<>();
Map<String, Double> ratio = new HashMap<>();  // parent / node

public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
    for (int i = 0; i < values.length; i++) {
        List<String> e = equations.get(i);
        union(e.get(0), e.get(1), values[i]);
    }

    double[] result = new double[queries.size()];
    for (int i = 0; i < queries.size(); i++) {
        List<String> q = queries.get(i);
        result[i] = query(q.get(0), q.get(1));
    }
    return result;
}

private String find(String u) {
    if (!parent.containsKey(u)) {
        ratio.put(u, 1.0);
        return u;
    }

    // path compression
    String p = parent.get(u);
    String gp = find(p);
    parent.put(u, gp);
    ratio.put(u, ratio.get(u) * ratio.get(p));  // gp = p * ratio(p) = u * ratio(u) * ratio(p)
    return gp;
}

// u / v = value
private void union(String u, String v, double value) {
    String uset = find(u);
    String vset = find(v);

    parent.put(vset, uset);
    // ratio = uset / vset
    //   = u * ratio(u) / (v * ratio(v))
    //   = value * ratio(u) / ratio(v)
    ratio.put(vset, value * ratio.get(u) / ratio.get(v));
}

private double query(String s1, String s2) {
    if (!ratio.containsKey(s1) || !ratio.containsKey(s2)) {
        return -1.0;
    }

    String p1 = find(s1), p2 = find(s2);
    if (!p1.equals(p2)) {
        return -1.0;
    }

    // s1 / s2 = p / ratio(s1) / (p / ratio(s2))
    //   = ratio(s2) / ratio(s1)
    return ratio.get(s2) / ratio.get(s1);
}
{% endhighlight %}

[Number of Connected Components in an Undirected Graph][number-of-connected-components-in-an-undirected-graph]

{% highlight java %}
public int countComponents(int n, int[][] edges) {
    this.parent = new int[n];
    Arrays.fill(this.parent, -1);

    for (int[] e : edges) {
        if (union(e[0], e[1])) {
            n--;
        }
    }

    return n;
}
{% endhighlight %}

[evaluate-division]: https://leetcode.com/problems/evaluate-division/
[number-of-connected-components-in-an-undirected-graph]: https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/
[redundant-connection]: https://leetcode.com/problems/redundant-connection/
