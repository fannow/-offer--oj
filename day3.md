# 打家劫舍Ⅰ
题目来源：[牛客网](https://www.nowcoder.com/practice/c5fbf7325fbd4c0ea3d0c3ea6bc6cc79?tpId=196&tqId=39325&rp=1&ru=/exam/oj&qru=/exam/oj&sourceUrl=/exam/oj?page=1&pageSize=50&search=%25E6%2589%2593%25E5%25AE%25B6%25E5%258A%25AB%25E8%2588%258D&tab=%25E7%25AE%2597%25E6%25B3%2595%25E7%25AF%2587&topicId=196&difficulty=undefined&judgeStatus=undefined&tags=&title=%E6%89%93%E5%AE%B6%E5%8A%AB%E8%88%8D)
## 1、题目描述
你是一个经验丰富的小偷，准备偷沿街的一排房间，每个房间都存有一定的现金，为了防止被发现，你不能偷相邻的两家，即，如果偷了第一家，就不能再偷第二家；如果偷了第二家，那么就不能偷第一家和第三家。
给定一个整数数组nums，数组中的元素表示每个房间存有的现金数额，请你计算在不被发现的前提下最多的偷窃金额。
数据范围：数组长度满足 1 \le n \le 2\times 10^5\ 1≤n≤2×10 
数组中每个值满足 1 \le num[i] \le 5000 \ 1≤num[i]≤5000 
## 2、思路解析
**思路：DP**
首先确定DP数组及下标的含义：dp[i]表示偷取到第i家偷去的最大的金钱数
确定递推公式：因为不能连续偷，所以偷第一家后就不能偷第二家，所以在偷第二家要比较选择一家最高的一家偷，所以dp[2]要偷前两家比较高的一家偷的金钱
dp[i]:有两种情况：
（1）第i家不偷钱数还是前一家的钱数dp[i-1]
（2）第i家偷取所以前一天就不能偷取，所以是前两家的钱数加上第i家的钱数dp[i-2]+nums[i];
递推公式：dp[i-1]=max(dp[i-1],dp[i-2]+nums[i])
初始化：
在偷取第0家时，所以dp[0]=nums[0];
在偷取第1家时，应该是前两家的偷取比较的高的一家钱数（下标i的函数是偷取第i家时所得钱数最大）
返回值：返回dp数组的最后一个
## 3、代码实现

```cpp
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param nums int整型vector 
     * @return int整型
     */
    int rob(vector<int>& nums) {
        // write code here
        int len=nums.size();
        vector<int>dp(len,0);
        dp[0]=nums[0];
        dp[1]=max(dp[0],nums[1]);//比较前两家是否要偷
        for(int i=2;i<len;i++){
            dp[i]=max(dp[i-1],dp[i-2]+nums[i]);
        }
        return dp[len-1];
    }
};
```
**时间复杂度：O(n);
空间复杂度：O(n)**
# 打家劫舍Ⅱ
题目来源：[牛客网](https://www.nowcoder.com/practice/a5c127769dd74a63ada7bff37d9c5815?tpId=196&tqId=39326&rp=1&ru=/exam/oj&qru=/exam/oj&sourceUrl=/exam/oj?page=1&pageSize=50&search=%25E6%2589%2593%25E5%25AE%25B6%25E5%258A%25AB%25E8%2588%258D&tab=%25E7%25AE%2597%25E6%25B3%2595%25E7%25AF%2587&topicId=196&difficulty=undefined&judgeStatus=undefined&tags=&title=%E6%89%93%E5%AE%B6%E5%8A%AB%E8%88%8D)
## 1、问题描述
你是一个经验丰富的小偷，准备偷沿湖的一排房间，每个房间都存有一定的现金，为了防止被发现，你不能偷相邻的两家，即，如果偷了第一家，就不能再偷第二家，如果偷了第二家，那么就不能偷第一家和第三家。沿湖的房间组成一个闭合的圆形，即第一个房间和最后一个房间视为相邻。
给定一个长度为n的整数数组nums，数组中的元素表示每个房间存有的现金数额，请你计算在不被发现的前提下最多的偷窃金额。

数据范围：数组长度满足 1 \le n \le 2\times10^5 \ 1≤n≤2×10 
数组中每个值满足 1 \le nums[i] \le 5000 \ 1≤nums[i]≤5000 
## 2、思路解析
**思路：DP**
思路还是上一题的思路是不过要考虑首尾情况，因为首家偷取了最后一家也偷去了就会造成了偷去的是一个连续的状态，所以选择投不投有中情况
（1）首家选择不偷取，尾家也选择不偷取，所以这是一个比较偷取比较少的情况
（2）选择首家偷取，尾家不偷取这样也不造成偷取连续的情况，相比上一个情况比较好一点
（3）选择尾家偷取，首家不偷取这样也不会造成偷取连续的情况，相比第一种情况也比较好一点。
因为第一种情况是一种比较保守的情况，所以2 3中情况包含了第一种情况只要考虑到了2.3中情况就不必考虑第1中情况
## 3、代码实现

```cpp
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param nums int整型vector 
     * @return int整型
     */
    //首尾都不偷
    //首头尾不透
    //首部头尾头
    int rob(vector<int>& nums) {
        // write code here
        int len=nums.size();
        int a=_rob(nums,0,len-2);
        int b=_rob(nums,1,len-1);
        return max(a,b);
        
    }
    int _rob(vector<int>&nums,int begin,int end){
        int len=end-begin+1;
        vector<int> dp(nums.size(),0);
        dp[begin]=nums[begin];
        dp[begin+1]=max(nums[begin],nums[begin+1]);
        for(int i=begin+2;begin<=end;begin++){
            dp[i]=max(dp[i-1],dp[i-2]+nums[i]);
        }
        return dp[end];
        
    }
};
```
# 打家劫舍Ⅲ
题目来源：[牛客网](https://www.nowcoder.com/practice/82b6dce6a7634419b272ee4397e26d89?tpId=196&tqId=39327&rp=1&ru=/exam/oj&qru=/exam/oj&sourceUrl=/exam/oj?page=1&pageSize=50&search=%25E6%2589%2593%25E5%25AE%25B6%25E5%258A%25AB%25E8%2588%258D&tab=%25E7%25AE%2597%25E6%25B3%2595%25E7%25AF%2587&topicId=196&difficulty=undefined&judgeStatus=undefined&tags=&title=%E6%89%93%E5%AE%B6%E5%8A%AB%E8%88%8D)
## 1、问题描述
你是一个经验丰富的小偷，经过上次在街边和湖边得手后你准备挑战一次自己，你发现了一个结构如二叉树的小区，小区内每个房间都存有一定现金，你观察到除了小区入口的房间以外每个房间都有且仅有一个父房间和至多两个子房间。
问，给定一个二叉树结构的小区，如之前两次行动一样，你无法在不触动警报的情况下同时偷窃两个相邻的房间，在不触动警报的情况下最多的偷窃金额。

1.如果输入的二叉树为{2,1,2,#,2,#,1}，那么形状结构如下:
## 2、思路解析
**思路：树形DP**
确定dp数组及下标含义：dp数组是一个长度为2的数组dp[0]表示偷取当前节点的钱数，dp[1]表示不偷取当前节点的钱数。因为小区是程二叉树形状，所以不可能直接开辟节点数长度的空间。因此在在递归函数给每个节点开辟一个dp数组用来存储当前节点偷取当前节点的钱数和不偷取当前节点的钱数。
确定递推公式：确定偷取当前节点就该是当前节点数+不偷取左右子节点的钱数
						不偷取当前节点钱数应当是偷取左节点的钱数和不偷取左节点钱数取最大值+偷取右节点的钱数和不偷取右节点钱数取最大值
				dp[0]=root->val+right[1]+right[1];
				dp[1]=max(right[1],right[0])+max(left[1],left[0]);
返回值：返回头结点的dp数组最大的钱数
## 3、代码实现

```cpp
/**
 * struct TreeNode {
 *	int val;
 *	struct TreeNode *left;
 *	struct TreeNode *right;
 *	TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 * };
 */
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @return int整型
     */
    //每一层偷取父节点或者子节点的钱数
    int rob(TreeNode* root) {
        // write code here
        if(root==NULL){
            return 0;
        }
        vector<int> dp=robtree(root);
        return max(dp[1],dp[0]);
    }
    vector<int> robtree(TreeNode*root){
        if(root==NULL) return {0,0};
        vector<int> left=robtree(root->left);
        vector<int> right=robtree(root->right);
        //选择取当前节点加上不偷取子节点的钱数
        int val=root->val+right[1]+left[1];
        //取子节点的最大钱数和
        int val2=max(left[1],left[0])+max(right[1],right[0]);
        return {val,val2};
        
    }
};
```
