滑动窗口这类问题一般需要用到双指针来进行求解，且大多是基于字符串和数组的，我们重点总结这种基于双指针的滑动窗口问题。另外一类比较特殊则是需要用到特定的数据结构，如TreeMap或PriorityQueue等，之后我会单独列举。
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
### 今天的题目
### [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:

输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

题解：可用int数组或者HashSet来保存出现过的字符，当出现重复字符时，将窗口lo位置右移且将对应字符从数组、Set移除。
使用Set的解法思路和操作更为简单，但是使用int数组的解法更为通用 -- 可以用于多个同类题目。这里我把两种解法都列举出来。
``` Java
//基于Set的解法，逻辑更简单
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Set<Character> set = new HashSet<>();
        int lo = 0, hi = 0, res = 0;
        while(hi < s.length()) {
            if(!set.contains(s.charAt(hi))) {
                set.add(s.charAt(hi++));
                res = Math.max(res, hi - lo);
            } else set.remove(s.charAt(lo++));
        }
        return res;
    }
}
```

```Java
//基于int数组的解法，该解法模板化，更通用
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s.length() < 2) return s.length();
        int[] hash = new int[128];
        int lo = 0, res = 0, cnt = 0;
        for(int i = 0; i < s.length(); i++) {
            if(hash[s.charAt(i)]++ == 0) cnt++;
            while(hash[s.charAt(i)] > 1) {
                if(--hash[s.charAt(lo++)] == 0) cnt--;  
            }
            res = Math.max(res, cnt);
        }
        return res;
    }
}
```
