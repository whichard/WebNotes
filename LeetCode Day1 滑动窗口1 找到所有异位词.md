今天是2019年8月15日，我的博客从今天开始更新LeetCode题解系列问题。

今天主要研究滑动窗口问题。

滑动窗口这类问题一般需要用到双指针来进行求解，另外一类比较特殊则是需要用到特定的数据结构，如TreeMap或PriorityQueue等，之后我会单独列举。对于一般的滑动窗口题目，大多是基于字符串和数组的，所以我们重点总结这种基于双指针的滑动窗口问题。
题目问法大致有这几种：

给两个字符串，一长一短，问其中短的是否在长的中满足一定的条件存在，例如：

- 求长的的最短子串，该子串必须涵盖短的的所有字符
- 短的的 anagram 在长的中出现的所有位置
- ...

给一个字符串或者数组，问这个字符串的子串或者子数组是否满足一定的条件，例如：

- 含有少于 k 个不同字符的最长子串
 所有字符都只出现一次的最长子串
- ...

除此之外，还有一些其他的问法，但是不变的是，这类题目脱离不开主串（主数组）和子串（子数组）的关系，**要求的时间复杂度往往是O(n)** ，空间复杂度往往是常数级的。之所以被称为滑动窗口，是因为，遍历的时候，两个指针一前一后夹着的子串（子数组）类似一个窗口，这个窗口大小和范围会随着前后指针的移动发生变化。
滑动窗口就是这类题目的重点，换句话说，窗口的移动就是重点。我们要控制前后指针的移动来控制窗口，这样的移动是有条件的，也就是要想清楚在什么情况下移动，在什么情况下保持不变，我在这里讲讲我的想法，我的思路是保证右指针每次往前移动一格，每次移动都会有新的一个元素进入窗口，这时条件可能就会发生改变，然后根据当前条件来决定左指针是否移动，以及移动多少格。

### 通用的解题格式
双指针 + Hash/数组/PriorityQueue

先扩大右边界，然后再收缩左边界

右指针每次往前移动一格，每次移动都会有新的一个元素进入窗口，这时条件可能就会发生改变，然后根据当前条件来决定左指针是否移动，以及移动多少格。 

### 今天的题目：
### [438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

思路：hash数组统计子串的字符数。遍历长串，依次将数组对应字符--，如果--过后仍>=0的，则该字符为有效字符。
窗口大小大于子串时，需要将左指针滑动，并同时将对应字符++归还。如果归还后>0，则归还字符为有效字符。
用cnt统计有效字符数的变化情况，在cnt == p.length()时，则该位置的子串满足条件。
 ```Java
   class Solution {
       public List<Integer> findAnagrams(String s, String p) {
           List<Integer> res = new ArrayList<>();
           if(s.length() < p.length()) return res;
           int[] hash = new int[128];
           for(char c : p.toCharArray()) hash[c]++;
           int lo = 0, cnt = 0;
           for(int i = 0; i < s.length(); i++) {
               if(--hash[s.charAt(i)] >= 0) cnt++;
               if(i >= p.length())
                   if(++hash[s.charAt(lo++)] > 0) cnt--;
               if(cnt == p.length()) res.add(lo);
           }
           return res;
       }
   }
   ```
