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

[Kth Smallest Element in a Sorted Matrix][kth-smallest-element-in-a-sorted-matrix]

{% highlight java %}
public int kthSmallest(int[][] matrix, int k) {
    Queue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> matrix[a[0]][a[1]]));
    for (int j = 0; j < matrix[0].length; j++) {
        pq.offer(new int[]{0, j});
    }

    int count = 0;
    while (!pq.isEmpty()) {
        int[] index = pq.poll();
        if (++count == k) {
            return matrix[index[0]][index[1]];
        }

        if (index[0] + 1 < matrix.length) {
            pq.offer(new int[]{index[0] + 1, index[1]});
        }
    }
    return 0;
}
{% endhighlight %}

[kth-smallest-element-in-a-sorted-matrix]: https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/
[maximize-sum-of-array-after-k-negations]: https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/
