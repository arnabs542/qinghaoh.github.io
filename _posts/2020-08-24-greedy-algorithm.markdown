---
layout: post
title:  "Greedy Algorithm"
tags: greedy
---
[Flower Planting With No Adjacent][flower-planting-with-no-adjacent]

No node has more than 3 neighbors, so there's always one possible color to pick.

{% highlight java %}
private final int numFlowers = 4;

public int[] gardenNoAdj(int N, int[][] paths) {
    // constructs the graph
    Map<Integer, List<Integer>> graph = new HashMap<>();
    for (int i = 1; i <= N; i++) {
        graph.put(i, new ArrayList<>());
    }
    for (int[] p : paths) {
        graph.get(p[0]).add(p[1]);
        graph.get(p[1]).add(p[0]);
    }

    int[] result = new int[N];
    for (int i = 1; i <= N; i++) {
        // flowers[i] indicates whether the (i + 1)th flower has been picked
        boolean[] flowers = new boolean[numFlowers];

        // finds the neighbors who have picked a flower
        for (int garden : graph.get(i)) {
            if (result[garden - 1] > 0) {
                flowers[result[garden - 1] - 1] = true;
            }
        }

        // picks the first available flower
        for (int f = 1; f <= numFlowers; f++) {
            if (!flowers[f - 1]) {
                result[i - 1] = f;
                break;
            }
        }    
    }
    return result;
}
{% endhighlight %}

[flower-planting-with-no-adjacent]: https://leetcode.com/problems/flower-planting-with-no-adjacent/
