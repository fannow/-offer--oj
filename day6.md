# 兑换零钱

题目来源：[leetcode]([https://leetcode.cn/problems/coin-change-2/](https://leetcode.cn/problems/coin-change-2/))

## 问题描述：

给你一个整数数组 coins 表示不同面额的硬币，另给一个整数 amount 表示总金额。

请你计算并返回可以凑成总金额的硬币组合数。如果任何硬币组合都无法凑出总金额，返回 0 。

假设每一种面额的硬币有无限个。 

题目数据保证结果符合 32 位带符号整数。

## 思路解析：

思路：dp

创建dp数组及明确下标含义：dp[i][j]表示当金钱数为j时在前i个金币所换取最小金币数为dp[i][j] 

递推公式：dp[i][j]  在前i个金币中寻找最小的和为j的最下金币数依赖于第i-1中情况也就是dp[i-1]    先保证前边dp[i-1]结果为最小   dp[i-1][j-coins[i]]   j-coins[i]:表示当前容量为j当拿去coins[i]时在前一种情况中可以找到容量为j-coins[i]所兑换最小金币数。所以递推公式为dp[i][j]=min[i-1][j-coins[i]]+1,dp[i][j])

初始化：当兑换金钱数为0时最小金钱为0

返回值；dp[coins.size()][amount];

## 代码实现：



```C++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
       
    vector<int>dp(amount+1,INT_MAX);
            dp[0]=0;
            for(int i=0;i<coins.size();i++){
                for(int j=coins[i];j<=amount;j++){
                    if(dp[j-coins[i]]!=INT_MAX){
                     dp[j]=min(dp[j-coins[i]]+1,dp[j]);
                    }
                }
            }
             if (dp[amount] == INT_MAX) return -1;
         return dp[amount];
    }
};
```

# [零钱兑换 II](https://leetcode.cn/problems/coin-change-2/)

题目来源：[leetcode]([https://leetcode.cn/problems/coin-change-2/](https://leetcode.cn/problems/coin-change-2/))

## 问题描述：

给你一个整数数组 coins 表示不同面额的硬币，另给一个整数 amount 表示总金额。

请你计算并返回可以凑成总金额的硬币组合数。如果任何硬币组合都无法凑出总金额，返回 0 。

假设每一种面额的硬币有无限个。 

题目数据保证结果符合 32 位带符号整数

## 思路解析：



思路：dp

确认dp数组及明确下标含义：dp[j]表示当前金钱树为j时前i个金币组合数为dp[j]

确认递推公式：

对于面额为coin 的硬币，当coin≤i≤amount 时，如果存在一种硬币组合的金额之和等于i−coin，则在该硬币组合中增加一个面额为 oin 的硬币，即可得到一种金额之和等于 i的硬币组合。因此需要遍历 coins，对于其中的每一种面额的硬币，更新数组 dp 中的每个大于或等于该面额的元素的值。

dp[j]+=dp[j-coins[i]];

初始化：dp[0]=1；

遍历 coins，对于其中的每个元素 coin，进行如下操作：遍历 i从coin 到amount，将 dp[i−coin] 的值加到 dp[i]

## 代码实现：



```C++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> dp(amount+1,0);
        dp[0]=1;
        for(int i=0;i<coins.size();i++){
            for(int j=coins[i];j<=amount;j++){
                cout<<dp[j]<<endl;
                dp[j]+=dp[j-coins[i]];
            }
        }
        return dp[amount];
    }
};
```

# 完全平方数

题目来源：[leetcode]([https://leetcode.cn/problems/perfect-squares/](https://leetcode.cn/problems/perfect-squares/))



## 问题描述：

给你一个整数 n ，返回 和为 n 的完全平方数的最少数量 。

完全平方数 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，1、4、9 和 16 都是完全平方数，而 3 和 11 不是。

## 思路解析

思路：DP



这一题和兑换零钱思路都是差不多都是将选数据和为固定的值最小的个数，这道题也是一道经典的的完全背包问题，将固定数字当作背包容量，完全平方数当作商品，每一个商品都是无限的，所以这是一个完全背包的问题

首先初始化长度为n+1 的数组 dp，每个位置都为0，对数组进行遍历，下标为 `i`，每次都将当前数字先更新为最大的结果，即 `dp[i]=i`，比如 `i=4`，最坏结果为 `4=1+1+1+1` 即为 `4` 个数字，动态转移方程为：`dp[i] = MIN(dp[i], dp[i - j * j] + 1)`，`i` 表示当前数字，`j*j` 表示平方数。

## 代码实现：



```C++
class Solution {
public:
    int numSquares(int n) {
        vector<int>dp(n+1,0);
     
        for(int i=1;i<=n;i++){
            dp[i]=i;
            for(int j=1;j*j<=i;j++){
                dp[i]=min(dp[i-j*j]+1,dp[i]);
            }
        }
        return dp[n];
    }
};
```

# [组合总和 Ⅳ](https://leetcode.cn/problems/combination-sum-iv/)

题目来源：[leetcode]([https://leetcode.cn/problems/combination-sum-iv/](https://leetcode.cn/problems/combination-sum-iv/))

## 问题描述

给你一个由 不同 整数组成的数组 nums ，和一个目标整数 target 。请你从 nums 中找出并返回总和为 target 的元素组合的个数。

题目数据保证答案符合 32 位整数范围。

## 思路解析：


**思路：dp**

确认dp数组及明确下标含义：dp[j]表示当前金钱树为j时前i个金币组合数为dp[j]

确认递推公式：

对于数组元素为num 的元素，当num≤i≤target时，如果存在一种组合的之和等于i−num，则在该组合中增加一个数据大小为num，即可得到一种元素之和等于 i的组合。因此需要遍历 nums，对于其中的每一个元素，更新数组 dp 中的每个大于或等于元素的值。

dp[j]+=dp[j-nums[i]];

初始化：dp[0]=1；

遍历 nums，对于其中的每个元素 num，进行如下操作：遍历 i从num到target，将 dp[i−num] 的值加到 dp[i]

## 代码实现：

```C++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<long>dp(target+1,0);
        dp[0]=1;
        for(int i=1;i<=target;i++){

            for(int j=0;j<nums.size();j++){
                if(i>=nums[j]&& dp[i] < INT_MAX - dp[i - nums[j]]){
                
                  dp[i]+=dp[i-nums[j]];
                }
            }
        }
        return dp[target];
    }
};
```





# [单词拆分](https://leetcode.cn/problems/word-break/)

题目来源：[leetcode]([https://leetcode.cn/problems/word-break/](https://leetcode.cn/problems/word-break/))

## 问题描述：

给你一个字符串 s 和一个字符串列表 wordDict 作为字典。请你判断是否可以利用字典中出现的单词拼接出 s 。

注意：不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

## 思路解析：

思路：DP

创建dp数组并明确下标含义：dp[i]表示前i个字符是否经过拼接在字典中找到

递推公式：假设0~j个字符已经在字典中匹，配过了并且是true，我们只需将区间【j,i】在字典中匹配，匹配上取前j个字符的匹配结构dp[j]做与运算将所有存在dp[i]，就行

初始化：dp[0]=true;

## 代码实现：







```C++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> qs(wordDict.begin(),wordDict.end());
        vector<bool> dp(s.size()+1,false);
        dp[0]=true;
        for(int i=0;i<=s.size();i++){
            for(int j=0;j<=i;j++){
                //先判断区间能不能在找到
                string ss=s.substr(j,i-j);
                if(qs.count(ss)&&dp[j]){
                    dp[i]=true;//区间能在匹配，并前j个字符组成的单词能匹配上如果是直接赋值
                }

            }
        }
        return dp[s.size()];

    }
};
```