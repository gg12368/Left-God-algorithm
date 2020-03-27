最多做 K 个项目的最大利润
题目：costs[]：花费 ，costs[i] 表示 i 号项目的花费 profits[]：利润， profits[i] 表示 i 号项目在扣除花费之后还能挣到的钱(利润)。一次只能做一个项目，最多做 k 个项目，m 表示你初始的资金。（说明：你每做完一个项目，马上获得的收益，可以支持你去做下一个项目）求你最后获得的最大钱数。

【分析】贪心：每次总是做能够做的项目中利润最大的。
准备一个小根堆和大根堆，小根堆放着全部的项目，按谁花费（成本）最低就在头部。
1、若小根堆不为空，项目也没做完 K 个，则每次先从小根堆解锁能够做的项目，放入大根堆（大根堆按照解锁的项目中谁的利润最大放在头部）；
2、大根堆不为空，从大根堆弹出堆顶项目来做（即利润最大的项目，每次只弹出堆顶一个项目来做）；
3、把 m 加上利润，初始资金增加，再重复1)、2）步骤。
举例说明：



注意：
1、解锁项目：只要项目成本在当前资金范围以内都可以被解锁，并不是一次只能解锁一个，然后按照利润的大小放进大根堆里，然后按照利润大的先做。所谓解锁项目其实就是资金肯定大于项目成本，该项目一定可以被做的，可以直观的感受下；
2、结束条件：可能做不到k个项目就会停了，因为可能存在成本比较高的项目，大根堆中可做的项目都做完了，总资金还是无法解锁成本比较大的项目，必须要停止了。
package com.offer.foundation.class5;
 
import java.util.Comparator;
import java.util.PriorityQueue;

public class IPO {
 
    // 项目节点
    public class Node{
        private int profit;    // 项目利润
        private int cost;      // 项目成本
 
        public Node(int profit, int cost){
            this.profit = profit;
            this.cost = cost;
        }
    }
 
    /**
     * @param k ：最多做k个项目
     * @param fund ：总的资金
     * @param profits ：每个项目的利润数组
     * @param cost ：每个项目的成本数组
     * @return
     */
    public int findMaxCapital(int k, int fund, int[] profits, int[] cost){
        // 初始化每个项目节点信息
        Node[] nodes = new Node[profits.length];
        for (int i = 0; i < profits.length; i++) {
            nodes[i] = new Node(profits[i], cost[i]);
        }
        // 优先级队列是谁小谁放在前面，比较器决定谁小
        PriorityQueue<Node> minCostQ = new PriorityQueue<>(new MinCostComparator());       // 成本小顶堆
        PriorityQueue<Node> maxProfitQ = new PriorityQueue<>(new MaxProfitComparator());   // 利润大顶堆
        for (int i = 0; i < nodes.length; i++) {
            minCostQ.add(nodes[i]);   // 将所有的项目插入成本堆中
        }
        // 开始解锁项目，赚取利润
        for (int i = 0; i < k; i++) {
            // 解锁项目的前提条件：成本堆中还有项目未被解锁并且该项目的成本小于当前的总资金
            while(!minCostQ.isEmpty() && minCostQ.peek().cost <= fund){
                maxProfitQ.add(minCostQ.poll());   // 将当前成本最小的项目解锁
            }
            if(maxProfitQ.isEmpty()){
                // 如果maxProfitQ为空，则说明没有当前资金能够解锁的新项目了，之前解锁的项目也做完了，即无项目可做了
                return fund;   // 最后的总金额
            }
            fund += maxProfitQ.poll().profit;   // 做利润最大的项目
        }
        return fund;   // k个项目都做完了
    }
 
    // 成本小顶堆：成本最小的在堆顶
    public class MinCostComparator implements Comparator<Node>{
        @Override
        public int compare(Node o1, Node o2) {
            return o1.cost - o2.cost;
        }
    }
 
    // 利润大顶堆：利润最大的在堆顶
    public class MaxProfitComparator implements Comparator<Node>{
        @Override
        public int compare(Node o1, Node o2) {
            return o2.profit - o1.profit;
        }
    }
}
