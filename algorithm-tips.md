# 学习技巧

- 坚持、刻意练习

- 练习缺陷 、弱点的地方 

- 不舒服、不爽、枯燥

### 计算机基础书籍 

算法导论
tcp/ip协议详解
深入理解计算机系统

# 算法的时间复杂度

1      logn      n     nlogn     n*n     2^n   n!



常见递归的时间复杂度

- 斐波那契额数列  2^n
- 二分查找 logn
- 二叉树遍历 n
- 归并排序 nlogn



# 算法思想

### 递归 

1. 递归的注意事项

> - 递归终止条件
> - 递归公式

2. 递归公式

   ```python
   def recursion(level,param1,param2)
   	# 终止条件
   	if(level>maxLevel){
   			return result;
   	}
   	
   	# 处理逻辑
   	process_data(level,data);
   	
   	# 递归调用下一层
   	self.recursion(level+1,p1)
   	
   	# 收尾工作，是否需要根据实际情况判断
   	reverse_state
   	
   ```

3. 重复的子操作，使用记录

# 回溯算法 46(全排列)  39

>  解决一个回溯问题，实际上就是一个决策树的遍历过程。你只需要思考 3 个问题：
>
> 1、路径：也就是已经做出的选择。
>
> 2、选择列表：也就是你当前可以做的选择。
>
> 3、结束条件：也就是到达决策树底层，无法再做选择的条件。
>
>  
>
> **代码方面，回溯算法的框架：**
>
> ```java
> result = []
> def backtrack(路径, 选择列表):
>     if 满足结束条件:
>         result.add(路径)
>         return
>         
> for 选择 in 选择列表:
>     做选择
>     backtrack(路径, 选择列表)
>     撤销选择
> ```
>
> 





### 分治 50

1. 分治经常使用递归算法来实现



### 贪心算法 122

1. 概念

   > 求解问题时，总是做出在当前看来最好的选择。

2. 例子：

   >  使用最少数量的人命币张数，来组成指定的数额；

3. 适用场景

   > 问题能够分解成子问题来解决，子问题的最优解通常能够地推到最终问题的最优解，这种子问题最优解称为最优子结构

4. 贪心和动态规划的算法区别

   > 贪心算法对于动态规划的算法不同在于它对每个子问题的解决方案都可以做出选择，不能回退；
   >
   > 动态规划则保存以往的运算结果，并根据以前的结果来选择，有回退的功能。



### 广度优先算法 BFS，一般使用队列  102

1. 适用于树和图以及集合，例子是：树的层序遍历。

   > ```python
   > def BFS(graph ,start,end):
   > 	
   > 	queue = [];
   > 	queue.apend([start]);
   >   # 对于树的是可以省略的
   > 	visited.add(start);
   > 	
   > 	while queue:
   > 			node = queue.pop();
   > 			visited.add(node);
   > 			
   > 			process(node)
   > 			nodes = generate_related_nodes(node);
   > 			queue.push(node);
   > 			
   > 			# other operation
   > 			
   > ```

### 深度优先算法 DFS，一般用栈。因为递归也是栈的思想，所以一般使用递归 104 111

1. 适用于树和图以及集合，例子是：树的先序遍历。

   > ```python
   > visited  = set();
   > def DFS (node,visited):
   > 		visited.add(node)
   > 		
   > 		# 处理当前节点
   > 		for next_node in node.children():
   > 				if not next_node in visited:
   > 				   dfs(nex_node,visited)
   >             
   >   ########################
   >   
   >   
   >  # 树的先序遍历的遍历实现            
   > ```



### 剪枝

1. 常用于搜索算法 
2. 列子：N皇后问题

###  滑动窗口 待补充

>  其实就是一个队列,比如例题中的 abcabcbb，进入这个队列（窗口）为 abc 满足题目要求，当再进入 a，队列变成了 abca，这时候不满足要求。所以，我们要移动这个队列！
>
> 如何移动？
>
> 我们只要把队列的左边的元素移出就行了，直到满足题目要求！
>
> 一直维持这样的队列，找出队列出现最长的长度时候，求出解！
>
> 时间复杂度：O(n)O(n)
>





### 二分查找

1. 有序
2. 存在上下界
3. 可以通过索引访问

### 位运算

1. 基本运算：

- & 与运算，两个位置都为1，结果才为1；
- |或运算，两个只要一个为1，结果就为1；
- ^异或运算，两个位相同为0，相异为1；
-  ~ 取反运算，0变1，1变0；
- << 左移 ，各二进制位做移动若干位，高位丢弃，低位补0；
- `>>`右移，对无符号数，高位补0；有符号数，有的补符号位，有的补0； 

2.  [常用位运算操作](https://leetcode-cn.com/problems/power-of-two/solution/2de-mi-by-leetcode/)
   
   - `2的n次幂表示二进制位中只有一个1；`
   
   - X&1  判断奇偶。 （x%2==1）
   
   - Y = X&(X-1)   ->等于清零最低位的1
   
     > (x - 1) 代表了将 x 最右边的 1 设置为 0，并且将较低位设置为 1。
     >
     > 再使用与运算：则 x 最右边的 1 和就会被设置为 0，因为 1 & 0 = 0。
     >
   
   - Y=X&-X => 得到最低位的1
   
     >  首先讨论为什么 x & (-x) 可以获取到二进制中最右边的 1，且其它位设置为 0。
     >
     > 在补码表示法中，-x = =¬x+1。换句话说，要计算 -x−x，则要将 xx 所有位取反再加 1。
     >
     > 在二进制表示中，¬x+1 表示将该 1 移动到 ¬x 中最右边的 0 的位置上，并将所有较低位的位设置为 0。而 ¬x 最右边的 0 的位置对应于 xx 最右边的 1 的位置。
     >
     > 

### 动态规划

```java
 public int climbStairs(int n) {
        if (n == 1) {
            return 1;
        }
        if (n == 2) {
            return 2;
        }
        return climbStairs(n-1)+climbStairs(n-2);
    }   
public int climbStairs(int n) {
        if (n == 1) {
            return 1;
        }
        int first = 1;
        int second = 2;
        for (int i = 3; i <= n; i++) {
            int third = first + second;
            first = second;
            second = third;
        }
        return second;
    }

```



1. 递归+记忆化--->递推
2. 状态的定义：opt[n]，dp[n]，fib[n]  。相当于上面的   climbStairs(int n) 表示到n台阶总走法个数 
3. 状态转移方程：opt[n] = best_of(opt[n-1],opt[n-2])
4. 最优子结构



### DP VS 回溯（递归）VS 贪心

- 回溯（递归）：会存在重复计算；
- 贪心：永远局部最优
- DP：记录局部最优子结构/多种记录值



