# Dijkstra

## Base

**Differences** between **Dijkstra** and **Minimum Spanning Tree (MST)**  are as follows:

1.Different Objectives

**Dijkstra's Algorithm**: To find the single-source shortest paths, i.e., the shortest paths from **a source node** (starting point) to **all other nodes** in the **directed graph**, such as navigation and routing. The shortest path from the source node A to  node B might not include all the node in the graph.

**Minimum Spanning Tree**: To construct **a tree that includes all the vertices** in **undirected**  graph, minimizing the total weight of the edges in the tree (while ensuring no cycles are formed).

2.Output Results

**Dijkstra's Algorithm**: The shortest paths from the source node to every other node and their corresponding shortest distances.

**Minimum Spanning Tree**: A spanning tree that connects all vertices in the graph with the minimum total edge weight.

------

### Dijkstra's Three-Step Process:

(*The `minDist` array stores the shortest distance from the source node to each node.*)

1. Select the unvisited node that is closest to the source.
2. Mark the selected node as `visited`.
3. Update the distances of unvisited nodes from the source (i.e., update the `minDist` array).

### Practice

https://leetcode.com/problems/network-delay-time/description/

code

1st time: 1h17min

```java
/*1. initialization
graph[n][n]: i->j time
visited[n]
minDist[n]
2. in a loop
find unvisited and nearest node
mark it as visited
uppdate the other unvisited node in minDist
3. output
*/
import java.util.*;
class Solution {
    public int networkDelayTime(int[][] times, int n, int k){
        // 1. initialization
        int[][] graph = new int[n+1][n+1];
        boolean[] visited = new boolean[n+1];
        int[] minDist = new int[n+1];
        Arrays.fill(minDist, Integer.MAX_VALUE);
        for(int i=0;i<n+1;i++){
            Arrays.fill(graph[i], Integer.MAX_VALUE);
        }
        for(int[] edge: times){
            graph[edge[0]][edge[1]] = edge[2];
        }
        // 2.loop
        minDist[k] = 0;
        for(int i =1;i<n+1;i++){
            // find unvisited and nearest node
            int minVal = Integer.MAX_VALUE;
            int cur = 1;
            for(int j=1;j<n+1;j++){
                if(visited[j]==false && minDist[j]<minVal){
                    minVal = minDist[j];
                    cur = j;
                }
            }
            // mark it as visited
            visited[cur] = true;
            // uppdate the other unvisited node in minDist
            for(int j=1;j<n+1;j++){
                if(!visited[j]&&graph[cur][j]<101&&graph[cur][j]+minDist[cur]<minDist[j]){
                    minDist[j] = graph[cur][j]+minDist[cur];
                }
            }
        }
        // 3.output
        int maxVal=-1;
        for(int i=1;i<n+1;i++){
            if(minDist[i]>maxVal){
                maxVal=minDist[i];
            }
        }
        if(maxVal==Integer.MAX_VALUE){
            return -1;
        }else{
            return maxVal;
        }
    }
}
```

## Dijkstra's Algorithm (Heap Optimized Version)

节点数量很多，边的数量 很少的时候（稀疏图），是不是可以换成从边的角度来求最短路呢？

其实思路依然是 Dijkstra 三部曲：

1. 第一步，选源点到哪个节点近且该节点未被访问过
2. 第二步，该最近节点被标记访问过
3. 第三步，更新非访问节点到源点的距离（即更新minDist数组）

只不过之前是 通过遍历节点来遍历边，通过两层for循环来寻找距离源点最近节点。 这次我们直接遍历边，且通过堆来对边进行排序，达到直接选择距离源点最近节点。

针对这三部曲，如果用堆来优化。

那么三部曲中的**第一步**（选源点到哪个节点近且该节点未被访问过），我们如何选？

我们要选择距离源点近的节点（即：该边的权值最小），所以 我们需要一个**小顶堆**来帮我们对边的权值排序，每次从小顶堆堆顶取边就是权值最小的边。

所以初始化时应初始化minDist,graph,visited还有小顶堆priority。

通过类似BFS里的循环完成对所有边的遍历。

## Practice

https://leetcode.com/problems/network-delay-time/description/

```java
/*1. initialization
pair{from to time}
graph[n][ArrayList<pair>]: i->j, time
visited[n]
minDist[n]
PriorityQueue<pair> priority
2. in a loop
find unvisited and nearest node
mark it as visited
uppdate the other unvisited node in minDist
3. output
*/
import java.util.*;
class Solution {
    class Pair{
        int from = 0;
        int to = 0;
        int time = 0;
        public Pair(int a, int b, int c){
            from=a;
            to=b;
            time=c;
        }
    }
    public int networkDelayTime(int[][] times, int n, int k){
        // 1. initialization
        ArrayList<ArrayList<Pair>> graph = new ArrayList<>();
        boolean[] visited = new boolean[n+1];
        int[] minDist = new int[n+1];
        PriorityQueue<Pair> priority = new PriorityQueue<>(Comparator.comparingInt(item->item.time));
        Arrays.fill(minDist, Integer.MAX_VALUE);
        for(int i=0;i<n+1;i++){
            graph.add(new ArrayList<Pair>());
        }
        for(int[] edge: times){
            graph.get(edge[0]).add(new Pair(edge[0], edge[1], edge[2]));
        }
        // 2.loop
        minDist[k] = 0;
        priority.offer(new Pair(k,k,0));
        while(!priority.isEmpty()){
            // find unvisited and nearest node
            Pair cur = priority.poll();
            // mark it as visited
            visited[cur.to] = true;
            // uppdate the other unvisited node in minDist
            for(Pair j:graph.get(cur.to)){
                if(!visited[j.to]&&j.time+minDist[j.from]<minDist[j.to]){
                    minDist[j.to] = j.time+minDist[j.from];
                    // update the time from nodefrom to nodeto in priority queue
                    priority.offer(new Pair(j.from,j.to,minDist[j.to]));
                }
            }
        }
        // 3.output
        int maxVal=-1;
        for(int i=1;i<n+1;i++){
            if(minDist[i]>maxVal){
                maxVal=minDist[i];
            }
        }
        if(maxVal==Integer.MAX_VALUE){
            return -1;
        }else{
            return maxVal;
        }
    }
}
```

**notice: when we update the other unvisited node in minDist and  add a node pair into priority queue, we need to update the weight in the node pair**