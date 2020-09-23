---
layout: post
title:  "Directed Graph"
tags: graph
---
[Directed graph](https://en.wikipedia.org/wiki/Directed_graph)

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
private Map<String, Map<String, Double>> graph = new HashMap<>();

public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
    buildGraph(equations, values);

    double[] result = new double[queries.size()];
    for (int i = 0; i < queries.size(); i++) {
        List<String> q = queries.get(i);
        result[i] = getPathWeight(q.get(0), q.get(1), new HashSet<>());
    }
    return result;
}

private double getPathWeight(String start, String end, Set<String> visited) {
    if (!graph.containsKey(start)) {
        return -1.0;
    }

    if (graph.get(start).containsKey(end)) {
        return graph.get(start).get(end);
    }

    // dfs
    visited.add(start);
    for (Map.Entry<String, Double> neighbour : graph.get(start).entrySet()) {
        if (!visited.contains(neighbour.getKey())) {
            double productWeight = getPathWeight(neighbour.getKey(), end, visited);
            if (productWeight != -1.0) {
                return neighbour.getValue() * productWeight;
            }
        }
    }

    return -1.0;
}

// A / B = k
// A : (B : k)
// B : (A : 1 / k)
private void buildGraph(List<List<String>> equations, double[] values) {
    for (int i = 0; i < equations.size(); i++) {
        List<String> e = equations.get(i);

        graph.putIfAbsent(e.get(0), new HashMap<>());
        graph.get(e.get(0)).put(e.get(1), values[i]);

        graph.putIfAbsent(e.get(1), new HashMap<>());
        graph.get(e.get(1)).put(e.get(0), 1 / values[i]);
    }
}
{% endhighlight %}

[evaluate-division]: https://leetcode.com/problems/evaluate-division/
[find-the-town-judge]: https://leetcode.com/problems/find-the-town-judge/
