# 含有重复元素集合的组合

题目来源：(leetcode)[[https://leetcode.cn/problems/4sjJUc/](https://leetcode.cn/problems/4sjJUc/)]

## 1、题目描述

给定一个可能有重复数字的整数数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用一次，解集不能包含重复的组合。 

## 2、思路解析

思路一：DFS

查看示例输出的数据是有序的所以第一件事就是将数据排序，接着调用DFS函数递归遍历出现每一种出席那的结果，当满足条件是就会加入到结果数组中，但是这有一个问题因为是含有重复数组，所以结构数据中必定会出现重复数组，所以要去重，find函数找不到就插入到数据但是因为每一次插入到数组都要判断所以导致时间复杂度加大。

思路二：回溯+递归
我们用dfs( pos,rest)表示递归的函数，其中 pos表示我们当前递归到了数组candidates 中的第pos个数，而rest表示我们还需要选择和为rest的数放入列表作为一个组合;
对于当前的第 pos个数，我们有两种方法:选或者不选。如果我们选了这个数，那么我们调用dfs(pos+ 1, rest - candidateslpos)进行递归，注意这里必须满足rest ≥ candidates[pos)。如果我们不选这个数，那么我们调用dfs(pos+ 1, rest)进行递归;
在某次递归开始前，如果rest的值为0，说明我们找到了一个和为 target的组合，将其放入答案中。每次调用递归函数前，如果我们选了那个数，就需要将其放入列表的末尾，该列表中存储了我们选的所有数。在回溯时，如果我们选了那个数，就要将其从列表的末尾删除。

## 3、代码实现



```C++
class Solution {
public:

void func(vector<vector<int>> &v,vector<int>&vm,int &&sum,
    vector<int>&nums,vector<int> num,int target,int &&index){
        if(sum>=target){
            if(sum==target){
                if(find(v.begin(),v.end(),vm)==v.end()){
                     v.push_back(vm);
                }
            }
            return;
        }
        for(int i=index;i<nums.size();i++){
                if(num[i]==0){
                    num[i]=1;
                    vm.push_back(nums[i]);
                    func(v,vm,sum+nums[i],nums,num,target,i+1);
                    num[i]=0;
                    vm.pop_back();
                }
            
        }

    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
    sort(candidates.begin(),candidates.end());
     vector<vector<int>> v;
         vector<int> vs;
          vector<int> num(candidates.size(),0);
        int inex=0;
         func(v,vs,0,candidates,num,target,0);
         return v;
    }
};
```



```C++
class Solution {
private:
    vector<pair<int, int>> freq;
    vector<vector<int>> ans;
    vector<int> sequence;

public:
    void dfs(int pos, int rest) {
        if (rest == 0) {
            ans.push_back(sequence);
            return;
        }
        if (pos == freq.size() || rest < freq[pos].first) {
            return;
        }

        dfs(pos + 1, rest);

        int most = min(rest / freq[pos].first, freq[pos].second);
        for (int i = 1; i <= most; ++i) {
            sequence.push_back(freq[pos].first);
            dfs(pos + 1, rest - i * freq[pos].first);
        }
        for (int i = 1; i <= most; ++i) {
            sequence.pop_back();
        }
    }

    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        for (int num: candidates) {
            if (freq.empty() || num != freq.back().first) {
                freq.emplace_back(num, 1);
            } else {
                ++freq.back().second;
            }
        }
        dfs(0, target);
        return ans;
    }
};


```

# [没有重复元素集合的全排列](https://leetcode.cn/problems/VvJkup/)

题目来源：[leetcode]([https://leetcode.cn/problems/VvJkup/](https://leetcode.cn/problems/VvJkup/))

## 1、问题描述：

给定一个不含重复数字的整数数组 `nums` ，返回其 **所有可能的全排列** 。可以 **按任意顺序** 返回答案。

## 2、思路解析

仔细观察可以发现，求不同全排列的问题其实是可以利用交换数组元素来完成的;
若数组长度为n，将第一个元素分别与后面每一个元素进行交换，生成n种不同的全排列;再用第二个元素与后面每一个元素进行交换，生成n -1种不同的全排列

## 3、代码实现



```C++
class Solution {
public:
    void func(vector<vector<int>> &v,vector<int> &vm,vector<int>&nums,vector<int>&num){
        if(vm.size()==nums.size()){
            v.push_back(vm);
            return;
        }
        for(int i=0;i<nums.size();i++){
            if(num[i]==0){
                num[i]=1;
                vm.push_back(nums[i]);
                func(v,vm,nums,num);
                vm.pop_back();
                num[i]=0;
            }
        }

    }
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> v;
        vector<int> vm;

        vector<int>num(nums.size(),0);
        func(v,vm,nums,num);
        return v;


    }
};
```

