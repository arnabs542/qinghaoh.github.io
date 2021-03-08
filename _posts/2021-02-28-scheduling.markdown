---
layout: post
title:  "Scheduling"
---
# Greedy

[Minimum Swaps to Make Strings Equal][minimum-swaps-to-make-strings-equal]

{% highlight java %}
public int minimumSwap(String s1, String s2) {
    int[] count = new int[2];
    for (int i = 0; i < s1.length(); i++) {
        // ignores matched positions
        if (s1.charAt(i) != s2.charAt(i)) {
            count[s1.charAt(i) - 'x']++;
        }
    }

    // case 3: "x" "y"
    if ((count[0] + count[1]) % 2 == 1) {
        return -1;
    }

    // count[0] % 2 == count[1] % 2
    // case1: "xx" "yy" - 1 swap, apply as much as possible
    // case2: "xy" "yx" - 2 swaps
    return count[0] / 2 + count[1] / 2 + count[0] % 2 * 2;
}
{% endhighlight %}

[Longest Happy String][longest-happy-string]

{% highlight java %}
public String longestDiverseString(int a, int b, int c) {
    return helper(a, b, c, "a", "b", "c");
}

private String helper(int a, int b, int c, String sa, String sb, String sc) {
    // preprocess, so that a >= b >= c
    if (a < b) {
        return helper(b, a, c, sb, sa, sc);
    }

    if (b < c) {
        return helper(a, c, b, sa, sc, sb);
    }

    if (b == 0) {
        return sa.repeat(Math.min(a, 2));
    }

    // greedy
    int aUsed = Math.min(a, 2), bUsed = a - aUsed >= b ? 1 : 0; 
    return sa.repeat(aUsed) + sb.repeat(bUsed) + helper(a - aUsed, b - bUsed, c, sa, sb, sc);
}
{% endhighlight %}

[String Without AAA or BBB][string-without-aaa-or-bbb]

[Task Scheduler][task-scheduler]

![Schedule](/assets/task_scheduler.png)

{% highlight java %}
public int leastInterval(char[] tasks, int n) {
    int[] count = new int[26];
    int maxFreq = 0, maxFreqCount = 0;  // count of the most frequent tasks
    for (char c : tasks) {
        count[c - 'A']++;
        if (maxFreq == count[c - 'A']) {
            maxFreqCount++;
        } else if (maxFreq < count[c - 'A']) {
            maxFreq = count[c - 'A'];
            maxFreqCount = 1;
        }
    }

    // no idle vs has idle
    return Math.max(tasks.length, (maxFreq - 1) * (n + 1) + maxFreqCount);
}
{% endhighlight %}

# EDF

[Earliest deadline first scheduling](https://en.wikipedia.org/wiki/Earliest_deadline_first_scheduling)

[Rearrange String k Distance Apart][rearrange-string-k-distance-apart]

{% highlight java %}
public String rearrangeString(String s, int k) {
    if (k == 0) {
        return s;
    }

    int[] count = new int[26];
    for (char c : s.toCharArray()) {
        count[c - 'a']++;
    }

    Queue<Integer> pq = new PriorityQueue<>(Comparator.comparingInt(i -> -count[i]));
    for (int i = 0; i < 26; i++) {
        if (count[i] > 0) {
            pq.offer(i);
        }
    }

    StringBuilder sb = new StringBuilder();
    Queue<Integer> q = new LinkedList<>();
    while (!pq.isEmpty()) {
        // picks the char with highest frequency
        int node = pq.poll();
        sb.append((char)(node + 'a'));
        count[node]--;

        // adds used char to the queue
        q.offer(node);

        // maintains fixed size k
        if (q.size() == k) {
            // front char is already at least k char apart
            int front = q.poll();
            if (count[front] > 0) {
                pq.offer(front);
            }
        }
    }
    return sb.length() == s.length() ? sb.toString() : "";
}
{% endhighlight %}

[flower-planting-with-no-adjacent]: https://leetcode.com/problems/flower-planting-with-no-adjacent/
[longest-happy-string]: https://leetcode.com/problems/longest-happy-string/
[maximum-number-of-events-that-can-be-attended]: https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/
[minimum-swaps-to-make-strings-equal]: https://leetcode.com/problems/minimum-swaps-to-make-strings-equal/
[rearrange-string-k-distance-apart]: https://leetcode.com/problems/rearrange-string-k-distance-apart/
[string-without-aaa-or-bbb]: https://leetcode.com/problems/string-without-aaa-or-bbb/
[task-scheduler]: https://leetcode.com/problems/task-scheduler/
[wiggle-sort]: https://leetcode.com/problems/wiggle-sort/
