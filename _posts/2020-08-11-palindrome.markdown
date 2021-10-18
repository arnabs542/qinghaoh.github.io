---
layout: post
title:  "Palindrome"
tags: string
---
# Palindrome String

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

# Palindrome Number

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

# Construction

[Prime Palindrome][prime-palindrome]

A positive integer (in decimal notation) is divisible by 11 iff the difference of the sum of the digits in even-numbered positions and the sum of digits in odd-numbered positions is divisible by 11.

{% highlight java %}
public int primePalindrome(int N) {
    if (N >= 8 && N <= 11) {
        return 11;
    }

    // palindrome with even number of digits is divisible by 11
    // x has at most 5 digits
    for (int x = 1; x < 100000; x++) {
        // builds palindrome with odd number of digits
        String s = String.valueOf(x), r = new StringBuilder(s).reverse().toString();
        int k = Integer.valueOf(s + r.substring(1));
        if (k >= N && isPrime(k)) {
            return k;
        }
    }
    return -1;
}

private boolean isPrime(int x) {
    if (x < 2 || x % 2 == 0) {
        return x == 2;
    }

    for (int i = 3; i * i <= x; i += 2) {
        if (x % i == 0)  {
            return false;
        }
    }
    return true;
}
{% endhighlight %}

[Super Palindromes][super-palindromes]

{% highlight java %}
public int superpalindromesInRange(String left, String right) {
    List<Long> palindromes = new ArrayList<>();
    for (long i = 1; i <= 9; i++) {
        palindromes.add(i);
    }

    // (10 ^ 18) ^ 0.5 = 10 ^ 9
    // the upper limit of left half is 10 ^ (9 / 2 + 1)
    for (long i = 1; i < 10000; i++) {
        String l = Long.toString(i), r = new StringBuilder(l).reverse().toString();
        // even
        palindromes.add(Long.parseLong(l + r));
        // odd
        for (long d = 0; d < 10; d++) {
            palindromes.add(Long.parseLong(l + d + r));
        }
    }

    int count = 0;
    long low = Long.parseLong(left), high = Long.parseLong(right);
    for (long palindrome : palindromes) {
        long square = palindrome * palindrome;
        if (!isPalindrome(Long.toString(square))) {
            continue;
        }
        if (low <= square && square <= high) {
            count++;
        }
    }
    return count;
}

private boolean isPalindrome(String s) {
}
{% endhighlight %}

[Palindrome Pairs][palindrome-pairs]

{% highlight java %}
public List<List<Integer>> palindromePairs(String[] words) {
    Map<String, Integer> map = new HashMap<>();
    for (int i = 0; i < words.length; i++) {
        map.put(words[i], i);
    }

    // finds all "word - reverse(word)" pairs
    // this case is handled separately to avoid duplicates
    List<List<Integer>> list = new ArrayList<>();
    for(int i = 0; i < words.length; i++){
        int index = map.getOrDefault(new StringBuilder(words[i]).reverse().toString(), i);
        if (index != i) {
            list.add(Arrays.asList(i, index));
        }
    }

    for (int i = 0; i < words.length; i++) {
        String w = words[i];
        for (int j = 0; j <= w.length(); j++) {
            // s1.substring(0, j) is palindrome
            // s1.substring(j + 1) == reverse(s2) => (s2, s1)
            if (j > 0 && isPalindrome(w.substring(0, j))) {
                int index = map.getOrDefault(new StringBuilder(w.substring(j)).reverse().toString(), i);
                if (index != i) {
                    list.add(Arrays.asList(index, i));
                }
            }

            // s1.substring(j + 1) is palindrome
            // s1.substring(0, j) == reverse(s2) => (s1, s2)
            if (j < w.length() && isPalindrome(w.substring(j))) {
                int index = map.getOrDefault(new StringBuilder(w.substring(0, j)).reverse().toString(), i);
                if (index != i) {
                    list.add(Arrays.asList(i, index));
                }
            }
        }
    }

    return list;
}

private boolean isPalindrome(String s) {
}
{% endhighlight %}

[Find the Closest Palindrome][find-the-closest-palindrome]

{% highlight java %}
public String nearestPalindromic(String n) {
    int len = n.length();
    if (len == 1) {
        return String.valueOf(Integer.valueOf(n) - 1);
    }

    if (n.equals("1" + "0".repeat(len - 2) + "1")) {
        return "9".repeat(len - 1);
    }

    if (n.equals("9".repeat(len))) {
        return "1" + "0".repeat(len - 1) + "1";
    }

    if (n.equals("1" + "0".repeat(len - 1))) {
        return "9".repeat(len - 1);
    }

    String root = n.substring(0, (len + 1) / 2);
    String root1 = String.valueOf(Integer.valueOf(root) - 1);
    String root2 = String.valueOf(Integer.valueOf(root) + 1);

    long nl = Long.valueOf(n);
    long p2 = palindrome(root2, len / 2);
    long pl = p2;
    long diff = Math.abs(p2 - nl);

    if (!isPalindrome(n)) {
        long p = palindrome(root, len / 2);
        if (Math.abs(p - nl) <= diff) {
            diff = Math.abs(p - nl);
            pl = p;
        }
    }

    long p1 = palindrome(root1, len / 2);
    if (Math.abs(p1 - nl) <= diff) {
        diff = Math.abs(p1 - nl);
        pl = p1;
    }
    return String.valueOf(pl);
}

private long palindrome(String root, int len) {
    return Long.valueOf(root + new StringBuilder(root.substring(0, len)).reverse().toString());
}
{% endhighlight %}

# Greedy

[Construct K Palindrome Strings][construct-k-palindrome-strings]

{% highlight java %}
public boolean canConstruct(String s, int k) {
    // bit vector
    int odd = 0;
    for (char c : s.toCharArray()) {
        odd ^= 1 << (c - 'a');
    }

    // if a bit is 1, it must be the center of a palindrome
    return s.length() >= k && Integer.bitCount(odd) <= k;
}
{% endhighlight %}

# Expand Around Center

[Palindromic Substring][palindromic-substring]

{% highlight java %}
public int countSubstrings(String s) {
    int n = s.length(), count = 0;
    for (int center = 0; center <= 2 * n - 1; center++) {
        int left = center / 2, right = left + center % 2;
        while (left >= 0 && right < n && s.charAt(left) == s.charAt(right)) {
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
// O(n ^ 2)
public String longestPalindrome(String s) {
    String result = "";
    int n = s.length(), max = 0;
    for (int center = 0; center <= 2 * n - 1; center++) {
        int left = center / 2, right = left + center % 2;
        while (left >= 0 && right < n && s.charAt(left) == s.charAt(right)) {
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

[Palindrome Partitioning II][palindrome-partitioning-ii]

{% highlight java %}
public int minCut(String s) {
    int n = s.length();
    // dp[i]: min cut of s.substring(0, i + 1)
    int[] dp = new int[n];
    // initialization
    // partitions into one-char groups
    for (int i = 0; i < n; i++) {
        dp[i] = i;
    }

    for (int center = 0; center <= 2 * n - 1; center++) {
        int left = center / 2, right = left + center % 2;
        while (left >= 0 && right < n && s.charAt(left) == s.charAt(right)) {
            // s.substring(0, left) + current palindrome == s.substring(0, right + 1);
            dp[right] = Math.min(dp[right], left == 0 ? 0 : dp[left - 1] + 1);
            left--;
            right++;
        }
    }
    return dp[n - 1];
}
{% endhighlight %}

# Manacher's Algorithm

[Manacher's algorithm](https://en.wikipedia.org/wiki/Longest_palindromic_substring#Manacher's_algorithm): find all palindromic substrings in `O(n)`.

[Longest Palindromic Substring][longest-palindromic-substring]

{% highlight java %}
// O(n)
public String longestPalindrome(String s) {
}
{% endhighlight %}

# Dynamic Programming

[Longest Palindromic Subsequence][longest-palindromic-subsequence]

{% highlight java %}
public int longestPalindromeSubseq(String s) {
    int n = s.length();
    // dp[i][j]: s.substring(i, j + 1)
    int[][] dp = new int[n][n];

    for (int i = n - 1; i >= 0; i--) {
        dp[i][i] = 1;
        for (int j = i + 1; j < n; j++) {
            if (s.charAt(i) == s.charAt(j)) {
                dp[i][j] = dp[i + 1][j - 1] + 2;
            } else {
                dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
    }

    return dp[0][n - 1];
}
{% endhighlight %}

[Maximize Palindrome Length From Subsequences][maximize-palindrome-length-from-subsequences]

{% highlight java %}
public int longestPalindrome(String word1, String word2) {
    String s = word1 + word2;
    int n = s.length(), n1 = word1.length();
    // dp[i][j]: s.substring(i, j + 1)
    int[][] dp = new int[n][n];

    int max = 0;
    for (int i = n - 1; i >= 0; i--) {
        dp[i][i] = 1;
        for (int j = i + 1; j < n; j++) {
            if (s.charAt(i) == s.charAt(j)) {
                dp[i][j] = dp[i + 1][j - 1] + 2;
                if (i < n1 && j >= n1) {
                    max = Math.max(max, dp[i][j]);
                }
            } else {
                dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
    }
    return max;
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
[find-the-closest-palindrome]: https://leetcode.com/problems/find-the-closest-palindrome/
[longest-palindromic-subsequence]: https://leetcode.com/problems/longest-palindromic-subsequence/
[longest-palindromic-substring]: https://leetcode.com/problems/longest-palindromic-substring/
[maximize-palindrome-length-from-subsequences]: https://leetcode.com/problems/maximize-palindrome-length-from-subsequences/
[palindrome-number]: https://leetcode.com/problems/palindrome-number/
[palindrome-pairs]: https://leetcode.com/problems/palindrome-pairs/
[palindrome-partitioning-ii]: https://leetcode.com/problems/palindrome-partitioning-ii/
[palindromic-substring]: https://leetcode.com/problems/palindromic-substring/
[prime-palindrome]: https://leetcode.com/problems/prime-palindrome/
[super-palindromes]: https://leetcode.com/problems/super-palindromes/
