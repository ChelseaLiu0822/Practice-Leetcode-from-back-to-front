# Disjoint Set(Union Find)

This method basically can help you find your ancestor.

if you want to figure out if  someone is in the same family with you, you need to trace back through your father , your father's father to see if there is a same origin. We use this origin to define the set(family), if you  are sharing a same origin you are in the same set(family).

when we need to add someone to the family, basically we need to add our ancestor as their ancestor's father. hahaha. So all the family members in their family tree need to change their ancestor.

Here is a **example**. This can show you what specific question can we solve by using Disjoint Set.

You want to determine if there is a **valid path** that exists from `node 1` to `node 3`.

![Q1](pics\day1_0.png)

## code

```java
int[] father = new int[num];
public void init(){
    for (int i = 0; i < num; ++i) {
        father[i] = i;
    }
}
public int find(int u){
    if(u==father[u]){
        return u;
    }else{
        father[u]=find(father[u]);
        return father[u];
    }
}
public int joint(int u, int v){
    int u=find(u);
    int v=find(v);
    if(u!=v){
        father[v]=u;
    }
}
public boolean judge(int u, int v){
	return find(u)==find(v);
}
```

# Minimum Spanning Tree

The basic problem of minimum spanning tree is to find the shortest distance connecting these nodes, as shown in the figure. There are two methods: Prim and Kruskal.

![Question](pics\day1_1.png)

## Prim

minDist[]: record the nearest distance from each node to the minimum spanning tree

3 key steps of prim: 

1. find the nearest node 
2. add this node to the tree 
3. update the minDist of the rest node

### code

```java
import java.util.*;
public class Main{
    public static void main(String[] args){
        // input V, E and edge
        Scanner sc = new Scanner(System.in);
        int V = sc.nextInt();
        int E = sc.nextInt();
        // initialize graph
        int[][] G = new int[V+1][V+1];
        for(int i=1;i<V+1;i++){
            Arrays.fill(G[i],Integer.MAX_VALUE);
        }
        for(int i=0;i<E;i++){
            int v1 = sc.nextInt();
            int v2 = sc.nextInt();
            int e = sc.nextInt();
            G[v1][v2]=e;
            G[v2][v1]=e;
        }
        // initialize minDist
        int[] minDist = new int[V+1];
        Arrays.fill(minDist,10001);
        boolean[] visited = new boolean[V+1];
        //  prim loop
        for(int i=1;i<V+1;i++){
            int cur = -1;
            int minVal = Integer.MAX_VALUE;
            // 1. find the nearest node
            for(int j=1;j<V+1;j++){
                if(!visited[j] && minDist[j]<minVal){
                    minVal=minDist[j];
                    cur=j;
                }
            }
            // 2. add current node into minimum spanning tree
            visited[cur]=true;
            // 3. update the minDist of nodes outside the tree
            for(int j=1;j<V+1;j++){
                if(!visited[j] && G[cur][j]<minDist[j]){
                    minDist[j]=G[cur][j];
                }
            }
        }
        int res = 0;
        for(int i=2;i<V+1;i++){
            res+=minDist[i];
        }
        System.out.println(res);
    }
}
```

## Kruskal

sorting the edges + union find

Kruskal's idea:

1. Sort the weights of the edges, because the smallest edge should be added to the spanning tree first
2. Iterate through the sorted edges
3. If the two nodes at the beginning and end of the edge are in the same set, it means that if this edge is connected, a cycle will appear in the graph
4. If the two nodes at the beginning and end of the edge are not in the same set, add them to the minimum spanning tree and add the two nodes to the same set

### code

```java
import java.util.*;

class Edge {
    int l, r, val;

    Edge(int l, int r, int val) {
        this.l = l;
        this.r = r;
        this.val = val;
    }
}

public class Main {
    private static int n = 10001;
    private static int[] father = new int[n];

    // Initialize the disjoint set
    public static void init() {
        for (int i = 0; i < n; i++) {
            father[i] = i;
        }
    }

    // Find operation for the disjoint set
    public static int find(int u) {
        if (u == father[u]) return u;
        return father[u] = find(father[u]);
    }

    // Union operation for the disjoint set
    public static void join(int u, int v) {
        u = find(u);
        v = find(v);
        if (u == v) return;
        father[v] = u;
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int v = scanner.nextInt(); // Number of vertices
        int e = scanner.nextInt(); // Number of edges
        List<Edge> edges = new ArrayList<>();
        int result_val = 0;

        for (int i = 0; i < e; i++) {
            int v1 = scanner.nextInt();
            int v2 = scanner.nextInt();
            int val = scanner.nextInt();
            edges.add(new Edge(v1, v2, val));
        }

        // Perform Kruskal's algorithm
        edges.sort(Comparator.comparingInt(edge -> edge.val));

        // Initialize the disjoint set
        init();

        // Traverse edges from the smallest weight
        for (Edge edge : edges) {
            int x = find(edge.l);
            int y = find(edge.r);

            if (x != y) {
                result_val += edge.val;
                join(x, y);
            }
        }

        System.out.println(result_val); // Output the total weight of the MST
        scanner.close();
    }
}

```





# Comparison

When the nodes is much more than the edges, we use Kruskal. Otherwise, we use prim.

# Practice

1 The length property in java is for arrays. For example, if you declare an array and want to know the length of the array, you use the length property.

2 The length() method in java is for strings. If you want to see the length of the string, you use the length() method.

3 The size() method in java is for generic collections. If you want to see how many elements the generic has, call this method to check!

## Q1 - Union Find

https://leetcode.com/problems/find-if-path-exists-in-graph?envType=problem-list-v2&envId=union-find&difficulty=EASY

![ans1](pics\day1_q1_notice.png)

## Q2 - MST Prim

https://leetcode.com/problems/min-cost-to-connect-all-points?envType=problem-list-v2&envId=minimum-spanning-tree

![q2](pics\day1_q2_notice.png)

## Q3 - MST Kruskal

https://leetcode.com/problems/find-critical-and-pseudo-critical-edges-in-minimum-spanning-tree/?envType=problem-list-v2&envId=minimum-spanning-tree

![q3](pics\day1_q3_notice.png)