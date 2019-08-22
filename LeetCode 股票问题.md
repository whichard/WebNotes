LeetCode有多道股票买卖相关的题目，前后存在递进的关联性，都是采用Dp进行求解。这里对他们进行一个总结。
###[121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)
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
