# [最长递增路径](https://leetcode.cn/problems/fpTFWP/)

题目来源：[leetcode]([https://leetcode.cn/problems/fpTFWP/](https://leetcode.cn/problems/fpTFWP/))

# 1、问题描述

给定一个 m x n 整数矩阵 matrix ，找出其中 最长递增路径 的长度。

对于每个单元格，你可以往上，下，左，右四个方向移动。 不能 在 对角线 方向上移动或移动到 边界外（即不允许环绕）。

![](https://secure2.wostatic.cn/static/tMFEN63viePPssA5n6KwGw/image.png)

## 2、思路解析

**思路：DFS**

因为要寻找图的最长递增序列，所以可能不是从当一个开始的。最长递增序列开始点可能是图中的任意一点，所以需要以每一个点作为开始点寻找以当前点为开始点的递增序列长度，并保存下来，每次和上一次的最大长度比较选一个最大值保存下来。

DFS函数用来寻找以当前点为开始点的递增序列的最大长度每次像当前点的上下左右寻找当更新点比当前点大时就将数量++，直到更新遇到更新点比当前点小的点时就停止更新保存序列长度，返回回溯到上一层再上一个点的周围继续寻找比自己点大的点

## 3、代码实现



```C++
class Solution {
    int mas=1;
    int net[4][2]={{-1,0},{1,0},{0,-1},{0,1}};
    void DFS(vector<vector<int>>& m,int&num,int row,int col,int newx,int newy,vector<vector<int>>& v){
        for(int i=0;i<4;i++){
            int x=newx+net[i][0];
            int y=newy+net[i][1];
            if(x>=row||x<0||y<0||y>=col)
                continue;
            if(v[x][y]==0&&m[x][y]>m[newx][newy]){
                 v[x][y]=1; 
                num++; 
                mas=max(mas,num);
                DFS(m,num,row,col,x,y,v);
                v[x][y]=0;
                num--;
            }else if(v[x][y]==0&&m[x][y]<=m[newx][newy]){
               
            }
        }

    }
public:
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        int row=matrix.size();
        int col=matrix[0].size();
 
        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                int s=1;
                vector<vector<int>> v(row,vector<int>(col,0));
                DFS(matrix,s,row,col,i,j,v);
            }
        }
    return mas;
    }
};
```

# [省份数量](https://leetcode.cn/problems/bLyHh0/)

题目来源：[leetcode]([https://leetcode.cn/problems/bLyHh0/](https://leetcode.cn/problems/bLyHh0/))

## 1、问题描述

有 n 个城市，其中一些彼此相连，另一些没有相连。如果城市 a 与城市 b 直接相连，且城市 b 与城市 c 直接相连，那么城市 a 与城市 c 间接相连。

省份 是一组直接或间接相连的城市，组内不含其他没有相连的城市。

给你一个 n x n 的矩阵 isConnected ，其中 isConnected[i][j] = 1 表示第 i 个城市和第 j 个城市直接相连，而 isConnected[i][j] = 0 表示二者不直接相连。

返回矩阵中 省份 的数量。

## 2、思路解析

**思路：DFS**

创建一个和节点数大小相等的数组用来标记使用的节点。从第一个节点开开始，将直接连接或者间接连接的节点标记在标记数组中，后续不再使用。接着继续遍历节点找到下一个没有标记的节点，作为当前节点将和当前节点连接或者间接连接的节点标记出来即可。

## 3、代码实现



```C++
class Solution {
    vector<int>vm;
    void DFS(vector<vector<int>>&is,vector<int>&v){
        for(int i=0;i<v.size();i++){
            if(v[i]==1&&vm[i]==0){
                vm[i]=1;
                DFS(is,is[i]);
            }
        }

    }
public:
    int findCircleNum(vector<vector<int>>& is) {
        int s=0;
        vm.resize(is.size(),0);
        for(int i=0;i<is.size();i++){
            if(vm[i]==0){
                DFS(is,is[i]);
                s++;
            }

        }
        return s;
    }
};
```

# [多余的边](https://leetcode.cn/problems/7LpjUW/)

题目来源：[leetcode]([https://leetcode.cn/problems/7LpjUW/](https://leetcode.cn/problems/7LpjUW/))

## **1、问题描述**

树可以看成是一个连通且 无环 的 无向 图。

给定往一棵 n 个节点 (节点值 1～n) 的树中添加一条边后的图。添加的边的两个顶点包含在 1 到 n 中间，且这条附加的边不属于树中已存在的边。图的信息记录于长度为 n 的二维数组 edges ，edges[i] = [ai, bi] 表示图中在 ai 和 bi 之间存在一条边。

请找出一条可以删去的边，删除后可使得剩余部分是一个有着 n 个节点的树。如果有多个答案，则返回数组 edges 中最后出现的边。

## 2、思路解析

**思路：拓扑排序**

树是一个无向不连通图，我们要将一个图去除一条边形成一个树，所以去除这条边之前这个图是一个无向有环图，我们将这个环的多于边去除掉之后就是一个树，但是我们该去除那个边。

我们可以从图的一个节点遍历节点。当遇到一个节点同时大于两条的边时整个点就可以连接多余边的点，我们就将连接这个点的删除就行。

## 3、代码实现



```C++
class Solution {
    unordered_map<int,int>mp;
    int finds(int i){
        int v=mp[i];
        if(v!=i){
            mp[i]=finds(v);
        }
        return mp[i];
    }
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int len=edges.size();
        for (int i = 0;i <= len;i++) {
            mp[i] = i;
          }
        vector<int> v1;
        for(int i=0;i<len;i++){
          v1=edges[i];
            int pi=finds(v1[0]);
            int pj=finds(v1[1]);
            if(pi!=pj){
                mp[pi]=pj;
            }else{
                return v1;
               break;
            }
        }
        return {};
        
    }
};
```

