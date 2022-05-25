#  连续子数组的最大和(二)
题目来源：[牛客网](https://www.nowcoder.com/practice/11662ff51a714bbd8de809a89c481e21?tpId=13&tqId=2282583&ru=/exam/oj/ta&qru=/ta/coding-interviews/question-ranking&sourceUrl=/exam/oj/ta?page=1&tpId=13&type=13)
## 1、问题描述
输入一个长度为n的整型数组array，数组中的一个或连续多个整数组成一个子数组，找到一个具有最大和的连续子数组。
1.子数组是连续的，比如[1,3,5,7,9]的子数组有[1,3]，[3,5,7]等等，但是[1,3,7]不是子数组
2.如果存在多个最大和的连续子数组，那么返回其中长度最长的，该题数据保证这个最长的只存在一个
3.该题定义的子数组的最小长度为1，不存在为空的子数组，即不存在[]是某个数组的子数组
4.返回的数组不计入空间复杂度计算
## 2、思路解析
**思路：DP+双指但是针**
解题思路和连续子数组的最大和差不多，但是要求求出具体的子树数组，所以难度比之前的连续子数组的最大和难度要大一些。思路还是之前的解题思路，递推公式还是dp[i]=max(dp[i-1]+v[i],v[i]),但是在求子数组时，要使用双指针。左指针指向子区间的左边，右指针指向子数组的的右区间。先看一个例子
![在这里插入图片描述](https://img-blog.csdnimg.cn/99417f20775c4942999f78708f68e18c.png)

## 3、代码实现

```cpp
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param array int整型vector 
     * @return int整型vector
     */
    vector<int> FindGreatestSumOfSubArray(vector<int>& array) {
        vector<int> dp=array;
        // write code here
        int left=0;
        int right=0;
        int r=0,l=0;
        int ret=array[0];
        //没有去前边的数字就更新区间
        for(int i=1;i<array.size();i++){
            if(array[i-1]+array[i]>=array[i]){
                array[i]=array[i-1]+array[i];
                //取前边的数字右区间更新
                right++;
            }else if(array[i-1]+array[i]<array[i]){
                //每有使用前边i-1的结果就要更新左区间
                array[i]=array[i];
                //没取前边的数字之和关心左右区间
                right=i;
                left=i;
            }
            
            if(ret<=array[i]){
                //保存最大连续子数组的区间
                
                r=right;
                l=left;
                ret=array[i];
            }
            
        }
        vector<int> v;
        for(int i=l;i<=r;i++){
            v.push_back(dp[i]);
        }
        
        return v;
    }
};
```
**时间复杂度:O(n)
空间复杂度:O(n)**
# 矩形覆盖
题目来源：[牛客网](https://www.nowcoder.com/practice/72a5a919508a4251859fb2cfb987a0e6?tpId=13&tqId=23283&ru=/exam/oj/ta&qru=/ta/coding-interviews/question-ranking&sourceUrl=/exam/oj/ta?page=1&tpId=13&type=13)
## 1、问题描述
我们可以用 2*1 的小矩形横着或者竖着去覆盖更大的矩形。请问用 n 个 2*1 的小矩形无重叠地覆盖一个 2*n 的大矩形，从同一个方向看总共有多少种不同的方法？
## 2、思路解析
**思路：dp**
先分解看一下过程：
![在这里插入图片描述](https://img-blog.csdnimg.cn/e9359bf2d6f442989912b74bd436411e.png)
可以看出这不就是斐波拉提数列吗但是这和数列不相同的是这是从1开始的，所以递归公式：f(n)=f(n-1)+f(n-2)
## 代码实现
递归算法
```cpp
class Solution {
public:
    
    int rectCover(int number) {
        
        if(number<=2){
            return number;
        }
        return rectCover(number-1)+rectCover(number-2);
    }
};
```
时间复杂度：O(n*n)
空间复杂度：O(n)
迭代算法

```c
class Solution {
public:
    
    int rectCover(int number) {
        if(number<=2){
            return number;
        }
        int fb=0;
        int f1=1;
        int f2=2;
        for(int i=3;i<=number;i++){
            fb=f1+f2;
            f1=f2;
            f2=fb;
        }
        return fb;
        
    }
};
```
时间复杂度：O(n)
空间复杂度：O(1)
# 礼物的最大价值
题目来源：[牛客网](https://www.nowcoder.com/practice/2237b401eb9347d282310fc1c3adb134?tpId=13&tqId=2276652&ru=/practice/046a55e6cd274cffb88fc32dba695668&qru=/ta/coding-interviews/question-ranking&sourceUrl=/exam/oj/ta?page=1&tpId=13&type=13)
## 1、问题描述
在一个m\times nm×n的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？
如输入这样的一个二维数组，
[
[1,3,1],
[1,5,1],
[4,2,1]
]
那么路径 1→3→5→2→1 可以拿到最多价值的礼物，价值为12
## 2、思路解析
**思路：DP**
因为要求从左上角到右下角的最大价值，假设到达[i,j]的最大价值为dp[i][j]，每个点只能右左边的点或者上边的点移动只要确保在这两个点中选一个最大的点就可以了，所以递推公式为dp[i][j]=max(dp[i-1][j],dp[i][j-1])+v[i][j].特殊点处理，对于第一行和第一列是一个特殊的路线，只用由前边的或者前边的点移动而来，所以在初始化时将这两条路线的值初始化为前边的点价值之和。最后返回值为右下角的点的价值
## 3、代码实现

```cpp
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param grid int整型vector<vector<>> 
     * @return int整型
     */
    int maxValue(vector<vector<int> >& grid) {
        // write code here
        int row=grid.size();
        int col=grid[0].size();
        vector<vector<int>> dp(row,vector<int>(col,0));
        int sum=0;
        int num=0;
       //特殊路线，第一列只能由上边的走
        for(int i=0;i<row;i++) {dp[i][0]=grid[i][0]+sum; sum=dp[i][0]; }
        //第一行只用有前边的移动
        for(int j=0;j<col;j++) {dp[0][j]=grid[0][j]+num;num=dp[0][j];}
         
        for(int i=1;i<row;i++){
            //特殊路线处理
            for(int j=1;j<col;j++){   
                dp[i][j]=max(dp[i-1][j],dp[i][j-1])+grid[i][j];
            }
        }
        
        return dp[row-1][col-1];
    }
};
```
时间复杂度：O(n*n)
空间复杂度：O(n*n)
代码优化

```c
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param grid int整型vector<vector<>> 
     * @return int整型
     */
    int maxValue(vector<vector<int> >& grid) {
        // write code here
        int row=grid.size();
        int col=grid[0].size();
  
        int sum=0;
        int num=0;
       //特殊路线，第一列只能由上边的走
        for(int i=0;i<row;i++) {grid[i][0]=grid[i][0]+sum; sum=grid[i][0]; }
        //第一行只用有前边的移动
        for(int j=0;j<col;j++) {grid[0][j]=grid[0][j]+num;num=grid[0][j];}
         
        for(int i=1;i<row;i++){
            //特殊路线处理
            for(int j=1;j<col;j++){   
                grid[i][j]=max(grid[i-1][j],grid[i][j-1])+grid[i][j];
            }
        }
        
        return grid[row-1][col-1];
    }
};
```
时间复杂度：O(n*n)
空间复杂度：O(n)