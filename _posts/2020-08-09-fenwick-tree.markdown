---
layout: post
title:  "Fenwick Tree"
tags: tree
---
[Fenwick Tree](https://en.wikipedia.org/wiki/Fenwick_tree)

A Fenwick tree or binary indexed tree is a data structure that can efficiently update elements and calculate prefix sums in a table of numbers.

{% highlight java %}
public class FenwickTree {
    private int[] A;
    private int size;

    public FenwickTree(int size) {
        this.size = size;
        // one based indexing is assumed
        A = new int[size + 1];
    }

    // Returns the sum from index 1 to i
    // O(log(n))
    public int sum(int i) {
        int sum = 0;
        while (i > 0)  {
            sum += A[i];
            i -= lsb(i);
        }             
        return sum;
    }

    // Adds k to element with index i
    // O(log(n))
    public void add(int i, int k) {
        while (i <= size) {
            A[i] += k;
            i += lsb(i);
        }
    }

    private int lsb(int i) {
        return i & (-i);
    }
}
{% endhighlight %}

[Range Sum Query - Mutable][range-sum-query-mutable]

[range-sum-query-mutable]: https://leetcode.com/problems/range-sum-query-mutable/
