---
layout: post
title:  "Java Conversions"
tags: java
---
# int[] -> Integer[]
{% highlight java %}
int[] a = {0, 1, 2};
Integer[] b = Arrays.stream(a).boxed().toArray(Integer[]::new);
{% endhighlight %}

# Integer[] -> int[]
{% highlight java %}
Integer[] a = {0, 1, 2};
int[] b = Arrays.stream(a).mapToInt(Integer::intValue).toArray();
{% endhighlight %}

# int[] -> List\<Integer\>
{% highlight java %}
int[] a = {0, 1, 2};
List<Integer> b = Arrays.stream(a).boxed().collect(Collectors.toList());
{% endhighlight %}

# Integer[] -> Set\<Integer\>
{% highlight java %}
Integer[] a = {0, 1, 2};
Set<Integer> b = Arrays.stream(a).collect(Collectors.toSet());

{% endhighlight %}
# Array -> Stream
[public static \<T\> Stream\<T\> stream(T\[\] array, int startInclusive, int endExclusive)](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#stream-T:A-int-int-)

# char + int
{% highlight java %}
char c = (char)('a' + i);
{% endhighlight %}

# char number -> int
{% highlight java %}
char c = '9';
int a = c - '0';
{% endhighlight %}


# Key with the max value in a map
{% highlight java %}
Collections.max(map.entrySet(), Map.Entry.comparingByValue()).getKey();
{% endhighlight %}
