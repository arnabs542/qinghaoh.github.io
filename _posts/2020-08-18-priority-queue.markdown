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

[Range Sum of Sorted Subarray Sums][range-sum-of-sorted-subarray-sums]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int rangeSum(int[] nums, int n, int left, int right) {
    // {subarray, next index}
    Queue<int[]> pq  = new PriorityQueue<>(n, Comparator.comparingInt(p -> p[0]));
    // enqueues all numbers in nums
    for (int i = 0; i < n; i++) {
        pq.offer(new int[]{nums[i], i + 1});
    }

    int sum = 0;
    // 1-indexed
    for (int i = 1; i <= right; i++) {
        // minimum subarray sum so far
        int[] p = pq.poll();
        if (i >= left) {
            sum = (sum + p[0]) % MOD;
        }

        // adds next number to the subarray sum
        if (p[1] < n) {
            p[0] += nums[p[1]++];
            pq.offer(p);
        }
    }
    return sum;
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

[Construct Target Array With Multiple Sums][construct-target-array-with-multiple-sums]

{% highlight java %}
public boolean isPossible(int[] target) {
    if (target.length == 1) {
        return target[0] == 1;
    }

    Queue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);
    int sum = 0;
    for (int t : target) {
        sum += t;
        pq.offer(t);
    }

    // the max num in the target array must have been chosen in the previous step
    while (pq.peek() > 1) {
        int num = pq.poll();
        int rest = sum - num;

        // target[i] >= 1
        // so rest == 1 means there's only one rest "1"
        if (rest == 1) {
            return true;
        }

        // % prevents LTE
        int remainder = num % rest;
        // sum could overflow
        // num < rest will not work when rest < 0
        // so we use remainder == num
        if (remainder == 0 || remainder == num) {
            return false;
        }

        pq.offer(remainder);
        sum = remainder + rest;
    }
    return true;
}
{% endhighlight %}

[The Skyline Problem][the-skyline-problem]

{% highlight java %}
public List<List<Integer>> getSkyline(int[][] buildings) {
    List<int[]> heights = new ArrayList<>();
    for (int[] b: buildings) {
        // height at start is stored as negative
        heights.add(new int[]{b[0], -b[2]});
        // height at start is stored as positive
        heights.add(new int[]{b[1], b[2]});
    }

    Collections.sort(heights, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);

    TreeMap<Integer, Integer> map = new TreeMap<>(Collections.reverseOrder());
    // a trick to deal with last building in a block
    map.put(0, 1);

    List<List<Integer>> list = new ArrayList<>();
    int prev = 0;
    for (int[] h: heights) {
        if (h[1] < 0) {
            // if it's start, puts/increments the height to map
            map.put(-h[1], map.getOrDefault(-h[1], 0) + 1);
        } else {
            // if it's end, removes/decrements the height from map
            int count = map.get(h[1]);
            if (count == 1) {
                map.remove(h[1]);
            } else {
                map.put(h[1], count - 1);
            }
        }

        // gets the max height
        int curr = map.firstKey();
        // no consecutive horizontal lines of equal height
        if (prev != curr) {
            list.add(Arrays.asList(h[0], curr));
            prev = curr;
        }
    }
    return list;
}
{% endhighlight %}

[Minimum Cost to Hire K Workers][minimum-cost-to-hire-k-workers]

{% highlight java %}
public double mincostToHireWorkers(int[] quality, int[] wage, int k) {
    int n = quality.length;
    Integer[] index = new Integer[n];
    for (int i = 0; i < n; i++) {
        index[i] = i;
    }

    // expect[i] = wage[i] / quality[i]
    // when expect[i] > expect[j],
    // if we pay j-th worker quality[j] * expect[i] = wage[j] / expect[j] * expect[i] > wage[j]
    // he will be more than happy
    Arrays.sort(index, Comparator.comparingDouble(i -> (double)wage[i] / quality[i]));

    // uses max heap to find the min quality sum with k workers
    Queue<Integer> pq = new PriorityQueue<>(Comparator.reverseOrder());
    double min = Double.MAX_VALUE, qSum = 0;
    // at least one worker will be paid their minimum wage expectation
    for (int i : index) {
        qSum += quality[i];
        pq.offer(quality[i]);

        // removes the max quality to make the window size k
        // it's possible the quality of the current worker i is removed
        // and later we still use his expect[i]
        // but it's OK - the same window is computed with a smaller expectation in the last iteration
        if (pq.size() > k) {
            qSum -= pq.poll();
        }

        // expect[i] is the max in the k-length window
        if (pq.size() == k) {
            min = Math.min(min, qSum * wage[i] / quality[i]);
        }
    }
    return min;
}
{% endhighlight %}

# Greedy

[Maximum Performance of a Team][maximum-performance-of-a-team]

{% highlight java %}
private static final int MOD = (int)1e9 + 7;

public int maxPerformance(int n, int[] speed, int[] efficiency, int k) {
    int[][] engineer = new int[n][2];
    for (int i = 0; i < n; i++) {
        engineer[i] = new int[] {efficiency[i], speed[i]};
    }

    // sorts efficiency in decreasing order
    Arrays.sort(engineer, (a, b) -> b[0] - a[0]);

    Queue<Integer> pq = new PriorityQueue<>(k, (a, b) -> a - b);
    long max = 0, sum = 0;
    for (int[] e : engineer) {
        pq.offer(e[1]);
        sum += e[1];
        // keeps pq size as k
        if (pq.size() > k) {
            sum -= pq.poll();
        }
        max = Math.max(max, sum * e[0]);
    }

    return (int)(max % MOD);
}
{% endhighlight %}

## "BFS"

[Minimum Number of Refueling Stops][minimum-number-of-refueling-stops]

{% highlight java %}
public int minRefuelStops(int target, int startFuel, int[][] stations) {
    // max heap of gas station liters
    Queue<Integer> pq = new PriorityQueue(Collections.reverseOrder());

    int i = 0, stops = 0;
    while (startFuel < target) {  // needs refueling
        // enqueues the liters of reachable stations
        while (i < stations.length && stations[i][0] <= startFuel) {
            pq.offer(stations[i++][1]);
        }

        // no stations to provide gas
        if (pq.isEmpty()) {
            return -1;
        }

        // refuels with the max liters
        startFuel += pq.poll();
        stops++;
    }

    return stops;
}
{% endhighlight %}

[construct-target-array-with-multiple-sums]: https://leetcode.com/problems/construct-target-array-with-multiple-sums/
[furthest-building-you-can-reach]: https://leetcode.com/problems/furthest-building-you-can-reach/
[k-th-smallest-prime-fraction]: https://leetcode.com/problems/k-th-smallest-prime-fraction/
[kth-smallest-element-in-a-sorted-matrix]: https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/
[maximize-sum-of-array-after-k-negations]: https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/
[maximum-performance-of-a-team]: https://leetcode.com/problems/maximum-performance-of-a-team/
[minimum-cost-to-hire-k-workers]: https://leetcode.com/problems/minimum-cost-to-hire-k-workers/
[minimize-deviation-in-array]: https://leetcode.com/problems/minimize-deviation-in-array/
[minimum-number-of-refueling-stops]: https://leetcode.com/problems/minimum-number-of-refueling-stops/
[range-sum-of-sorted-subarray-sums]: https://leetcode.com/problems/range-sum-of-sorted-subarray-sums/
[smallest-range-covering-elements-from-k-lists]: https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/
[the-skyline-problem]: https://leetcode.com/problems/the-skyline-problem/
