2、动态规划版本

判断是否为无后效性：是无后效性的；
确定可变参数：i 值，sum 值，aim 值是固定的；
确定二维状态表（两个可变参数）。


状态表如上图所示，横坐标为 m 的值，纵坐标为 i 的值。从 basecase 可以看出最后一行的状态值是可以确定的，所以从最后一行往上推导，一直推导到左上角的位置处，如果为 True，则返回 True（图中空白处都为false）。

怎么通过下面一行的状态值得出上面一行的状态值呢？看递归的代码：

process(arr, i + 1, pre, aim) || process(arr, i + 1, pre + arr[i], aim)
因此：

i 行为 True 的位置，其对应 i - 1 行正上方位置也为 True；
i 行为 True 的位置处的值减去 i - 1 行对应的值，得到的在 sum 范围内的值对应的位置处为 True。
 

public class SumToAim {
   
    // 递归版本
    public static boolean isSumToAim2(int[] arr, int aim){
        if(arr == null || arr.length == 0){
            return false;
        }
 
        // 状态表：需要注意到底需要几行
        boolean[][] dp = new boolean[arr.length + 1][aim + 1];
 
        // 填好最后一行:i为横坐标，pre为纵坐标
        for(int i = arr.length, sum = 0; sum <= aim; sum++){
            if(sum == aim){
                dp[i][sum] = true;   // 目标值处设置为true
            }else{
                dp[i][sum] = false;
            }
        }
 
        // 按照递归填好状态表中的每一个位置：从下一行推导出上一行的状态值
        for(int i = arr.length - 1; i >= 0; i--){
            for(int sum = aim; sum >= 0; sum--){
                if(sum + arr[i] > aim){
                    dp[i][sum] = dp[i + 1][sum];
                }else{
                    // dp[i][sum]值为true的两种情况：正下方值为true || dp[i+1][sum+arr[i]]的值为true，有一个为ture就行
                    dp[i][sum] = dp[i + 1][sum] || dp[i + 1][sum + arr[i]];
                }
            }
        }
        return dp[0][0];
    }
}
