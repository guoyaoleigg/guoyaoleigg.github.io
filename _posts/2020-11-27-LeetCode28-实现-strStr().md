---
title: LeetCode28 实现 strStr()
date: 2020-11-27 23:04:54
tags: 算法

---

LeetCode28 [实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/) 难度：简单

实现[strStr()](https://baike.baidu.com/item/strstr/811469) 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。

```javascript
示例 1:
输入: haystack = "hello", needle = "ll"
输出: 2

示例 2:
输入: haystack = "aaaaa", needle = "bba"
输出: -1
```

说明:

当 needle 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 needle 是空字符串时我们应当返回 0 。这与C语言的  [strstr()](https://baike.baidu.com/item/strstr/811469) 以及 Java的 [indexOf()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String))  定义相符。

### 1.我的解法--偷鸡

查看题目要求，发现Java中String类的indexOf方法完美匹配。省时省力，偷得一手好鸡

```java
class Solution {
        public int strStr(String haystack, String needle) {
        if(needle==null||"".equals(needle)){
            return 0;
        }
        int flag = 0;
        flag = haystack.indexOf(needle);
        return flag;
    }
}
```

```javascript
执行结果：通过
显示详情执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户
内存消耗：37 MB, 在所有 Java 提交中击败了86.76%的用户
```

### 2.我的第二种解法--截取字符串比较

解题思路：

首先判断两组特殊值，当needle为空时，返回0；当haystack等于needle时，返回0

然后处理普通情况：遍历haystack，判断截取needle长度的haystack是否和needle相等，如果相等，那么当前索引值就是返回的结果。

```java
class Solution {
        public int strStr(String haystack, String needle) {
        if(needle==null||"".equals(needle)){
            return 0;
        }
        if (haystack.equals(needle)){
            return 0;
        }
        int flag = -1;
        for(int i=0;i<haystack.length()-needle.length()+1;i++){
            if(needle.equals(haystack.substring(i,i+needle.length()))){
                flag=i;
                break;
            }
        }
        return flag;
    }
}
```

```java
执行结果：通过
显示详情
执行用时：1 ms, 在所有 Java 提交中击败了74.81%的用户
内存消耗：38.1 MB, 在所有 Java 提交中击败了42.70%的用户
```

### 3.[官方解法](https://leetcode-cn.com/problems/implement-strstr/solution/shi-xian-strstr-by-leetcode/)

官方解法用到了上述我用到的截取字符串比较，还使用了双指针法和Rabin Karp - 常数复杂度