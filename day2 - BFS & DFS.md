# Topological Sort

In general, given a directed graph, **converting this directed graph into a linear order** is called topological sorting.

Of course, topological sorting also needs to detect whether the directed graph has a cycle, that is, whether there is a circular dependency, because this situation cannot be linearly sorted.

Therefore, topological sorting is also a common method in graph theory to determine directed acyclic graphs.

There are 2 algorithms for implementing topological sorting: **Kahn's algorithm (BFS) and DFS**

When we do topological sorting, we should give priority to finding nodes with **in-degree 0**. Only when in-degree 0 is it the starting node.

1. Find the node with in-degree 0 and add it to the result set
2. Remove the node from the graph
3. Repeat the above two steps until all nodes are removed from the graph

The order of the result set is the topological sorting order we want.

**What if there is a directed graph?**

For this graph, we can only connect node 0 with in-degree 0 to the result set.

After that, nodes 1, 2, 3, and 4 form a loop, and no node with in-degree 0 can be found, so there is only one element in the result set at this time.

Then if we find that **the number of elements in the result set** **is not equal to the number of nodes in the graph**, we can determine that there must be a directed loop in the graph.

## code

# Practice

https://leetcode.com/problems/course-schedule/description/?envType=problem-list-v2&envId=topological-sort&difficulty=MEDIUM

1. Initialization
  `indegrees[numCourses]`: record the in-degree of the node
  `List<List<Integer>> graph`: record the out-degree of the node
  `List<Integer> result[numCourses]`: Topological sort result
  `Queue<Integer> zeroindegree`: record nodes with zero in-degree
2. Steps
Build indegrees and graph
Traverse indegree: `zeroindegree.offer(i)`
When `zeroindegree` is not empty:
-Take out the first node in `zeroindegree`
-Traverse all out-degrees of the node and delete the indegrees of the corresponding node
3. Result output
`result.length==numCourses`: true
else: false

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // initialization
        int[] indegrees = new int[numCourses];
        List<List<Integer>> graph = new ArrayList<>();
        List<Integer> result = new ArrayList<>();
        Queue<Integer> zeroindegree = new LinkedList<>();
        for(int i=0; i<numCourses; i++){
            graph.add(new ArrayList<>());
        }
        // construct indegrees and graph
        for(int[] pair:prerequisites){
            indegrees[pair[0]]+=1;
            graph.get(pair[1]).add(pair[0]);
        }
        // Iterate indegree
        for(int i=0;i<numCourses;i++){
            if(indegrees[i]==0){
                zeroindegree.offer(i);
            }
        }
        // till zeroindegree is null
        while(!zeroindegree.isEmpty()){
            int cur = zeroindegree.poll();
            result.add(cur);
            for(int out:graph.get(cur)){
                indegrees[out]-=1;
                if(indegrees[out]==0){
                    zeroindegree.offer(out);
                }
            }
            // graph.remove(cur);
        }
        // output result
        return numCourses==result.size();
    }
}
```

