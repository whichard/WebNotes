LeetCode有多道股票买卖相关的题目，前后存在递进的关联性，从股票只能买卖1次、可以买卖无数次、可以买卖有限次依次递进，最后是一个状态机相关题目：包含冻结期的股票买卖。都是采用Dp进行求解。这里对他们进行一个总结。

### [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)
题意：用一个数组表示股票每天的价格，数组的第i个数表示股票在第i天的价格。 如果只允许进行一次交易，也就是说只允许买一支股票并卖掉，求最大的收益。

分析：这是股票系列最基础的题目。从前向后遍历数组，记录当前出现过的最低价格，作为买入价格，并计算以当天价格出售的收益，作为可能的最大收益，整个遍历过程中，出现过的最大收益就是所求。

代码：O(n)时间，O(1)空间。
```Java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length == 0) return 0;
        int min = prices[0], max = 0;
        for(int i : prices) {
            min = Math.min(min, i);
            max = Math.max(max, i - min);
        }
        return max;
    }
}
```

### [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

题目：用一个数组表示股票每天的价格，数组的第i个数表示股票在第i天的价格。交易次数不限，但一次只能交易一支股票，也就是说手上最多只能持有一支股票，求最大收益。

分析：贪心法。从前向后遍历数组，只要当天的价格高于前一天的价格，就算入收益。

代码：时间O(n)，空间O(1)。

```Java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length < 2) return 0;
        int res = 0;
        for(int i = 1; i < prices.length; i++)
            if(prices[i] > prices[i - 1]) res += prices[i] - prices[i - 1];
        return res;
    }
}
```
