---
layout: post
title:  "Palindrome"
tags: string
---
## Palindrome String

{% highlight java %}
public boolean isPalindrome(String s) {
    int left = 0, right = s.length() - 1;
    while (left < right) {
        if (s.charAt(left++) != s.charAt(right--)) {
            return false;
        }
    }
    return true;
}
{% endhighlight %}

## Palindrome Number

[Palindrome Number][palindrome-number]

{% highlight java %}
public boolean isPalindrome(int x) {
    if (x < 0 || (x % 10 == 0 && x != 0)) {
        return false;
    }

    // half revert x
    int reverted = 0;
    while (x > reverted) {
        reverted = reverted * 10 + x % 10;
        x /= 10;
    }
    return x == reverted || x == reverted / 10;
}
{% endhighlight %}

## Greedy

[Construct K Palindrome Strings][construct-k-palindrome-strings]

{% highlight java %}
public boolean canConstruct(String s, int k) {
    int odd = 0;
    for (char c : s.toCharArray()) {
        odd ^= 1 << (c - 'a');
    }
    return s.length() >= k && Integer.bitCount(odd) <= k;
}
{% endhighlight %}

## Dynamic Programming

### Expand Around Center

[Palindromic Substring][palindromic-substring]

{% highlight java %}
public int countSubstrings(String s) {
    int count = 0;
    for (int center = 0; center <= 2 * s.length() - 1; center++) {
        int left = center / 2;
        int right = left + center % 2;
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            count++;
            left--;
            right++;
        }
    }
    return count;
}
{% endhighlight %}

[Longest Palindromic Substring][longest-palindromic-substring]

{% highlight java %}
public String longestPalindrome(String s) {
    String result = "";
    int max = 0;
    for (int center = 0; center <= 2 * s.length() - 1; center++) {
        int left = center / 2, right = left + center % 2;
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        if (right - left - 1 > max) {
            max = right - left - 1;
            result = s.substring(left + 1, right);
        }
    }
    return result;
}
{% endhighlight %}

[Manacher's algorithm](https://en.wikipedia.org/wiki/Longest_palindromic_substring#Manacher's_algorithm)

[Longest Palindromic Subsequence][longest-palindromic-subsequence]

{% highlight java %}
public int longestPalindromeSubseq(String s) {
    // dp[i][j]: s.substring(i, j + 1)
    int[][] dp = new int[s.length()][s.length()];

    for (int i = s.length() - 1; i >= 0; i--) {
        dp[i][i] = 1;
        for (int j = i + 1; j < s.length(); j++) {
            if (s.charAt(i) == s.charAt(j)) {
                dp[i][j] = dp[i + 1][j - 1] + 2;
            } else {
                dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
    }

    return dp[0][s.length() - 1];
}
{% endhighlight %}

[Count Different Palindromic Subsequences][count-different-palindromic-subsequences]

{% highlight java %}
private static final int MOD = (int)1e9 + 7, NUM = 4;

public int countPalindromicSubsequences(String S) {
    int n = S.length();
    int[] prev = new int[n + 1], next = new int[n + 1];

    int[] index = new int[NUM];
    Arrays.fill(index, -1);
    for (int i = 0; i < n; i++) {
        prev[i] = index[S.charAt(i) - 'a'];
        index[S.charAt(i) - 'a'] = i;
    }

    Arrays.fill(index, n);
    for (int i = n - 1; i >= 0; i--) {
        next[i] = index[S.charAt(i) - 'a'];
        index[S.charAt(i) - 'a'] = i;
    }

    int[][] dp = new int[n][n];

    // "a"
    for (int i = 0; i < n; i++) {
        dp[i][i] = 1;
    }

    for (int len = 1; len < n; len++) {
        for (int i = 0, j = i + len; j < n; i++, j++) {
            if (S.charAt(i) != S.charAt(j)) {
                dp[i][j] = dp[i][j - 1] + dp[i + 1][j] - dp[i + 1][j - 1];
            } else {
                // [i, j]
                int low = next[i], high = prev[j];
                dp[i][j] = dp[i + 1][j - 1] * 2;  // w/ and w/o S(i) & S(j)
                if (low == high) {  // a...a...a, only one char 'a' inside
                    dp[i][j]++;  // +{"aa"}
                } else if (low > high) {  // a...a, no char 'a' inside
                    dp[i][j] += 2;  // +{"a", "aa"}
                } else {  // a...a...a...a
                    dp[i][j] -= dp[low + 1][high - 1];
                }
            }
            dp[i][j] = Math.floorMod(dp[i][j], MOD);
        }
    }
    return dp[0][n - 1];
}
{% endhighlight %}

[construct-k-palindrome-strings]: https://leetcode.com/problems/construct-k-palindrome-strings/
[count-different-palindromic-subsequences]: https://leetcode.com/problems/count-different-palindromic-subsequences/
[longest-palindromic-subsequence]: https://leetcode.com/problems/longest-palindromic-subsequence/
[longest-palindromic-substring]: https://leetcode.com/problems/longest-palindromic-substring/
[palindrome-number]: https://leetcode.com/problems/palindrome-number/
[palindromic-substring]: https://leetcode.com/problems/palindromic-substring/
