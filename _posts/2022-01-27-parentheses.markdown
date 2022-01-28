---
layout: post
title:  "Parentheses"
---

[Reverse Substrings Between Each Pair of Parentheses][reverse-substrings-between-each-pair-of-parentheses]

{% highlight java %}
public String reverseParentheses(String s) {
    Deque<Integer> st = new ArrayDeque<>();
    int[] pairs = new int[s.length()];
    for (int i = 0; i < s.length(); ++i) {
        if (s.charAt(i) == '(')
            st.push(i);
        if (s.charAt(i) == ')') {
            int j = st.pop();
            pairs[i] = j;
            pairs[j] = i;
        }
    }

    StringBuilder sb = new StringBuilder();
    for (int i = 0, d = 1; i < s.length(); i += d) {
        if (s.charAt(i) == '(' || s.charAt(i) == ')') {
            i = pairs[i];
            d = -d;  // changes direction
        } else {
            sb.append(s.charAt(i));
        }
    }

    return sb.toString();
}
{% endhighlight %}

# Regress to Counter

[Remove Outermost Parentheses][remove-outermost-parentheses]

{% highlight java %}
public string removeouterparentheses(string s) {
    stringbuilder sb = new stringbuilder();
    int open = 0;
    for (char c : s.tochararray()) {
        if ((c == '(' && open++ > 0) || (c == ')' && --open > 0)) {
            sb.append(c);
        }
    }
    return sb.tostring();
}
{% endhighlight %}

[Minimum Add to Make Parentheses Valid][minimum-add-to-make-parentheses-valid]

{% highlight java %}
public int minAddToMakeValid(String S) {
    int notOpened = 0;  // '(' needed to make the String balanced
    int notClosed = 0;  // ')' needed to make the String balanced
    for (char c : S.toCharArray()) {
        if (c == '(') {
            notClosed++;
        } else if (notClosed == 0) {
            notOpened++;
        } else {
            notClosed--;
        }
    }

    return notOpened + notClosed;
}
{% endhighlight %}

[Minimum Insertions to Balance a Parentheses String][minimum-insertions-to-balance-a-parentheses-string]

{% highlight java %}
public int minInsertions(String s) {
    int count = 0;
    int notClosed = 0;  // ')' needed to make the String balanced
    for (char c : s.toCharArray()) {
        if (c == '(') {
            if (notClosed % 2 > 0) {
                notClosed--;
                count++;
            }
            notClosed += 2;
        } else {
            notClosed--;
            if (notClosed < 0) {
                notClosed += 2;
                count++;
            }
        }
    }

    return count + notClosed;
}
{% endhighlight %}

[Maximum Nesting Depth of Two Valid Parentheses Strings][maximum-nesting-depth-of-two-valid-parentheses-strings]

{% highlight java %}
public int[] maxDepthAfterSplit(String seq) {
    int[] result = new int[seq.length()];
    int opened = 0;
    for (int i = 0; i < seq.length(); i++) {
        if (seq.charAt(i) == '(') {
            opened++;
        }

        result[i] = opened % 2;  // split by parity

        if (seq.charAt(i) == ')') {
            opened--;
        }
    }

    return result;
}
{% endhighlight %}

[Score of Parentheses][score-of-parentheses]

{% highlight java %}
public int scoreOfParentheses(String S) {
    Deque<Integer> st = new ArrayDeque<>();
    int curr = 0;
    for (char c : S.toCharArray()) {
        if (c == '(') {
            st.push(curr);
            curr = 0;
        } else {
            curr = st.pop() + Math.max(curr * 2, 1);
        }
    }
    return curr;
}
{% endhighlight %}

{% highlight java %}
public int scoreOfParentheses(String S) {
    int score = 0, opened = 0;
    for (int i = 0; i < S.length(); i++) {
        if (S.charAt(i) == '(') {
            opened++;
        } else {
            opened--;
            if (S.charAt(i - 1) == '(') {
                // number of exterior sets of parentheses that contains this core
                score += 1 << opened;
            }
        }
    }
    return score;
}
{% endhighlight %}

[Longest Valid Parentheses][longest-valid-parentheses]

TODO: what's the invariant/intuiation behind this?

{% highlight java %}
public int longestValidParentheses(String s) {
    Deque<Integer> st = new ArrayDeque<>();
    st.push(-1);  // imagine there's a ')' at index -1

    int max = 0;
    for (int i = 0; i < s.length(); i++) {
        if (s.charAt(i) == '(') {
            st.push(i);
        } else {
            st.pop();
            if (st.isEmpty()) {
                st.push(i);
            } else {
                max = Math.max(max, i - st.peek());
            }
        }
    }
    return max;
}
{% endhighlight %}

[Valid Parenthesis String][valid-parenthesis-string]

{% highlight java %}
public boolean checkValidString(String s) {
    // number of open parenthses is in [min, max];
    int min = 0, max = 0;
    for (char c : s.toCharArray()) {
        if (c == '(') {
            max++;
            min++;
        } else if (c == ')') {
            max--;
            min--;
        } else {
            // '*' -> '('
            max++;
            // '*' -> ')'
            min--; // if `*` become `)` then openCount--
        }

        if (max < 0) {
            return false;
        }
        min = Math.max(min, 0);
    }
    return min == 0;
}
{% endhighlight %}

Similar: [Check if a Parentheses String Can Be Valid][check-if-a-parentheses-string-can-be-valid]

{% highlight java %}
public boolean canBeValid(String s, String locked) {
    return s.length() % 2 == 0 && checkValidString(s, locked);
}
{% endhighlight %}

[check-if-a-parentheses-string-can-be-valid]: https://leetcode.com/problems/check-if-a-parentheses-string-can-be-valid/
[longest-valid-parentheses]: https://leetcode.com/problems/longest-valid-parentheses/
[maximum-nesting-depth-of-two-valid-parentheses-strings]: https://leetcode.com/problems/maximum-nesting-depth-of-two-valid-parentheses-strings/
[minimum-add-to-make-parentheses-valid]: https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/
[minimum-insertions-to-balance-a-parentheses-string]: https://leetcode.com/problems/minimum-insertions-to-balance-a-parentheses-string/
[remove-outermost-parentheses]: https://leetcode.com/problems/remove-outermost-parentheses/
[reverse-substrings-between-each-pair-of-parentheses]: https://leetcode.com/problems/reverse-substrings-between-each-pair-of-parentheses/
[score-of-parentheses]: https://leetcode.com/problems/score-of-parentheses/
[valid-parenthesis-string]: https://leetcode.com/problems/valid-parenthesis-string/
