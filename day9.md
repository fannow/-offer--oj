# [字符串中的变位词](https://leetcode.cn/problems/MPnaiL/)

题目来源：[leetcode]([https://leetcode.cn/problems/MPnaiL/](https://leetcode.cn/problems/MPnaiL/))

## 问题描述



给定两个字符串 `s1` 和 `s2`，写一个函数来判断 `s2` 是否包含 `s1` 的某个变位词。

换句话说，第一个字符串的排列之一是第二个字符串的 **子串** 。

## 思路解析



**思路：滑动窗口**

题目要判断字符串中有没有存在变位词，所以就不能将分割为子串直接去判断，因为直接去判断付出代价有些大。因为字符是由26位字符组成我们只要判断出现的字符数量是不是相等就可以了。需要长度为26的两个数组。

首先构建滑动窗口：遍历s1所有字符将字符出现的次数放到数组A中 同时遍历s2前n个字符将出现次数加入到数组B中这就构建一个长度n的窗口。数值中数据就是窗口出现的字符的次数

更新数组B遍历数组s2剩余字符，将没有出现在窗口的字符次数删除，同时添加窗口出现字符的出现次数

判断A是不是等于B



## 代码实现

```C++
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        if(s1.size()>s2.size()){return false;}
        vector<int> v(26,0);
        vector<int> s(26,0);
        for(int i=0;i<s1.size();i++){
            v[s1[i]-'a']++;
            s[s2[i]-'a']++;
        }
        if(v==s){
            return true;
        }
        int n=s1.size();
        for(int i=s1.size();i<s2.size();i++){
            s[s2[i-n]-'a']--;
            s[s2[i]-'a']++;
            if(s==v){
                return true;
            }

        }
        return false;

    }
};
```

# [字符串中的所有变位词](https://leetcode.cn/problems/VabMRr/)

题目来源：[leetcode]([https://leetcode.cn/problems/VabMRr/](https://leetcode.cn/problems/VabMRr/))



## 问题描述

给定两个字符串 s 和 p，找到 s 中所有 p 的 变位词 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

变位词 指字母相同，但排列不同的字符串。

## 思路解析



**思路：滑动窗口**

题目要判断字符串中有没有存在变位词，所以就不能将分割为子串直接去判断，因为直接去判断付出代价有些大。因为字符是由26位字符组成我们只要判断出现的字符数量是不是相等就可以了。需要长度为26的两个数组。

首先构建滑动窗口：遍历s1所有字符将字符出现的次数放到数组A中 同时遍历s2前n个字符将出现次数加入到数组B中这就构建一个长度n的窗口。数值中数据就是窗口出现的字符的次数

更新数组B遍历数组s2剩余字符，将没有出现在窗口的字符次数删除，同时添加窗口出现字符的出现次数

判断A是不是等于B

## 代码实现

```C++
class Solution {
public:
    vector<int> findAnagrams(string s1, string s2) {
          int len1=s2.size();
        int len2=s1.size();
        if(len1>len2){
            return vector<int>();
        }
        vector<int> ans;
        vector<int> s(26);
        vector<int> t(26);
        //先将前len1字符装进数组
        for(int i=0;i<len1;i++){
            s[s1[i]-'a']++;
            t[s2[i]-'a']++;
        }
        if(s==t){
            ans.push_back(0);
        }
        for(int i=len1;i<len2;i++){
             ++s[s1[i]-'a'];//添加新元素
             --s[s1[i-len1]-'a'];//删除窗口之前的的元素
              if(s==t){
                  ans.push_back(i-len1+1);
             }
        }
        return ans;

        
 
    }
};
```

# [最多删除一个字符得到回文](https://leetcode.cn/problems/RQku0D/)

题目来源:[leetcode]([https://leetcode.cn/problems/RQku0D/](https://leetcode.cn/problems/RQku0D/))

## 问题说明

给定一个非空字符串 `s`，请判断如果 **最多** 从字符串中删除一个字符能否得到一个回文字符串。

## 思路解析

**思路：双指针**

首先判断原始串是不是回文串，如果是直接返回true，如果不是就判断删除字符后是不是回文串

两个指针分别指向字符串的尾部和首部 ,当两个字符不相等时就说明这一定不是一个回文串，所以判断当删除其中之一时是不是回文串就可以判断这个字符串至少删除一个字符后是不是回文串。

定义左右指针，初始时分别指向字符串的第一个字符和最后一个字符，每次判断左右指针指向的字符是否相同，如果不相同，则不是回文串；如果相同，则将左右指针都往中间移动一位，直到左右指针相遇，则字符串是回文串。

## 代码实现

```C++
class Solution {
public:
   bool isPalindrome(string&s,int begin,int end) {

    while (begin < end) {
        if (begin < end && s[end] != s[begin]) {
            return false;
        }
        end--;
        begin++;
    }
    return true;
}
bool validPalindrome(string&s) {
   int begin=0;
   int end=s.size()-1;
   if(isPalindrome(s,begin,end)){
       return true;
   }
   while(begin<end){
      if(s[begin]!=s[end]){
          //遇到不相等的字符就尝试 删除左边一个字符然后判断中间字符串是不是回文，如果不是回文字符串就说明删除一个字符是无法达到回文串
          return isPalindrome(s,begin+1,end)||isPalindrome(s,begin,end-1);
      }
      begin++;
      end--;
   }
    return true;
}
};
```

