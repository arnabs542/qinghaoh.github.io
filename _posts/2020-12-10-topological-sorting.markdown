---
layout: post
title:  "Topological Sorting"
tags: graph
---

[Topological sorting](https://en.wikipedia.org/wiki/Topological_sorting)

[Course Schedule II][course-schedule-ii]

{% highlight java %}
// Kahn's algorithm
public int[] findOrder(int numCourses, int[][] prerequisites) {
    int[][] graph = new int[numCourses][numCourses];
    int[] indegree = new int[numCourses];

    for (int[] p : prerequisites) {
        graph[p[0]][p[1]] = 1;
        indegree[p[0]]++;
    }

    // zero indegree
    Queue<Integer> q = new LinkedList<>();
    for (int i = 0; i < numCourses; i++) {
        if (indegree[i] == 0) {
            q.offer(i);
        }
    }

    int[] order = new int[numCourses];
    int count = 0;
    while (!q.isEmpty()) {
        int course = q.poll();
        for (int i = 0; i < numCourses; i++) {
            if (graph[i][course] != 0 && --indegree[i] == 0) {
                q.offer(i);
            }
        }
        order[count++] = course;
    }

    return count == numCourses ? order : new int[0];
}
{% endhighlight %}

[Tree Diameter][tree-diameter]

[course-schedule-ii]: https://leetcode.com/problems/course-schedule-ii/
[minimum-height-trees]: https://leetcode.com/problems/minimum-height-trees/
[tree-diameter]: https://leetcode.com/problems/tree-diameter/
