---
layout: post
title:  "Tree Recursion"
tags: tree
---
# Global State

[Convert BST to Greater Tree][convert-bst-to-greater-tree]

{% highlight java %}
private int sum = 0;  // global state

public TreeNode convertBST(TreeNode root) {
    if (root != null) {
        convertBST(root.right);
        sum += root.val;
        root.val = sum;
        convertBST(root.left);
    }
    return root;
}
{% endhighlight %}

[Minimum Absolute Difference in BST][minimum-absolute-difference-in-bst]

{% highlight java %}
private int d = Integer.MAX_VALUE;
private TreeNode prev = null;

public int getMinimumDifference(TreeNode root) {
    if (root == null) {
        return Integer.MAX_VALUE;
    }

    getMinimumDifference(root.left);

    // inorder
    if (prev != null) {
        d = Math.min(d, root.val - prev.val);
    }
    prev = root;

    getMinimumDifference(root.right);

    return d;
}
{% endhighlight %}

[Diameter of Binary Tree][diameter-of-binary-tree]

{% highlight java %}
private int diameter = 0;

public int diameterOfBinaryTree(TreeNode root) {
    height(root);
    return diameter;
}

private int height(TreeNode node) {
    if (node == null) {
        return 0;
    }

    int left = height(node.left);  // DFS
    int right = height(node.right);
    diameter = Math.max(diameter, left + right);
    return Math.max(left, right) + 1;
}
{% endhighlight %}

[Diameter of N-Ary Tree][diameter-of-n-ary-tree]

[Binary Tree Maximum Path Sum][binary-tree-maximum-path-sum]

{% highlight java %}
private int sum;

public int maxPathSum(TreeNode root) {
    sum = Integer.MIN_VALUE;
    dfs(root);
    return sum;
}

/**
 * Max sum of paths which start from node.
 * @param node the root of the subtree
 * @return the max sum of paths starting from node
 */
private int dfs(TreeNode node) {
    if (node == null) {
        return 0;
    }

    int left = Math.max(0, dfs(node.left));
    int right = Math.max(0, dfs(node.right));

    sum = Math.max(sum, node.val + left + right);
    return node.val + Math.max(left, right);
}
{% endhighlight %}

[Longest Univalue Path][longest-univalue-path]

{% highlight java %}
private int path;

public int longestUnivaluePath(TreeNode root) {
    path = 0;
    length(root);
    return path;
}

/**
 * Max length of univalue paths which start from node.
 * @param node the root of the subtree
 * @return the max length of univalue paths starting from node
 */
private int length(TreeNode node) {
    if (node == null) {
        return 0;
    }

    // max univalue paths from the child nodes
    int left = length(node.left);
    int right = length(node.right);

    // max univalue paths from the current node
    int leftPath = 0, rightPath = 0;
    if (node.left != null && node.left.val == node.val) {
        leftPath = left + 1;
    }
    if (node.right != null && node.right.val == node.val) {
        rightPath = right + 1;
    }

    path = Math.max(path, leftPath + rightPath);
    return Math.max(leftPath, rightPath);
}
{% endhighlight %}

[Find Bottom Left Tree Value][find-bottom-left-tree-value]

{% highlight java %}
private int bottomLeft = 0;
private int depth = -1;

public int findBottomLeftValue(TreeNode root) {
    dfs(root, 0);
    return bottomLeft;
}

private void dfs(TreeNode node, int d) {
    if (depth < d) {
        bottomLeft = node.val;
        depth = d;
    }

    if (node.left != null) {
        dfs(node.left, d + 1);
    }
    if (node.right != null) {
        dfs(node.right, d + 1);
    }

    return;
}
{% endhighlight %}

Another solution is right-to-left-BFS.

[Longest ZigZag Path in a Binary Tree][longest-zigzag-path-in-a-binary-tree]

{% highlight java %}
public int longestZigZag(TreeNode root) {
    return dfs(root)[2];
}

private int[] dfs(TreeNode node) {
    if (node == null) {
        // max zigzag path at:
        // [0]: left child node
        // [1]: right child node
        // [2]: this node
        return new int[]{-1, -1, -1};
    }

    int[] left = dfs(node.left);
    int[] right = dfs(node.right);

    // left[1], right[0] makes the path zigzag
    // leaves will have zigzag path == -1 + 1 == 0
    int path = Math.max(Math.max(left[1], right[0]) + 1, Math.max(left[2], right[2]));

    return new int[]{left[1] + 1, right[0] + 1, path};
}
{% endhighlight %}

[Second Minimum Node in a Binary Tree][second-minimum-node-in-a-binary-tree]

{% highlight java %}
private int min;
private long secondMin = Long.MAX_VALUE;

public int findSecondMinimumValue(TreeNode root) {
    // root has the minimum value
    min = root.val;
    dfs(root);
    return secondMin < Long.MAX_VALUE ? (int)secondMin : -1;
}

private void dfs(TreeNode node) {
    if (node == null) {
        return;
    }

    if (min < node.val && node.val < secondMin) {
        secondMin = node.val;
    } else if (min == node.val) {
        dfs(node.left);
        dfs(node.right);
    }
}
{% endhighlight %}

{% highlight java %}
public int findSecondMinimumValue(TreeNode root) {
    // tree leaf
    if (root.left == null) {
        return -1;
    }

    // The second minimum value on left path including root
    int left = root.left.val == root.val ? findSecondMinimumValue(root.left) : root.left.val;
    // The second minimum value on right path including root
    int right = root.right.val == root.val ? findSecondMinimumValue(root.right) : root.right.val;

    // if left == -1 && right == -1, returns -1
    // else left == -1 || right == -1, returns none -1
    // else returns the lesser of the two second minimum values
    return left == -1 || right == -1 ? Math.max(left, right) : Math.min(left, right);
}
{% endhighlight %}

[Lowest Common Ancestor of Deepest Leaves][lowest-common-ancestor-of-deepest-leaves]

{% highlight java %}
public TreeNode lcaDeepestLeaves(TreeNode root) {
    return dps(root).getValue();
}

// <depth, lowest_common_ancestor>
private Pair<Integer, TreeNode> dps(TreeNode node) {
    if (node == null) {
        return new Pair(0, null);
    }

    Pair<Integer, TreeNode> l = dps(node.left), r = dps(node.right);

    int d1 = l.getKey(), d2 = r.getKey();
    return new Pair(Math.max(d1, d2) + 1, d1 == d2 ? node : d1 > d2 ? l.getValue() : r.getValue());
}
{% endhighlight %}

[N-ary Tree Level Order Traversal][n-ary-tree-level-order-traversal]

{% highlight java %}
private List<List<Integer>> list;

public List<List<Integer>> levelOrder(Node root) {
    list = new ArrayList<>();
    dfs(root, 0);
    return list;
}

private void dfs(Node node, int level) {
    if (node == null) {
        return;
    }

    if (list.size() == level) {
        list.add(new ArrayList<>());
    }
    list.get(level).add(node.val);

    for (Node child : node.children) {
        dps(child, level + 1);
    }
}
{% endhighlight %}

Intuitively BFS also works for level order traversal. The condition to increment level is:

{% highlight java %}
int size = q.size();
List<Integer> currLevel = new ArrayList<>();
for (int i = 0; i < size; i++) {
    Node node = q.poll();
    currLevel.add(node.val);
    for (Node child : node.children) {
        q.offer(child);
    }
}
{% endhighlight %}

[Increasing Order Search Tree][increasing-order-search-tree]

{% highlight java %}
private TreeNode curr;  // current node of the list

public TreeNode increasingBST(TreeNode root) {
    TreeNode head = new TreeNode(0);
    curr = head;
    inorder(root);
    return head.right;
}

private void inorder(TreeNode node) {
    if (node == null) {
        return;
    }

    inorder(node.left);
    node.left = null;
    curr.right = node;
    curr = node;
    inorder(node.right);
}
{% endhighlight %}

{% highlight java %}
public TreeNode increasingBST(TreeNode root) {
    return inorder(root, null);
}

/**
 * Inorder traverses and rearranges the tree.
 * @param node current tree node
 * @param next next ancestor node in inorder traversal
 * @return head of the list after rearrangement
 */
public TreeNode inorder(TreeNode node, TreeNode next) {
    // If the current node is a left child, next will be its parent
    // else if the current node is a right child, next will be its "leftmost" parent's parent
    if (node == null) {
        return next;
    }

    TreeNode left = inorder(node.left, node);
    node.left = null;
    // If node.right == 0, it links the next ancesotr to the rearranged right list
    // otherwise it links the rearranged right list to the current node
    node.right = inorder(node.right, next);
    return left;
}
{% endhighlight %}

# Top-down Local State

[Count Good Nodes in Binary Tree][count-good-nodes-in-binary-tree]

{% highlight java %}
public int goodNodes(TreeNode root) {
    return dfs(root, root.val);
}

private int dfs(TreeNode node, int max) {
    if (node == null) {
        return 0;
    }

    int count = node.val >= max ? 1 : 0;

    max = Math.max(max, node.val);
    count += dfs(node.left, max);
    count += dfs(node.right, max);

    return count;
}
{% endhighlight %}

[Maximum Difference Between Node and Ancestor][maximum-difference-between-node-and-ancestor]

{% highlight java %}
private int diff = 0;

public int maxAncestorDiff(TreeNode root) {
    dfs(root, root.val, root.val);
    return diff;
}

private void dfs(TreeNode node, int min, int max) {
    if (node == null) {
        return;
    }

    min = Math.min(min, node.val);
    max = Math.max(max, node.val);
    diff = Math.max(diff, max - min);

    dfs(node.left, min, max);
    dfs(node.right, min, max);
}
{% endhighlight %}

[Pseudo-Palindromic Paths in a Binary Tree][pseudo-palindromic-paths-in-a-binary-tree]

{% highlight java %}
public int pseudoPalindromicPaths (TreeNode root) {
    return dfs(root, 0);
}

/**
 * Count pseudo palindromic paths by DFS.
 * In a pseudo palindromic path, each digit appears at most once.
 * @param node current node
 * @param vector an integer whose i-th bit indicates the presence of digit i
 * @return count of pseudo palinddromic paths
 */
private int dfs(TreeNode node, int vector) {
    if (node == null) {
        return 0;
    }

    vector ^= 1 << (node.val - 1);
    int count = dfs(node.left, vector) + dfs(node.right, vector);

    // leaf node
    if (node.left == node.right && (vector & (vector - 1)) == 0) {
        count++;
    }

    return count;
}
{% endhighlight %}

# Nested DFS

[Path Sum III][path-sum-iii]

{% highlight java %}
public int pathSum(TreeNode root, int sum) {
    if (root == null) {
        return 0;
    }

    return pathSumFromNode(root, sum) + pathSum(root.left, sum) + pathSum(root.right, sum);
}

/**
 * Counts paths with target sum which start from node.
 * @param node the root of the subtree
 * @param sum target sum
 * @return the count of paths with target sum which start from node
 */
private int pathSumFromNode(TreeNode node, int sum) {
    if (node == null) {
        return 0;
    }

    return (sum == node.val ? 1 : 0) + pathSumFromNode(node.left, sum - node.val) + pathSumFromNode(node.right, sum - node.val);
}
{% endhighlight %}

# Backtracking

[Path Sum III][path-sum-iii]

{% highlight java %}
public int pathSum(TreeNode root, int sum) {
    Map<Integer, Integer> prefixSum = new HashMap();
    prefixSum.put(0,1);
    return dfs(root, 0, sum, prefixSum);
}

public int dfs(TreeNode node, int currSum, int target, Map<Integer, Integer> prefixSum) {
    if (node == null) {
        return 0;
    }

    currSum += node.val;
    int count = prefixSum.getOrDefault(currSum - target, 0);

    // backtracking
    prefixSum.compute(currSum, (k, v) -> v == null ? 1 : v + 1);
    count += dfs(node.left, currSum, target, prefixSum) + dfs(node.right, currSum, target, prefixSum);
    prefixSum.compute(currSum, (k, v) -> v - 1);

    return count;
}
{% endhighlight %}

[binary-tree-maximum-path-sum]: https://leetcode.com/problems/binary-tree-maximum-path-sum/
[convert-bst-to-greater-tree]: https://leetcode.com/problems/convert-bst-to-greater-tree/
[count-good-nodes-in-binary-tree]: https://leetcode.com/problems/count-good-nodes-in-binary-tree/
[diameter-of-binary-tree]: https://leetcode.com/problems/diameter-of-binary-tree/
[diameter-of-n-ary-tree]: https://leetcode.com/problems/diameter-of-n-ary-tree/
[find-bottom-left-tree-value]: https://leetcode.com/problems/find-bottom-left-tree-value/
[increasing-order-search-tree]: https://leetcode.com/problems/increasing-order-search-tree/
[longest-univalue-path]: https://leetcode.com/problems/longest-univalue-path/
[longest-zigzag-path-in-a-binary-tree]: https://leetcode.com/problems/longest-zigzag-path-in-a-binary-tree/
[lowest-common-ancestor-of-deepest-leaves]: https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves/
[maximum-difference-between-node-and-ancestor]: https://leetcode.com/problems/maximum-difference-between-node-and-ancestor/
[minimum-absolute-difference-in-bst]: https://leetcode.com/problems/minimum-absolute-difference-in-bst/
[n-ary-tree-level-order-traversal]: https://leetcode.com/problems/n-ary-tree-level-order-traversal/
[path-sum-iii]: https://leetcode.com/problems/path-sum-iii/
[pseudo-palindromic-paths-in-a-binary-tree]: https://leetcode.com/problems/pseudo-palindromic-paths-in-a-binary-tree/
[second-minimum-node-in-a-binary-tree]: https://leetcode.com/problems/second-minimum-node-in-a-binary-tree/
