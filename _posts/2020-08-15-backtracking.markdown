---
layout: post
title:  "Backtracking"
tags: dfs
---

Backtracking = DFS + pruning

[Permutations][permutations]

{% highlight java %}
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> list = new ArrayList<>();
    backtrack(list, new ArrayList<>(), nums);
    return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tmpList, int[] nums) {
    if (tmpList.size() == nums.length) {
        list.add(new ArrayList<>(tmpList));
        return;
    }

    // increment
    for (int i = 0; i < nums.length; i++) { 
        if (!tmpList.contains(nums[i])) {
            tmpList.add(nums[i]);
            backtrack(list, tmpList, nums);
            tmpList.remove(tmpList.size() - 1);
        }
    }
}
{% endhighlight %}

{% highlight java %}
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> list = new ArrayList<>();
    backtrack(list, Arrays.stream(nums).boxed().collect(Collectors.toList()), 0);
    return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tmpList, int index) {
    if (index == tmpList.size()) {
        list.add(new ArrayList<>(tmpList));
        return;
    }

    // swap
    for (int i = index; i < tmpList.size(); i++) {
        Collections.swap(tmpList, i, index);
        backtrack(list, tmpList, index + 1);
        Collections.swap(tmpList, index, i);
    }
}
{% endhighlight %}

[Permutations II][permutations-ii]

{% highlight java %}
public List<List<Integer>> permuteUnique(int[] nums) {
    List<List<Integer>> list = new ArrayList<>();
    Arrays.sort(nums);
    backtrack(list, new ArrayList<>(), nums, new boolean[nums.length]);
    return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tmpList, int[] nums, boolean[] used) {
    if (tmpList.size() == nums.length) {
        list.add(new ArrayList<>(tmpList));
        return;
    }

    // increment
    for (int i = 0; i < nums.length; i++) {
        if (used[i] || i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
            continue;
        }

        used[i] = true;
        tmpList.add(nums[i]);
        backtrack(list, tmpList, nums, used);
        used[i] = false;
        tmpList.remove(tmpList.size() - 1);
    }
}
{% endhighlight %}

[Palindrome Permutation II][palindrome-permutation-ii]

[Beautiful Arrangement][beautiful-arrangement]

[Subsets][subsets]

{% highlight java %}
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> list = new ArrayList<>();
    backtrack(list, new ArrayList<>(), nums, 0);
    return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tmpList, int[] nums, int index) {
    list.add(new ArrayList<>(tmpList));

    for (int i = index; i < nums.length; i++) { 
        tmpList.add(nums[i]);
        backtrack(list, tmpList, nums, i + 1);
        tmpList.remove(tmpList.size() - 1);
    }
}
{% endhighlight %}

[Subsets II][subsets-ii]

{% highlight java %}
public List<List<Integer>> subsetsWithDup(int[] nums) {
    List<List<Integer>> list = new ArrayList<>();
    Arrays.sort(nums);
    backtrack(list, new ArrayList<>(), nums, 0);
    return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tmpList, int[] nums, int index) {
    list.add(new ArrayList<>(tmpList));

    for (int i = index; i < nums.length; i++) {
        if (i > index && nums[i] == nums[i - 1]) {
            continue;
        }

        tmpList.add(nums[i]);
        backtrack(list, tmpList, nums, i + 1);
        tmpList.remove(tmpList.size() - 1);
    }
}
{% endhighlight %}

[Combination Sum][combination-sum]

{% highlight java %}
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> list = new ArrayList<>();
    backtrack(list, new ArrayList<>(), candidates, 0, target);
    return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tmpList, int[] nums, int index, int target) {
    if (target == 0) {
        list.add(new ArrayList<>(tmpList));
        return;
    }

    for (int i = index; i < nums.length; i++) {
        if (nums[i] <= target) {
            tmpList.add(nums[i]);
            backtrack(list, tmpList, nums, i, target - nums[i]);
            tmpList.remove(tmpList.size() - 1);
        } 
    }
}
{% endhighlight %}

[Combination Sum II][combination-sum-ii]

{% highlight java %}
public List<List<Integer>> combinationSum2(int[] candidates, int target) {
    List<List<Integer>> list = new ArrayList<>();
    Arrays.sort(candidates);
    backtrack(list, new ArrayList<>(), candidates, 0, target);
    return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tmpList, int[] nums, int index, int target) {
    if (target == 0) {
        list.add(new ArrayList<>(tmpList));
        return;
    }

    for (int i = index; i < nums.length; i++) {
        if (i > index && nums[i] == nums[i - 1]) {
            continue;
        }

        if (nums[i] <= target) {
            tmpList.add(nums[i]);
            backtrack(list, tmpList, nums, i + 1, target - nums[i]);
            tmpList.remove(tmpList.size() - 1);
        } 
    }
}
{% endhighlight %}

[Combination Sum III][combination-sum-iii]

{% highlight java %}
private final int max = 9;

public List<List<Integer>> combinationSum3(int k, int n) {
    List<List<Integer>> list = new ArrayList<>();
    backtrack(list, new ArrayList<>(), 1, k, n);
    return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tmpList, int start, int k, int n) {
    if (k == 0 && n == 0) {
        list.add(new ArrayList<>(tmpList));
        return;
    }

    if (k < 0 || n < 0) {
        return;
    }

    for (int i = start; i <= max; i++) {
        if (i <= n) {
            tmpList.add(i);
            backtrack(list, tmpList, i + 1, k - 1, n - i);
            tmpList.remove(Integer.valueOf(i));
        } 
    }
}
{% endhighlight %}

[Factor Combinations][factor-combinations]

{% highlight java %}
public List<List<Integer>> getFactors(int n) {
    List<List<Integer>> list = new ArrayList<>();
    backtrack(list, new ArrayList<>(), 2, n);
    return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tmpList, int index, int n) {
    if (n == 1) {
        if (tmpList.size() > 1) {
            list.add(new ArrayList<Integer>(tmpList));
        }
        return;
    }

    for (int i = index; i <= n; i++) {
        if (n % i == 0) {
            tmpList.add(i);
            backtrack(list, tmpList, i, n / i);
            tmpList.remove(tmpList.size() - 1);
        }
    }
}
{% endhighlight %}

[Palindrome Partitioning][palindrome-partitioning]

{% highlight java %}
public List<List<String>> partition(String s) {
    List<List<String>> list = new ArrayList<>();
    backtrack(list, new ArrayList<>(), s, 0);
    return list;
}

private void backtrack(List<List<String>> list, List<String> tmpList, String s, int index) {
    if (index == s.length()) {
        list.add(new ArrayList<>(tmpList));
    }

    for (int i = index + 1; i <= s.length(); i++) {
        String str = s.substring(index, i);
        if (isPalindrome(str)) {
            tmpList.add(str);
            backtrack(list, tmpList, s, i);
            tmpList.remove(tmpList.size() - 1);
        }
    }
}

private boolean isPalindrome(String s) {
    int left = 0, right = s.length() - 1;
    while (left < right) {
        if (s.charAt(left++) != s.charAt(right--)) {
            return false;
        }
    }
    return true;
}
{% endhighlight %}

[Partition Equal Subset Sum][partition-equal-subset-sum]

{% highlight java %}
public boolean canPartition(int[] nums) {
    int sum = 0;
    for (int i = 0; i < nums.length; i++) {
        sum += nums[i];
    }

    if (sum % 2 == 1) {
        return false;
    }

    Arrays.sort(nums);
    return backtrack(nums, 0, sum / 2);
}

private boolean backtrack(int[] nums, int index, int target) {
    if (target == 0) {
        return true;
    }

    for (int i = index; i < nums.length; i++) {
        // skips duplicates
        if (i > index && nums[i] == nums[i - 1]) {
            continue;
        }

        if (nums[i] <= target && backtrack(nums, i + 1, target - nums[i])) {
            return true;
        }
    }
    return false;
}
{% endhighlight %}

[Generalized Abbreviation][generalized-abbreviation]

{% highlight java %}
private String word;

public List<String> generateAbbreviations(String word) {
    this.word = word;
    List<String> list = new ArrayList<>();
    backtrack(list, new StringBuilder(), 0, 0);
    return list;
}

// k is the count of consecutive abbreviated characters
private void backtrack(List<String> list, StringBuilder sb, int index, int k) {
    int length = sb.length();
    if (index == word.length()) {
        // abbreviates the last k letters
        if (k > 0) {
            sb.append(k);
        }
        list.add(sb.toString());
        sb.setLength(length);
        return;
    }

    // the branch that word.charAt(index) is abbreviated
    backtrack(list, sb, index + 1, k + 1);

    // the branch that word.charAt(index) is kept
    // abbreviates the last k letters
    if (k > 0) {
        sb.append(k);
    }
    // appends word.charAt(index)
    sb.append(word.charAt(index));
    backtrack(list, sb, index + 1, 0);
    sb.setLength(length);
}
{% endhighlight %}

# Subset Sum Problem

[Subset sum problem](https://en.wikipedia.org/wiki/Subset_sum_problem): NP-complete

[Partition to K Equal Sum Subsets][partition-to-k-equal-sum-subsets]

{% highlight java %}
public boolean canPartitionKSubsets(int[] nums, int k) {
    int sum = 0, max = 0;
    for (int num : nums) {
        sum += num;
        max = Math.max(max, num);
    }

    if (sum % k != 0 || max > sum / k) {
        return false;
    }

    Arrays.sort(nums);
    // searches in reverse order, so that subset sizes decrease faster
    return backtrack(nums, sum / k, nums.length - 1, new int[k]);
}

private boolean backtrack(int[] nums, int target, int index, int[] subsets) {
    // all elements are placed into subsets
    if (index < 0) {
        return true;
    }

    for (int i = 0; i < subsets.length; i++) {
        if (subsets[i] + nums[index] <= target) {
            subsets[i] += nums[index];
            // no need to clone subsets
            if (backtrack(nums, target, index - 1, subsets)) {
                return true;
            }
            subsets[i] -= nums[index];
        }

        // after unwinding, if current subset is empty,
        // we know nums[index] can't be placed in any empty subset.
        // all the subsets following current subset are empty,
        // so we skip all of them.
        if (subsets[i] == 0) {
            break;
        }
    }
    return false;
}
{% endhighlight %}

{% highlight java %}
public boolean canPartitionKSubsets(int[] nums, int k) {
    int sum = 0, max = 0;
    for (int num : nums) {
        sum += num;
        max = Math.max(max, num);
    }

    if (sum % k != 0 || max > sum / k) {
        return false;
    }

    Arrays.sort(nums);
    // searches in reverse order, so that subset sizes decrease faster
    return backtrack(nums, nums.length - 1, new boolean[nums.length], k, 0, sum / k);
}

private boolean backtrack(int[] nums, int index, boolean[] visited, int k, int sum, int target) {
    if (k == 1) {
        return true;
    }

    if (sum == target) {
        return backtrack(nums, nums.length - 1, visited, k - 1, 0, target);
    }

    for (int i = index; i >= 0; i--) {
        if (!visited[i] && sum + nums[i] <= target) {
            visited[i] = true;
            if (backtrack(nums, i - 1, visited, k, sum + nums[i], target)) {
                return true;
            }  
            visited[i] = false;
        }
    }

    return false;
}
{% endhighlight %}

[Matchsticks to Square][matchsticks-to-square]

[beautiful-arrangement]: https://leetcode.com/problems/beautiful-arrangement/
[combination-sum]: https://leetcode.com/problems/combination-sum/
[combination-sum-ii]: https://leetcode.com/problems/combination-sum-ii/
[combination-sum-iii]: https://leetcode.com/problems/combination-sum-iii/
[factor-combinations]: https://leetcode.com/problems/factor-combinations/
[generalized-abbreviation]: https://leetcode.com/problems/generalized-abbreviation/
[matchsticks-to-square]: https://leetcode.com/problems/matchsticks-to-square/
[palindrome-partitioning]: https://leetcode.com/problems/palindrome-partitioning/
[palindrome-permutation-ii]: https://leetcode.com/problems/palindrome-permutation-ii/
[partition-equal-subset-sum]: https://leetcode.com/problems/partition-equal-subset-sum/
[partition-to-k-equal-sum-subsets]: https://leetcode.com/problems/partition-to-k-equal-sum-subsets/
[permutations]: https://leetcode.com/problems/permutations/
[permutations-ii]: https://leetcode.com/problems/permutations-ii/
[subsets]: https://leetcode.com/problems/subsets/
[subsets-ii]: https://leetcode.com/problems/subsets-ii/
