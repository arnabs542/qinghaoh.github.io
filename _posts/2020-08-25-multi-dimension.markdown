---
layout: post
title:  "Multi-dimension"
tags: array
---
# Search

## Reduce to One-dimension
[Search a 2D Matrix][search-a-2d-matrix]

## Monotonic in Each Dimenstion
[Find Positive Integer Solution for a Given Equation][find-positive-integer-solution-for-a-given-equation]

{% highlight java %}
public List<List<Integer>> findSolution(CustomFunction customfunction, int z) {
    List<List<Integer>> result = new ArrayList<>();
    // starts from bottom-right
    int x = 1, y = 1000;
    while (x <= 1000 && y > 0) {
        int v = customfunction.f(x, y);
        if (v < z) {
            x++;
        } else if (v > z) {
            y--;
        } else {
            result.add(Arrays.asList(x++, y--));
        }
    }
    return result;
}
{% endhighlight %}

[find-positive-integer-solution-for-a-given-equation]: https://leetcode.com/problems/find-positive-integer-solution-for-a-given-equation/
[search-a-2d-matrix]: https://leetcode.com/problems/search-a-2d-matrix/
