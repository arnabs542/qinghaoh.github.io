---
layout: post
title:  "Line Sweep"
tags: math
usemathjax: true
---
# Ordered Map

[Describe the Painting][describe-the-painting]

{% highlight java %}
public List<List<Long>> splitPainting(int[][] segments) { 
    Map<Integer, Long> map = new TreeMap<>();
    for (int[] s : segments) {
        map.put(s[0], map.getOrDefault(s[0], 0l) + s[2]);
        map.put(s[1], map.getOrDefault(s[1], 0l) - s[2]);
    }

    List<List<Long>> painting = new ArrayList<>();
    int prev = 0;
    long color = 0;
    for (int curr : map.keySet()) {
        // color == 0 means this segment is not painted
        if (prev > 0 && color > 0) {
            painting.add(Arrays.asList((long)prev, (long)curr, color));
        }

        color += map.get(curr);
        prev = curr;
    }
    return painting;
}
{% endhighlight %}

[Average Height of Buildings in Each Segment][average-height-of-buildings-in-each-segment]

{% highlight java %}
public int[][] averageHeightOfBuildings(int[][] buildings) {
    // {height sum, count}
    Map<Integer, int[]> map = new TreeMap<>();
    for (int[] b : buildings) {
        if (map.containsKey(b[0])) {
            map.get(b[0])[0] += b[2];
            map.get(b[0])[1]++;
        } else {
            map.put(b[0], new int[]{b[2], 1});
        }
        if (map.containsKey(b[1])) {
            map.get(b[1])[0] -= b[2];
            map.get(b[1])[1]--;
        } else {
            map.put(b[1], new int[]{-b[2], -1});
        }
    }

    List<int[]> list = new ArrayList<>();
    int sum = 0, count = 0;
    for (int p : map.keySet()) {
        // updates end of prev int[]
        if (sum > 0) {
            list.get(list.size() - 1)[1] = p;
        }

        // accumulates sum and count
        sum += map.get(p)[0];
        count += map.get(p)[1];

        // - empty list
        // - curr != end of prev int[]
        // - average != average of prev int[]
        if (count > 0 &&
           (list.isEmpty() ||
            list.get(list.size() - 1)[1] != p ||
            list.get(list.size() - 1)[2] != sum / count)) {
            list.add(new int[]{p, p, sum / count});
        }
    }

    int[][] street = new int[list.size()][];
    for (int i = 0; i < list.size(); i++) {
        street[i] = list.get(i);
    }
    return street;
}
{% endhighlight %}

[The Skyline Problem][the-skyline-problem]

{% highlight java %}
public List<List<Integer>> getSkyline(int[][] buildings) {
    List<int[]> heights = new ArrayList<>();
    for (int[] b: buildings) {
        // height at start is stored as negative
        heights.add(new int[]{b[0], -b[2]});
        // height at end is stored as positive
        heights.add(new int[]{b[1], b[2]});
    }

    Collections.sort(heights, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);

    TreeMap<Integer, Integer> map = new TreeMap<>();
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
            map.put(h[1], map.get(h[1]) - 1);
            map.remove(h[1], 0);
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

# Priority Queue

[Average Height of Buildings in Each Segment][average-height-of-buildings-in-each-segment]

{% highlight java %}
public int[][] averageHeightOfBuildings(int[][] buildings) {
    // {point, (+/-)height}
    Queue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
    for (int[] b : buildings) {
        pq.offer(new int[]{b[0], b[2]});
        pq.offer(new int[]{b[1], -b[2]});
    }

    Deque<int[]> dq = new ArrayDeque<>();
    int prev = 0, sum = 0, count = 0;
    while (!pq.isEmpty()) {
        int[] curr = pq.poll();
        if (count != 0 && curr[0] != prev) {
            int avg = sum / count;
            // updates end and average of prev int[]
            if (!dq.isEmpty() && dq.peekLast()[1] == prev && dq.peekLast()[2] == avg) {
                dq.peekLast()[1] = curr[0];
                dq.peekLast()[2] = avg;
            } else {
                dq.offerLast(new int[] {prev, curr[0], avg});
            }
        }
        prev = curr[0];
        sum += curr[1];
        count += curr[1] > 0 ? 1 : -1;
    }

    int[][] street = new int[dq.size()][];
    for (int i = 0; i < street.length; i++) {
        street[i] = dq.pollFirst();
    }
    return street;
}
{% endhighlight %}

[average-height-of-buildings-in-each-segment]: https://leetcode.com/problems/average-height-of-buildings-in-each-segment/
[describe-the-painting]: https://leetcode.com/problems/describe-the-painting/
[the-skyline-problem]: https://leetcode.com/problems/the-skyline-problem/
