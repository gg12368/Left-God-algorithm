（背包问题）从数组任意选择数字，能不能累加得到 aim
给你一个数组 arr，和一个整数 aim。如果可以任意选择 arr 中的数字，能不能累加得到 aim，返回 true 或者 false。

1、递归版本

【分析】：每个位置 i 有 要和不要 两种选择；叶节点会看自己这里的结果是不是 aim，从而向父结点返回 true 或 false，父结点比较子节点的结果，有一个为 true 就一直返回 true，否则返回 false。


如上图所示：数组 arr = {3, 2, 5} ，aim = 7：

f(0, 0)：代表0位置处状态值为0的点；

f(2, 5)：代表2位置处状态值为5的点。

只要有叶节点的值等于 aim 的值，则会返回 true。

package com.offer.foundation.class6;
 
/**
 * @author pengcheng
 * @date 2019/4/4 - 19:27
 * @content:
 */
public class SumToAim {
    public static boolean IsSumToAim(int[] arr, int aim){
        if(arr == null){
            return false;
        }
        return process(arr, 0, 0, aim);
    }
 
    // pre:是 0 ~ （i - 1）随意相加产生的结果
    // 用于判断pre+i及其后面的数字随意相加，是否能够得到aim
    public static boolean process(int[] arr, int i, int pre, int aim){
        if(i == arr.length){
            return pre == aim;
        }
        // 位置i有两种选择：要或不要，有一个等于aim，即返回true
        return process(arr, i + 1, pre, aim) || process(arr, i + 1, pre + arr[i], aim);
    }
}
