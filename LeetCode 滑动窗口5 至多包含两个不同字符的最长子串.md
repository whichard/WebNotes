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

### [159. 至多包含两个不同字符的最长子串](https://leetcode-cn.com/problems/longest-substring-with-at-most-two-distinct-characters/)

这题窗口大小还是变化的，重点还是在何时更新左指针。这里的 count 可以用来记录当前有多少种不同的字符，只要当前进入窗口的字符使 count 不满足题目条件，我们就移动左指针让 count 满足条件。这里 result 初始化为 1，因为只要输入的字串不是空的，窗口大小不可能小于 1。

```Java
public int lengthOfLongestSubstringTwoDistinct(String s) {
    if (s.length() == 0) return 0;
    int[] hash = new int[128];
    int lo = 0, cnt = 0, res = 1;
    for (int i = 0; i < sArr.length; ++r) {
        if (++hash[s.charAt(i)] == 1) cnt++; 
        while (cnt > 2) 
            if (--hash[s.charAt(lo++)] == 0) cnt--;
        res = Math.max(res, i - lo + 1);
    }
    return res;
}
```

### 进阶 
### [340. 至多包含 K 个不同字符的最长子串](https://leetcode-cn.com/problems/longest-substring-with-at-most-k-distinct-characters/)
此题如何使用了上一题的模板，则此处直接套用即可！把2改为k即可。
```Java
public int lengthOfLongestSubstringTwoDistinct(String s, int k) {
    if (s.length() == 0) return 0;
    int[] hash = new int[128];
    int lo = 0, cnt = 0, res = 1;
    for (int i = 0; i < sArr.length; ++r) {
        if (++hash[s.charAt(i)] == 1) cnt++; 
        while (cnt > k) 
            if (--hash[s.charAt(lo++)] == 0) cnt--;
        res = Math.max(res, i - lo + 1);
    }
    return res;
}
```

