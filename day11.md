## [二叉树剪枝](https://leetcode.cn/problems/pOCWxh/)

题目来源：[leetcode]([https://leetcode.cn/problems/pOCWxh/](https://leetcode.cn/problems/pOCWxh/))

## 问题描述

给定一个二叉树 根节点 root ，树的每个节点的值要么是 0，要么是 1。请剪除该二叉树中所有节点的值为 0 的子树。

节点 node 的子树为 node 本身，以及所有 node 的后代。

## 思路解析

**思路：递归**

先分析分析一下剪枝其实就是删除节点，但是删除节点是还要考虑该节点有没有子节点所以不能直接删除节点为0的节点，该节点的字节点可能还存在字节点为1的字节点，所以我们应该先删除叶子节点为0的节点，当该节节点的所以字节点都为0时都会被删除，该节点为0时也会被删除：



1. 左子树为空
2. 右子树为空
3. node.val == 0

当满足以上三个条件时，将该叶子节点删除(node = null)，即可。
通过深度优先搜索每个节点的子节点，递归返回，就是最终的答案了。
这里需要注意下root节点为空时，特殊判断。

## 代码实现

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
/*
节点同时满足子节点为NULL，当前节点值为0直接删除节点
*/
    TreeNode* pruneTree(TreeNode* root) {
        if(root==NULL){
            return NULL;
        }
       root->left= pruneTree(root->left);
       
       root->right= pruneTree(root->right);
        if(root->left==NULL&&root->right==NULL&&root->val==0){
            root=NULL;
       
        }
        return root;



    }
};
```

# [节点之和最大的路径](https://leetcode.cn/problems/jC7MId/)

题目来源：[leetcode]([https://leetcode.cn/problems/jC7MId/](https://leetcode.cn/problems/jC7MId/))

## 问题描述

路径 被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。同一个节点在一条路径序列中 至多出现一次 。该路径 至少包含一个 节点，且不一定经过根节点。

路径和 是路径中各节点值的总和。

给定一个二叉树的根节点 root ，返回其 最大路径和，即所有路径上节点值之和的最大值。

## 思路解析

思路：递归

树中出现的最大路径和包含当前节点还包括当前节点的左子树和右子树的路径，所以每个叶节点最大路径和包含当前节点的值加上左子树和右子树较大的一个。定义一个全局变量用来当前节点的所在子树的最大路径和，每次递归都将左子树的最大路径和保存在left，将右子树的最大路径和保存在right中。ans每次都在ans和left+right+root→val选取一个较大的作为ans。最后返回全局变量ans

## 代码实现

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int ans = INT_MIN;

    int maxPathSum(TreeNode* root) {
        dfs(root);
        return ans;
    }

    int dfs(TreeNode* root) {
        if (root == NULL) return 0;

        int left = max(0, dfs(root->left));
        int right = max(0, dfs(root->right));

        ans = max(ans, left + right + root->val);

        return max(left, right) + root->val;
    }
};
```





# [向下的路径节点之和](https://leetcode.cn/problems/6eUYwP/)

题目来源：[leetcode]([https://leetcode.cn/problems/6eUYwP/](https://leetcode.cn/problems/6eUYwP/))

## 问题描述

给定一个二叉树的根节点 root ，和一个整数 targetSum ，求该二叉树里节点值之和等于 targetSum 的 路径 的数目。

路径 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

![](https://secure2.wostatic.cn/static/fEcQcL1tSGdPUTgFkV29FP/image.png)

## 思路解析



**思路：全局搜索**

寻找路径之和为sum的录井个数。我们不需要将节点值相加然后告诉给字节点，我们只要告诉sum减去当前节点的值就可以，这样字节点只要判断段root-sum是否为0就将出现的次数++。

题目说明不需要从该节点开始，所以每个节点作为开始节点的路径之和都有可能，所以递归遍历每个节点让每个节点作为开始节点像根节点似的遍历寻找出现的次数



## 代码实现

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int dfs(TreeNode*root,int&&num){
        if(root==NULL){
            return 0;
        }
        int sum=0;
        if(root->val==num){
            sum++;
        }
        sum+=dfs(root->left,num-root->val); //在当前节点的左子树寻找是不是存在
        sum+=dfs(root->right,num-root->val);
        return sum;

    }
    
    int pathSum(TreeNode* root, int targetSum) {
        if(root==NULL){
            return 0;
        }
        int ans=dfs(root,move(targetSum));
        ans+=pathSum(root->left,targetSum); //以每个节点为根节点遍历寻找路径
        ans+=pathSum(root->right,targetSum);
        return ans;

    }
};
```

# [二叉搜索树迭代器](https://leetcode.cn/problems/kTOapQ/)

题目来源：[leetcode]([https://leetcode.cn/problems/kTOapQ/](https://leetcode.cn/problems/kTOapQ/))

## 问题描述

实现一个二叉搜索树迭代器类BSTIterator ，表示一个按中序遍历二叉搜索树（BST）的迭代器：

BSTIterator(TreeNode root) 初始化 BSTIterator 类的一个对象。BST 的根节点 root 会作为构造函数的一部分给出。指针应初始化为一个不存在于 BST 中的数字，且该数字小于 BST 中的任何元素。
boolean hasNext() 如果向指针右侧遍历存在数字，则返回 true ；否则返回 false 。
int next()将指针向右移动，然后返回指针处的数字。
注意，指针初始化为一个不存在于 BST 中的数字，所以对 next() 的首次调用将返回 BST 中的最小元素。

可以假设 next() 调用总是有效的，也就是说，当调用 next() 时，BST 的中序遍历中至少存在一个下一个数字。

## 思路解析

**思路一：哈希**

将搜索树所以节点在构造函数时通过中序遍历将所以节点插入到哈希数组中，通过一个所以index来访问，next函数：每次屌用next函数之前都将索引index++然后返回索引对应的值。s

**思路二：栈**

除了递归的方法外，我们还可以利用栈这一数据结构，通过迭代的方式对二叉树做中序遍历。此时，我们无需预先计算出中序遍历的全部结果，只需要实时维护当前栈的情况即可。

每次调用next函数后第一件事情就是判断当前节点是不是为NULL。不为NULL就将当前节点的左子节点入栈，这样做是为了符合中序遍历原则。保存当前值后就赋值为右子树节点

![](https://secure2.wostatic.cn/static/n6XZAUhGdGZCzGTmSTXVDJ/image.png)

## 代码实现

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class BSTIterator {
    vector<int> v;
    int index=-1;
    void func(TreeNode*root){
        if(root==NULL){
            return;
        }
        func(root->left);
        v.push_back(root->val);
        func(root->right);
    }
public:
    BSTIterator(TreeNode* root) {
        func(root);
    }
    
    int next() {
        index++;
        return v[index];
    }
    bool hasNext() {
        if(index+1>=v.size()){
            return false;
        }
        return true;

    }
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```

```C++
class BSTIterator {
private:
    TreeNode* cur;
    stack<TreeNode*> stk;
public:
    BSTIterator(TreeNode* root): cur(root) {}
    
    int next() {
        while (cur != nullptr) {
            stk.push(cur);
            cur = cur->left;
        }
        cur = stk.top();
        stk.pop();
        int ret = cur->val;
        cur = cur->right;
        return ret;
    }
    
    bool hasNext() {
        return cur != nullptr || !stk.empty();
    }
};


```