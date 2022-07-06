# [生成匹配的括号](https://leetcode.cn/problems/IDBivT/)

题目来源：[leetcode]([https://leetcode.cn/problems/IDBivT/](https://leetcode.cn/problems/IDBivT/))

## 1 问题描述

正整数 `n` 代表生成括号的对数，请设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

## 2 思路解析

思路一：暴力解法

生成n对括号因为每队括号有两个所以生成的括号的排列方式为2^2n但是符合有效组合的括号不一定为2^2n。所以我们先生成长度为n的括号接着判断是不是又有效括号组合。判断有效括号组合，只有一对指针判断是不是相对应的。生成括号序列使用递归，只需要在长度为n-1的字符序列前加一个"("或者")"，当长度达到指定长度时才会判断是不是有效括号

思路二：算法优化

上边的暴力解法优化，暴力算法是组合括号，当达到固定数量时才会判断是不是有效括号组合，这就遍历了每一种组，我们可以每加一个括号后就判度是不是有效的括号组合，这样就不用遍历每一种序列组合了

如果左括号数量不大于 n，我们可以放一个左括号。如果右括号数量小于左括号的数量，我们可以放一个右括号。

## 3 代码实现



```C++
class Solution {
    bool Isfunc(string &str){

        int balance = 0;
        for (char c : str) {
            if (c == '(') {
                ++balance;
            } else {
                --balance;
            }
            if (balance < 0) {
                return false;
            }
        }
        return balance == 0;
    }
    void func(vector<string>&v,string &str,int n){
        if(str.size()==n){
            if(Isfunc(str)){
                v.push_back(str);
            }
            return;
        }
        str+='(';
        func(v,str,n);
        str.pop_back();
        str+=')';
        func(v,str,n);
        str.pop_back();


    }
public:

    vector<string> generateParenthesis(int n) {
        vector<string>s;
        string ss="";

        func(s,ss,2*n);
        return s;
    }
};
```



```C++
class Solution {
public:

    
    void func(vector<string>&v,string &str,int r,int l,int n){
        if(str.size()==n*2){
                v.push_back(str);
            return;
        }
        if(r<n){//表明左括号没有达到预测数量
            str+='(';
            func(v,str,r+1,l,n);
            str.pop_back();
        }
        if(l<r){
            str+=')';
            func(v,str,r,l+1,n);
            str.pop_back();
        }


    }
public:

    vector<string> generateParenthesis(int n) {
        vector<string>s;
        string ss="";

        func(s,ss,0,0,n);
        return s;
    }

};
```

# [分割回文子字符串](https://leetcode.cn/problems/M99OJA/)

题目来源：[leetcode]([https://leetcode.cn/problems/M99OJA/](https://leetcode.cn/problems/M99OJA/))

## 1 问题描述

给定一个字符串 `s` ，请将 `s` 分割成一些子串，使每个子串都是 **回文串** ，返回 s 所有可能的分割方案。

**回文串** 是正着读和反着读都一样的字符串。

# 2 思路解析

思路：DFS

以字符串google为例，处理顺序如下：

当处理到第一个字符g时，此时包括字符g在内后面一共有6个字符，面临6个选项，即可以分割出6个以字符g开头的子字符串，分别为g、go、goo、goog、googl、google；

对上面的每个子字符串做回文判断，只取回文的选项，然后进入下一层dfs。

## 3 代码实现



```C++
class Solution {
    bool Isfunc(string &s,int l,int r){
        while(l<r){
            if(s[l]!=s[r]){
                return false;
            }
            l++;
            r--;
        }
        return true;

    }
    void func(vector<vector<string>>&v,vector<string>&ss,string s,int index){
        if(index==s.size()){
            v.push_back(ss);
        }
        for(int i=index;i<s.size();i++){
            //判断是不是回文串如果是
            if(Isfunc(s,index,i)){
                ss.push_back(s.substr(index,i-index+1));
                func(v,ss,s,i+1);
                ss.pop_back();
            }
        }
        

    }
public:

    vector<vector<string>> partition(string s) {
    vector<vector<string>> v;
    vector<string> ss;
    func(v,ss,s,0);
    return v;
    }
};
```