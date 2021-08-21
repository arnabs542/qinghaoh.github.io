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

[Hand of Straights][hand-of-straights]

{% highlight java %}
public boolean isNStraightHand(int[] hand, int W) {
    Map<Integer, Integer> count = new TreeMap<>();
    for (int h : hand) {
        count.put(h, count.getOrDefault(h, 0) + 1);
    }

    // number of groups that are not closed
    int prev = -1, opened = 0;
    // The i-th queue element is the number of opened groups starting from the i-th key
    Queue<Integer> q = new LinkedList<>();
    for (var e : count.entrySet()) {
        int k = e.getKey(), v = e.getValue();
        if ((opened > 0 && k > prev + 1) || opened > v) {
            return false;
        }

        q.add(v - opened);
        prev = k;
        opened = v;
        if (q.size() == W) {
            opened -= q.poll();
        }
    }
    return opened == 0;
}
{% endhighlight %}

[Wiggle Sort][wiggle-sort]

{% highlight java %}
public void wiggleSort(int[] nums) {
    for (int i = 0; i < nums.length - 1; i++) {
        if ((i % 2 == 0) == (nums[i] > nums[i + 1])) {
            swap(nums, i, i + 1);
        }
    }
}

private void swap(int[] A, int i, int j) {
    int tmp = A[i];
    A[i] = A[j];
    A[j] = tmp;
}
{% endhighlight %}

[Minimum Factorization][minimum-factorization]

{% highlight java %}
public int smallestFactorization(int a) {
    if (a < 10) {
        return a;
    }

    long b = 0, t = 1;
    for (int i = 9; i >= 2; i--) {
        while (a % i == 0) {
            a /= i;
            b += t * i;
            t *= 10;
        }
    }
    return a == 1 && b <= Integer.MAX_VALUE ? (int)b : 0;
}
{% endhighlight %}

[Put Boxes Into the Warehouse I][put-boxes-into-the-warehouse-i]

{% highlight java %}
public int maxBoxesInWarehouse(int[] boxes, int[] warehouse) {
    Arrays.sort(boxes);

    int i = boxes.length - 1, j = 0, count = 0;
    while (i >= 0 && j < warehouse.length) {
        // all following boxes will fit in warehouse[j]
        if (boxes[i] <= warehouse[j]) {
            j++;
            count++;
        }
        i--;
    }
    return count;
}
{% endhighlight %}

[Maximum Binary String After Change][maximum-binary-string-after-change]

{% highlight java %}
public String maximumBinaryString(String binary) {
    // we can always make the string contain at most one '0'
    int ones = 0, zeros = 0, n = binary.length();
    StringBuilder sb = new StringBuilder("1".repeat(n));
    for (int i = 0; i < n; i++) {
        if (binary.charAt(i) == '0') {
            zeros++;
        } else if (zeros == 0) {
            // leading '1's
            ones++;
        }
    }
    if (ones < n) {
        sb.setCharAt(ones + zeros - 1, '0');
    }
    return sb.toString();
}
{% endhighlight %}

[broken-calculator]: https://leetcode.com/problems/broken-calculator/
[flower-planting-with-no-adjacent]: https://leetcode.com/problems/flower-planting-with-no-adjacent/
[hand-of-straights]: https://leetcode.com/problems/hand-of-straights/
[jump-game]: https://leetcode.com/problems/jump-game/
[maximum-binary-string-after-change]: https://leetcode.com/problems/maximum-binary-string-after-change/
[minimum-factorization]: https://leetcode.com/problems/minimum-factorization/
[put-boxes-into-the-warehouse-i]: https://leetcode.com/problems/put-boxes-into-the-warehouse-i/
[split-array-into-consecutive-subsequences]: https://leetcode.com/problems/split-array-into-consecutive-subsequences/
[video-stitching]: https://leetcode.com/problems/video-stitching/
[wiggle-sort]: https://leetcode.com/problems/wiggle-sort/
