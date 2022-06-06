# [最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)

题目来源：[leetcode]([https://leetcode.cn/problems/longest-increasing-subsequence/](https://leetcode.cn/problems/longest-increasing-subsequence/))

## 问题描述

给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。

子序列 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。

![](https://secure2.wostatic.cn/static/iRzheCSgXo4yPcL5jCH4aT/image.png)

## **思路解析**

**思路：dp**

创建dp数组明确下标的含义：dp[i]表示最大连续递增子序列为dp[i] 

递推公式：dp[i]是前i个中前i元素中最大递增连续子系列，本题不像之前的那些题一样第i个元素可以不选，这里必须加上第i个元素。所以我们可以在遍历前i-1个元素 寻找比nums[i]小的元素nums[j] ,nums[j]对应前边比他小得元素个数为dp[j]。所以递推公式为dp[i]=max(dp[i],dp[j]+1)

初始化：因为每个元素默认长度为1所以dp数组每个位置置为1

返回值：返回dp数组中最大值

## 代码实现

```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if(nums.size()==0){
            return 0;
        }
        //初始化为1，因为每个数据长度默认为1
        vector<int> v(nums.size(),1);
        int ret=v[0];
        for(int i=1;i<nums.size();i++){
            //在前边i-1中找比nums[i]小的数组并且将dp[i] 和dp[j]+1(因为比较是已经选出num[i]>num[j]所以要给dp[j]+1)中选一个最大的
            for(int j=0;j<i;j++){
                if(nums[i]>nums[j]){
                    v[i]=max(v[i],v[j]+1);
                }
            }
            ret=max(ret,v[i]);
        }
        //最后选出数组中最大的；

        return ret;
    }
};
```



# [最长连续递增序列](https://leetcode.cn/problems/longest-continuous-increasing-subsequence/)

题目来源：[leetcode]([https://leetcode.cn/problems/longest-continuous-increasing-subsequence/](https://leetcode.cn/problems/longest-continuous-increasing-subsequence/))

## 问题描述

给定一个未经排序的整数数组，找到最长且 连续递增的子序列，并返回该序列的长度。

连续递增的子序列 可以由两个下标 l 和 r（l < r）确定，如果对于每个 l <= i < r，都有 nums[i] < nums[i + 1] ，那么子序列 [nums[l], nums[l + 1], ..., nums[r - 1], nums[r]] 就是连续递增子序列。

## 思路解析：

**思路一：滑动窗口+三指针**

题目要求连续最长子序列，我们可以使用滑动窗口让窗口中元素保持增长的状态，还有三个指针begin、end、in。begin指向窗口的左侧，end指向窗口的右侧，in指向end后一个元素用来比较是不是递增的

实现过程是end和in指针循环比较当遇到in>end。就说明当前创窗口元素递增介绍，保存当前窗口长度，更新begin指向end位置，end和in往后移动一位。

**思路二：DP**

创建dp数组明确下标含义：dp[i]表示前i个元素连续小于第i个元素数

递推公式：dp[i]前i个元素连续小于第i个元素数 ,因为是连续我们只需要判断nums[i-1]是不是小于nums[i],因为dp[i-1]是由前边结果求出我们直接拿来用 如果小于nums[i]我们就取dp[i-1]+1,所以递推公式为dp[i]=max(dp[i],dp[i-1]+1);

初始化：

初始化：因为每个元素默认长度为1所以dp数组每个位置置为1

返回值：返回dp数组中最大值

代码实现：



```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int begin=0;
        int end=1;
        int ans=1;
        int in=0;
        while(end<nums.size()){
      
            while(end<nums.size()&&nums[end]>nums[in]){

                end++;
                in++;
                
            }
            ans=max(ans,end-begin);
            begin=end;
            end++;
            in++;
        }
        return ans;
        


    }
};
```



```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        if(nums.size()==0){
            return 0;
        }
        vector<int>dp(nums.size(),1);
        int ret=dp[0];
        for(int i=1;i<nums.size();i++){
            if(nums[i]>nums[i-1]){
                dp[i]+=dp[i-1];
            }
            ret=max(ret,dp[i]);
        }
        return ret;
    }
};
```





# [最长重复子数组](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/)

题目来源：[leetcode]([https://leetcode.cn/problems/maximum-length-of-repeated-subarray/](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/))

## 问题描述

  给两个整数数组 `nums1` 和 `nums2` ，返回 *两个数组中 ****公共的**** 、长度最长的子数组的长度* 。

![](https://secure2.wostatic.cn/static/4FiSxCuLwS5PP1HM4UnrdR/image.png)

## 思路解析

**思路：DP**

创建dp数组明确下标含义：dp[i][j]数组表示在nums1数组中连续包括nums[i]和nums2数数连续包括nums[j]在内相同元素个数

递推公式：令 dp[i][j] 表示 A 和 B的最长公共前缀，那么答案即为所有 dp[i][j] 中的最大值。如果 A[i] == B[j]，那么 dp[i][j] = dp[i + 1][j + 1] + 1，否则 dp[i][j] = 0。

初始化：dp[i][0] dp[0][j]无意义初始化为0

## 代码实现：





```C++
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        int len1=nums1.size();
        int len2=nums2.size();
        int ans=0;
        vector<vector<int>> dp(len1+1,vector<int>(len2+1,0));
        for(int i=1;i<=nums1.size();i++){
            for(int j=1;j<=nums2.size();j++){
                if(nums1[i-1]==nums2[j-1]){
                    dp[i][j]=max(dp[i-1][j-1]+1,dp[i][j]);
                } 
                ans=max(dp[i][j],ans);
            }
           
        }
        return ans;

    }
};
```





# [最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)&&[不相交的线](https://leetcode.cn/problems/uncrossed-lines/)

题目来源：leetcode 

## 问题描述：


**最长公共子序列：**

给定两个字符串 text1 和 text2，返回这两个字符串的最长 公共子序列 的长度。如果不存在 公共子序列 ，返回 0 。

一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。
两个字符串的 公共子序列 是这两个字符串所共同拥有的子序列。

**不相交的线：**

在两条独立的水平线上按给定的顺序写下 nums1 和 nums2 中的整数。

现在，可以绘制一些连接两个数字 nums1[i] 和 nums2[j] 的直线，这些直线需要同时满足满足：

nums1[i] == nums2[j]
且绘制的直线不与任何其他连线（非水平线）相交。
请注意，连线即使在端点也不能相交：每个数字只能属于一条连线。

以这种方法绘制线条，并返回可以绘制的最大连线数。

## 思路解析

为什么将这两道题目放在一起？
先解析一下**不相交的线** 题目说明要求不能让绘制为直线不能和其他连线相交，所以这要保证这些元素不仅时相等的，而且连接线没有相交而且要保证最长这不是求最长公共子序列吗所以这一道题本质是最长公共子序列的一道变题。

**思路：DP**

创建dp数组明确下标含义：dp[i][j]表示数组A前i个元素和数组B前j个元素最长公共子序列为dp[i][j]

递推公式：

当 i>0i>0 且 j>0j>0 时，考虑 dp[i][j] 的计算：

当 A[i-1]==B[j-1]时，将这两个相同的字符称为公共字符，考虑A(0,i-1) 和B（0，j-1）的最长公共子序列，再增加一个字符（即公共字符）即可得到A和B 的最长公共子序列，因此 dp[i][j]=dp[i-1][j-1]+1

当A[i-1]≠B[j-1]时：(1) A(0,i-1)和B(0,j)的最长公共子序列（2）A(0,i)和B(0,j-1)的最长公共子序列

要得到 A(0,i)和B(0,j) 的最长公共子序列，应取两项中的长度较大的一项，因此   dp[i][j]=max(dp[i−1][j],dp[i][j−1])。

## 代码实现



```C++
class Solution {
public:
    int longestCommonSubsequence(string A, string B) {
         vector<vector<int>> dp(A.size() + 1, vector<int>(B.size() + 1, 0));
        for (int i = 1; i <= A.size(); i++) {
            for (int j = 1; j <= B.size(); j++) {
                if (A[i - 1] == B[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[A.size()][B.size()];
    }
};
```

