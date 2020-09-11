---
layout: post
title:  "Disjoint Set"
tags: union
---
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

[redundant-connection]: https://leetcode.com/problems/redundant-connection/
