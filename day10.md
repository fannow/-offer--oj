# [往完全二叉树添加节点](https://leetcode.cn/problems/NaqhDT/)

题目来源：[leetcode]([https://leetcode.cn/problems/NaqhDT/](https://leetcode.cn/problems/NaqhDT/))

## 题目说明

完全二叉树是每一层（除最后一层外）都是完全填充（即，节点数达到最大，第 n 层有 2n-1 个节点）的，并且所有的节点都尽可能地集中在左侧。

设计一个用完全二叉树初始化的数据结构 CBTInserter，它支持以下几种操作：

CBTInserter(TreeNode root) 使用根节点为 root 的给定树初始化该数据结构；
CBTInserter.insert(int v)  向树中插入一个新节点，节点类型为 TreeNode，值为 v 。使树保持完全二叉树的状态，并返回插入的新节点的父节点的值；
CBTInserter.get_root() 将返回树的根节点。

## 思路解析

**思路：深度优先遍历**

![](https://secure2.wostatic.cn/static/psBbCRZWyHK4aGdQCfUmN6/image.png)



如图插入节点分为2中情况：

  （1）插入当前节点之前的节点还存在子节点，保存当前行没有子节点的节点

   （2）插入节点当前行节点都没有子节点，保存当前行节点。

  首先定义一个队列，讲头节点插入到队列中，判断对头节点的左节点的是不是存在，如果存在就属于第一种情况在循环取出节点时，判断左子节点或者右子节点为空时就将节点插入到v数组中。，如果不存在就属于第二种情况在循环取出节点时，讲节点插入到另一个vm数组中。

  插入节点时遍历v数组当前遇到左子节点或者右子节点为空时就将节点插入到当前节点的子节点中同时返回当前节点。判断v数组最后一个节点的右子节点是不是为NUll ，不为NULL就说明树的当前行已经满了。更新v数组（v=vm）。

  返回根节点值直接return root→val.

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
class CBTInserter {
    TreeNode*root;
    vector<TreeNode*>vn;
    vector<TreeNode*>v;
    vector<TreeNode*>vm;
    vector<TreeNode*>vs;
public:
    CBTInserter(TreeNode* root) {
        this->root=root;
        //获取最后一行的节点
        queue<TreeNode*>q;
        q.push(root);
     
        while(!q.empty()){
            int size=q.size();
            if(q.front()->left!=NULL){
                while(size--){
                    TreeNode*node=q.front();
                    q.pop();
                    if(node->left==NULL||node->right==NULL){
                        vn.push_back(node);
                    }
                    if(node->left){
                        q.push(node->left);
                    }
                    if(node->right){
                        q.push(node->right);
                    }
                }
            }
            else{
                while(size--){
                    TreeNode*node=q.front();
                    q.pop();
                    v.push_back(node);
                }
            }
        }

        vs=v;
    }
    
    int insert(int v1) {
        if(vn.size()==0||vn.back()->right!=NULL){         
               if(vs==v){
                 vn=v; 
                v.clear();
               }else{
                  vn=vm;     
               }
           
        }
        for(int i=0;i<vn.size();i++){
            if(vn[i]->left==NULL){
                vn[i]->left=new TreeNode(v1);
                //讲新添加的节点加入到列表中
                vm.push_back(vn[i]->left);
                return vn[i]->val;
            }
            if(vn[i]->right==NULL){
                vn[i]->right=new TreeNode(v1);
                vm.push_back(vn[i]->right);
                return vn[i]->val;
            }
        }
        return -1;

    }
    
    TreeNode* get_root() {
        return root;

    }
};

/**
 * Your CBTInserter object will be instantiated and called as such:
 * CBTInserter* obj = new CBTInserter(root);
 * int param_1 = obj->insert(v);
 * TreeNode* param_2 = obj->get_root();
 */
```

# [滑动窗口的平均值](https://leetcode.cn/problems/qIsx9U/)

题目来源：[leetcode]([https://leetcode.cn/problems/qIsx9U/](https://leetcode.cn/problems/qIsx9U/))

## 问题说明

给定一个整数数据流和一个窗口大小，根据该滑动窗口的大小，计算滑动窗口里所有数字的平均值。

实现 MovingAverage 类：

MovingAverage(int size) 用窗口大小 size 初始化对象。
double next(int val) 成员函数 next 每次调用的时候都会往滑动窗口增加一个整数，请计算并返回数据流中最后 size 个值的移动平均值，即滑动窗口里所有数字的平均值。
 

## 思路解析

定义3个成员对象 ，size表示初始化的窗口的大小，v用来存储窗口中的元素，sum用来计算窗口中元素的和

在构造函数中初始化窗口的大小，在next函数实现插入功能，首先判断v.size()和窗口大小是不是相等，相等就要删除v的头元素，接着sum减去v的头元素。

当数组v中元素个数还有没有窗口大小的时直接插入数据，最后计算平均值，并返回

## 代码实现

```C++
class MovingAverage {
    int sum=0;
    vector<int> v;
    int size=0;
public:
    /** Initialize your data structure here. */
    MovingAverage(int size) {
        this->size=size;
    }
    
    double next(int val) {
        if(v.size()==size){
            sum-=v[0];
            v.erase(v.begin());
        }
        v.push_back(val);
        sum+=val;
        double d=(sum*1.0)/v.size();
        return d;

    }
};

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage* obj = new MovingAverage(size);
 * double param_1 = obj->next(val);
 */
```

# [二叉树的右侧视图](https://leetcode.cn/problems/WNC0Lk/)

题目来源：[leetcode]([https://leetcode.cn/problems/WNC0Lk/](https://leetcode.cn/problems/WNC0Lk/))

## 问题说明

给定一个二叉树的 **根节点** `root`，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

## 思路解析

思路：广度优先遍历

思路和树的层序遍历实现过程是一样的，线创建一个队列用来存储每一层的节点。首先讲头节点存储到队列中遍历队列：

  （1）保存队列中节点数size

  （2）节点出队判断节点的的左子节点和右子节点是不是为NULL，不为NULL直接插入到队列中

  （3）判断size是不是到达0当size到达0就说明当前节点为最后一个节点，将节点插入到数组中

  （4）遍历队列直到队列为NULL最后然会数组v

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
    vector<int> rightSideView(TreeNode* root) {
         vector<int>v;
        if(root==NULL){
            return v;
        }
        queue<TreeNode*>q;

        q.push(root);
       
        while(!q.empty()){
            int s=q.size();
 
      
            while(s--){
                TreeNode*root=q.front();
                q.pop();
                if(s==0){
                    v.push_back(root->val);
                }
                if(root->left){
                    q.push(root->left);

                }
                if(root->right){
                    q.push(root->right);

                }


            }
        }
        return v;

    }
};
```

# [设计循环队列](https://leetcode.cn/problems/design-circular-queue/)

  题目来源：[leetcode]([https://leetcode.cn/problems/design-circular-queue/](https://leetcode.cn/problems/design-circular-queue/))

  ## 题目说明

  设计你的循环队列实现。 循环队列是一种线性数据结构，其操作表现基于 FIFO（先进先出）原则并且队尾被连接在队首之后以形成一个循环。它也被称为“环形缓冲器”。

  循环队列的一个好处是我们可以利用这个队列之前用过的空间。在一个普通队列里，一旦一个队列满了，我们就不能插入下一个元素，即使在队列前面仍有空间。但是使用循环队列，我们能使用这些空间去存储新的值。

  你的实现应该支持如下操作：

  MyCircularQueue(k): 构造器，设置队列长度为 k 。
Front: 从队首获取元素。如果队列为空，返回 -1 。
Rear: 获取队尾元素。如果队列为空，返回 -1 。
enQueue(value): 向循环队列插入一个元素。如果成功插入则返回真。
deQueue(): 从循环队列中删除一个元素。如果成功删除则返回真。
isEmpty(): 检查循环队列是否为空。
isFull(): 检查循环队列是否已满。

  

## 思路解析

**思路：双指针**

用数组来存放元素，begin指向头元素没删除一个元素begin就会往后移动，end指向最后一个元素，当插入一个元素后就会往后移动一个位置。

构造函数实现：初始化循环列表的大小 接着开辟数组容量大小的val

插入函数：先判断首元素有没有插入，没有插入就先插入到首元素接着end++，接着判断队列有没有满，如果满直接返回false,没有满插入end位置同时更新end

删除元素：删除元素时先判断队列是不是为NULL。删除begin位置元素，同时begin往后移动一位并更新begin

判空函数；遍历数组如果全是-1就证明为NULL

判满函数：遍历数组如果没有出现-1就证明为满

## 代码实现

```C++
class MyCircularQueue {
        vector<int>v;
        int size=0;
        int begin=0;
        int end=0;
public:
    MyCircularQueue(int k) {
        size=k;
       
        v.resize(k,-1);


    }
    
    bool enQueue(int value) {
            if((begin==end)&&v[end]==-1){
                    v[begin]=value;
                    return true;
            }
            if(isFull()){
                return false;
            }
            end++;
            if(end>=size){
                end%=size;
            }
            v[end]=value;
            return true;
    }
    
    bool deQueue() {
        if(isEmpty()){
           return false;
        }
        v[begin++]=-1;
        if(begin>=size){
            begin%=size;
        }
        if(begin>=end){
                end=begin;
        }


      return   true;

    }
    
    int Front() {
            if(isEmpty()){
                    return -1;
            }
        return v[begin];

    }
    
    int Rear() {
            if(isEmpty()){
                    return -1;
            }
    
        return v[end];

    }
    
    bool isEmpty() {
       for(int i=0;i<size;i++){
               if(v[i]!=-1){
                       return false;
               }
       }
       return true;

    }
    
    bool isFull() {
       for(int i=0;i<size;i++){
               if(v[i]==-1){
                       return false;
               }
       }
       return true;

    }
};

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * MyCircularQueue* obj = new MyCircularQueue(k);
 * bool param_1 = obj->enQueue(value);
 * bool param_2 = obj->deQueue();
 * int param_3 = obj->Front();
 * int param_4 = obj->Rear();
 * bool param_5 = obj->isEmpty();
 * bool param_6 = obj->isFull();
 */
```