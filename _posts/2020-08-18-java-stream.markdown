---
layout: post
title:  "Java Stream"
tags: java
---
# Summing numbers
## Stream.reduce()
[T reduce(T identity, BinaryOperator\<T\> accumulator)](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#reduce-T-java.util.function.BinaryOperator-)

{% highlight java %}
List<Integer> integers = Arrays.asList(0, 1, 2);
Integer sum = integers.stream()
  .reduce(0, (a, b) -> a + b);
{% endhighlight %}

{% highlight java %}
List<Integer> integers = Arrays.asList(0, 1, 2);
Integer sum = integers.stream()
  .reduce(0, Integer::sum);
{% endhighlight %}

## Stream.collect() 
{% highlight java %}
List<Integer> integers = Arrays.asList(0, 1, 2);
Integer sum = integers.stream()
  .collect(Collectors.summingInt(Integer::intValue));
{% endhighlight %}

## IntStream.sum()
{% highlight java %}
List<Integer> integers = Arrays.asList(0, 1, 2);
Integer sum = integers.stream()
  .mapToInt(Integer::intValue)
  .sum();
{% endhighlight %}
