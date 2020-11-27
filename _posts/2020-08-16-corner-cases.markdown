---
layout: post
title:  "Corner Cases"
---
[Reverse Integer][reverse-integer]

{% highlight java %}
public int reverse(int x) {
    int res = 0;
    while (x != 0) {
        int d = x % 10;
        int tmpRes = 10 * res + d;
        // avoid overflow
        if ((tmpRes - d) / 10 != res) {
            return 0;
        }
        res = tmpRes;
        x /= 10;
    }
    return res;
}
{% endhighlight %}

[Minimum Time Difference][minimum-time-difference]

{% highlight java %}
public int findMinDifference(List<String> timePoints) {
    int[] minutes = timePoints.stream()
        .mapToInt(this::convert)
        .sorted()
        .toArray();

    int d = 24 * 60;
    for (int i = 0; i < minutes.length - 1; i++) {
        d = Math.min(d, minutes[i + 1] - minutes[i]);
    }

    // comparison of circluar array element distances
    d = Math.min(d, minutes[0] + 24 * 60 - minutes[minutes.length - 1]);
    return d;
}

private int convert(String t) {
    String[] s = t.split(":");
    return Integer.valueOf(s[0]) * 60 + Integer.valueOf(s[1]);
}
{% endhighlight %}

[Most Visited Sector in a Circular Track][most-visited-sector-in-a-circular-track]

{% highlight java %}
public List<Integer> mostVisited(int n, int[] rounds) {
    List<Integer> result = new ArrayList<>();

    // start <= end
    if (rounds[0] <= rounds[rounds.length - 1]) {
        for (int i = rounds[0]; i <= rounds[rounds.length - 1]; i++) {
            result.add(i);
        }
    } else {
        // start > end
        for (int i = 1; i <= rounds[rounds.length - 1]; i++) {
            result.add(i);
        }   
        for (int i = rounds[0]; i <= n; i++) {
            result.add(i);
        } 
    }

    return result;
}
{% endhighlight %}

[Rabbits in Forest][rabbits-in-forest]

{% highlight java %}
public int numRabbits(int[] answers) {
    int[] c = new int[1000];
    int count = 0;
    for (int a : answers) {
        // If c[a] % (a + 1) == 0, there are c[a] / (a + 1) groups of (a + 1) rabbits
        // If c[a] % (a + 1) != 0, there are c[a] / (a + 1) + 1 groups of (a + 1) rabbits
        if (c[a]++ % (a + 1) == 0) {
            count += a + 1;
        }
    }

    return count;
}
{% endhighlight %}

[Heaters][heaters]

{% highlight java %}
public int findRadius(int[] houses, int[] heaters) {
    Arrays.sort(houses);
    Arrays.sort(heaters);

    int i = 0, radius = 0;
    for (int house : houses) {
        while (i < heaters.length - 1 && heaters[i] + heaters[i + 1] <= house * 2) {
            i++;
        }
        radius = Math.max(radius, Math.abs(heaters[i] - house));
    }
    return radius;
}
{% endhighlight %}

![Heaters](/assets/heaters.png)

[Sum of All Odd Length Subarrays][sum-of-all-odd-length-subarrays]

{% highlight java %}
public int sumOddLengthSubarrays(int[] arr) {
    int sum = 0;
    for (int i = 0; i < arr.length; i++) {
        // number of subarrays containing A[i]:
        // left: i + 1
        // right: n - i
        sum += ((i + 1) * (n - i) + 1) / 2 * A[i];
    }
    return sum;
}
{% endhighlight %}

[Sum of Mutated Array Closest to Target][sum-of-mutated-array-closest-to-target]

{% highlight java %}
public int findBestValue(int[] arr, int target) {
    Arrays.sort(arr);

    // after the loop, value is in (arr[i - 1], arr[i]]
    // arr.length - i is the number of remaining elements in the array
    int i = 0;
    while (i < arr.length && target > arr[i] * (arr.length - i)) {
        target -= arr[i++];
    }

    // value > arr[arr.length - 1]
    // i.e. target > sum(arr)
    if (i == arr.length) {
        return arr[arr.length - 1];
    }

    // ceiling function
    int value = target / (arr.length - i);
    // 3 * 5 < 19 < 4 * 5
    // 3 * 5 <= 15 < 4 * 5
    if (target - value * (arr.length - i) > (value + 1) * (arr.length - i) - target) {
        value++;
    }

    return value;
}
{% endhighlight %}

[Circle and Rectangle Overlapping][circle-and-rectangle-overlapping]

{% highlight java %}
public boolean checkOverlap(int radius, int x_center, int y_center, int x1, int y1, int x2, int y2) {
    // finds the closest point of the rectangle to the center.
    // if the center is in the rectangle, the center itself is the point
    int x = closest(x_center, x1, x2);
    int y = closest(y_center, y1, y2);

    int dx = x_center - x;
    int dy = y_center - y;

    return dx * dx + dy * dy <= radius * radius;
}

private int closest(int value, int min, int max) {
    return Math.max(min, Math.min(max, value));
}
{% endhighlight %}

[][swap-for-longest-repeated-character-substring]

{% highlight java %}
public int maxRepOpt1(String text) {
    // char : list of indexes
    Map<Character, List<Integer>> map = new HashMap<>();
    for (int i = 0; i < text.length(); i++) {
        map.computeIfAbsent(text.charAt(i), k -> new ArrayList<>()).add(i);
    }

    int result = 0;
    for (List<Integer> list : map.values()) {
        // count of chars in previous and current block
        int prev = 0, curr = 1, sum = 1;
        for (int i = 1; i < list.size(); i++) {
            if (list.get(i) == list.get(i - 1) + 1) {
                curr++;
            } else {
                // if previous block is more than 1 char away, clears it
                prev = list.get(i) == list.get(i - 1) + 2 ? curr : 0;
                curr = 1;
            }
            sum = Math.max(sum, curr + prev);
        }
        // if sum < list.size(), there are more of that char somewhere in the string 
        result = Math.max(result, sum + (sum < list.size() ? 1 : 0));
    }
    return result;
}
{% endhighlight %}

[Number of Steps to Reduce a Number in Binary Representation to One][number-of-steps-to-reduce-a-number-in-binary-representation-to-one]

{% highlight java %}
public int numSteps(String s) {
    int step = 0, carry = 0;
    for (int i = s.length() - 1; i > 0; i--) {
        step++;
        // additional step:
        // first encounter of '1'
        // or, encounter of '0' after the first '1' is encountered
        if (s.charAt(i) - '0' + carry == 1) {
            // carry is always 1 after the first '1' is encountered
            carry = 1;
            step++;
        }
    }
    return step + carry;
}
{% endhighlight %}

[Maximum Length of a Concatenated String with Unique Characters][maximum-length-of-a-concatenated-string-with-unique-characters]

{% highlight java %}
public int maxLength(List<String> arr) {
    List<Integer> list = new ArrayList<>();
    list.add(0);

    int max = 0;
    for (String s : arr) {
        int num = 0, hasDup = 0;
        for (char c : s.toCharArray()) {
            int mask = 1 << (c - 'a');
            hasDup |= num & mask;
            num |= mask;
        }

        if (hasDup == 0) {
            for (int i = list.size() - 1; i >= 0; i--) {
                if ((list.get(i) & num) == 0) {
                    int concat = list.get(i) | num;
                    list.add(concat);
                    max = Math.max(max, Integer.bitCount(concat));
                }
            }
        }
    }
    return max;
}
{% endhighlight %}

[Count Number of Teams][count-number-of-teams]

{% highlight java %}
public int numTeams(int[] rating) {
    int count = 0;
    // i is the middle soldier
    for (int i = 1; i < rating.length - 1; i++) {
        int[] less = new int[2], greater = new int[2];
        for (int j = 0; j < rating.length; j++) {
            // 0: left, 1: right
            int index = j > i ? 1 : 0;
            if (rating[i] < rating[j]) {
                less[index]++;
            }
            if (rating[i] > rating[j]) {
                greater[index]++;
            }
        }
        count += less[0] * greater[1] + greater[0] * less[1];
    }
    return count;
}
{% endhighlight %}

[circle-and-rectangle-overlapping]: https://leetcode.com/problems/circle-and-rectangle-overlapping/
[count-number-of-teams]: https://leetcode.com/problems/count-number-of-teams/
[heaters]: https://leetcode.com/problems/heaters/
[maximum-length-of-a-concatenated-string-with-unique-characters]: https://leetcode.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters/
[minimum-time-difference]: https://leetcode.com/problems/minimum-time-difference/
[most-visited-sector-in-a-circular-track]: https://leetcode.com/problems/most-visited-sector-in-a-circular-track/
[number-of-steps-to-reduce-a-number-in-binary-representation-to-one]: https://leetcode.com/problems/number-of-steps-to-reduce-a-number-in-binary-representation-to-one/
[rabbits-in-forest]: https://leetcode.com/problems/rabbits-in-forest/
[reverse-integer]: https://leetcode.com/problems/reverse-integer/
[sum-of-all-odd-length-subarrays]: https://leetcode.com/problems/sum-of-all-odd-length-subarrays/
[sum-of-mutated-array-closest-to-target]: https://leetcode.com/problems/sum-of-mutated-array-closest-to-target/
[swap-for-longest-repeated-character-substring]: https://leetcode.com/problems/swap-for-longest-repeated-character-substring/
