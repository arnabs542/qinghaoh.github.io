---
layout: post
title:  "Backtracking"
tags: dfs
---
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

[Palindrome Partitioning][palindrome-partitioning]

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

[Beautiful Arrangement][beautiful-arrangement]

[beautiful-arrangement]: https://leetcode.com/problems/beautiful-arrangement/
[combination-sum-ii]: https://leetcode.com/problems/combination-sum-ii/
[combination-sum]: https://leetcode.com/problems/combination-sum/
[palindrome-partitioning]: https://leetcode.com/problems/palindrome-partitioning/
[partition-equal-subset-sum]: https://leetcode.com/problems/partition-equal-subset-sum/
[permutations]: https://leetcode.com/problems/permutations/
[permutations-ii]: https://leetcode.com/problems/permutations-ii/
[subsets]: https://leetcode.com/problems/subsets/
[subsets-ii]: https://leetcode.com/problems/subsets-ii/
