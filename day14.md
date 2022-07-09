# [矩阵中的距离](https://leetcode.cn/problems/2bCMpM/)

题目来源：[leetcode]([https://leetcode.cn/problems/2bCMpM/](https://leetcode.cn/problems/2bCMpM/))

## 1、问题描述

给定一个由 0 和 1 组成的矩阵 mat ，请输出一个大小相同的矩阵，其中每一个格子是 mat 中对应位置元素到最近的 0 的距离。

两个相邻元素间的距离为 1 。

## 2、思路解析

**思路：BFS**

寻找离矩阵中1的位置最小的0的距离，假设点a1为1坐标为（i1,j1）离点a1最近点a0为0坐标为（i0,j0）两点距离为|j1-j0|+|i0-i1|但是这需要遍历1点并寻找最近0点才可以算出最近距离。这样太过麻烦。我们只需将所有的0点插入到队列中，然后一个一个取出0点寻找里当前0点四周有没有1点，有1点并计算距离，接着将计算过1点距离的坐标插入到队列。接着寻找这个点周围的1点并计算距离，因为前一个点得到的距离是最小值，所以当前点的最小距离是上一个点距离+1。

## 3、代码实现



```C++
class Solution {
    int net[4][2]={{-1,0},{1,0},{0,-1},{0,1}};
public:
    
    vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
        int row=mat.size();
        int col=mat[0].size();
        vector<vector<int>>seen(row,vector<int>(col,0)); //标记坐标为0的节点使用情况
        vector<vector<int>>dist(row,vector<int>(col,0));
        queue<pair<int,int>>q; //将节点坐标存储在队列中
        //将节点为0插入到队列中
        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                if(mat[i][j]==0){
                    q.emplace(i,j);
                    seen[i][j]=1;

                }
            }
        }  
     
        while(!q.empty()){
            auto [newx,newy]=q.front();
            q.pop();

             for(int i=0;i<4;i++){
                 int x=newx+net[i][0];
                 int y=newy+net[i][1];
               
                    //标记位置为1的位置
                 if(x>=0&&x<row&&y<col&&y>=0&&!seen[x][y]){
                      dist[x][y] = dist[newx][newy] + 1;
                    q.emplace(x, y);
                    seen[x][y] = 1;

                 }
             }
        }

      
     return dist;
    }
};
```

# [二分图](https://leetcode.cn/problems/vEAB3K/)

题目来源：[leetcode]([https://leetcode.cn/problems/vEAB3K/](https://leetcode.cn/problems/vEAB3K/))

## 1、问题描述

存在一个 无向图 ，图中有 n 个节点。其中每个节点都有一个介于 0 到 n - 1 之间的唯一编号。

给定一个二维数组 graph ，表示图，其中 graph[u] 是一个节点数组，由节点 u 的邻接节点组成。形式上，对于 graph[u] 中的每个 v ，都存在一条位于节点 u 和节点 v 之间的无向边。该无向图同时具有以下属性：

不存在自环（graph[u] 不包含 u）。
不存在平行边（graph[u] 不包含重复值）。
如果 v 在 graph[u] 内，那么 u 也应该在 graph[v] 内（该图是无向图）
这个图可能不是连通图，也就是说两个节点 u 和 v 之间可能不存在一条连通彼此的路径。
二分图 定义：如果能将一个图的节点集合分割成两个独立的子集 A 和 B ，并使图中的每一条边的两个节点一个来自 A 集合，一个来自 B 集合，就将这个图称为 二分图 。

如果图是二分图，返回 true ；否则，返回 false 。

## 2 思路解析

**思路：BFS**

如果给定的无向图连通，那么我们就可以任选一个节点开始，给它染成红色。随后我们对整个图进行遍历，将该节点直接相连的所有节点染成绿色，表示这些节点不能与起始节点属于同一个集合。我们再将这些绿色节点直接相连的所有节点染成红色，以此类推，直到无向图中的每个节点均被染色。

如果我们能够成功染色，那么红色和绿色的节点各属于一个集合，这个无向图就是一个二分图；如果我们未能成功染色，即在染色的过程中，某一时刻访问到了一个已经染色的节点，并且它的颜色与我们将要给它染上的颜色不相同，也就说明这个无向图不是一个二分图。

具体步骤如下：

  首先随便选一个点染成红色，遍历该点连接的点集合将连接点全然成绿色，插入到对列中，接着拿出对列中点接着遍历连接点集合。将连接点全然成绿色，如果连接点点是染色的不处理，但是如果连接点是有色并且和要染成的颜色不相同，就说明不是一个二分图

  我们任选一个节点开始，将其染成红色，并从该节点开始对整个无向图进行遍历；

  在遍历的过程中，如果我们通过节点 u 遍历到了节点 v（即 u和 v 在图中有一条边直接相连），那么会有两种情况：

  如果 v 未被染色，那么我们将其染成与 u 不同的颜色，并对 v 直接相连的节点进行遍历；

  如果 v 被染色，并且颜色与 u 相同，那么说明给定的无向图不是二分图。我们可以直接退出遍历并返回 false 作为答案。

  当遍历结束时，说明给定的无向图是二分图，返回 true 作为答案。

  

## 3、代码实现



```C++
class Solution {
    int uncolor=0;
   int red=1;
    int grad=2;
  

public:
    bool isBipartite(vector<vector<int>>& graph) {
        int n=graph.size();
          vector<int> v(n,uncolor);
          queue<int>q;
          for(int i=0;i<n;i++){
               if(v[i]==uncolor){
                  q.push(i);
                    while(!q.empty()){
                        int node=q.front();
                        int color=(v[node]==red?grad:red);
                        q.pop();
                        for(auto o:graph[node]){
                            
                            if(v[o]==uncolor){
                                q.push(o);
                                v[o]=color;
                            }else if(color!=v[o]){
                                return false;

                            }
                        }


                    }
              }
          }
        return true;
    }
};
```

# [所有路径](https://leetcode.cn/problems/bP4bmD/)

题目来源：[leetcode]([https://leetcode.cn/problems/bP4bmD/](https://leetcode.cn/problems/bP4bmD/))

## 1、问题描述

给定一个有 n 个节点的有向无环图，用二维数组 graph 表示，请找到所有从 0 到 n-1 的路径并输出（不要求按顺序）。

graph 的第 i 个数组中的单元都表示有向图中 i 号节点所能到达的下一些结点（译者注：有向图是有方向的，即规定了 a→b 你就不能从 b→a ），若为空，就是没有下一个节点了。

## 2、思路解析

思路：DFS

本题要寻找从0开始的一条路径，必须先开始先遍历0连接点的集合，拿到第一个数据，接着遍历连接第一个数据的的结合直到访问到最后一个结合为NULL。接着回溯到上一层继续遍历上一层g集合。



函数DFS有一个参数g表示当前遍历的集合，开始时被调用函数被调函数传参为连接0点的集合，接着将当前点插入到数组中保存下来，接着递归到下一层递归，g传参时传参为griph[g[i]]以g[i]为起始点的集合满足条件就是连接最后一个点得集合为NULL

## 3、代码实现



```C++
class Solution {
        void DFS(vector<vector<int>>& s, vector<int>& v,  vector<vector<int>>& graph, vector<int>& g,int index) {
        if (index == graph.size()-1) {
            s.push_back(v);
            return;
        }
        for (int i = 0;i < g.size();i++) {

            v.push_back(g[i]);
  
            DFS(s, v, graph, graph[g[i]],g[i]);
        
            v.pop_back();
        }

    }
public:
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {

        vector<vector<int>> v;
        vector<int>s;
        vector<int>g;
        int index = 0;
        s.push_back(0);
        DFS(v, s, graph, graph[0],index);


        return v;
    }
};
```