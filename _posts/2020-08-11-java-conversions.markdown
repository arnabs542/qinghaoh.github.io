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

# Array -> Stream
[public static \<T\> Stream\<T\> stream(T\[\] array, int startInclusive, int endExclusive)](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#stream-T:A-int-int-)

# char + int
{% highlight java %}
char c = (char)('a' + i);
{% endhighlight %}
