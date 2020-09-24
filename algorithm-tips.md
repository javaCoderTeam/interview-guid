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



#### 字典树

>  ```java
> static final int ALPHABET_SIZE = 256;
> static class TrieNode{
>   TrieNode[] children new TrieNode[ALPHABET_SIZE];
>   boolean isEndOfWord = false;
>   TrieNode(){
>     isEndOfWord = false;
>     for(int i = 0 ; i < ALPHABET_SIZE; i++){
>       children[i] = null;
>     }
>   }
> }
> 
> 
>  ```
>
> - 根节点不包含任何字符，处根节点外每个节点只包含一个字符；
> - 从根节点到某一个节点，路径上经过的字符拼接起来，为该节点对应的字符串；
> - 每个节点的所有子节点包含的字符都不相同；





#### 二叉树遍历

1. 前序遍历：是深度优先遍历，一般使用Stack来实现；

   ```java
   Stack<TreeNode> stack = new Stack<>();
           stack.push(root);
           while(!stack.isEmpty()){
   
             TreeNode node = stack.pop();
             res.add(node.val);
             
             if(node.right != null){
                 stack.push(node.right);
             }
             
             if(node.left != null){
                 stack.push(node.left);
             }
   
           }
   ```

   

2. 中序遍历：也是使用栈来遍历，先先所有的树的做节点遍历到最深处，然后在依次取出，放入右节点。`二叉排序树的中序遍历输出结果是有序的`

   ```java
   Stack<TreeNode> stack = new Stack<>();
           TreeNode curr = root;
           while(curr != null || !stack.isEmpty()){
               while(curr != null){
                   stack.push(curr);
                   curr = curr.left;
               }
   
               curr = stack.pop();
               res.add(curr.val);
               curr = curr.right;
           }
   ```

   

3. 后续遍历：

4. 层序遍历：广度优先遍历，一般使用队列来实现；

   ```java
           Queue<TreeNode> queue = new LinkedList<>();
           queue.add(root);
   				while(!queue.isEmpty()){
               int size = queue.size();
               List<Integer> list = new ArrayList<>();
               res.add(list);
               for(int i  = 0 ; i <size; i++){
                   TreeNode node = queue.remove();
                   list.add(node.val);
                   
                   if(node.left != null){
                       queue.add(node.left);
                   }
                   if(node.right != null){
                       queue.add(node.right);
                   }
                   
               }
           }
   ```

   

### 递归 

1. 递归的注意事项

> - 递归终止条件
> - 递归公式

2. 递归公式

   ```python
   def recursion(level,maxLevel,param2)
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
> https://leetcode-cn.com/problems/subsets/solution/hui-su-suan-fa-by-powcai-5/





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
   > ########################
   > 
   > 
   > # 树的先序遍历的遍历实现     
   > 
   > 
   > ```



### 剪枝

1. 常用于搜索算法 
2. 列子：N皇后问题

###  滑动窗口 待补充

>  其实就是一个队列,比如例题中的 abcabcbb，进入这个队列（窗口）为 abc 满足题目要求，当再进入 a，队列变成了 abca，这时候不满足要求。所以，我们要移动这个队列！
>
>  如何移动？
>
>  我们只要把队列的左边的元素移出就行了，直到满足题目要求！
>
>  一直维持这样的队列，找出队列出现最长的长度时候，求出解！
>
>  时间复杂度：O(n)O(n)
>
>  -  列子
>
>  ```java
>          int i = 1;
>          int j = 1;
>          int sum = 0;
>          //https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/solution/shi-yao-shi-hua-dong-chuang-kou-yi-ji-ru-he-yong-h/
>          List<int []> res = new ArrayList<>();
>  
>          while( i <= target/2){
>              if(sum < target){
>                  sum = sum + j;
>                  j++;
>              }else if( sum > target){
>                  sum = sum -i;
>                  i++;
>              }else {
>                  int [] arr = new int[j-i];
>                  for(int k = i; k < j; k++){
>                      // k-i 初始值等于 0；
>                      arr[k-i] = k;
>                  }
>                  res.add(arr);
>                  sum = sum - i;
>                  i++;
>              }
>          }
>  ```

#### 单调队列

> 「单调队列」的核心思路和「单调栈」类似。单调队列的 push 方法依然在队尾添加元素，但是要把前面比新元素小的元素都删掉：
>
>  [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

#### 双指针

>  [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/) 双指针
>
> 



### 二分查找

1. 有序
2. 存在上下界
3. 可以通过索引访问

```java
Left =0, right = len(array)-1;
while(left<= right){
  mid = (left+right)/2;
  if(arra[mid] == target){
    //TODO 
    break or return result;
  }else if (array[mid]<target){
    left = mid + 1;
  }else{
    right = mid -1;
  }
  
}

```



### 位运算

1. 基本运算：

- & 与运算，两个位置都为1，结果才为1；

- |或运算，两个只要一个为1，结果就为1；

- ^异或运算，两个位相同为0，相异为1；

  > 1. x^0 = x;
  > 2. x^ 1s(全是1的) = ~x;
  > 3. x^(~x) =1s;
  > 4. x^x =0;
  > 5. a^b =c    ======> a^c=b      b^c =a;

- ~ 取反运算，0变1，1变0；

- << 左移 ，各二进制位做移动若干位，高位丢弃，低位补0；

- `>>`右移，对无符号数，高位补0；有符号数，有的补符号位，有的补0； 

2.  [常用位运算操作](https://leetcode-cn.com/problems/power-of-two/solution/2de-mi-by-leetcode/)
   
   - `2的n次幂表示二进制位中只有一个1；`
   
   - X&1==1 or ==0  判断奇偶。 （x%2==1）
   
   - Y = X&(X-1)   ->等于清零最低位的1
   
     > (x - 1) 代表了将 x 最右边的 1 设置为 0，并且将较低位设置为 1。
     >
     > 再使用与运算：则 x 最右边的 1 和就会被设置为 0，因为 1 & 0 = 0。
     >
   
   - Y=X&-X => 得到最低位的1
   
     >  首先讨论为什么 x & (-x) 可以获取到二进制中最右边的 1，且其它位设置为 0。
     >
     > 在补码表示法中，-x = =¬x+1。换句话说，要计算 -x  ，则要将 xx 所有位取反再加 1。
     >
     > 在二进制表示中，¬x+1 表示将该 1 移动到 ¬x 中最右边的 0 的位置上，并将所有较低位的位设置为 0。而 ¬x 最右边的 0 的位置对应于 xx 最右边的 1 的位置。
     >
   
3.  不用加减乘除做加法运算

    > ```
    > n=a^b  非进位和：异或运算
    > c=a&b<<1  进位：与运算+左移一位
    >   
    > （和 s ）==（非进位和 n ）++（进位 c ）。即可将 s = a + b 转化为：s=a+b⇒s=n+c
    > ```

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
4. 最优子结构、无后效性（1.可以根据前一次操作推断出后一步操作，2.不需要关心前一次操作之前的计算过程）

**典型的状态定义**

> - 188 股票买卖系列中的 dp[i] [j] [k] 表示到了第i天的第j次交易中 k（持股或者不持股）的最大收益；
>
> - 322 零钱兑换中的 dp[i] 表示组成  i 额度的面值的最小硬币数；（自下而上）
>
>   >  ```java
>   >         for(int i = 0 ; i<= amount; i++){
>   >             for(int coin: coins){
>   >                 if(coin<= i){
>   >                      dp[i] = Math.min(dp[i],dp[i-coin]+1);
>   >                 }
>   >             }
>   >         }
>   >  ```
>
> - 300 最长上升子序列中 dp[i]表示第i个位置的元素的，最长上升子序列的长度；
>
> - 64 最小路径和中表示 dp[i] [j] 该位置的走过的最小的位置长度（自下而上）
>
> - 72 编辑距离中 dp[i] [j] 表示 第一个单词的前i个字符变成第二个单词的前j个字符所需要的最少的操作步数；
>
>   >  ```java
>   > if(w[i]==w2[j]){
>   > 	dp[i][j]= dp[i-1][j-1];
>   > }else{
>   >    //dp[i-1,j] 表示插入一个字符
>   >   //dp[i][j-1] 表示删除一个字符
>   >   //dp[i-1][j-1] 表示替换一个字符
>   > 	dp[i][j] = Math.min( dp[i-1,j],dp[i][j-1],dp[i-1][j-1]);
>   > }
>   >  ```
>
> - #### [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/) 的dp[i] 表示组成i的最小的完全平方数的个数。
>
>   >  ```java
>   >         for(int i = 1 ; i <= n; i++){
>   >             dp[i] = i;
>   >             for(int j = 1 ; i - j*j >= 0; j++){
>   >                 dp[i] = Math.min(dp[i],dp[i-j*j]+1);
>   >             }
>   >         }
>   >  ```
>
> 







### DP VS 回溯（递归）VS 贪心

- 回溯（递归）：会存在重复计算；
- 贪心：永远局部最优
- DP：记录局部最优子结构/多种记录值













## 常见基础算法题

**1、**在一个给定的从1到100的整型数组中，如何快速找到缺失的数字？
**2、**如何找到一个给定的整型数组中的重复数字？
**3、**在一个未排序的整型数组中，如何找到最大和最小的数字？
**4、**在一个整型数组中，如何找到一个所有成对的数字，满足它们的和等于一个给定的数字？
**5、**如果一个数组包含多个重复元素，如何找到这些重复的数字？
**6、**用 Java 实现从一个给定数组中删除重复元素？
**7、**如何利用快速排序对一个整型数组进行排序？
**8、**如何从一个数组中删除重复元素？
**9、**用 Java 实现数组反转？
**10、**如何不借助库实现从数组中删除重复元素？

# 链表

### 常见基础算法题

**1、**在一次遍历中，怎样发现单个链表的中间元素?
**2、**怎样验证给定的链表是环形的? 怎样发现这个环的起始节点?
**3、**怎样翻转链表?
**4、**不使用递归，怎样反转单个链表?
**5、**在未排序链表中，怎样移除重复的节点?
**6、**怎样找出单个链表的长度?
**7、**从单个链表的结尾处，怎样找出链表的第三个节点?
**8、**怎样使用栈计算两个链表的和?



## 常见基础算法

**1、**如何输出字符串中的重复字符？
**2、**如何判断两个字符串是否互为回文？
**3、**如何从字符串中输出第一个不重复字符？
**4、**如何使用递归实现字符串反转?
**5、**如何检查字符仅包含数字字符？
**6、**如何在字符串中找到重复字符？
**7、**如何对给定字符串中的元音及辅音进行计数？
**8、**如何计算给定字符传中特定字符出现的次数？
**9、**如何找到一个字符串的全排列？
**10、**在不使用任何库方法的情况下如何反转给定语句中的单词？
**11、**如何判断两个字符串是否互为旋转？
**12、**如何判断给定字符串是否是回文？



### 二叉树的常见算法

**1、**二叉搜索树是如何实现的？
**2、**如何在给定二叉树上实现前序遍历？
**3、**不使用递归如何按照前序遍历给定二叉树？
**4、**如何在给定二叉树上实现中序遍历？
**5、**不使用递归情况下如何使用中序遍历输出给定二叉树所有节点？
**6、**如何实现后序遍历算法？
**7、**如何不使用递归实现二叉树的后续遍历？
**8、**如何输出二叉搜索树的所有叶节点？
**9、**如何在给定二叉树中计算叶节点数目？
**10、**如何在给定数组中执行二分搜索？



# 编程杂项

**1、**冒泡排序是如何实现的?
**2、**迭代式快排算法是如何实现的？
**3、**你如何实现插入排序算法？
**4、**合并排序算法是如何实现的？
**5、**桶排序算法是如何实现的？
**6、**计数排序算法是如何实现的？
**7、**基数排序算法是如何实现的？
**8、**在不使用第三个变量的前提下如何交换两个数？
**9、**如何检查两个矩形是否重叠？
**10、**如何设计一个自动售货机？

[数据结构大梳理](https://zhuanlan.zhihu.com/p/64860368)

[动画的形式呈现解LeetCode题目的思路](https://github.com/MisterBooo/LeetCodeAnimation)















