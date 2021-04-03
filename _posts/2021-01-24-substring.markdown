---
layout: post
title:  "Subtring"
tags: string
---
# Dynamic Programming

[Longest Repeating Substring][longest-repeating-substring]

{% highlight java %}
// O(n ^ 2)
public int longestRepeatingSubstring(String S) {
    int n = S.length();
    // dp[i][j]: length of longeset common suffix of S.substring(0, i + 1) and S.substring(0, j + 1)
    int[][] dp = new int[n + 1][n + 1];
    int max = 0;
    for (int i = 1; i <= n; i++) {
        for (int j = i + 1; j <= n; j++) {
            if (S.charAt(i - 1) == S.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
                max = Math.max(max, dp[i][j]);
            }
        }
    }
    return max;
}
{% endhighlight %}

# String Matching

[Rabin-Karp algorithm](https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp_algorithm): a string-searching algorithm that uses hashing to find an exact match of a pattern string in a text. It uses a rolling hash to quickly filter out positions of the text that cannot match the pattern, and then checks for a match at the remaining positions.

[Longest Duplicate Substring][longest-duplicate-substring]

{% highlight java %}
// O(nlog(n))
public String longestDupSubstring(String s) {
    // binary search
    int low = 0, high = s.length() - 1;
    while (low < high) {
        int mid = low + (high - low + 1) / 2;
        if (search(s, mid) != null) {
            low = mid;
        } else {
            high = mid - 1;
        }
    }
    return search(s, low);
}

/**
 * Searchs input string for duplicate substring with target length by Rabin-Karp Algorithm.
 * @param s input string
 * @param len target length
 * @return duplicate substring with target length; null if not found
 */
private String search(String s, int len) {
    if (len == 0) {
        return "";
    }

    // polynomial rolling hash
    long mod = (1 << 31) - 1, a = 256, h = 0;
    for (int i = 0; i < len; i++) {
        h = (h * a + s.charAt(i)) % mod;
    }

    long coff = 1;
    for (int i = 1; i < len; i++) {
        coff = (coff * a) % mod;
    }

    // hash : start indexes
    Map<Long, List<Integer>> map = new HashMap<>();

    // start index == 0
    map.computeIfAbsent(h, k -> new ArrayList<>()).add(0);

    // start index > 0
    int start = 0;
    while (start + len < s.length()) {
        // rolling hash
        h = ((h + mod - coff * s.charAt(start) % mod) * a + s.charAt(start + len)) % mod;
        start++;

        // Rabin-Karp
        map.putIfAbsent(h, new ArrayList<>());
        for (int i : map.get(h)) {
            if (s.substring(start, start + len).equals(s.substring(i, i + len))) {
                return s.substring(i, i + len);
            }
        }
        map.get(h).add(start);
    }

    // no duplicate substring found
    return null;
}
{% endhighlight %}

# Greedy

[Stamping The Sequence][stamping-the-sequence]

{% highlight java %}
private int questions = 0;  // count of wildcard '?'
private String stamp;
private char[] t;

// reverse
public int[] movesToStamp(String stamp, String target) {
    int m = stamp.length(), n = target.length();
    this.stamp = stamp;
    this.t = target.toCharArray();

    // visited[i] indicates the string t.substring(i, i + m) is stamped or not
    // this avoids stamps at the same place
    boolean[] visited = new boolean[n - m + 1];
    List<Integer> list = new ArrayList<>();
    while (questions < n) {
        boolean stamped = false;
        for (int i = 0; i <= n - m && questions < n; i++) {
            if (!visited[i] && canStamp(i)) {
                doStamp(i);
                stamped = true;
                list.add(0, i);
                visited[i] = true;
            }
        }

        if (!stamped) {
            return new int[0];
        }
    }

    return list.stream().mapToInt(Integer::valueOf).toArray();
}

private boolean canStamp(int pos) {
    for (int i = 0; i < stamp.length(); i++) {
        if (t[pos + i] != '?' && t[pos + i] != stamp.charAt(i)) {
            return false;
        }
    }
    return true;
}

private void doStamp(int pos) {
    for (int i = 0; i < stamp.length(); i++) {
        if (t[pos + i] != '?') {
            t[pos + i] = '?';
            questions++;
        }
    }
}
{% endhighlight %}

[longest-duplicate-substring]: https://leetcode.com/problems/longest-duplicate-substring/
[longest-repeating-substring]: https://leetcode.com/problems/longest-repeating-substring/
[stamping-the-sequence]: https://leetcode.com/problems/stamping-the-sequence/
