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

[Jump Game][jump-game]

{% highlight java %}
public boolean canJump(int[] nums) {
    int i = 0;
    for (int reach = 0; i <= reach && i < nums.length; i++) {
        reach = Math.max(reach, i + nums[i]);
    }
    return i == nums.length;
}
{% endhighlight %}

[Video Stitching][video-stitching]

{% highlight java %}
public int videoStitching(int[][] clips, int T) {
    Arrays.sort(clips, (a, b) -> a[0] - b[0]);

    int i = 0, start = 0, end = 0, result = 0;
    while (start < T) {
        while (i < clips.length && clips[i][0] <= start) {
            end = Math.max(end, clips[i++][1]);
        }

        if (start == end) {
            return -1;
        }

        start = end;
        result++;
    }

    return result;
}
{% endhighlight %}

[Jump Game II][jump-game-ii]

{% highlight java %}
public int jump(int[] nums) {
    int i = 0, start = 0, end = 0, jumps = 0;
    while (start < nums.length - 1) {
        while (i < nums.length && i <= start) {
            end = Math.max(i + nums[i], end);
            i++;
        }
        if (start == end) {
            return - 1;
        }
        start = end;
        jumps++;
    }
    return jumps;
}
{% endhighlight %}

[Broken Calculator][broken-calculator]

{% highlight java %}
public int brokenCalc(int X, int Y) {
    int s = 0;
    while (Y > X) {
        Y = Y % 2 == 0 ? Y / 2 : Y + 1;
        s++;
    }
    return s + X - Y;
}
{% endhighlight %}

[Maximum Number of Events That Can Be Attended][maximum-number-of-events-that-can-be-attended]

{% highlight java %}
public int maxEvents(int[][] events) {
    // sorts events by start day
    Arrays.sort(events, (a, b) -> Integer.compare(a[0], b[0]));

    // keeps the current open events
    Queue<Integer> pq = new PriorityQueue<>();
    int i = 0, count = 0, day = 0;
    while (!pq.isEmpty() || i < events.length) {
        // if no events are open on this day,
        // flies time to the start day of the next open event
        if (pq.isEmpty()) {
            day = events[i][0];
        }

        // adds new events that can be attended on this day
        while (i < events.length && events[i][0] <= day) {
            pq.offer(events[i++][1]);
        }

        // attends the event that will end the earliest
        pq.poll();
        count++;
        day++;

        // removes closed events
        while (!pq.isEmpty() && pq.peek() < day) {
            pq.poll();
        }   
    }

    return count;
}
{% endhighlight %}

[Split Array into Consecutive Subsequences][split-array-into-consecutive-subsequences]

{% highlight java %}
public boolean isPossible(int[] nums) {
    int prev = Integer.MIN_VALUE, curr = 0;
    // count of consecutive subsequences with size i, ending at prev/curr
    int p1 = 0, p2 = 0, p3 = 0;
    int c1 = 1, c2 = 0, c3 = 0;

    for (int i = 0; i < nums.length; prev = curr, p1 = c1, p2 = c2, p3 = c3) {
        int count = 0;
        for (curr = nums[i]; i < nums.length && curr == nums[i]; i++) {
            count++;
        }

        // fills the sets greedily: 1 -> 2 -> 3
        if (curr != prev + 1) {
            if (p1 != 0 || p2 != 0) {
                return false;
            }
            c1 = count;
            c2 = 0;
            c3 = 0;
        } else {
            if (count < p1 + p2) {
                return false;
            }
            c1 = Math.max(0, count - (p1 + p2 + p3));
            c2 = p1;
            c3 = p2 + Math.min(p3, count - (p1 + p2));
        }
    }

    return p1 == 0 && p2 == 0;
}
{% endhighlight %}

[broken-calculator]: https://leetcode.com/problems/broken-calculator/
[flower-planting-with-no-adjacent]: https://leetcode.com/problems/flower-planting-with-no-adjacent/
[jump-game]: https://leetcode.com/problems/jump-game/
[jump-game-ii]: https://leetcode.com/problems/jump-game-ii/
[maximum-number-of-events-that-can-be-attended]: https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/
[split-array-into-consecutive-subsequences]: https://leetcode.com/problems/split-array-into-consecutive-subsequences/
[video-stitching]: https://leetcode.com/problems/video-stitching/
