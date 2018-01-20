---
title: Remove Invalid Parentheses
date: 2017-10-09 15:20:16
tags: [algorithm]
---

## 前言

- 本博客作为《算法与设计》课程第 5 周的作业
- 题目来源：[LeetCode #301](https://leetcode.com/problems/remove-invalid-parentheses)
- 题目类型：DFS
- 编程语言：C++
- 运行结果：![](images/result.png)

## 问题描述

![](images/description.png)

## 问题分析

对于括号匹配的问题其实并不陌生，用 stack 遇见左括号就进栈，遇见右括号就出栈即可，或者我们可以用更简单的——用一个 count 模拟栈，遇见左括号 ++count，遇见右括号就 -–count。因为只有 ( 和 ) 我们先对 ) 进行处理，至于 (，我们只需要倒着再做一次即可。

注意：在循环当中当且仅当我们找到一个多余的 ) 时，count < 0（此处也保证了题目要求的最少量）。此时我们需要对当前的子串进行处理，即移除其中某个 )，问题是，哪一个呢？实际上，我们只需要移除任意一个即可。例子：

`
0 1 2 3 4 5 6<br/>
( ) ( ) ) ( )
`

我们找到第 4 个括号时，发现需要移除一个 )，此时我们可以移除 s[1], s[3] 或者 s[4]，但又要注意，当有一连串 ) 排列在一起时，我们移除其中任意一个得到的答案是相同的。如移除 s[3] 或者 s[4]，我们都得到 ( ) ( ) ( )，因此我们默认规定移除一连串 ) 中的第一个 )。

当进行完以上这些步骤之后，当前子串已经是合法的了，剩下的我们只需要递归调用（类似 DFS）对后面的进行处理即可。再在返回之前检测是否左右括号都进行了处理，若都进行了处理，就是正确答案。

## 源代码

```C++
class Solution {
public:
    char pa[2] = {'(' , ')'} ; //检测右括号是否多余
    char pa_re[2] = {')' , '('} ; //检测左括号是否多余

    vector<string> removeInvalidParentheses(string s) {
        vector<string> ans ;
        _remove(s , ans , 0 , 0 , pa) ;
        return ans ;
    }

    void _remove(string s , vector<string> &ans , int last_i , int last_j , char pa[]){
        for(int i = last_i , count = 0 ; i < s.size() ; ++i){
            if(s[i] == pa[0]) ++count ;
            if(s[i] == pa[1]) --count ;
            //直到我们找到有且仅有产生一个括号多余的情况
            if(count >= 0) continue ;
            //前面的任意一个括号都可以去掉，如果有多个连续，则默认去掉第一个
            for(int j = last_j ; j <= i ; ++j)
                if(s[j] == pa[1] && (j == last_j || s[j - 1] != pa[1]) ){
                    string newStr = s.substr(0,j) + s.substr(j+1) ;
                    _remove(newStr , ans , i , j , pa) ;
                }
            return ;
        }

        //倒转字符串
        string reversed_str ;
        reversed_str.clear() ;
        for(int i = s.size() - 1 ; i >= 0 ; --i)
            reversed_str.push_back(s[i]) ;

        //确认我们是否已经检测过左括号，如果已经检测过，则可以放入答案中，如果还没有检测则检测左括号
        if(pa[0] == '('){
            //说明还没检测过
            _remove(reversed_str , ans , 0 , 0 , pa_re) ;
        }
        else
            //已经检测过
            ans.push_back(reversed_str) ;
    }
};
```