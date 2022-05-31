# 斐波那契数

题目来源：[leetcode]([https://leetcode.cn/problems/fibonacci-number/](https://leetcode.cn/problems/fibonacci-number/))

**斐波那契数** （通常用 `F(n)` 表示）形成的序列称为 **斐波那契数列** 。该数列由 `0` 和 `1`开始，后面的每一项数字都是前面两项数字的和。也就是：

![](https://secure2.wostatic.cn/static/tiLHgrVMWkFieRw87V1ZKP/image.png)

## 思路解析

**思路：递归**

这时斐波拉提数列最经典的做法，也是最暴力的做法，存在大量的计算。计算过程就是根据递推公式

F(n)=F(n−1)+F(n−2)，每一次递归都会向上一层返回当前结果，因为是递归就会开辟大量的栈帧，如果当n越大时时间就会越长，可能会导致栈溢出。所以不推荐

**思路：DP**

  当i=0时，F(0)=0;当i=1.F(1)=1，当i>1时就会有递推公式F(n)=F(n−1)+F(n−2)由于斐波那契数存在递推关系，因此可以使用动态规划求解。动态规划的状态转移方程即为上述递推关系。

  初始化：

    因为数列是由0和1开始，所以F(0)=0，F(1)=1

  返回值：
返回F(n);

## 代码实现：



```C++
class Solution {
public:
    int fib(int n) {
        if(n<2){
            return n;
        }
        return fib(n-1)+fib(n-2);

    }
};
```

时间复杂度：O(2^n)

空间复杂度：O(1)



```C++
class Solution {
public:
    int fib(int n) {
        if(n<2){
            return n;
        }
       int f1=0;
       int f2=1;
       int fb=0;
       for(int i=2;i<=n;i++){
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

# 爬楼梯

题目来源：[leetcode]([https://leetcode.cn/problems/climbing-stairs/](https://leetcode.cn/problems/climbing-stairs/))

## 题目描述：

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

![](https://secure2.wostatic.cn/static/wVtUV1hjYmLu6jiEgLkMBd/image.png)

## 思路解析：

**思路：DP**

确定DP数组及下标含义：dp[i]表示爬到第i阶台阶共有多少种方法

递推公式：

i=1时：{1}

i=2时：{1，1}，{2}

i=3时：{1，1，1}，{1，2}，{2，1}

i=4时：{1，1，1，1}，{1，2，1}，{2，1，1}，{2，2}，{1，1，2}

可以看出除了第1次和第2次都是爬了1次和2次，后边的都是前边两次之和也就是斐波拉提数列

因为每次可以爬1阶或者2阶.所以当爬i阶时，当到达i-1阶可以一步到第i阶台阶，当到达i-2阶可以一步到第i阶台阶 所以爬上i层楼梯直接将爬上第i-1阶台阶加上爬上第i-2阶台阶就可以了

初始化：因为每次可以爬1阶或者2阶，所以dp[1]=1,dp[2]=2

返回值：

返回dp[n];

## 代码实现：





```C++
class Solution {
public:
    int climbStairs(int n) {
        if(n<=2){
            return n;
        }
        int f1=1;
        int f2=2;
        int fb=0;
        for(int i=3;i<=n;i++){
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

# 疯狂爬楼梯

题目来源：[牛客网]([https://www.nowcoder.com/practice/22243d016f6b47f2a6928b4313c85387?tpId=196&tqId=39712&rp=1&ru=/exam/oj&qru=/exam/oj&sourceUrl=%2Fexam%2Foj%3Fpage%3D1%26pageSize%3D50%26search%3D%E8%B7%B3%E5%8F%B0%E9%98%B6%26tab%3D%E7%AE%97%E6%B3%95%E7%AF%87%26topicId%3D196&difficulty=undefined&judgeStatus=undefined&tags=&title=跳台阶](https://www.nowcoder.com/practice/22243d016f6b47f2a6928b4313c85387?tpId=196&tqId=39712&rp=1&ru=/exam/oj&qru=/exam/oj&sourceUrl=/exam/oj?page=1&pageSize=50&search=%E8%B7%B3%E5%8F%B0%E9%98%B6&tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=196&difficulty=undefined&judgeStatus=undefined&tags=&title=跳台阶))

## 题目描述：


一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶(n为正整数)总共有多少种跳法。

![](https://secure2.wostatic.cn/static/oPM7cUpgmjrQ7FFjhdhDFW/image.png)

## 思路解析：

**思路：dp**

确定DP数组及下标含义：dp[i]表示爬到第i阶台阶共有多少种方法

递推公式：

i=1时：{1}

i=2时：{1，1}，{2}

i=3时：{1，1，1}，{1，2}，{2，1}，{3}

i=4时：{1，3}，{1，1，2}，{2，2}，{1，2，1}，{2，1，1}，{1，1，1，1}，{3，1}，{4}

看一下i=4的情况

 {1，3} 这个是先到第一阶台阶，别管怎么站上去的，现在只需要一次跨3阶就可以直接上第4阶

{1，1，2}，{2，2}这个是先到第2阶台阶，还是别管怎么怎么站到第2阶，现在只需要一次跨2阶就可以直接上第4阶

{1，2，1}，{2，1，1}，{1，1，1，1}，{3，1} 这个是先到第3阶台阶，还是别管怎么怎么站到第3阶，现在只需要一次跨1阶就可以直接上第4阶

这些结果最后加1就是最后的结果,所以：
dp[n]=dp[0]+dp[1]+dp[2]+...+dp[n-2]+dp[n-1]

dp[n-1]=dp[n-2]+..+dp[0]

dp[n]=2*dp[[n-1];

初始化：

初始化：因为每次可以爬1阶或者2阶，所以dp[1]=1,dp[2]=2

## 代码实现：



```C++
class Solution {
public:
    int jumpFloorII(int number) {
        vector<int> dp(number + 1);
        //初始化前面两个
        dp[0] = 1; 
        dp[1] = 1;
        //依次乘2
        for(int i = 2; i <= number; i++) 
            dp[i] = 2 * dp[i - 1];
        return dp[number];
    }
};

```

时间复杂度：O(n)
空间复杂度：O(n)

# 使用最小花费爬楼梯

题目来源：[leetcode]([https://leetcode.cn/problems/min-cost-climbing-stairs/](https://leetcode.cn/problems/min-cost-climbing-stairs/))

## 问题描述：

给你一个整数数组 cost ，其中 cost[i] 是从楼梯第 i 个台阶向上爬需要支付的费用。一旦你支付此费用，即可选择向上爬一个或者两个台阶。

你可以选择从下标为 0 或下标为 1 的台阶开始爬楼梯。

请你计算并返回达到楼梯顶部的最低花费。

![](https://secure2.wostatic.cn/static/57uiNzzaZC7VxxHGvPQ5Cf/image.png)

## 思路解析：

**思路：dp**

确定dp数组及下标含义：dp[i]表示爬到第i阶台阶所花费最小的费用

递推公式：

你可以选择从下标为 `0` 或下标为 `1` 的台阶开始爬楼梯，所以dp[1],dp[0]==0意思是爬上第1阶或者爬上第0阶台阶是不收费的，我们采用先收费在爬楼梯就是先收取cost[i]

在爬上第i阶台阶。

第i阶台阶只有两种可能被爬上来要不就是从第i-1爬上1阶就直接到第i阶台阶，要不就是先爬上i-2阶台阶再一次爬2阶台阶直接上第i阶台阶。都不必考虑是怎么上的i-1或者i-2阶台阶只需前边最小的收费的就行。

dp[i]=min(dp[i-1],dp[i-2])+cost[i];

初始化：选择从下标为 `0` 或下标为 `1` 的台阶开始爬楼梯，所以dp[1],dp[0]==0意思是爬上第1阶或者爬上第0阶台阶是不收费的

返回值：因为是先付钱后爬台阶就是先收取cost在往上爬，但是最后一步是不需要收取费用的所以只取倒数第一步和倒数第二步最小的一个



## 
代码实现：



```C++
class Solution {
public:
    /*
    第一步是消耗体力
    最后一步是不需要消耗体力值
    因为题意说明“你支付此费用，即可选择向上爬一个或者两个台阶。”
    就是先支付后消费，但是到了爬上楼顶时并不需要支付值，因为倒数第一步爬上楼顶已经消耗相对应的体力值，所以不需要消耗体力值
    */
    int minCostClimbingStairs(vector<int>& cost) {
        int n=cost.size();
        vector<int> dp(n+1,0);
        //初始化
        //因为开始从下表为1或下标为0开始爬时将dp数组的下表为0和下表为1初始化为cost的下标所对应的值
        dp[0]=cost[0];
        dp[1]=cost[1];
        for(int i=2;i<n;i++){
            //取最小值
            dp[i]=min(dp[i-1],dp[i-2])+cost[i];
        }
        //因为前边计算式是先将取体力在跳上台阶，所以这里最后结果取倒数第一个和倒数第二个中的最小值
        return min(dp[n-1],dp[n-2]);

    }
};
```

 

# 不同路径

  题目来源：(leetcode)[[https://leetcode.cn/problems/unique-paths/](https://leetcode.cn/problems/unique-paths/)]

  

  ## 问题描述：

  一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。

  机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

  问总共有多少条不同的路径？

  ## 思路解析：

我们用 f(i, j)f(i,j) 表示从左上角走到 (i, j)(i,j) 的路径数量，其中 ii 和 jj 的范围分别是 [0, m)[0,m) 和 [0, n)[0,n)。

由于我们每一步只能从向下或者向右移动一步，因此要想走到 (i, j)(i,j)，如果向下走一步，那么会从 (i-1, j)(i−1,j) 走过来；如果向右走一步，那么会从 (i, j-1)(i,j−1) 走过来。因此我们可以写出动态规划转移方程：

f(i, j) = f(i-1, j) + f(i, j-1)
f(i,j)=f(i−1,j)+f(i,j−1)

需要注意的是，如果 i=0i=0，那么 f(i-1,j)f(i−1,j) 并不是一个满足要求的状态，我们需要忽略这一项；同理，如果 j=0j=0，那么 f(i,j-1)f(i,j−1) 并不是一个满足要求的状态，我们需要忽略这一项。

初始条件为 f(0,0)=1f(0,0)=1，即从左上角走到左上角有一种方法。

最终的答案即为 f(m-1,n-1)f(m−1,n−1)。

## 代码实现：



```C++
class Solution {
public:
    /*
    对于坐标为（i，j）来说只能通过上边的坐标或者前边的坐标走到，所以达到坐标（i，j）的路径条数就是将到达前边坐标（i-1，j）的路径条数加上到达上边坐标（i，j-1）的路径条数
    特殊路线因为对于坐标（i,0）只能由上边的坐标到达，还有坐标（0,i）只能由前边的坐标到达所以到达这些坐标的路径只有一条
    dp数组的初始化
        初始化特殊路线，因为到达这些特殊坐标路径只有一条，所以将特殊坐标置为1
    返回值
        返回dp数组的最右下角值

    */
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m,vector<int> (n,0));
        //初始化
        for(int i=0;i<m;i++) dp[i][0]=1;
        for(int i=0;i<n;i++) dp[0][i]=1;
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                dp[i][j]=dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m-1][n-1];

    }
};
```

# 整数拆分

题目来源：(leetcode)[[https://leetcode.cn/problems/integer-break/](https://leetcode.cn/problems/integer-break/)]

## 问题描述:

给定一个正整数 `n` ，将其拆分为 `k` 个 **正整数** 的和（ `k >= 2` ），并使这些整数的乘积最大化。

返回 *你可以获得的最大乘积* 。

![](https://secure2.wostatic.cn/static/7brkdH6zKNsKEvgnLn2Ltr/image.png)

## 思路描述：

思路：dp

  确定dp数组及下标含义：dp[i]表示将i拆封为几个正整数的和，这些最大和为dp[i]

  确定递推公式：
i=2：1+1  1x1=1

  i=3:{1+1+1 1x1x1=1}  {1+2 1x2=2}

  i=4:{1+3 1x3=3},{2+2 dp[2]*dp[2]=4}  取2的最大值得到的就是最大值

  对于dp[i]l来说可以将i拆封为至少两个数，第一个拆分的数为j剩余的数据为i-j,如果i-j不可再分dp[i]=j*(i-j);如果还可以拆风

  当i>2时，假设第一个拆分的数字是j有如下两个方案：

  将i 拆分成 j 和 i-j 的和，且i−j 不再拆分成多个正整数，此时的乘积是 j \times (i-j)j×(i−j)；

  将 i 拆分成 j 和 i−j 的和，且 i−j 继续拆分成多个正整数，此时的乘积是 j \times \textit{dp}[i-j]j×dp[i−j]。

  因此，当 jj 固定时，有 \textit{dp}[i]=\max(j \times (i-j), j \times \textit{dp}[i-j])dp[i]=max(j×(i−j),j×dp[i−j])。由于 jj 的取值范围是 11 到 i-1i−1，需要遍历所有的 jj 得到 \textit{dp}[i]dp[i] 的最大值，因此可以得到状态转移方程如下：

  dp[i]=max(dp[i-j]*j,(j-i)*j);

  

## 代码实现：



```C++
class Solution {
public:
    int integerBreak(int n) {
        vector<int>dp(n+1,0);
        dp[2]=1;
        int  ans=0;
        for(int i=3;i<=n;i++){
            for(int j=0;j<i;j++){
                ans=max(ans,max(j*(i-j),j*dp[i-j]));
            }
            dp[i]=ans;
        }
        return dp[n];
    }
};
```

时间复杂度：O(m*n)

空间复杂度：O(n)

