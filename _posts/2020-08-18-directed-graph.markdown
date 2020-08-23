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

[find-the-town-judge]: https://leetcode.com/problems/find-the-town-judge/
