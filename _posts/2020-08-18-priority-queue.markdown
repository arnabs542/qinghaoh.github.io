---
layout: post
title:  "Priority Queue"
tags: queue
---
Time complexity:
* Build: `O(n)`

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
    for (int i = 0; i < matrix.length; i++) {
        pq.offer(new int[]{i, 0});
    }

    while (--k > 0) {
        int[] index = pq.poll();

        if (++index[1] < matrix.length) {
            pq.offer(index);
        }
    }
    return matrix[pq.peek()[0]][pq.peek()[1]];
}
{% endhighlight %}

[Kth Smallest Prime Fraction][k-th-smallest-prime-fraction]

[Minimize Deviation in Array][minimize-deviation-in-array]

{% highlight java %}
public int minimumDeviation(int[] nums) {
    // max heap
    Queue<Integer> pq = new PriorityQueue<>(Comparator.comparingInt(a -> -a));
    int min = Integer.MAX_VALUE, d = Integer.MAX_VALUE;
    for (int num : nums) {
        // doubles odd numbers, so we only decrease numbers later
        if (num % 2 == 1) {
            num *= 2;
        }
        pq.offer(num);
        min = Math.min(min, num);
    }

    while (pq.peek() % 2 == 0) {
        int num = pq.poll();
        d = Math.min(d, num - min);
        min = Math.min(min, num / 2);
        pq.offer(num / 2);
    }
    return Math.min(d, pq.peek() - min);
}
{% endhighlight %}

[Furthest Building You Can Reach][furthest-building-you-can-reach]

{% highlight java %}
public int furthestBuilding(int[] heights, int bricks, int ladders) {
    Queue<Integer> pq = new PriorityQueue<>();
    for (int i = 0; i < heights.length - 1; i++) {
        int d = heights[i + 1] - heights[i];
        if (d > 0) {
            pq.add(d);
        }

        // it's optimal to use ladders in largest jumps
        if (pq.size() > ladders) {
            bricks -= pq.poll();
        }

        if (bricks < 0) {
            return i;
        }
    }
    return heights.length - 1;
}
{% endhighlight %}

[Smallest Range Covering Elements from K Lists][smallest-range-covering-elements-from-k-lists]

{% highlight java %}
public int[] smallestRange(List<List<Integer>> nums) {
    int[] range = new int[]{-(int)1e5, (int)1e5};
    int end = -(int)1e5;

    // coordinate {i, j} of an element in nums
    Queue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> nums.get(a[0]).get(a[1])));
    for (int i = 0; i < nums.size(); i++) {
        pq.offer(new int[]{i, 0});
        end = Math.max(end, nums.get(i).get(0));
    }

    while (!pq.isEmpty()) {
        int[] node = pq.poll();
        int num = nums.get(node[0]).get(node[1]);

        if (end - num < range[1] - range[0]) {
            range[0] = num;
            range[1] = end;
        }

        // returns if all elements of any list are visited
        if (node[1] + 1 == nums.get(node[0]).size()) {
            return range;
        }

        pq.offer(new int[]{node[0], node[1] + 1});
        end = Math.max(end, nums.get(node[0]).get(node[1] + 1));
    }

    return range;
}
{% endhighlight %}

[furthest-building-you-can-reach]: https://leetcode.com/problems/furthest-building-you-can-reach/
[k-th-smallest-prime-fraction]: https://leetcode.com/problems/k-th-smallest-prime-fraction/
[kth-smallest-element-in-a-sorted-matrix]: https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/
[maximize-sum-of-array-after-k-negations]: https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/
[minimize-deviation-in-array]: https://leetcode.com/problems/minimize-deviation-in-array/
[smallest-range-covering-elements-from-k-lists]: https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/
