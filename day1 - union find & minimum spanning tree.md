# disjoint set(union find)

This method basically can help you find your ancestor.

if you want to figure out if  someone is in the same family with you, you need to trace back through your father , your father's father to see if there is a same origin. We use this origin to define the set(family), if you  are sharing a same origin you are in the same set(family).

when we need to add someone to the family, basically we need to add our ancestor as their ancestor's father. hahaha. So all the family members in their family tree need to change their ancestor.



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

## practice

https://leetcode.com/problems/find-if-path-exists-in-graph?envType=problem-list-v2&envId=union-find&difficulty=EASY

# minimum spanning tree - prim

minDist[]: record the nearest distance from each node to the minimum spanning tree

3 key steps of prim: 

1. find the nearest node 
2. add this node to the tree 
3. update the minDist of the rest node

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

![image-20241116181604937](D:\sde\algorithm\pics\image-20241116181604937.png)

# minimum spanning tree - kruskal

sorting the edges + union find

3 key

# Comparison

When the nodes is much more than the edges, we use Kruskal. Otherwise, we use prim.



## practice

https://leetcode.com/problems/min-cost-to-connect-all-points?envType=problem-list-v2&envId=minimum-spanning-tree

1 The length property in java is for arrays. For example, if you declare an array and want to know the length of the array, you use the length property.

2 The length() method in java is for strings. If you want to see the length of the string, you use the length() method.

3 The size() method in java is for generic collections. If you want to see how many elements the generic has, call this method to check!