---
layout: post
title:  "Recursion"
tags: recursion
---

[Strobogrammatic Number II][strobogrammatic-number-ii]

{% highlight java %}
public List<String> findStrobogrammatic(int n) {
    return findStrobogrammatic(n, n);
}

private List<String> findStrobogrammatic(int n, int initialN) {
    if (n == 0) {
        return new ArrayList<>(Arrays.asList(""));
    }

    if (n == 1) {
        return new ArrayList<>(Arrays.asList("0", "1", "8"));
    }

    List<String> list = new ArrayList<>();
    for (String s : findStrobogrammatic(n - 2, initialN)) {
        if (n != initialN) {
            list.add("0" + s + "0");
        }
        list.add("1" + s + "1");
        list.add("8" + s + "8");
        list.add("6" + s + "9");
        list.add("9" + s + "6");
    }
    return list;
}
{% endhighlight %}

[Largest Merge Of Two Strings][largest-merge-of-two-strings]

{% highlight java %}
public String largestMerge(String word1, String word2) {
    if (word1.length() == 0  || word2.length() == 0) {
        return word1 + word2;
    }

    return word1.compareTo(word2) > 0 ?
        word1.charAt(0) + largestMerge(word1.substring(1), word2) : word2.charAt(0) + largestMerge(word1, word2.substring(1));
}
{% endhighlight %}

[Remove Invalid Parentheses][remove-invalid-parentheses]

{% highlight java %}
public List<String> removeInvalidParentheses(String s) {
    List<String> list = new ArrayList<>();
    dfs(s, 0, 0, '(', ')', list);
    return list;
}

public void dfs(String s, int iStart, int jStart, char c1, char c2, List<String> list) {
    // conceptually, c1 is the open parenthesis, c2 is the open parenthesis
    int open = 0, closed = 0;
    for (int i = iStart; i < s.length(); i++) {
        char c = s.charAt(i);
        if (c == c1) {
            open++;
        }
        if (c == c2) {
            closed++;
        }

        // there's an extra closed parenthesis needs to be removed
        if (closed > open) {
            for (int j = jStart; j <= i; j++) {
                // removes the first closed parenthesis in each sequence of consecutive closed parentheses
                // skips duplicates
                if (s.charAt(j) == c2 && (j == jStart || s.charAt(j - 1) != c2)) {
                    // now open == closed until i
                    // j is actually the original j + 1
                    dfs(s.substring(0, j) + s.substring(j + 1), i, j, c1, c2, list);
                }
            }
            return;
        }
    }

    // reverses the String and removes invalid open parentheses
    String reversed = new StringBuilder(s).reverse().toString();
    if (c1 == '(') {
        dfs(reversed, 0, 0, ')','(', list);
    } else {
        list.add(reversed);
    }
}
{% endhighlight %}

[largest-merge-of-two-strings]: https://leetcode.com/problems/largest-merge-of-two-strings/
[remove-invalid-parentheses]: https://leetcode.com/problems/remove-invalid-parentheses/
[strobogrammatic-number-ii]: https://leetcode.com/problems/strobogrammatic-number-ii/
