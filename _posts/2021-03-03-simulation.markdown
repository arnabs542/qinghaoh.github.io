---
layout: post
title:  "Simulation"
---
[Pour Water][pour-water]

{% highlight java %}
public int[] pourWater(int[] heights, int V, int K) {
    while (V > 0) {
        int i = K;
        while (i > 0 && heights[i] >= heights[i - 1]) {
            i--;
        }
        while (i < heights.length - 1 && heights[i] >= heights[i + 1]) {
            i++;
        }
        while (i > K && heights[i] == heights[i - 1]) {
            i--;
        }
        heights[i]++;
        V--;
    }
    return heights;
}
{% endhighlight %}

[Champagne Tower][champagne-tower]

{% highlight java %}
public double champagneTower(int poured, int query_row, int query_glass) {
    double[][] glass = new double[query_row + 2][query_row + 2];
    glass[0][0] = poured;

    for (int i = 0; i <= query_row; i++) {
        for (int j = 0; j <= i; j++) {
            if (glass[i][j] > 1) {
                double water = (glass[i][j] - 1) / 2;
                glass[i + 1][j] += water;
                glass[i + 1][j + 1] += water;
            }
        }
    }

    return Math.min(glass[query_row][query_glass], 1);
}
{% endhighlight %}

Reduced to 1D:

{% highlight java %}
public double champagneTower(int poured, int query_row, int query_glass) {
    double[] glass = new double[query_row + 2];
    glass[0] = poured;

    for (int i = 1; i <= query_row; i++) {
        for (int j = i; j >= 0; j--) {
            double water = Math.max((glass[j] - 1) / 2, 0);
            glass[j] = water;
            glass[j + 1] += water;
        }
    }

    return Math.min(glass[query_glass], 1);
}
{% endhighlight %}

[Dota2 Senate][dota2-senate]

{% highlight java %}
public String predictPartyVictory(String senate) {
    Queue<Integer> r = new LinkedList<>(), d = new LinkedList<>();
    int n = senate.length();
    for (int i = 0; i< n; i++) {
        if (senate.charAt(i) == 'R') {
            r.offer(i);
        } else {
            d.offer(i);
        }
    }

    while (!r.isEmpty() && !d.isEmpty()) {
        int ri = r.poll(), di = d.poll();
        if (ri < di) {
            r.offer(ri + n);
        } else {
            d.offer(di + n);
        }
    }
    return r.size() > d.size() ? "Radiant" : "Dire";
}
{% endhighlight %}

[champagne-tower]: https://leetcode.com/problems/champagne-tower/
[dota2-senate]: https://leetcode.com/problems/dota2-senate/
[pour-water]: https://leetcode.com/problems/pour-water/
