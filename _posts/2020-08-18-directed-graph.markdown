---
layout: post
title:  "Directed Graph"
tags: graph
---
## Degree
[Find the Town Judge][find-the-town-judge]

{% highlight java %}
public int findJudge(int N, int[][] trust) {
    int[] degree = new int[N];
    for (int[] t : trust) {
        degree[t[0] - 1]--;
        degree[t[1] - 1]++;
    }

    for (int i = 0; i < N; i++) {
        if (degree[i] == N - 1) {
            return i + 1;
        }
    }
    return -1;
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
    ratio.put(u, ratio.get(u) * ratio.get(p));  // gp = p * ratio(p) = ratio(u) * ratio(p)
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

[evaluate-division]: https://leetcode.com/problems/evaluate-division/
[find-the-town-judge]: https://leetcode.com/problems/find-the-town-judge/
