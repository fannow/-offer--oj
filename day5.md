# 分割等和子集

题目来源：[leetcode]([https://leetcode.cn/problems/partition-equal-subset-sum/](https://leetcode.cn/problems/partition-equal-subset-sum/))

## 问题描述

给你一个 **只包含正整数** 的 **非空** 数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

![](https://secure2.wostatic.cn/static/hZs8Wm5V2S8QTXXgeJWcHr/image.png)

## 思路解析：

**思路：dp**

题目要求这个数组的子数组，这就说明这两个数组的和是相等的，这也说明这个数组的和是一个偶数，不是奇数，因为和为奇数就不可能分割为和为相等的两个子数组。

这是一个经典的01背包的应用我们可以将子树和当作背包容量，将数组分别当作商品的大小和商品的价值

想象一下先看开辟一个和大小的target的数组，把这个当作背包，我们选取商品就是选取原来数组的元素商品价值和大小用同一个元素表示。01背包dp[i]背包中最大价为dp[i] 商品大小可能小于背包大小，但是这里求的是dp[i] 商品大小等于背包大小最后判断结果是不是存在相等的情况。

遍历顺序：我们采用滚动数组的方式当前数组沿用的上一次循环的结果，所以要从后往前遍历这是因为正向遍历会改变之前的结果。

## 代码实现：

```C++
class Solution {
public:
    //分割为相等的子数组，子数组的和一定就是原来的子树和的一半，所以
    //将数组和的一半当作背包容量
    //将数组分别当作商品的大小和商品的价值

    //数组和不是二的倍数就说明不能被分割
    bool canPartition(vector<int>& nums) {
        int sum=0;
        for(int i=0;i<nums.size();i++){
            sum+=nums[i];
        }
        int ba=sum/2;
        if(sum%2==1) return false;
        //创建dp数组
        vector<int>dp(ba+1,0);
        for(int i=0;i<nums.size();i++){//遍历商品
            for(int j=ba;j>=nums[i];j--){
                dp[j]=max(dp[j],dp[j-nums[i]]+nums[i]);
            }

        }
        if(dp[ba]==ba){
            return true;
        }
        return false;
    }
};
```

# [最后一块石头的重量 II](https://leetcode.cn/problems/last-stone-weight-ii/)

题目来源：(leetcode)[[https://leetcode.cn/problems/last-stone-weight-ii/](https://leetcode.cn/problems/last-stone-weight-ii/)]

## 问题描述

有一堆石头，用整数数组 stones 表示。其中 stones[i] 表示第 i 块石头的重量。

每一回合，从中选出任意两块石头，然后将它们一起粉碎。假设石头的重量分别为 x 和 y，且 x <= y。那么粉碎的可能结果如下：

如果 x == y，那么两块石头都会被完全粉碎；
如果 x != y，那么重量为 x 的石头将会完全粉碎，而重量为 y 的石头新重量为 y-x。
最后，最多只会剩下一块 石头。返回此石头 最小的可能重量 。如果没有石头剩下，就返回 0。

![](https://secure2.wostatic.cn/static/mYgt8pwVwdZy396Hd253eA/image.png)

## 思路解析：

**思路：dp**

  **dp[j]表示容量（这里说容量更形象，其实就是重量）为j的背包，最多可以背dp[j]这么重的石头**。

  **两块石头**互相碰撞求剩下的最小的重量，最理想的状态就是可以将石块分割为两堆重量相同的石堆，要是这种情况这就是前边的一道题思路差不多相同了，这种情况下剩下得最小重量为0.其他情况下，可以先求出当背包容量的为数组和的一半时能装下最大石块的重量，在这种情况下剩余最小重量就是sum -dp[ba]-dp[ba].**在计算target的时候，target = sum / 2 因为是向下取整，所以sum - dp[target] 一定是大于等于dp[target]的**

## 代码实现：



```C++
class Solution {
public:
   /*
    将每个石头重量看作是即使石头的体积也是石头的价值
    所以这和之前得01背包问题差不多
    将石头重量分割为重量差不多得两堆，让两堆石头相互撞击，因为两堆石头重量差不多，所以最后剩下的石头重量一定是最小的
    我们只要将石头装进背包容量为石头总重量得半就可以
    */
    int lastStoneWeightII(vector<int>& stones) {
        int sum=0;
        for(int i=0;i<stones.size();i++){
            sum+=stones[i];
        }
        int ba=sum/2;
        vector<int> dp(ba+1,0);
        for(int i=0;i<stones.size();i++){
            for(int j=ba;j>=stones[i];j--){
                dp[j]=max(dp[j],dp[j-stones[i]]+stones[i]);
            }
        }
        return sum-2*dp[ba];
              /*
        最后dp[target]⾥是容量为target的背包所能背的最⼤重量。
那么分成两堆⽯头，⼀堆⽯头的总重量是dp[target]，另⼀堆就是sum - dp[target]。
在计算target的时候，target = sum / 2 因为是向下取整，所以sum - dp[target] ⼀定是⼤于等于
dp[target]的。那么相撞之后剩下的最⼩⽯头重量就是 (sum - dp[target]) - dp[target]。
        */
    }
};
```



# 目标和

题目来源：[leetcode]([https://leetcode.cn/problems/target-sum/](https://leetcode.cn/problems/target-sum/))

## 问题描述：

给你一个整数数组 nums 和一个整数 target 。

向数组中的每个整数前添加 '+' 或 '-' ，然后串联起所有整数，可以构造一个 表达式 ：

例如，nums = [2, 1] ，可以在 2 之前添加 '+' ，在 1 之前添加 '-' ，然后串联起来得到表达式 "+2-1" 。
返回可以通过上述方法构造的、运算结果等于 target 的不同 表达式 的数目。



## 思路解析：

思路：dp

dp[i][j]表示在前i个元素选取若干元素组成的和为j的的方案数

当没有元素时并且j=0对应的方案为0

对于数组nums 中的第 i 个元素num，遍历 0≤j≤neg，计算dp[i][j] 的值：

如果 j<num，则不能选 num，此时有dp[i][j]=dp[i−1][j]；

如果 j≥num，则如果不选 num，方案数是dp[i−1][j]，如果选num，方案数是 dp[i−1][j−num]，此时有 dp[i][j]=dp[i−1][j]+dp[i−1][j−num]。

由于 dp 的每一行的计算只和上一行有关，因此可以使用滚动数组的方式，去掉 dp 的第一个维度，将空间复杂度优化到 O(n)

实现时，内层循环需采用倒序遍历的方式，这种方式保证转移来的是dp[i−1][] 中的元素值。

## 代码实现：

```C++
class Solution {
public:
        /*
    首先整数target是由数组组合而后来所以target不能大于数组之和
    还有组合看作是left组合-right组合=target
    其中left为加组合right为减法组合
    将将加法组合看作是deg，所以减法组合为sum-deg,
    所以deg-(sum-deg)=target --->deg=(sum-target)/2
    因为deg是一个整数，所以sum-target必然是一个偶数

    所以只要求出加法组合得组合数就可以知道组合为这个数的组合数了

    */
int findTargetSumWays(vector<int>& nums, int target) {
    int sum = 0;
    int n = nums.size();
    for (int i = 0;i < n;i++) {
        sum += nums[i];
    }
    if (target > sum) {
        return 0;
    }
    if ((sum - target) % 2 == 1) {
        return 0;
    }
    int deg = (sum - target) / 2;
    //dp数组
    vector<vector<int>> dp(n + 1, vector<int>(deg + 1));
    dp[0][0] = 1;
    for (int i = 1;i <= n;i++) {//遍历商品
        for (int j = 0;j <= deg;j++) {//遍历背包容量

                //背包无法装下商品i
            dp[i][j] = dp[i - 1][j];
            if (j >= nums[i - 1]) {
                //当前组合数加上之前的组合数
                dp[i][j] += dp[i - 1][j - nums[i - 1]];

            }

        }

    }
    return dp[n ][deg];

}
};
```

```C++
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int sum = 0;
        for (int& num : nums) {
            sum += num;
        }
        int diff = sum - target;
        if (diff < 0 || diff % 2 != 0) {
            return 0;
        }
        int neg = diff / 2;
        vector<int> dp(neg + 1);
        dp[0] = 1;
        for (int& num : nums) {
            for (int j = neg; j >= num; j--) {
                dp[j] += dp[j - num];
            }
        }
        return dp[neg];
    }
};


```

# 一和零

题目来源:(leetcode)[[https://leetcode.cn/problems/ones-and-zeroes/](https://leetcode.cn/problems/ones-and-zeroes/)]

# 问题描述：

给你一个二进制字符串数组 strs 和两个整数 m 和 n 。

请你找出并返回 strs 的最大子集的长度，该子集中 最多 有 m 个 0 和 n 个 1 。

如果 x 的所有元素也是 y 的元素，集合 x 是集合 y 的 子集 。





![](https://secure2.wostatic.cn/static/a1SkhsEbvrrjM8B5TnYHdz/image.png)

## 思路解析：



**思路：dp**

确定dp数组以及下标的含义：
dp[i][j]：最多有i个0和j个1的strs的最⼤⼦集的⼤⼩为dp[i][j]。
确定递推公式：
dp[i][j] 可以由前⼀个strs⾥的字符串推导出来，strs⾥的字符串有zeroNum个0，oneNum个1。
dp[i][j] 就可以是 dp[i - zeroNum][j - oneNum] + 1。
然后我们在遍历的过程中，取dp[i][j]的最⼤值。
所以递推公式：dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);
01背包的递推公式：dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
对⽐⼀下就会发现，字符串的zeroNum和oneNum相当于物品的重量（weight[i]），字符串本身的个数
相当于物品的价值（value[i]）。
这就是⼀个典型的01背包！ 只不过物品的重量有了两个维度⽽已。
dp数组如何初始化
因为物品价值不会是负数，初始为0，保证递推的时候dp[i][j]不会被初始值覆盖。
确定遍历顺序
物品就是strs⾥的字符串，背包容量就是题⽬描述中的m和n。

## 代码实现：

```C++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> dp(m+1,vector<int>(n+1,0));
        for(int i=0;i<strs.size();i++){
            int zeronum=0,onenum=0;
            for(int j=0;j<strs[i].size();j++){
                if(strs[i][j]=='0'){
                    zeronum++;
                }else if(strs[i][j]=='1'){
                    onenum++;
                }
                
            }for(int a=m;a>=zeronum;a--){
                    for(int b=n;b>=onenum;b--){
                        dp[a][b]=max(dp[a][b],dp[a-zeronum][b-onenum]+1);
                    }
                }
        }
         return dp[m][n];
    }
};
```