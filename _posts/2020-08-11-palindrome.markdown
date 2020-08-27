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

## Expand Around Center

[Longest Palindromic Substring][longest-palindromic-substring]

{% highlight java %}
public String longestPalindrome(String s) {
    int longest = 0;
    String res = "";
    for (int i = 0; i < s.length(); i++) {
        int length1 = expand(s, i, i);
        int length2 = expand(s, i, i + 1);
        int length = Math.max(length1, length2);
        if (longest < length) {
            longest = length;
            res = s.substring(i - (length - 1) / 2, i + length / 2 + 1);
        }
    }
    return res;
}

private int expand(String s, int left, int right) {
    while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
        left--;
        right++;
    }
    return right - left - 1;
}
{% endhighlight %}

[Manacher's algorithm](https://en.wikipedia.org/wiki/Longest_palindromic_substring#Manacher's_algorithm)

## Dynamic Programming

[longest-palindromic-substring]: https://leetcode.com/problems/longest-palindromic-substring/
[palindrome-numbre]: https://leetcode.com/problems/palindrome-number/
