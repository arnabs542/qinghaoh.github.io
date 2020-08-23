---
layout: post
title:  "Prioirty Queue"
tags: queue
---
[Maximize Sum Of Array After K Negations][maximize-sum-of-array-after-k-negations]

{% highlight java %}
public int largestSumAfterKNegations(int[] A, int K) {
    Queue<Integer> pq = new PriorityQueue<>();
    Arrays.stream(A).forEach(pq::offer);

    while (K-- > 0) {
        pq.offer(-pq.poll());
    }
    return pq.stream().reduce(0, Integer::sum);
}
{% endhighlight %}

[maximize-sum-of-array-after-k-negations]: https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/
