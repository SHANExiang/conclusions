


排序算法：快速排序、归并排序、计数排序 
搜索算法：回溯、递归、剪枝技巧 
图论：最短路、最小生成树、网络流建模 
动态规划：背包问题、最长子序列、计数问题 
基础技巧：分治、倍增、二分、贪心 

时间复杂度 
时间复杂度是一个函数，它定性描述该算法的运行时间。 我们在软件开发中，时间复杂度就是用来方便开发者估算出程序运行的答题时间。
算法导论给出的解释：大 O 用来表示上界的，当用它作为算法的最坏情况运行时间的上
界，就是对任意数据输入的运行时间的上界。都知道快速排序是 O(nlogn)，但是当数据
已经有序情况下，快速排序的时间复杂度是 O(n^2) 的，所以严格从大 O 的定义来讲，
快速排序的时间复杂度应该是 O(n^2)。 
但是我们依然说快速排序是 O(nlogn)的时间复杂度，这个就是业内的一个默认规定，这
里说的 O 代表的就是一般情况，而不是严格的上界。 
 

# 1. 查找 
二分查找 
排序数组中的搜索问题，首先想到二分法解决。 
```python
class Solution: 
    def search(self, nums: List[int], target: int) -> int: 
        left, right = 0, len(nums) - 1 
        while left <= right: 
            pivot = left + (right - left) // 2 
            if nums[pivot] == target: 
                return pivot 
            if target < nums[pivot]: 
                right = pivot - 1 
            else: 
                left = pivot + 1 
        return -1 
     
    def my_sqrt(self, x): 
        left = 0 
        right = x 
        ans = -1 
        while left <= right: 
            mid = (right + left) // 2 
            if mid * mid <= x: 
                ans = mid 
                left = mid + 1 
            else: 
                right = mid - 1 
                return ans
```

查找二维数组，可以将矩阵逆时针旋转 45° ，并将其转化为图形式，发现其类似于二叉搜索树 ，即对于每个元素，其左分支元素更小、右分支元素更大。 
因此，通过从“根节点” 开始搜索，遇到比 target 大的元素就向左，反之向右，即可找到目标值target 。 


# 2. 快慢指针 
使用 “快慢指针” 思想，找出循环：“快指针” 每次走两步，“慢指针” 每次走一
步，当二者相等时，即为一个循环周期。 

# 3. 动态规划 
动态规划一般分为一维、二维、多维（使用状态压缩），对应形式为 dp(i)、dp(i)(j)、二
进制 dp(i)(j)。 
 
动态规划解析： 
状态定义： 设 dp 为一维数组 dp=[0]*n，其中 dp[i] 的值代表 斐波那契数列第 i 个数字。 
转移方程： dp[i + 1] = dp[i] + dp[i - 1] ，即对应数列定义 f(n + 1) = f(n) + f(n - 1) 
初始状态： dp[0] = 0, dp[1] = 1，即初始化前两个数字； 
返回值：dp[n]，即斐波那契数列的第 n 个数字。 
 
滚动数组
a, b = b, a+b    ---使用滚动数组代替创建 dp 数组，降低空间复杂度；
 

# 4. 排序 
## 快速排序 
快速排序算法有两个核心点，分别为 “哨兵划分” 和 “递归” 。 
哨兵划分操作：  
以数组某个元素（一般选取首元素）为基准数 ，将所有小于基准数的元素移动至其左边，大于基准数的元素移动至其右边。 
递归： 
对左子数组和右子数组递归执行哨兵划分，直至子数组长度为 1 时终止递归，即可完成对整个数组的排序。 


快速排序使用分治法（Divide and conquer）策略来把一个序列（list）分为较小和较大的2 个子序列，然后递归地排序两个子序列。 

步骤为： 
1. 挑选基准值：从数列中挑出一个元素，称为"基准"（pivot）; 
2. 分割：重新排序数列，所有比基准值小的元素摆放在基准前面，所有比基准值大的元
素摆在基准后面（与基准值相等的数可以到任何一边）。在这个分割结束之后，对基准
值的排序就已经完成; 
3. 递归排序子序列：递归地将小于基准值元素的子序列和大于基准值元素的子序列排序。 
递归到最底部的判断条件是数列的大小是零或一，此时该数列显然已经有序。 
def quick_sort(lists,i,j): 
    if i >= j: 
        return lists 
    pivot = lists[i] 
    low = i 
    high = j 
    while i < j: 
        while i < j and lists[j] >= pivot:   
            j -= 1  # 如果 i 与 j 未重合，j(右边)指向的元素大于等于基准元素，则 j
向左移动 
        lists[i]=lists[j]  # 到此位置时 j 指向一个比基准元素小的元素,将 j 指向的元素
放到 i 的位置上,此时 j 指向的位置空着,接下来移动 i 找到符合条件的元素放在此处; 
        while i < j and lists[i] <=pivot: 
            i += 1  # 如果 i 与 j 未重合，i 指向的元素比基准元素小，则 i 向右移动 
        lists[j]=lists[i]  # 此时 i 指向一个比基准元素大的元素,将 i 指向的元素放到 j
空着的位置上,此时 i 指向的位置空着,之后进行下一次循环,将 j 找到符合条件的元素填
到此处 
    lists[j] = pivot  # 退出循环后，i 与 j 重合，此时所指位置为基准元素的正确位置,左
边的元素都比基准元素小,右边的元素都比基准元素大 
    quick_sort(lists,low,i-1) # 对基准元素左边的子序列进行快速排序 
    quick_sort(lists,i+1,high) # 对基准元素右边的子序列进行快速排序 
    return lists 
 
 
## 归并排序 
    def merge(self, nums, left, mid, right): 
        p, q = left, mid + 1 
        temp = [0] * len(nums) 
        for i in range(left, right+1): 
            temp[i] = nums[i] 
        for j in range(left, right+1): 
            if p > mid:  # 当左半边用尽时，取右半边的元素 
                nums[j] = temp[q] 
                q += 1 
            elif q > right: # 当右半边用尽时，取左半边的元素 
                nums[j] = temp[p] 
                p += 1 
            elif temp[p] < temp[q]: # 当左半边小于右半边时，取左半边的元素 
                nums[j] = temp[p] 
                p += 1 
            else: # 当右半边小于左半边时，取右半边的元素 
                nums[j] = temp[q] 
                q += 1 
 
    def merge_sort(self, nums, left, right): 
        if left >= right: 
            return 
        mid = (left + right) >> 1 
        self.merge_sort(nums, left, mid) 
        self.merge_sort(nums, mid+1, right) 
        self.merge(nums, left, mid, right) 
 
    def main(self, nums): 
        left, right = 0, len(nums) - 1 
        self.merge_sort(nums, left, right) 
        return nums 
排序算法哪些是稳定的，为什么直接插入排序是稳定的，各种排序算法的时间复杂度和
空间复杂度？ 什么时候快速排序的时间复杂度最高。讲一下什么情况该使用什么排序
算法？堆排序算法如何实现？ 
 
影响排序的因素有很多，平均时间复杂度低的算法并不一定就是最优的。相反，
有时平均时 间复杂度高的算法可能更适合某些特殊情况。同时，选择算法时还
得考虑它的可读性，以利 于软件的维护。一般而言，需要考虑的因素有以下四
点： 
a)待排序的记录数目 n 的大小； 
b)记录本身数据量的大小，也就是记录中除关键字外的其他信息量的大小； 
c)关键字的结构及其分布情况； 
d)对排序稳定性的要求。 
1）当 n 较大，则应采用时间复杂度为 O(nlog2n)O(nlog2n) 的排序方法：快速排
序、堆排序或归并排序。 
快速排序：是目前基于比较的内部排序中被认为是最好的方法，当待排序的关键
字是随机分 布时，快速排序的平均时间最短； 
堆排序 ：堆排序所需的辅助空间少于快速排序，并且不会出现快速排序可能出
现的最坏情况，也就是排序效率稳定。 
归并排序：它有一定数量的数据移动，所以我们可能过与插入排序组合，先获得
一定长度的 序列，然后再合并，在效率上将有所提高。 
2）当 n 较大，内存空间允许，且要求稳定性 => 归并排序 
3） 当 n 较小，可采用直接插入或直接选择排序。 
4） 一般不使用或不直接使用传统的冒泡排序。 
5） 基数排序 
它是一种稳定的排序算法，但有一定的局限性： 
a) 关键字可分解。 
b) 记录的关键字位数较少，如果密集更好 
c) 如果是数字时，最好是无符号的，否则将增加相应的映射复杂度，可先将其正
负分开 排序。 
 
 
 
# 5. 位运算 
1) 使用 n & 1 得到二进制末尾是否为 1；把 n 右移 1 位 
2) n&(n - 1) ，这个代码可以把 n 的二进制中，最后一个出现的 1 改写成 0 
判断奇偶 
(x & 1) == 1 ---等价---> (x % 2 == 1) 
(x & 1) == 0 ---等价---> (x % 2 == 0) 
x / 2 ---等价---> x >> 1 
x &= (x - 1) ------> 把 x 最低位的二进制 1 给去掉 
x & -x -----> 得到最低位的 1 
x & ~x -----> 0 
 
指定位置的位运算 
将 X 最右边的 n 位清零：x & (~0 << n) 
获取 x 的第 n 位值：(x >> n) & 1 
获取 x 的第 n 位的幂值：x & (1 << n) 
仅将第 n 位置为 1：x | (1 << n) 
仅将第 n 位置为 0：x & (~(1 << n)) 
将 x 最高位至第 n 位（含）清零：x & ((1 << n) - 1) 
将第 n 位至第 0 位（含）清零：x & (~((1 << (n + 1)) - 1)) 
 
 
 
设二进制数字 n，则有： 
若 n & 1 = 0，则 n 二进制最右一位为 0； 
若 n & 1 = 1，则 n 二进制最右一位为 1。 

 
## <a name='-1'></a>不用+号，两个数相加 
def test_add_without_plus_operator(a, b): 
    while b != 0: 
        data = a & b 
        a = a ^ b 
        b = data << 1 
    return a 
 
def test_add_without_plus_operator(a, b): 
    x = 0xffffffff 
    a, b = a & x, b & x 
    while b != 0: 
        a, b = (a ^ b), (a & b) << 1 & x 
    return a if a <= 0x7fffffff else ~(a ^ x) 
 
 
 
&----按位与操作，只有 1&1 为 1，其它情况为 0； 
~----逐位取反； 
^----异或，相同为 0，相异为 1；任何数和 0 做异或运算，结果仍然是原来的数，任何数
和其自身做异或运算，结果是 0；异或运算满足交换律 a^b^a = a^a^b = b 所以数组经过
异或运算，单独的值就剩下了； 
<<----左移操作，各二进制位全部左移若干位，高位丢弃，低位补 0，2 的幂相关； 
>>----右移操作，各二进制位全部右移若干位，2 的幂相关； 
 
 
# 快速幂 
比如要求 x^11,正常的乘积需要循环乘 11 次，时间复杂度为 O(n) 
快速幂的思想就是将指数 11 可以转成二进制数 1011，则原来的式子可以转化成 
x^11 = x^(2^3) + 2^1 + 2^0，此时只运算了 3 次乘积，时间复杂度降至 O(logn) 
 
 
 
 
 
# 6. 前缀和 
前缀和其实我们很早之前就了解过的，我们求数列的和时，Sn = a1+a2+a3+...an; 此时 Sn
就是数列的前 n 项和。例 S5 = a1 + a2 + a3 + a4 + a5; S2 = a1 + a2。所以我们完全可以
通过 S5-S2 得到 a3+a4+a5 的值，这个过程就和我们做题用到的前缀和思想类似。我们
的前缀和数组里保存的就是前 n 项的和。 
 for (int i = 0; i < nums.length; i++) { 
      presum[i+1] = nums[i] + presum[i]; 
 } 
 
 
# 7. 滑动窗口 
和为 s 的连续正数序列 
设连续正整数序列的左边界 i 和右边界 j，则可构建滑动窗口从左向右滑动。循环中，每
轮判断滑动窗口内元素和与目标值 target 的大小关系，若相等则记录结果，若大于 target
则移动左边界 i（以减小窗口内的元素和），若小于 target 则移动右边界 j（以增大窗口
内的元素和）。 
算法流程： 
初始化： 左边界 i = 1，右边界 j = 2 ，元素和 s = 3 ，结果列表 res ； 
循环： 当 i≥j 时跳出； 
当 s > target 时： 向右移动左边界 i = i + 1 ，并更新元素和 s ； 
当 s < target 时： 向右移动右边界 j = j + 1 ，并更新元素和 s ； 
当 s = target 时： 记录连续整数序列，并向右移动左边界 i = i + 1 ； 
返回值： 返回结果列表 res； 
 
class Solution: 
    def findContinuousSequence(self, target: int) -> List[List[int]]: 
        i, j, s, res = 1, 2, 3, [] 
        while i < j: 
            if s == target: 
                res.append(list(range(i, j + 1))) 
            if s >= target: 
                s -= i 
                i += 1 
            else: 
                j += 1 
                s += j 
        return res 
 
# 8. 回溯 
回溯法（back tracking）（探索与回溯法）是一种选优搜索法，又称为试探法，按选优条
件向前搜索，以达到目标。但当探索到某一步时，发现原先选择并不优或达不到目标，
就退回一步重新选择，这种走不通就退回再走的技术为回溯法，而满足回溯条件的某个
状态的点称为“回溯点”。 
 
 
回溯算法的框架 
result = [] 
def backtrack(路径, 选择列表): 
    if 满足结束条件: 
        result.add(路径) 
        return 
 
    for 选择 in 选择列表: 
        做选择 
        backtrack(路径, 选择列表) 
        撤销选择 
         
 
## 全排列 
class Solution: 
    def permute(self, nums: List[int]) -> List[List[int]]: 
        ''' 
        如果 nums = [1, 2, 3] 
        其全排列有: [1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1] 
        该题适合用 DFS 之回溯法求解从树的根节点[]开始按照深度逐个遍历 
        回溯法算需要时刻关注两大变量： 
            (1) 递归的终止条件(回溯的本质是递归) 
            (1) 回溯过程中某时间点的当前状态 
         
        下面首先对必要变量进行初始化 
        depth: 当前所在位置所处树的深度 
        path: 记录当前遍历过的数字序列, i.e. [1], [1, 2], [1, 2, 3] 
        res: 最后输出的包含所有的全序列的数组 
        ls_used: 由于全排列同一个数字不可出现 2 次, 所以需创建一个额外数组 
                 跟踪每一个数字的使用情况, 其初始为全 False  
        ''' 
        depth, path, res = 0, [], [] 
        ls_used = [False for _ in nums] 
 
        # 初始化所有必要变量后开始从树的根节点([])进行深度遍历 
        self.dfs(nums, depth, ls_used, path, res) 
        return res 
 
    def dfs(self, nums, depth, ls_used, path, res): 
        # 递归终止条件: 达到树的尾部, 则将 path 中存储的数字加到 res 中 
        if depth == len(nums): 
            # 这里用 path[:]才能取得 path 里面存储的值，否则 path 是空值 
            res.append(path[:]) 
            return 
         
        # 在当前节点挨个尝试所有没有被探索过的数字 
        for (i, used) in enumerate(ls_used): 
            # 跳过已经在 path 中出现的数字 
            if used: 
                continue 
            # 如果该数字没有被使用, 则添加到 path 中, 并将数字状态改为 True 表
示其已经被遍历 
            path.append(nums[i]) 
            ls_used[i] = True 
 
            # 递归: 往下一层进一步探索 
            self.dfs(nums, depth + 1, ls_used, path, res) 
 
            # 回溯到原来位置, 把 path 中最后新加入的弹出, 之前使用过数字现在变
成未使用 
            path.pop() 
            ls_used[i] = False 
 
# 9. 深度优先搜索 DFS 
DFS 通过递归，先朝一个方向搜到底，再回溯至上个节点，沿另一个方向搜索，以此类推。 


# 10. 剪枝 
在搜索中，遇到 这条路不可能和目标字符串匹配成功 的情况（例如：此矩阵元素
和目标字符不同、此元素已被访问），则应立即返回，称之为 可行性剪枝 。 

# 11. 广度优先搜索 BFS 
BFS 通常借助队列的先入先出特性来实现。 
Python 中使用 collections 中的双端队列 deque()，其 popleft()方法可达到 O(1)时间复杂
度； 
 
## 从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行 
class Solution: 
    def levelOrder(self, root: TreeNode) -> List[List[int]]: 
        if not root: 
            return [] 
        import collections 
        queue, res = collections.deque(), [] 
        queue.append(root) 
        while queue: 
            tmp = [] 
            for _ in range(len(queue)): 
                node = queue.popleft() 
                tmp.append(node.val) 
                if node.left: 
                    queue.append(node.left) 
                if node.right: 
                    queue.append(node.right) 
            res.append(tmp) 
        return res 
 
    
BFS 算法组成的 3 元素：队列，入队出队的节点，已访问的集合。 
队列：先入先出的容器； 
节点：最好写成单独的类，比如本例写成 (value,step) 元组。也可写成 (value,visited)，
看自己喜好和题目； 
已访问集合：为了避免队列中插入重复的值 
BFS 算法组成的套路：# 完全平方数为例 
初始化三元素： 
Node = node(n) queue = [Node] visited = set([Node.value]) 
操作队列 —— 弹出队首节点： 
vertex = queue.pop(0) 
操作弹出的节点 —— 根据业务生成子节点（一个或多个）： 
[node(vertex.value - n*n, Node.step+1) for n in range(1,int(vertex.value**.5)+1)] 
判断这些节点 —— 符合业务条件，则 return，不符合业务条件，且不在已访问集合，
则追加到队尾，并加入已访问集合： 
if i==0:                    
    return new_vertex.step        
elif i not in visited: 
    queue.append(new_vertex) 
    visited.add(i)``` 
若以上遍历完成仍未 return，下面操作返回未找到代码： 
 
# 12. 分治算法 
分治算法的思想，先固定高位，向低位递归 

# 13. 贪心 
尽可能到大最远位置； 
 
# 14. 把数组视为哈希表 
哈希表是一个可以支持快速查找的数据结构：给定一个元素，我们可以在 O(1)的时间查找该元素是否在哈希表中。因此，我们可以考虑将给定的数组设计成哈希表的「替代产品」。 

有限状态自动机 

# 15. 埃拉托斯特尼 埃氏筛选法 
## 计算某个数之前的质数的个数； 
如果 x 是质数，那么大于 x 的 x 的倍数 2x,3x,… 一定不是质数，因此我们可以从这里入手。 
设 isPrime[i] 表示数 i 是不是质数，如果是质数则为 1，否则为 0。从小到大遍历每个
数，如果这个数为质数，则将其所有的倍数都标记为合数（除了该质数本身），即 0，
这样在运行结束的时候我们即能知道质数的个数。 
 
    def countPrimes2(self, n: int) -> int: 
        isNumPrimes = [True] * n  # 将所有数，展开所有数 标记质数真 
        count = 0  # 质数计数器 因为 1 不是质数 所以 0 
        # 遍历 2，n 数，判断是否是质数，从 2 开始对应-质数 3 [1,2,3]  1 不算质数 
        for i in range(2, n): 
            if isNumPrimes[i]: 
                count += 1 
                # 使用埃拉托斯特尼 筛选法进行过滤 将合数去除 
                for j in range(i * i, n, i):  # 遍历 i*i  2 倍 i 值 开始，结束 n, 步数 i 
(倍数递增) 
                    isNumPrimes[j] = False  # 把合数置为 False 
        return count 

## 最大公约数和最小公倍数 
lcm，取到两个数中的最大的数，然后循环加 1，直到两个数都能整除 
def get_leaset_common_multiple(num1, num2): 
    if num1 > num2: 
        z = num1 
    else: 
        z = num2 
    while True: 
        if (z % num1 == 0) and (z % num2 == 0): 
            lcm = z 
            break 
        z += 1 
    return lcm 
 
gcd，随便取一个数，得到其中值，从大到小开始遍历，直到两个数都能整除 
def get_greatest_common_divisor(num1, num2): 
    gcd = 1 
    if num1 % num2 == 0: 
        return num2 
 
    for x in range(int(num1 / 2), 0, -1): 
        if num1 % x == 0 and num2 % x == 0: 
            gcd = x 
            break 
    return gcd 
 
 

## 从三个不同数组中选取 3 个元素等于一个目标值 
import itertools 
from functools import partial 
 
 
def check_sum_array(T, *nums): 
    if sum(x for x in nums) == T: 
        return True, nums 
    else: 
        return False, nums 
 
def test_sum_three_element_from_three_array_equal_target(): 
    x = [10, 20, 20, 20] 
    y = [10, 20, 30, 40] 
    z = [10, 30, 40, 20] 
    T = 70 
    pro = itertools.product(x, y, z) # 产生所有组合 
    func = partial(check_sum_array, T) # 先将 target value 摘出来 
    sums = list(itertools.starmap(func, pro)) 
    result = set() 
    for r in sums: 
        if r[0] == True and r[1] not in result: 
            result.add(r[1]) 
            print(result) 

## 从一个不同数字的集合中组成多个不同的组合 
nums = [2, 3, 4, 5] 
 
 
def test_create_permutations(nums): 
    result_perms = [[]] 
    for n in nums: 
        new_perms = [] 
        for perm in result_perms: 
            for i in range(len(perm) + 1): 
                new_perms.append(perm[:i] + [n] + perm[i:]) 
                result_perms = new_perms 
    return result_perms 
 
 
print(test_create_permutations(nums)) 
 
 
## 从一个数字（1 到 9）字符串中获取所有可能的两个数字字母组合。 
```python
string_maps = { 
"1": "abc", 
"2": "def", 
"3": "ghi", 
"4": "jkl", 
"5": "mno", 
"6": "pqrs", 
"7": "tuv", 
"8": "wxy", 
"9": "z" 
} 
 
 
def get_letter_combinations1(digit_str): 
    result = [] 
    a = digit_str[0] 
    b = digit_str[1] 
    if a in string_maps and b in string_maps: 
        a_map = string_maps.get(a) 
        b_map = string_maps.get(b) 
        import itertools 
        for x in itertools.product(a_map, b_map): 
            result.append(x[0] + x[1]) 
    return result 
 
 
def get_letter_combinations2(digit_str): 
    result = [""] 
    for x in digit_str: 
        temp = [] 
        for s in result: 
            for char in string_maps[x]: 
                temp.append(s + char) 
        result = temp 
    return result 
 
# print(get_letter_combinations1("34")) 
# print(get_letter_combinations2('56')) 
# ['gj', 'gk', 'gl', 'hj', 'hk', 'hl', 'ij', 'ik', 'il'] 
# ['mp', 'mq', 'mr', 'ms', 'np', 'nq', 'nr', 'ns', 'op', 'oq', 'or', 'os']
```


## 求“数组中出现次数超过一半的数字”
本题常见的三种解法： 
哈希表统计法： 遍历数组 nums ，用 HashMap 统计各数字的数量，即可找出众数。 此方法时间和空间复杂度均为 O(N)O(N) 。
数组排序法： 将数组 nums 排序，数组中点的元素一定为众数。 
摩尔投票法： 核心理念为票数正负抵消。此方法时间和空间复杂度分别为 O(N)和 O(1)，为本题的最佳解法。 
 
 
## 解决回文问题 
1. 递归； 
2. 栈； 
3. 双指针 
 
有两种常用的列表实现，分别为数组列表和链表。如果我们想在列表中存储值，它们
是如何实现的呢？ 
数组列表底层是使用数组存储值，我们可以通过索引在 O(1)的时间访问列表任何位置的
值，这是由基于内存寻址的方式。 
链表存储的是称为节点的对象，每个节点保存一个值和指向下一个节点的指针。访问某
个特定索引的节点需要 O(n)的时间，因为要通过指针获取到下一个位置的节点。 
确定数组列表是否回文很简单，可以使用双指针法来比较两端的元素，并向中间移动。
一个指针从起点向中间移动，另一个指针从终点向中间移动。这需要 O(n)的时间，因为
访问每个元素时间是 O(1)，而有 n 个元素要访问。 
 
然而同样的方法在链表上操作并不简单，因为不论是正向访问还是反向访问都不是
O(1)。而将链表的值复制到数组列表中是 O(n)，因此最简单的方法就是将链表的值复制
到数组列表中，再使用双指针法判断。 
 
 
数据结构 
数组与链表：单/双向链表、跳舞链 
栈与队列  
树与图：最近公共祖先、并查集 
哈希表 
堆：大/小根堆、可并堆 
字符串：字典树、后缀树 
 
 
数组与链表是数据存储方式的概念，数组在连续的空间中存储数据，而链表可以在非连
续的空间中存储数据； 
队列和堆栈是描述数据存取方式的概念，队列是先进先出，而堆栈是后进先出；队列和
堆栈可以用数组来实现，也可以用链表实现。 
1. 树 tree 
树是一种非线性数据结构，根据子节点数量可分为 「二叉树」 和 「多叉树」，最顶层
的节点称为「根节点 root」。以二叉树为例，每个节点包含三个成员变量：「值 val」、
「左子节点 left」、「右子节点 right」  
 
class TreeNode: 
    def __init__(self, x): 
        self.val = x      # 节点值 
        self.left = None  # 左子节点 
        self.right = None # 右子节点 
 
         
         
树的遍历方式总体分为两类：深度优先搜索（DFS）、广度优先搜索（BFS）； 
常见的 DFS ： 先序遍历、中序遍历、后序遍历； 
常见的 BFS ： 层序遍历（即按层遍历）。 
 
## 叉树前序遍历的顺序为 
先遍历根节点； 
随后递归地遍历左子树； 
最后递归地遍历右子树。 
 

 
## 二叉树中序遍历的顺序为 
1. ****先递归地遍历左子树--->随后遍历根节点--->最后递归地遍历右子树。 
2. ****不断往左子树方向走，每走一次就将当前节点保存到栈中,当前节点为空，说明左
边走到头了，从栈中弹出节点并保存,然后转向右边节点，继续上面整个过程; 
 
 
## 二叉搜索树 Binary Search Tree，简称 BST 
二叉搜索树保证了左子树的节点的值均小于根节点的值，根节点的值均小于右子树的值 
BST 遍历框架： 
void BST(TreeNode root, int target) { 
    if (root.val == target) 
        // 找到目标，做点什么 
    if (root.val < target)  
        BST(root.right, target); 
    if (root.val > target) 
        BST(root.left, target); 
} 
 
 
 
## 高度平衡二叉树 
一个二叉树每个节点的左右两个子树的高度差的绝对值不超过 1； 
 
 
## 完全二叉树定义 
设二叉树深度为 k，若二叉树除第 k 层外的其它各层（第 1 至 k-1 层）的节点达到最大
个数，且处于第 k 层的节点都连续集中在最左边，则称此二叉树为完全二叉树。 
 
 
 
## 二叉树的深度 
1. 左子树与右子树的深度最大值+1,递归实现  # 后序遍历（DFS） 
2. 每遍历一层，则计数器+1，直到遍历完成，则可得到树的深度。  # 层序遍历（BFS） 
 

## 二叉树算法的设计的总路线：明确一个节点要做的事情，然后剩下的事抛给框架。 
void traverse(TreeNode root) { 
    // root 需要做什么？在这做。 
    // 其他的不用 root 操心，抛给框架 
    traverse(root.left); 
    traverse(root.right); 
} 
 
 
## 镜像二叉树--两种方式 
1. 通过构造新 node = TreeNode()对象，root.right-->node.left,root.left-->node.right; 
2. 算法流程： 
特例处理：当 root 为空时，直接返回 null ； 
初始化： 栈（或队列），并加入根节点 root。 
循环交换： 当栈 stack 为空时跳出； 
出栈： 记为 node； 
添加子节点： 将 node 左和右子节点入栈； 
交换： 交换 node 的左 / 右子节点。 
返回值： 返回根节点 root。 
 
 
## 对称二叉树 
于树中任意两个对称节点 L 和 R ，一定有： 
L.val = R.val ：即此两对称节点值相等。 
L.left.val = R.right.val ：即 L 的左子节点和 R 的右子节点对称； 
L.right.val = R.left.val ：即 L 的右子节点和 RR 的左子节点对称。 
考虑从顶至底递归，判断每对左右节点是否对称，从而判断树是否为对称二叉树。 
 
 
## 祖先的定义： 若节点 pp 在节点 rootroot 的左（右）子树中，或 p = rootp=root ，
则称 rootroot 是 pp 的祖先。 
 
## 最近公共祖先的定义： 设节点 rootroot 为节点 p, qp,q 的某公共祖先，若其左子节
点 root.leftroot.left 和右子节点 root.rightroot.right 都不是 p,qp,q 的公共祖先，则称 
rootroot 是 “最近的公共祖先” 。 
根据以上定义，若 rootroot 是 p, qp,q 的 最近公共祖先 ，则只可能为以下情况之一： 
p 和 q 在 root 的子树中，且分列 root 的 异侧（即分别在左、右子树中）； 
p = rootp=root ，且 q 在 root 的左或右子树中； 
q = rootq=root ，且 p 在 root 的左或右子树中； 
 
 
如何进行二叉树的各种遍历的非递归算法实现？简要讲述 
从当前节点开始遍历：（当入栈时访问节点内容，则为前序遍历；出栈时访问，
则为中序遍 历） 
1. 若当前节点存在，就存入栈中，并访问左子树； 
2. 直到当前节点不存在，就出栈，并通过栈顶节点访问右子树； 
3. 不断重复 1 和 2，直到当前节点不存在且栈空。 只需要在入栈、出栈的时候，分
别进行节点访问输出，即可分别得到前序、中序的非递归遍 历代码！从当前节
点开始遍历： 
4. 若当前节点存在，就存入栈中，第一次访问，然后访问其左子树； 
5. 前节点不存在，需要回退，这里有两种情况： a) 从左子树回退，通过栈顶节点
访问其右子树（取栈顶节点用，但不出栈） b) 从右子树回退，这时需出栈，并
取出栈节点做访问输出。（需要注意的是，输出完 毕需要置当前节点为空，以
便继续回退。具体可参考代码中的 cur == null） 不断重复 12，直到当前节点不
存在且栈空。 
红黑树和平衡二叉树？ 
排序二叉树虽然可以快速检索，但在最坏的情况下：如果插入的节点集本身就是
有序的，要 么是由小到大排列，要么是由大到小排列，那么最后得到的排序二
叉树将变成链表：所有节 点只有左节点（如果插入节点集本身是大到小排列）；
或所有节点只有右节点（如果插入节 点集本身是小到大排列）。在这种情况下，
排序二叉树就变成了普通链表，其检索效率就会 很差。 
红黑树 
性质 1：节点非红即黑。 
性质 2：根节点永远是黑色的。 
性质 3：所有的叶节点都是空节点（即 null），并且是黑色的。 
性质 4：每个红色节点的两个子节点都是黑色。（从每个叶子到根的路径上不会
有两个连续的红色节点） 
性质 5：从任一节点到其子树中每个叶子节点的路径都包含相同数量的黑色节点。 
红黑树最重要的性质：从根到叶子的最长的可能路径小于等于最短的可能路径的
两倍长。 红黑树并不是真正意义上的平衡二叉树，但在实际应用中，红黑树的
统计性能要高于平衡二叉树，但极端性能略差。(对于 AVL 树，任何一个节点的
两个子树高度差不会超过 1；对于红黑树，则是不会相差两倍以上)对于给定的
黑色高度为 N 的红黑树，从根到叶子节点的最短路径长度为 N-1，最长路径长
度为 2 * (N-1)2∗(N−1)。对于红黑树，插入，删除，查找的复杂度都是 O(log N)。
任何不平衡都会在 3 次旋转之内解决。 
红黑树通过上面这种限制来保证它大致是平衡的——因为红黑树的高度不会无
限增高，这样 保证红黑树在最坏情况下都是高效的，不会出现普通排序二叉树
的情况。 
由于红黑树只是一个特殊的排序二叉树，因此对红黑树上的只读操作与普通排序
二叉树上的 只读操作完全相同，只是红黑树保持了大致平衡，因此检索性能比
排序二叉树要好很多。 
但在红黑树上进行插入操作和删除操作会导致树不再符合红黑树的特征，因此插
入操作和删 除操作都需要进行一定的维护，以保证插入节点、删除节点后的树
依然是红黑树。 
平衡二叉树的最差情形： 
由平衡二叉树的定义可知，左子树和右子树最多可以相差 1 层高度，那么多个在
同一层的子树 就可以依次以相差 1 层的方式来递减子树的高度，如下图所示是
一个拥有 4 棵子树的树的层高 最大差情形。也就是说，一颗高度为 H 的平衡二
叉树，其内部子树高度差最多为 [H / 2]。 
红黑树的最差情形： 
红黑树中红节点的父亲和孩子必须是黑节点，且从根到叶子节点经过的黑节点个
数相同，因此红黑树最小深度是路径上只有黑节点，最大深度是路径上红黑节点
相互间隔(重要)，因此最大深度 <=最小深度的两倍，最大深度是 2 * log2（n+1）
2∗log2（n+1）。 
对于 AVL 树，任何一个节点的两个子树高度差不会超过 1；对于红黑树，则是
不会相差两倍以上。红黑树的插入删除元素的效率高于平衡二叉树，而查询时间
差于平衡二叉树。红黑树的树高 可能更高。 
2. 栈 stack 
栈是一种具有「先入后出」特点的抽象数据结构，可使用数组或链表实现。 
stack = [] # Python 可将列表作为栈使用 
 
 
枚举； 
collections; 
数组--array 模块； 
堆---heapq 模块； 
排序并且插入后还是原来的顺序---bisect.insort(list, item); 
3. 队列---queue 模块 
队列是一种具有 「先入先出」 特点的抽象数据结构，可使用链表实现。 
# Python 通常使用双端队列 collections.deque 
from collections import deque 
queue = deque() 
 
 
队列 
import queue 
queue = queue.Queue() 
queue.put(value)  # 排队添加数据 
queue.get()    # 获取最前面的数据 

# 链表---linked list 
Linkedlist 
链表以节点为单位，每个元素都是一个独立对象，在内存空间的存储是非连续的。链表
的节点对象具有两个成员变量：「值 val」，「后继节点引用 next」  
## 单链表 
链表是一种通过指针串联在一起的线性结构，每一个节点是又两部分组成，一个是数据
域一个是指针域（存放指向下一个节点的指针），最后一个节点的指针域指向 null（空
指针的意思）。 
 
## 双链表 
每一个节点有两个指针域，一个指向下一个节点，一个指向上一个节点。 
双链表既可以向前查询也可以向后查询。 
 
 
##  循环链表，顾名思义，就是链表首尾相连。 
循环链表可以用来解决约瑟夫环问题。 
 
 
## 链表内存分布 
数组是在内存中是连续分布的，但是链表在内存中可不是连续分布的。 
链表是通过指针域的指针链接在内存中各个节点。 
所以链表中的节点在内存中不是连续分布的 ，而是散乱分布在内存中的某地址上，分
配机制取决于操作系统的内存管理。 
 
 
##  删除倒数第 n 个节点 
a. 构造 dummy = LinkedList(0, head)   先获得链表的长度，之后遍历进行赋值； 
b. 构造 dummy = LinkedList(0, head)  将各个 node 放入栈中，遍历 n 次进行 pop()，之
后赋值； 
c. 构造 dummy = LinkedList(0, head)  双指针，一个链表先向前 n 步，这样两个链表相
差 n,当第一个链表走到最后的时候，第二个链表就是倒数第 n 个元素； 
 
 
## 删除重复节点 
a. 指定 cur 指针指向头部 head；或者 dummy = ListNode(0, next=head) 
b. 当 cur 和 cur.next 的存在为循环结束条件，当二者有一个不存在时说明链表没有去
重复的必要了; 
c. 当 cur.val 和 cur.next.val 相等时说明需要去重，则将 cur 的下一个指针指向下一个
的下一个，这样就能达到去重复的效果; 
d. 如果不相等则 cur 移动到下一个位置继续循环 
 
## 反转链表 
class Solution: 
    def reverseList(self, head: ListNode) -> ListNode: 
        cur, pre = head, None 
        while cur: 
            tmp = cur.next # 暂存后继节点 cur.next 
            cur.next = pre # 修改 next 引用指向 
            pre = cur      # pre 暂存 cur 
            cur = tmp      # cur 访问下一节点 
        return pre 
 
class Solution: 
    def reverseList(self, head: ListNode) -> ListNode: 
        def recur(cur, pre): 
            if not cur: return pre     # 终止条件 
            res = recur(cur.next, cur) # 递归后继节点 
            cur.next = pre             # 修改节点引用指向 
            return res                 # 返回反转链表的头节点 
         
        return recur(head, None)       # 调用递归并返回 
 
     
## 复制链表 
1. ----遍历链表，每轮建立新节点 + 构建前驱节点 pre 和当前节点 node 的引用指向即
可。 
2. ----构建原链表节点和新链表对应节点的键值对映射关系，再遍历构建新链表各节点
的 next 和 random 引用指向即可； 
 
 
## 相交链表 
考虑构建两个节点指针 A , B 分别指向两链表头节点 headA , headB ，做如下操作： 
指针 A 先遍历完链表 headA ，再开始遍历链表 headB ，当走到 node 时，共走步数
为：a + (b - c) 
指针 B 先遍历完链表 headB ，再开始遍历链表 headA ，当走到 node 时，共走步数
为：b + (a - c) 
如下式所示，此时指针 A , B 重合，并有两种情况： 
a + (b - c) = b + (a - c) 
若两链表 有 公共尾部 (即 c > 0) ：指针 A , B 同时指向「第一个公共节点」node 。 
若两链表 无 公共尾部 (即 c = 0) ：指针 A , B 同时指向 null。 
 
def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode: 
        A, B = headA, headB 
        while A != B: 
            A = A.next if A else headB 
            B = B.next if B else headA 
        return A 
 
# 图 
图是一种非线性数据结构，由「节点（顶点）vertex」和「边 edge」组成，每条边连接
一对顶点。根据边的方向有无，图可分为「有向图」和「无向图」。 

# 散列表 
散列表是一种非线性数据结构，通过利用 Hash 函数将指定的「键 key」映射至对应的
「值 value」，以实现高效的元素查找。 
 
## 初始化散列表 
dic = {} 
 
## 添加 key -> value 键值对 
dic["小力"] = 10001 
dic["小特"] = 10002 
dic["小扣"] = 10003 
 
从姓名查找学号 
dic["小力"] # -> 10001 
dic["小特"] # -> 10002 
dic["小扣"] # -> 10003 
 
# 堆 
堆是一种基于「完全二叉树」的数据结构，可使用数组实现。以堆为原理的排序算法称
为「堆排序」，基于堆实现的数据结构为「优先队列」。 
堆分为「大顶堆」和「小顶堆」，大（小）顶堆：任意节点的值不大于（小于）其父节
点的值。 
 
将一个元素放入优先队列的时间复杂度为 O(logn)； 
 
通过使用「优先队列」的「压入 push()」和「弹出 pop()」操作，即可完成堆排序，实
现代码如下： 
from heapq import heappush, heappop 
 
## 初始化小顶堆 
heap = [] 
 
## 元素入堆，大的放入堆低，最小的放入堆顶 
heappush(heap, 1) 
heappush(heap, 4) 
heappush(heap, 2) 
heappush(heap, 6) 
heappush(heap, 8) 
 
## 元素出堆（从小到大） 
heappop(heap) # -> 1 
heappop(heap) # -> 2 
heappop(heap) # -> 4 
heappop(heap) # -> 6 
heappop(heap) # -> 8 
 
 
q = [(-nums[i], i) for i in range(k)] 
heapq.heapify(q) 
q[0]就是最小的元素； 


# leetcode
## <a name='-1'></a>1. 合并两个无序链表
```python
class Solution: 
   def sortList(self, head: ListNode) -> ListNode: 
       def sort_func(head, tail): 
           if not head: 
               return head 
           if head.next == tail: 
               head.next = None 
               return head 
           slow, fast = head, head 
           while fast != tail: 
               slow = slow.next 
               fast = fast.next 
               if fast != tail: 
                   fast = fast.next 
           mid = slow 
           return merge(sort_func(head, mid), sort_func(mid, tail)) 
 
       def merge(node1, node2): 
           dummy = ListNode(0) 
           temp, temp1, temp2 = dummy, node1, node2 
           while temp1 and temp2: 
               if temp1.val <= temp2.val: 
                   temp.next = temp1 
                   temp1 = temp1.next 
               else: 
                   temp.next = temp2 
                   temp2 = temp2.next 
               temp = temp.next 
           temp.next = temp1 if temp1 else temp2 
           return dummy.next 
       return sort_func(head, None)
```

## <a name='ok'></a>2. 一次遍历获取列表第二大值 ok
````python
class Solution(object): 
   def get_2th_num(self, nums): 
       max_num = float('-INF') 
       second_num = float('-INF') 
       for num in nums: 
           if num > max_num: 
               second_num = max_num 
               max_num = num 
           else: 
               if num > second_num: 
                   second_num = num 
       return second_num, max_num
````

## <a name='-1'></a>3. 快排 
a. 挑选基准值：从数列中挑出一个元素，称为"基准"（pivot）; 
b. 分割：重新排序数列，所有比基准值小的元素摆放在基准前面，所有比基准值大的
元素摆在基准后面（与基准值相等的数可以到任何一边）。在这个分割结束之后，对
基准值的排序就已经完成; 
c. 递归排序子序列：递归地将小于基准值元素的子序列和大于基准值元素的子序列排
序。 
递归到最底部的判断条件是数列的大小是零或一，此时该数列显然已经有序。 
```python
def quick_sort(lists,i,j): 
   if i >= j: 
       return lists 
   pivot = lists[i] 
   low = i 
   high = j 
   while i < j: 
       while i < j and lists[j] >= pivot:   
           j -= 1  # 如果 i 与 j 未重合，j(右边)指向的元素大于等于基准元素，则 j 向左移动 
       lists[i]=lists[j]  # 到此位置时 j 指向一个比基准元素小的元素,将 j 指向的元素放到i 的位置上,此时 j 指向的位置空着,接下来移动 i 找到符合条件的元素放在此处; 
       while i < j and lists[i] <= pivot: 
           i += 1  # 如果 i 与 j 未重合，i 指向的元素比基准元素小，则 i 向右移动 
       lists[j]=lists[i]  # 此时 i 指向一个比基准元素大的元素,将 i 指向的元素放到 j 空着的位置上,此时 i 指向的位置空着,之后进行下一次循环,将 j 找到符合条件的元素填到此处 
   lists[j] = pivot  # 退出循环后，i 与 j 重合，此时所指位置为基准元素的正确位置,左边的元素都比基准元素小,右边的元素都比基准元素大 
   quick_sort(lists,low,i-1) # 对基准元素左边的子序列进行快速排序 
   quick_sort(lists,i+1,high) # 对基准元素右边的子序列进行快速排序 
   return lists


def quick_sort(lists): 
   if not lists: 
       return lists 
   smaller = [] 
   equal = [] 
   greater = [] 
   pivot = random.choice(lists) 
   for num in lists: 
       if num < pivot: 
           smaller.append(num) 
       elif num == pivot: 
           equal.append(num) 
       else: 
           greater.append(num) 
   return quick_sort(smaller) + equal + quick_sort(greater) 

``` 
 
## <a name='-1'></a>4. 归并排序
```python
# 方法一 
def merge_sort(nums): 
    n = len(nums) 
    if n <= 1: 
        return nums 
    def merge(a, b): 
        na, nb = len(a), len(b) 
        i, j = 0, 0 
        ans = [] 
        while i < na and j < nb: 
            if a[i] <= b[j]: 
                ans.append(a[i]) 
                i += 1 
            else: 
                ans.append(a[j]) 
                j += 1 
        if i < na: 
           ans.extend(a[i:]) 
        else: 
            ans.extend(b[j:]) 
        return ans 
    mid = n // 2 
    left = merge_sort(nums[0:mid]) 
    right = merge_sort(nums[mid:]) 
    return merge(left, right) 
 
 
# 方法二 
def merge(self, nums, left, mid, right): 
   p, q = left, mid + 1 
   temp = [0] * len(nums) 
   for i in range(left, right+1): 
	   temp[i] = nums[i] 
   for j in range(left, right+1): 
	   if p > mid:  # 当左半边用尽时，取右半边的元素 
		   nums[j] = temp[q] 
		   q += 1 
	   elif q > right: # 当右半边用尽时，取左半边的元素 
		   nums[j] = temp[p] 
		   p += 1 
	   elif temp[p] < temp[q]: # 当左半边小于右半边时，取左半边的元素 
		   nums[j] = temp[p] 
		   p += 1 
	   else: # 当右半边小于左半边时，取右半边的元素 
		   nums[j] = temp[q] 
		   q += 1 
 
   def merge_sort(self, nums, left, right): 
       if left >= right: 
           return 
       mid = (left + right) >> 1 
       self.merge_sort(nums, left, mid) 
       self.merge_sort(nums, mid+1, right) 
       self.merge(nums, left, mid, right) 
 
   def main(self, nums): 
       left, right = 0, len(nums) - 1 
       self.merge_sort(nums, left, right) 
       return nums
```


## 5. 最大子列表和 ok
```python
# 贪心算法 
class Solution(object): 
   def max_subarray(self, nums): 
       ans = float('-inf') 
       cur = 0 
       for num in nums: 
           if cur >= 0: 
               cur += num 
           else: 
               cur = num 
        ans = max(cur, ans) 
       return ans 
 
# 动态规划 
class Solution(object): 
   def max_subarray(self, nums): 
       n = len(nums)
       dp = [nums[0]] + [float('-inf')] * (n - 1) 
       for i in range(1, n): 
           dp[i] = max(dp[i - 1], 0) + nums[i] 
       return max(dp)
```

6. 数组中重复的数字 ok 
在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。 
 
class Test(object): 
   def func(self, nums): 
       for index, num in enumerate(nums): 
           while index != nums[index]: 
               if nums[nums[index]] == nums[index]: 
                   return nums[index] 
               nums[nums[index]], nums[index] = nums[index], nums[nums[index]] 
       return -1 


## 7. 最小的 k 个数 ok 
```python
class Solution: 
   def getLeastNumbers(self, arr: List[int], k: int) -> List[int]: 
       n = len(arr) 
       if k == 0: 
           return [] 
       res = list() 
       for i in range(k): 
           res.append(arr[i]) 
       for j in range(k, n): 
           temp = max(res) 
           if arr[j] < temp: 
               res[res.index(temp)] = arr[j] 
       return res 
 
# 用 python 中的最小堆 
class Solution: 
   def getLeastNumbers(self, arr: List[int], k: int) -> List[int]: 
       n = len(arr) 
       if k == 0: 
           return [] 
       heap = [-x for x in arr[0:k]] 
       heapq.heapify(heap) 
       for num in arr[k:]: 
           if -num > heap[0]: 
               heapq.heapreplace(heap, -num) 
       return heap
```

## 8. 两个栈实现一个队列 ok 
```python
class MyQueue: 
 
   def __init__(self): 
       self.in_list = [] 
       self.out_list = [] 
 
   def push(self, x: int) -> None: 
       self.in_list.append(x) 
 
   def pop(self) -> int: 
       if self.out_list: 
           return self.out_list.pop() 
       else: 
           for _ in range(len(self.in_list)): 
               self.out_list.append(self.in_list.pop()) 
           return self.out_list.pop() 

```
 
## 9. 单链表相交的入口节点 ok 
```python
class Solution: 
 
    def detectCycle1(self, head: ListNode) -> ListNode: 
        seen = set() 
        cur = head 
        while cur: 
            if cur in seen: 
                return cur 
            seen.add(cur) 
            cur = cur.next 
        return None 
 
    # 从头结点出发一个指针，从相遇节点也出发一个指针，这两个指针每次只走一个节点， 那么当这两个指针相遇的时候就是 环形入口的节点。 
    def detectCycle2(self, head: ListNode) -> ListNode: 
        slow, fast = head, head 
        while fast and fast.next: 
            slow = slow.next 
            fast = fast.next.next 
            # 如果相遇 
            if slow == fast: 
                p = head 
                q = slow 
                while p != q: 
                    p = p.next 
                    q = q.next 
                return p 
        return None
```
 
设链表中环外部分的长度为 a。 slow 指针进入环后，又走了 b 的距离与 fast 相遇。此时， fast 指针已经走完了环的 n 圈，因此它走过的总距离为 a+n(b+c)+b=a+(n+1)b+nc。 
根据题意，任意时刻，fast 指针走过的距离都为 slow 指针的 2 倍。因此，我们有 a+(n+1)b+nc=2(a+b)⟹a=c+(n−1)(b+c) 
有了 a=c+(n-1)(b+c)a=c+(n−1)(b+c) 的等量关系，我们会发现：从相遇点到入环点的距离加上 n-1 圈的环长，恰好等于从链表头部到入环点的距离。 
因此，当发现 slow 与 fast 相遇时，我们再额外使用一个指针 ptr。起始，它指向链表头部；随后，它和 slow 每次向后移动一个位置。最终，它们会在入环点相遇。 


## 10. 三数之和 97 
给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。 
注意：答案中不可以包含重复的三元组。 
 
输入：nums = [-1,0,1,2,-1,-4] 
输出：[[-1,-1,2],[-1,0,1]] 
```python
class Solution: 
# 双指针法 
    def threeSum3(self, nums): 
        n = len(nums) 
        if n < 3: 
            return list() 
        res = [] 
        nums.sort() 
        for index, num in enumerate(nums): 
            left, right = index + 1, n - 1 
            while left < right: 
                if nums[left] + nums[right] == -num: 
                    temp = [num, nums[left], nums[right]] 
                    if temp not in res: 
                        res.append(temp)  
                    left += 1 
                    right -= 1 
                elif nums[left] + nums[right] + num < 0: 
                    left += 1 
                else: 
                    right -= 1 
        return res 
 
    def two_sum(self, start, nums, target): 
        dic = dict() 
        res = [] 
        for index, num in enumerate(nums): 
            if target - num not in dic: 
                dic[num] = index 
            else: 
                res.append([start + dic[target - num], start + index]) 
        return res 
 
    # 获取索引列表 
    def threeSum4(self, nums): 
        res = [] 
        for index, num in enumerate(nums): 
            idx = self.two_sum(index + 1, nums[index+1:], 0 - num) 
            for l in idx: 
                l.append(index) 
            res.extend(idx) 
        return res
```
 

11. 搜索二维矩阵 ok 
编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target 。该矩阵具有以
下特性： 
每行的元素从左到右升序排列。 
每列的元素从上到下升序排列。 
 
class Solution: 
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool: 
        m = len(matrix) 
        n = len(matrix[0]) 
        i, j = 0, n - 1 
        while i < m and j >= 0: 
            if target == matrix[i][j]: 
                return True 
            elif target > matrix[i][j]: 
                i += 1 
            else: 
                j -= 1 
        return False 
 
 
12. 从前序与中序遍历序列构造二叉树 ok 
给定两个整数数组 preorder 和 inorder ，其中 preorder 是二叉树的先序遍历， 
inorder 是同一棵树的中序遍历，请构造二叉树并返回其根节点。 
 
class Solution: 
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]: 
        if not preorder: 
            return None 
        root_val = preorder[0] 
        root_index = inorder.index(root_val) 
        left_pre = preorder[1:1+root_index] 
        right_pre = preorder[1+root_index:] 
        left_inorder = inorder[0:root_index] 
        right_inorder = inorder[root_index+1:] 
        left = self.buildTree(left_pre, left_inorder) 
        right = self.buildTree(right_pre, right_inorder) 
        root = TreeNode(root_val, left, right) 
        return root 
 
 
 
 
13. 旋转图像 
给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。 
你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另
一个矩阵来旋转图像。 
```python
class Solution: 
    def rotate(self, matrix) -> None: 
        n = len(matrix) 
        for i in range(n // 2): 
            for j in range((n + 1) // 2): 
                matrix[i][j], matrix[n - j - 1][i], matrix[n - i - 1][n - j - 1], matrix[j][n - i - 
1] = matrix[n - j - 1][i], matrix[n - i - 1][n - j - 1], matrix[j][n - i - 1], matrix[i][j] 
        print(matrix) 
```

 
14. 反转链表 ok 
给你单链表的头节点 head ，请你反转链表，并返回反转后的链表 
class Solution: 
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]: 
        pre = None 
        cur = head 
        while cur: 
            node = cur.next 
            cur.next = pre 
            pre = cur 
            cur = node 
        return pre 
 
 
## 15. K 个一组反转链表 
给你链表的头节点 head ，每 k 个节点一组进行翻转，请你返回修改后的链表。 
k 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。 
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。
```python
class Solution: 
    # 翻转一个子链表，并且返回新的头与尾 
    def reverse(self, head: ListNode, tail: ListNode): 
        prev = tail.next 
        p = head 
        while prev != tail: 
            nex = p.next 
            p.next = prev 
            prev = p 
            p = nex 
        return tail, head 
 
    def reverseKGroup(self, head: ListNode, k: int) -> ListNode: 
        hair = ListNode(0) 
        hair.next = head 
        pre = hair 
        while head: 
            tail = pre 
            # 查看剩余部分长度是否大于等于 k 
            for i in range(k): 
                tail = tail.next 
                if not tail: 
                    return hair.next 
            nex = tail.next 
            head, tail = self.reverse(head, tail) 
            # 把子链表重新接回原链表 
            pre.next = head 
            tail.next = nex 
            pre = tail 
            head = tail.next 
        return hair.next
``` 

## 16. 相交链表 ok 
给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 null 。 
```python
class Solution: 
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) ->Optional[ListNode]: 
        curA, curB = headA, headB 
        while curA != curB: 
            curA = curA.next if curA else headB 
            curB = curB.next if curB else headA 
        return curA 

```  
 
# 17. 反转链表 II 
给你单链表的头指针 head 和两个整数 left 和 right ，其中 left <= right 。请你反转从
位置 left 到位置 right 的链表节点，返回反转后的链表 。 
```python
class Solution: 
    def reverseBetween(self, head: Optional[ListNode], left: int, right: int) -> Optional[ListNode]: 
        dummy = ListNode(-1) 
        cur = temp = head 
        i = 1 
        pre = dummy 
        while i < left: 
            pre.next = cur 
            cur = cur.next 
            temp = temp.next 
            pre = pre.next 
            i += 1 
        while left <= right: 
            temp = temp.next 
            left += 1 
        tail = temp 
        while cur != tail: 
            node = cur.next 
            cur.next = temp 
            temp = cur 
            cur = node 
        pre.next = temp 
        return dummy.next 
 
class Solution: 
    def reverseBetween(self, head: ListNode, left: int, right: int) -> ListNode: 
        # 设置 dummyNode 是这一类问题的一般做法 
        dummy_node = ListNode(-1) 
        dummy_node.next = head 
        pre = dummy_node 
        for _ in range(left - 1): 
            pre = pre.next 
 
        cur = pre.next 
        for _ in range(right - left): 
            next = cur.next 
            cur.next = next.next 
            next.next = pre.next 
            pre.next = next 
        return dummy_node.next
``` 
 
 
18. 合并 K 个升序链表 
给你一个链表数组，每个链表都已经按升序排列。 
请你将所有链表合并到一个升序链表中，返回合并后的链表。 
 
class Solution: 
def mergeKLists(self, lists: List[ListNode]) -> ListNode: 
        if not lists:return  
        n = len(lists) 
        return self.merge(lists, 0, n-1) 
    def merge(self,lists, left, right): 
        if left == right: 
            return lists[left] 
        mid = left + (right - left) // 2 
        l1 = self.merge(lists, left, mid) 
        l2 = self.merge(lists, mid+1, right) 
        return self.mergeTwoLists(l1, l2) 
    def mergeTwoLists(self,l1, l2): 
        if not l1:return l2 
        if not l2:return l1 
        if l1.val < l2.val: 
            l1.next = self.mergeTwoLists(l1.next, l2) 
            return l1 
        else: 
            l2.next = self.mergeTwoLists(l1, l2.next) 
            return l2 
 
 
 
 
19. 堆排序 
堆排序（Heapsort）是指利用堆这种数据结构所设计的一种排序算法。堆积是一个近似
完全二叉树的结构，并同时满足堆积的性质：即子结点的键值或索引总是小于（或者
大于）它的父节点。堆排序可以说是一种利用堆的概念来排序的选择排序。分为两种
方法： 
大顶堆：每个节点的值都大于或等于其子节点的值，在堆排序算法中用于升序排列； 
小顶堆：每个节点的值都小于或等于其子节点的值，在堆排序算法中用于降序排列； 
堆排序的平均时间复杂度为 Ο(nlogn)。 
 
堆排序的过程分为两步： 
第一步是建堆，将一个无序的数组建立为一个堆 
第二步是排序，将堆顶元素取出，然后对剩下的元素进行堆化，反复迭代，直到所有
元素被取出为止 
 
1. 算法步骤 
创建一个堆 H[0……n-1]； 
把堆首（最大值）和堆尾互换； 
把堆的尺寸缩小 1，并调用 shift_down(0)，目的是把新的数组顶端数据调整到相应位
置； 
重复步骤 2，直到堆的尺寸为 1。 
 
 
def buildMaxHeap(arr): 
    import math 
    for i in range(math.floor(len(arr)/2),-1,-1): 
        heapify(arr,i) 
 
def heapify(arr, i): 
    left = 2*i+1 
    right = 2*i+2 
    largest = i 
    if left < arrLen and arr[left] > arr[largest]: 
        largest = left 
    if right < arrLen and arr[right] > arr[largest]: 
        largest = right 
 
    if largest != i: 
        swap(arr, i, largest) 
        heapify(arr, largest) 
 
def swap(arr, i, j): 
    arr[i], arr[j] = arr[j], arr[i] 
 
def heapSort(arr): 
    global arrLen 
    arrLen = len(arr) 
    buildMaxHeap(arr) 
    for i in range(len(arr)-1,0,-1): 
        swap(arr,0,i) 
        arrLen -=1 
        heapify(arr, 0) 
    return arr 
 
 
 
 
## 20. 双向链表 
```python
class Node: 
    def __init__(self, key, value): 
        """ 
        初始化方法 
        :param key: 
        :param value: 
        """ 
        self.key = key 
        self.value = value 
        self.prev = None 
        self.next = None 
 
class DoubleLinkedList: 
  
    def __init__(self, capacity=0xffffffff): 
        """ 
        双向链表 
        :param capacity: 链表容量 初始化为 int 的最大值 2^32-1 
        :return: 
        """ 
        self.capacity = capacity 
        self.size = 0 
        self.head = None 
        self.tail = None 
  
    def __add_head(self, node): 
        """ 
        向链表头部添加节点 
            头部节点不存在 新添加节点为头部和尾部节点 
            头部节点已存在 新添加的节点为新的头部节点 
        :param node: 要添加的节点 
        :return: 已添加的节点 
        """ 
        # 头部节点为空 
        if not self.head: 
            self.head = node 
            self.tail = node 
            self.head.next = None 
            self.tail.prev = None 
        # 头部节点不为空 
        else: 
            node.next = self.head 
            self.head.prev = node 
            self.head = node 
            self.head.prev = None 
        self.size += 1 
  
        return node 
  
    def __add_tail(self, node): 
        """ 
        向链表尾部添加节点 
            尾部节点不存在 新添加的节点为头部和尾部节点 
            尾部节点已存在 新添加的节点为新的尾部节点 
        :param node: 添加的节点 
        :return: 已添加的节点 
        """ 
        # 尾部节点为空 
        if not self.tail: 
            self.tail = node 
            self.head = node 
            self.head.next = None 
            self.tail.prev = None 
        # 尾部节点不为空 
        else: 
            node.prev = self.tail 
            self.tail.next = node 
            self.tail = node 
            self.tail.next = None 
        self.size += 1 
  
        return node 
  
    def __remove_head(self): 
        """ 
        删除头部节点 
            头部节点不存在 返回 None 
            头部节点已存在 判断链表节点数量 删除头部节点 
        :return: 头部节点 
        """ 
        # 头部节点不存在 
        if not self.head: 
            return None 
  
        # 链表至少存在两个节点 
        head = self.head 
        if head.next: 
            head.next.prev = None 
            self.head = head.next 
        # 只存在头部节点 
        else: 
            self.head = self.tail = None 
        self.size -= 1 
  
        return head 
  
    def __remove_tail(self): 
        """ 
        删除尾部节点 
            尾部节点不存在 返回 None 
            尾部节点已存在 判断链表节点数量 删除尾部节点 
        :return: 尾部节点 
        """ 
        # 尾部节点不存在 
        if not self.tail: 
            return None 
  
        # 链表至少存在两个节点 
        tail = self.tail 
        if tail.prev: 
            tail.prev.next = None 
            self.tail = tail.prev 
        # 只存在尾部节点 
        else: 
            self.head = self.tail = None 
        self.size -= 1 
  
        return tail 
  
    def __remove(self, node): 
        """ 
        删除任意节点 
            被删除的节点不存在 默认删除尾部节点 
            删除头部节点 
            删除尾部节点 
            删除其他节点 
        :param node: 被删除的节点 
        :return: 被删除的节点 
        """ 
        # 被删除的节点不存在 
        if not node: 
            node = self.tail 
  
        # 删除的是头部节点 
        if node == self.head: 
            self.__remove_head() 
        # 删除的是尾部节点 
        elif node == self.tail: 
            self.__remove_tail() 
        # 删除的既不是头部也不是尾部节点 
        else: 
            node.next.prev = node.prev 
            node.prev.next = node.next 
            self.size -= 1 
  
        return node 
  
    def pop(self): 
        """ 
        弹出头部节点 
        :return: 头部节点 
        """ 
        return self.__remove_head() 
  
    def append(self, node): 
        """ 
        添加尾部节点 
        :param node: 待追加的节点 
        :return: 尾部节点 
        """ 
        return self.__add_tail(node) 
  
    def append_front(self, node): 
        """ 
        添加头部节点 
        :param node: 待添加的节点 
        :return: 已添加的节点 
        """ 
        return self.__add_head(node) 
  
    def remove(self, node=None): 
        """ 
        删除任意节点 
        :param node: 待删除的节点 
        :return: 已删除的节点 
        """ 
        return self.__remove(node)
```  


## 21. LRU 缓存机制 
请你设计并实现一个满足 LRU (最近最少使用) 缓存约束的数据结构。 
实现 LRUCache 类： 
LRUCache(int capacity) 以正整数作为容量 capacity 初始化 LRU 缓存 
int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。 
void put(int key, int value) 如果关键字 key 已经存在，则变更其数据值 value ；如果不存在，则向缓存中插入该组 key-value 。如果插入操作导致关键字数量超过 capacity ，则应该逐出最久未使用的关键字。 
函数 get 和 put 必须以 O(1) 的平均时间复杂度运行。 
```python
class DLinkedNode: 
    def __init__(self, key=0, value=0): 
        self.key = key 
        self.value = value 
        self.prev = None 
        self.next = None 
 
 
class LRUCache: 
 
    def __init__(self, capacity: int): 
        self.cache = dict() 
        # 使用伪头部和伪尾部节点     
        self.head = DLinkedNode() 
        self.tail = DLinkedNode() 
        self.head.next = self.tail 
        self.tail.prev = self.head 
        self.capacity = capacity 
        self.size = 0 
 
    def get(self, key: int) -> int: 
        if key not in self.cache: 
            return -1 
        # 如果 key 存在，先通过哈希表定位，再移到头部 
        node = self.cache[key] 
        self.moveToHead(node) 
        return node.value 
 
    def put(self, key: int, value: int) -> None: 
        if key not in self.cache: 
            # 如果 key 不存在，创建一个新的节点 
            node = DLinkedNode(key, value) 
            # 添加进哈希表 
            self.cache[key] = node 
            # 添加至双向链表的头部 
            self.addToHead(node) 
            self.size += 1 
            if self.size > self.capacity: 
                # 如果超出容量，删除双向链表的尾部节点 
                removed = self.removeTail() 
                # 删除哈希表中对应的项 
                self.cache.pop(removed.key) 
                self.size -= 1 
        else: 
            # 如果 key 存在，先通过哈希表定位，再修改 value，并移到头部 
            node = self.cache[key] 
            node.value = value 
            self.moveToHead(node) 
 
    def addToHead(self, node): 
        node.prev = self.head 
        node.next = self.head.next 
        self.head.next.prev = node 
        self.head.next = node 
     
    def removeNode(self, node): 
        node.prev.next = node.next 
        node.next.prev = node.prev 
 
    def moveToHead(self, node): 
        self.removeNode(node) 
        self.addToHead(node) 
 
    def removeTail(self): 
        node = self.tail.prev 
        self.removeNode(node) 
        return node
```

22. 缺失的第一个正数 
给你一个未排序的整数数组 nums ，请你找出其中没有出现的最小的正整数。 
请你实现时间复杂度为 O(n) 并且只使用常数级别额外空间的解决方案。 
示例 1： 
 
输入：nums = [1,2,0] 
输出：3 
示例 2： 
 
输入：nums = [3,4,-1,1] 
输出：2 
示例 3： 
 
输入：nums = [7,8,9,11,12] 
输出：1 
 
class Solution: 
    def firstMissingPositive(self, nums: List[int]) -> int: 
        n = len(nums) 
        for i in range(n): 
            while 1 <= nums[i] <= n and nums[nums[i] - 1] != nums[i]: 
                nums[nums[i] - 1], nums[i] = nums[i], nums[nums[i] - 1] 
        for i in range(n): 
            if nums[i] != i + 1: 
                return i + 1 
        return n + 1 
 
 
23. 只出现一次的数字 ok 
给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。
找出那个只出现了一次的元素。 
说明： 
你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？ 
 
class Solution: 
    def singleNumber(self, nums: List[int]) -> int: 
        res = 0 
        for num in nums: 
            res ^= num 
        return res 
 
 
24. 全排列 
class Solution: 
    def permute(self, nums: List[int]) -> List[List[int]]: 
        ''' 
        如果 nums = [1, 2, 3] 
        其全排列有: [1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1] 
       该题适合用 DFS 之回溯法求解从树的根节点[]开始按照深度逐个遍历 
        回溯法算需要时刻关注两大变量： 
            (1) 递归的终止条件(回溯的本质是递归) 
            (1) 回溯过程中某时间点的当前状态 
         
        下面首先对必要变量进行初始化 
        depth: 当前所在位置所处树的深度 
        path: 记录当前遍历过的数字序列, i.e. [1], [1, 2], [1, 2, 3] 
        res: 最后输出的包含所有的全序列的数组 
        ls_used: 由于全排列同一个数字不可出现 2 次, 所以需创建一个额外数组跟踪
每一个数字的使用情况, 其初始为全 False  
        ''' 
        depth, path, res = 0, [], [] 
        ls_used = [False for _ in nums] 
        # 初始化所有必要变量后开始从树的根节点([])进行深度遍历 
        self.dfs(nums, depth, ls_used, path, res) 
        return res 
 
    def dfs(self, nums, depth, ls_used, path, res): 
        # 递归终止条件: 达到树的尾部, 则将 path 中存储的数字加到 res 中 
        if depth == len(nums): 
            # 这里用 path[:]才能取得 path 里面存储的值，否则 path 是空值 
            res.append(path[:]) 
            return 
         
        # 在当前节点挨个尝试所有没有被探索过的数字 
        for (i, used) in enumerate(ls_used): 
            if used: # 跳过已经在 path 中出现的数字 
                continue 
            path.append(nums[i])  # 如果该数字没有被使用, 则添加到 path 中, 并将
数字状态改为 True 表示其已经被遍历 
            ls_used[i] = True 
            self.dfs(nums, depth + 1, ls_used, path, res) # 递归: 往下一层进一步探索 
            path.pop()  # 回溯到原来位置, 把 path 中最后新加入的弹出, 之前使用过
数字现在变成未使用 
            ls_used[i] = False 
 
 
# 25. 无重复字符的最长子串 100 
给定一个字符串 s ，请你找出其中不含有重复字符的最长子串的长度。 
```python
class Solution: 
    def lengthOfLongestSubstring(self, s: str) -> int: 
        # 哈希集合，记录每个字符是否出现过 
        occ = set() 
        n = len(s) 
        # 右指针，初始值为-1，相当于我们在字符串的左边界的左侧，还没有开始移动 
        rk, ans = -1, 0 
        for i in range(n): 
            if i != 0: 
                # 左指针向右移动一格，移除一个字符 
                occ.remove(s[i - 1]) 
            while rk + 1 < n and s[rk + 1] not in occ: 
                occ.add(s[rk + 1])   # 不断地移动右指针 
                rk += 1 
            # 第 i 到 rk 个字符是一个极长的无重复字符子串 
            ans = max(ans, rk - i + 1) 
        return ans
```

# 26. 数组中的第 K 个最大元素 99 
给定整数数组 nums 和整数 k，请返回数组中第 k 个最大的元素。 
请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。 
你必须设计并实现时间复杂度为 O(n)的算法解决此问题。 
```python
class Solution: 
    def find_kth_largest(self, nums, k): 
        n = len(nums) 
        l = 0 
        r = n - 1 
        while True: 
            idx = self.partition(nums, l, r) 
            if idx == k - 1: 
                return nums[idx] 
            elif idx < k - 1: 
                l = idx + 1 
            else: 
                r = idx - 1 
 
    def partition(self, nums, l, r): 
        pivot = nums[l] 
        begin = l 
        while l < r: 
            while l < r and nums[r] <= pivot: 
                r -= 1 
            while l < r and nums[l] >= pivot: 
                l += 1 
            if l < r: 
                nums[l], nums[r] = nums[r], nums[l] 
        nums[begin], nums[l] = nums[l], nums[begin] 
        return l 

```
 
 
27. 二叉树的锯齿形层序遍历 98 ok 
给你二叉树的根节点 root，返回其节点值的锯齿形层序遍历。（即先从左往右，再从
右往左进行下一层遍历，以此类推，层与层之间交替进行）。 
 
from collections import deque 
class Solution: 
    def zigzagLevelOrder(self, root: Optional[TreeNode]) -> List[List[int]]: 
        if not root: return [] 
        dq, res, flag = deque(), [], 1 
        dq.append(root) 
        while len(dq) != 0: 
            temp= [] 
            for i in range(len(dq)): 
                node = dq.popleft() 
                temp.append(node.val) 
                if node.left: dq.append(node.left) 
                if node.right: dq.append(node.right) 
            if flag == 1: 
                res.append(temp[:]) 
                flag = 0 
            else: 
                res.append(temp[::-1]) 
                flag = 1 
        return res 
 
 
 
28. 买卖股票的最佳时机 ok 
 
class Solution: 
    ''' 
    给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价
格。 
    你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该
股票。设计一个算法来计算你所能获取的最大利润。 
    返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。 
    ''' 
    # 暴力法 
    def max_profit1(self, prices) -> int: 
        max_profit = 0 
        for i in range(0, len(prices)): 
            for j in range(i+1, len(prices)): 
                if prices[j] - prices[i] > max_profit: 
                    max_profit = prices[j] - prices[i] 
        return max_profit 
 
    # 一次遍历 
    def max_profit2(self, prices): 
        inf = int(1e9) 
        min_price = inf 
        max_price = 0 
        for price in prices: 
            max_price = max(price - min_price, max_price) 
            min_price = min(min_price, price) 
        return max_price 
 
    # 动态规划 
    def max_profit3(self, prices): 
        n = len(prices) 
        if n == 0: 
            return 0 
        dp = [0] * n 
        min_price = prices[0] 
        for i in range(1, n): 
            min_price = min(min_price, prices[i]) 
            dp[i] = max(dp[i - 1], prices[i] - min_price) 
            print(dp[i]) 
        return dp[-1] 

# 29. 二叉树的最近公共祖先 
给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。 
百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。” 

例如，给定如下二叉搜索树: root =[6,2,8,0,4,7,9,null,null,3,5] 

示例 1:
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8 
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。 
示例 2: 
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4 
输出: 2 
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。 
```python
# Definition for a binary tree node. 
class TreeNode: 
    def __init__(self, x): 
        self.val = x 
        self.left = None 
        self.right = None 
 
 
class Solution: 
    def lowestCommonAncestor(self, root, p, q): 
        while root: 
            if root.val < p.val and root.val < q.val: 
                root = root.right 
            elif root.val > p.val and root.val > q.val: 
                root = root.left 
            else: 
                break 
        return root 
 
    def lowest_common_ancestor(self, root, p, q): 
        if root.val < p.val and root.val < q.val: 
            return self.lowest_common_ancestor(root.right, p, q) 
        elif root.val > p.val and root.val > q.val: 
            return self.lowest_common_ancestor(root.left, p, q) 
        else: 
            return root
```

# 30. 字符串相加
给定两个字符串形式的非负整数 num1 和 num2 ，计算它们的和并同样以字符串形式返回。 
你不能使用任何內建的用于处理大整数的库（比如 BigInteger）， 也不能直接将输入的字符串转换为整数形式。 
```python
class Solution: 
    def addStrings(self, num1: str, num2: str) -> str: 
        res = "" 
        i, j, carry = len(num1) - 1, len(num2) - 1, 0 
        while i >= 0 or j >= 0: 
            n1 = int(num1[i]) if i >= 0 else 0 
            n2 = int(num2[j]) if j >= 0 else 0 
            tmp = n1 + n2 + carry 
            carry = tmp // 10 
            res = str(tmp % 10) + res 
            i, j = i - 1, j - 1 
        return "1" + res if carry else res
``` 

31. 接雨水 
给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨
之后能接多少雨水。 
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1] 
输出：6 
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 
个单位的雨水（蓝色部分表示雨水）。  
 
##### Solution 6 --- Monotonic stack 
class Solution: 
    def trap(self, height: List[int]) -> int: 
        n = len(height) 
        if n < 3: return 0 
        water = 0 
        stack = [] 
        current_idx = 0 
         
        while current_idx < n: 
            # If stack is not empty and current height bigger than stack top's height 
            while stack and height[current_idx]>height[stack[-1]]: 
                h = height[stack.pop()] 
                if not stack: 
                    break 
                # Distance between wall 
                distance = current_idx - stack[-1] -1 
                min_height = min(height[stack[-1]], height[current_idx]) 
                water += distance*(min_height-h) 
             
            stack.append(current_idx) 
            current_idx += 1 
         
        return water 
 
# ##### Solution 5 --- DP and Two pointers O(n), O(1) 
# class Solution: 
#     def trap(self, height: List[int]) -> int: 
#         n = len(height) 
#         if n < 3: return 0 
#         max_left = height[0] 
#         max_right = height[-1] 
#         water = 0 
         
#         left, right = 1, n-2 
#         for _ in range(1, n-1): 
#             # Decide which side is shorter, calculate water and use 
#             # max value of that side to update water. 
#             if height[left-1] < height[right+1]: 
#                 max_left = max(height[left-1], max_left) 
#                 if max_left > height[left]: 
#                     water += max_left - height[left] 
#                 left += 1 
#             else: 
#                 max_right = max(height[right+1], max_right) 
#                 if max_right > height[right]: 
#                     water += max_right - height[right] 
#                 right -= 1 
         
#         return water 
 
# ##### Solution 4 --- DP O(n), O(n) 
# class Solution: 
#     def trap(self, height: List[int]) -> int: 
#         n = len(height) 
#         if n < 3: return 0 
#         max_left = height[0] 
#         max_right = [height[-1]]*n 
#         water = 0 
         
#         for i in range(n-2,-1,-1): 
#             max_right[i] = max(max_right[i+1], height[i+1]) 
             
#         for i in range(1, n-1): 
#             max_left = max(max_left, height[i-1]) 
#             h = min(max_left, max_right[i]) 
#             if h > height[i]: 
#                 water += h-height[i] 
         
#         return water 
 
# ##### Solution 3 --- DP O(n), O(n) 
# class Solution: 
#     def trap(self, height: List[int]) -> int: 
#         n = len(height) 
#         if n < 3: return 0 
#         max_left = [height[0]]*n 
#         max_right = [height[-1]]*n 
#         water = 0 
 
#         for i in range(1, n): 
#             max_left[i] = max(max_left[i-1], height[i-1]) 
#         for i in range(n-2,-1,-1): 
#             max_right[i] = max(max_right[i+1], height[i+1]) 
             
#         for i in range(1, n-1): 
#             h = min(max_left[i], max_right[i]) 
#             if h > height[i]: 
#                 water += h-height[i] 
         
#         return water 
 
# ##### Solution 2 --- naive O(n^2), O(1) 
# class Solution: 
#     def trap(self, height: List[int]) -> int: 
#         # Check for i-th column's left highest wall and right one, 
#         # pick the short one and compare it with current height. 
#         # If current is lower, add height difference to water, else, 
#         # else there won't be any water. 
#         n = len(height) 
#         if n < 3: return 0 
 
#         water = 0 
#         h_left, h_right = height[0], height[-1] 
#         # Skip first and last column, cuz there won't be water. 
#         for i in range(1, n-1): 
#             h_right = max(height[i+1:]) 
#             h = min(h_right, h_left) 
#             if h > height[i]: 
#                 water += h - height[i] 
#             h_left = max(height[i], h_left) 
         
#         return water 
 
##### Solution 1  O(n), O(1) 
# class Solution: 
#     def trap(self, height: List[int]) -> int: 
#         ans = 0 
#         h1 = 0 
#         h2 = 0 
#         for i in range(len(height)): 
#             h1 = max(h1,height[i]) 
#             h2 = max(h2,height[-i-1]) 
#             ans += h1 + h2 -height[i] 
#         return  ans - len(height)*h1 

32. 二叉树的右视图 ok 
给定一个二叉树的 根节点 root，想象自己站在它的右侧，按照从顶部到底部的顺序，
返回从右侧所能看到的节点值。 
class Solution: 
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]: 
        if not root: 
            return list() 
        from collections import deque 
        queue = deque([root]) 
        ans = list() 
        while queue: 
            ans.append(queue[-1].val) 
            for _ in range(len(queue)): 
                node = queue.popleft() 
                if node.left: 
                    queue.append(node.left) 
                if node.right: 
                    queue.append(node.right) 
        return ans 
 
# 33. 合并两个有序数组 
给你两个按 非递减顺序 排列的整数数组 nums1 和 nums2，另有两个整数 m 和 n ，分别表示 nums1 和 nums2 中的元素数目。 
 
请你合并 nums2 到 nums1 中，使合并后的数组同样按非递减顺序排列。 
 
注意：最终，合并后数组不应由函数返回，而是存储在数组 nums1 中。为了应对这种情况，nums1 的初始长度为 m + n，其中前 m 个元素表示应合并的元素，后 n 个元素为 0 ，应忽略。nums2 的长度为 n 。 
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3 
输出：[1,2,2,3,5,6] 
解释：需要合并 [1,2,3] 和 [2,5,6] 。 
合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。
```python
class Solution: 
    def merge(self, nums1, m, nums2, n): 
        """ 
        Do not return anything, modify nums1 in-place instead. 
        """ 
        p1, p2 = m - 1, n - 1 
        tail = m + n - 1 
        while p1 >= 0 or p2 >= 0: 
            if p1 == -1: 
                nums1[tail] = nums2[p2] 
                p2 -= 1 
            elif p2 == -1: 
                nums1[tail] = nums1[p1] 
                p1 -= 1 
            elif nums1[p1] > nums2[p2]: 
                nums1[tail] = nums1[p1] 
                p1 -= 1 
            else: 
                nums1[tail] = nums2[p2] 
                p2 -= 1 
            tail -= 1
```

34. 搜索旋转排序数组 
整数数组 nums 按升序排列，数组中的值 互不相同 。 
 
在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 
旋转，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]
（下标 从 0 开始 计数）。例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变
为 [4,5,6,7,0,1,2] 。 
给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 
target ，则返回它的下标，否则返回 -1 。 
你必须设计一个时间复杂度为 O(log n) 的算法解决此问题。 
输入：nums = [4,5,6,7,0,1,2], target = 0 
输出：4 
 
class Solution: 
    def search(self, nums: List[int], target: int) -> int: 
        n = len(nums) 
        left, right = 0, n - 1 
        while left <= right: 
            mid = (left + right) // 2 
            if nums[mid] == target: 
                return mid 
            if nums[0] <= nums[mid]: 
                if nums[0] <= target < nums[mid]: 
                    right = mid - 1 
                else: 
                    left = mid + 1 
            else: 
                if nums[mid] < target <= nums[n-1]: 
                    left = mid + 1 
                else: 
                    right = mid - 1 
        return -1 
 
 
 
# 35. 螺旋矩阵 
给你一个 m 行 n 列的矩阵 matrix ，请按照 顺时针螺旋顺序 ，返回矩阵中的所有
元素。 
```python
class Solution: 
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]: 
        m = len(matrix) 
        n = len(matrix[0]) 
        res = list() 
        i, j = 0, n - 1 
        x, y = m - 1, 0 
        while i <= x and j >= y: 
            for o in range(y, j + 1): 
                res.append(matrix[i][o]) 
            for p in range(i + 1, x + 1): 
                res.append(matrix[p][j]) 
            if y < j and i < x: 
                for q in range(j - 1, y - 1, -1): 
                    res.append(matrix[x][q]) 
                for r in range(x - 1, i, -1): 
                    res.append(matrix[r][y]) 
            i += 1 
            j -= 1 
            x -= 1 
            y += 1 
        return res
```


# 36. 二叉树的层序遍历 ok 
给你二叉树的根节点 root ，返回其节点值的 层序遍历 。 （即逐层地，从左到右访问所有节点）。 
```python

class Solution: 
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]: 
        if not root: 
            return list() 
        from collections import deque 
        queue = deque([root]) 
        ans = list() 
        while queue: 
            temp = list() 
            for _ in range(len(queue)): 
                node = queue.popleft() 
                temp.append(node.val) 
                if node.left: 
                    queue.append(node.left) 
                if node.right: 
                    queue.append(node.right) 
            ans.append(temp) 
        return ans
```
 
# 37. 有效的括号 ok 
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。 
有效字符串需满足： 
左括号必须用相同类型的右括号闭合。 
左括号必须以正确的顺序闭合。 
每个右括号都有一个对应的相同类型的左括号。 
```python
class Solution: 
    def isValid(self, s: str) -> bool: 
        stack = list() 
        s_map = {')': '(', '}': '{', ']': '['} 
        for ch in s: 
            if ch in s_map.values(): 
                stack.append(ch) 
            else: 
                if stack and stack[-1] == s_map[ch]: 
                    stack.pop() 
                else: 
                    return False 
        return True if not stack else False
``` 

38. 数组中的逆序对 
在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆
序对。输入一个数组，求出这个数组中的逆序对的总数。 
示例 1: 
输入: [7,5,6,4] 
输出: 5 
 
 
class Solution: 
    def reversePairs(self, nums: List[int]) -> int: 
        def merge_sort(l, r): 
            # 终止条件 
            if l >= r: return 0 
            # 递归划分 
            m = (l + r) // 2 
            res = merge_sort(l, m) + merge_sort(m + 1, r) 
            # 合并阶段 
            i, j = l, m + 1 
            tmp[l:r + 1] = nums[l:r + 1] 
            for k in range(l, r + 1): 
                  # 代表左子数组已合并完 
                if i == m + 1: 
                    nums[k] = tmp[j] 
                    j += 1 
                elif j == r + 1 or tmp[i] <= tmp[j]: 
                    nums[k] = tmp[i] 
                    i += 1 
                else: 
                    nums[k] = tmp[j] 
                    j += 1 
                    res += m - i + 1 # 统计逆序对 
            return res 
         
        tmp = [0] * len(nums) 
        return merge_sort(0, len(nums) - 1) 
 
39. 岛屿数量 
给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。 
岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形
成。 
此外，你可以假设该网格的四条边均被水包围。 
 
class Solution: 
    def numIslands(self, grid: List[List[str]]) -> int: 
        m, n = len(grid), len(grid[0]) 
        res = 0 
 
        def dfs(i, j, grid): 
            grid[i][j] = '0' 
            for x, y in [(i+1, j), (i-1, j), (i, j-1), (i, j+1)]: 
                if 0 <= x < m and 0 <= y < n and grid[x][y] == '1': 
                    dfs(x, y, grid) 
        for i in range(m): 
            for j in range(n): 
                if grid[i][j] == '1': 
                    res += 1 
                    dfs(i, j, grid) 
        return res 
 
# 40. 合并区间 
以数组 intervals 表示若干个区间的集合，其中单个区间为 intervals[i] = [starti, endi] 。请你合并所有重叠的区间，并返回 一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间。 
示例 1： 
输入：intervals = [[1,3],[2,6],[8,10],[15,18]] 
输出：[[1,6],[8,10],[15,18]] 
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6]. 
```python
class Solution: 
    def merge(self, intervals): 
        n = len(intervals) 
        if n == 1: 
            return intervals 
        intervals.sort(key=lambda x:x[0]) 
        i, res = 0, [] 
        while i < n: 
            if i == 0 or res[-1][1] < intervals[i][0]: 
                res.append(intervals[i]) 
            else: 
                res[-1][1] = max(res[-1][1], intervals[i][1]) 
            i += 1 
        return res
``` 

 
41. 递增的三元子序列 
给你一个整数数组 nums ，判断这个数组中是否存在长度为 3 的递增子序列。 
 
如果存在这样的三元组下标 (i, j, k) 且满足 i < j < k ，使得 nums[i] < nums[j] < 
nums[k] ，返回 true ；否则，返回 false 。 
 
 
 
class Solution: 
    def increasingTriplet2(self, nums): 
        small, mid = float('inf'), float('inf') 
        for x in nums: 
            if x <= small: 
                small = x 
            elif x <= mid: 
                mid = x 
            elif x > mid: 
                return True 
        return False 
 
42. 最长递增子序列 
给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。 
子序列是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素 
的顺序。例如，[3,6,2,7]是数组 [0,3,1,6,2,2,7]的子序列。 
 
# Dynamic programming. 
class Solution: 
    def lengthOfLIS(self, nums: List[int]) -> int: 
        if not nums: return 0 
        dp = [1] * len(nums) 
        for i in range(len(nums)): 
            for j in range(i): 
                if nums[j] < nums[i]: # 如果要求非严格递增，将此行 '<' 改为 
'<=' 即可。 
                    dp[i] = max(dp[i], dp[j] + 1) 
        return max(dp) 
 
 
43. x 的平方根 
给你一个非负整数 x ，计算并返回 x 的 算术平方根 。 
由于返回类型是整数，结果只保留 整数部分 ，小数部分将被 舍去 。 
注意：不允许使用任何内置指数函数和算符，例如 pow(x, 0.5) 或者 x ** 0.5 。 
class Solution: 
    def mySqrt(self, x: int) -> int: 
        l, r, ans = 0, x, -1 
        while l <= r: 
            mid = (l + r) // 2 
            if mid * mid <= x: 
                ans = mid 
                l = mid + 1 
            else: 
                r = mid - 1 
        return ans 
 
# 44. 二叉树中的最大路径和 
路径 被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。
同一个节点在一条路径序列中至多出现一次。该路径至少包含一个节点，且不一定经过根节点。 
 
路径和是路径中各节点值的总和。 
给你一个二叉树的根节点 root ，返回其最大路径和。 
```python

class Solution: 
    def __init__(self): 
        self.maxSum = float("-inf") 
 
    def maxPathSum(self, root: TreeNode) -> int: 
        def maxGain(node): 
            if not node: 
                return 0 
 
            # 递归计算左右子节点的最大贡献值 
            # 只有在最大贡献值大于 0 时，才会选取对应子节点 
            leftGain = max(maxGain(node.left), 0) 
            rightGain = max(maxGain(node.right), 0) 
             
            # 节点的最大路径和取决于该节点的值与该节点的左右子节点的最大贡献值 
            priceNewpath = node.val + leftGain + rightGain 
             
            # 更新答案 
            self.maxSum = max(self.maxSum, priceNewpath) 
         
            # 返回节点的最大贡献值 
            return node.val + max(leftGain, rightGain) 
    
        maxGain(root) 
        return self.maxSum
``` 

# 45. 最长回文子串 
给你一个字符串 s，找到 s 中最长的回文子串。 
dp[i][j]) 表示字符串 s 的第 i 到 j 个字母组成的串（表示成 s[i:j]）是否为回文串 
```python
class Solution: 
    def longestPalindrome(self, s: str) -> str: 
        if not s: 
            return "" 
        begin, n, max_len = 0, len(s), 1 
        if n < 2: 
            return s 
        dp = [[False]*n for _ in range(n)] 
        for index in range(n): 
            dp[index][index] = True 
        for j in range(1, n): 
            for i in range(0, j): 
                if s[i] != s[j]: 
                    dp[i][j] = False  
                else: 
                    if j - i < 3: 
                        dp[i][j] = True 
                    else: 
                        dp[i][j] = dp[i+1][j-1] 
                if max_len < j - i + 1 and dp[i][j]: 
                    begin = i 
                    max_len = j - i + 1 
        return s[begin:begin+max_len]
```

46. 二叉树中序遍历 
class Solution: 
    def inorderTraversal(self, root): 
        if not root: 
            return [] 
        res = [] 
        def inner(node): 
            if node.left: 
                inner(node.left) 
            res.append(node.val) 
            if node.right: 
                inner(node.right) 
        inner(root) 
        return res 
 
    def inorder_traversal(self, root): 
        res = [] 
        stack = [] 
        while stack or root: 
            if root: 
                stack.append(root) 
                root = root.left 
            else: 
                tmp = stack.pop() 
                res.append(tmp.val) 
                root = tmp.right 
        return res 
 
47. 最小栈 
设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。 
实现 MinStack 类: 
 
MinStack() 初始化堆栈对象。 
void push(int val) 将元素 val 推入堆栈。 
void pop() 删除堆栈顶部的元素。 
int top() 获取堆栈顶部的元素。 
int getMin() 获取堆栈中的最小元素。 
class MinStack: 
 
    def __init__(self): 
        """ 
        initialize your data structure here. 
        """ 
        self.stack = list() 
        self.min_stack = list() 
 
    def push(self, x: int) -> None: 
        self.stack.append(x) 
        if not self.min_stack or x <= self.min_stack[-1]: 
            self.min_stack.append(x) 
 
    def pop(self) -> None: 
        if self.stack.pop() == self.min_stack[-1]: 
            self.min_stack.pop() 
 
    def top(self) -> int: 
        return self.stack[-1] 
 
    def min(self) -> int: 
        return self.min_stack[-1] 
 
 
# 48. 重排链表 
给定一个单链表 L 的头节点 head ，单链表 L 表示为： 
L0 → L1 → … → Ln - 1 → Ln 
请将其重新排列后变为： 
L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → … 
不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。 
```python
class Solution: 
    def reorderList(self, head: ListNode) -> None: 
        if not head: 
            return 
         
        mid = self.middleNode(head) 
        l1 = head 
        l2 = mid.next 
        mid.next = None 
        l2 = self.reverseList(l2) 
        self.mergeList(l1, l2) 
     
    def middleNode(self, head: ListNode) -> ListNode: 
        slow = fast = head 
        while fast.next and fast.next.next: 
            slow = slow.next 
            fast = fast.next.next 
        return slow 
     
    def reverseList(self, head: ListNode) -> ListNode: 
        prev = None 
        curr = head 
        while curr: 
            nextTemp = curr.next 
            curr.next = prev 
            prev = curr 
            curr = nextTemp 
        return prev 
 
    def mergeList(self, l1: ListNode, l2: ListNode): 
        while l1 and l2: 
            l1_tmp = l1.next 
            l2_tmp = l2.next 
 
            l1.next = l2 
            l1 = l1_tmp 
 
            l2.next = l1 
            l2 = l2_tmp
```

 
49. 对称的二叉树 
请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一
样，那么它是对称的。 
 
 
class Solution: 
    def isSymmetric(self, root: TreeNode) -> bool: 
        if not root: 
            return True 
        queue = [root] 
        while queue: 
            tmp = [] 
            node_list = [] 
            for node in queue: 
                if node.left: 
                    tmp.append(node.left.val) 
                    node_list.append(node.left) 
                else: 
                    tmp.append('null') 
                if node.right: 
                    tmp.append(node.right.val) 
                    node_list.append(node.right) 
                else: 
                    tmp.append('null') 
            if tmp == tmp[::-1]: 
                queue = node_list 
            else: 
                return False 
        return True 
 
    def is_symmetric(self, root: TreeNode) -> bool: 
        def is_sysmmetric_tree(left, right): 
            if left is None and right is None: 
                return True 
            if left is None or right is None: 
                return False 
            if left.val != right.val: 
                return False 
            return is_sysmmetric_tree(left.left, right.right) & \ 
                   is_sysmmetric_tree(right.left, left.right) 
        if not root: 
            return True 
        return is_sysmmetric_tree(root.left, root.right) 
 
# 50. 路径总和 II 
给你二叉树的根节点 root 和一个整数目标和 targetSum ，找出所有 从根节点到叶子节点 路径总和等于给定目标和的路径。 
叶子节点是指没有子节点的节点。 
```python
class Solution: 
    def pathSum(self, root: TreeNode, targetSum: int) -> List[List[int]]: 
        ret = list() 
        path = list() 
         
        def dfs(root: TreeNode, targetSum: int): 
            if not root: 
                return 
            path.append(root.val) 
            targetSum -= root.val 
            if not root.left and not root.right and targetSum == 0: 
                ret.append(path[:]) 
            dfs(root.left, targetSum) 
            dfs(root.right, targetSum) 
            path.pop() 
         
        dfs(root, targetSum) 
        return ret
```



51. 二叉树的完全性检验 
给定一个二叉树的 root ，确定它是否是一个 完全二叉树 。 
 
在一个 完全二叉树 中，除了最后一个关卡外，所有关卡都是完全被填满的，并且最
后一个关卡中的所有节点都是尽可能靠左的。它可以包含 1 到 2h 节点之间的最后一
级 h 。 
 
class Solution: 
    def isCompleteTree(self, root: Optional[TreeNode]) -> bool: 
        q = collections.deque([root]) 
        while q: 
            node = q.popleft() 
            if node:  
                q.extend([node.left, node.right]) 
            else: 
                return not any(q) 
 
 
52. 爬楼梯 
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。 
 
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？ 
 
 
class Solution: 
    def climbStairs(self, n: int) -> int: 
        dp = dict() 
        dp[1] = 1 
        dp[2] = 1 
        for i in range(3, n+1): 
            dp[i] = dp[i-1] + dp[i-2] 
        return dp[n] 
 
# 53. 链表中倒数第 k 个节点 ok 
输入一个链表，输出该链表中倒数第 k 个节点。为了符合大多数人的习惯，本题从 1 开始计数，即链表的尾节点是倒数第 1 个节点。 
 
例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。 
示例： 
给定一个链表: 1->2->3->4->5, 和 k = 2. 
返回链表 4->5. 
```python
class Solution: 
    def getKthFromEnd(self, head, k: int): 
        fast, slow = head, head 
        while fast and k >= 1: 
            fast = fast.next 
            k -= 1 
        while fast: 
            fast = fast.next 
            slow = slow.next 
        return slow
``` 
 
# 54. 验证二叉搜索树 
给你一个二叉树的根节点 root ，判断其是否是一个有效的二叉搜索树。 
 
有效 二叉搜索树定义如下： 
节点的左子树只包含 小于 当前节点的数。 
节点的右子树只包含 大于 当前节点的数。 
所有左子树和右子树自身必须也是二叉搜索树。 
```python
class Solution: 
    # 递归 
    def is_valid_BST(self, root): 
        if not root: 
            return True 
 
        def DFS(root, left, right): 
            if not root: 
                return True 
            return left < root.val < right and DFS(root.left, left, root.val) and DFS(root.right, root.val, right) 
 
        return DFS(root, float('-inf'), float('inf')) 
 
    # 中序遍历 
    def is_valid_BST2(self, root): 
        stack, inorder = list(), float('-inf') 
        while stack or root: 
            while root: 
                stack.append(root) 
                root = root.left 
            root = stack.pop() 
            if root.val <= inorder: 
                return False 
            inorder = root.val 
            root = root.right 
        return True
```
 
 
55. 组合总和 
给你一个 无重复元素 的整数数组 candidates 和一个目标整数 target ，找出 
candidates 中可以使数字和为目标数 target 的 所有 不同组合 ，并以列表形式返回。
你可以按 任意顺序 返回这些组合。 
candidates 中的 同一个 数字可以 无限制重复被选取 。如果至少一个数字的被选数量
不同，则两种组合是不同的。 
对于给定的输入，保证和为 target 的不同组合数少于 150 个。 
输入：candidates = [2,3,6,7], target = 7 
输出：[[2,2,3],[7]] 
解释： 
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。 
7 也是一个候选， 7 = 7 。 
仅有这两种组合。 
 
 
 
class Solution: 
    def combinationSum(self, candidates, target): 
        def dfs(candidates, begin, size, path, res, target): 
            if target < 0: 
                return 
            if target == 0: 
                res.append(path) 
                return 
 
            for index in range(begin, size): 
                dfs(candidates, index, size, path + [candidates[index]], 
res, target - candidates[index]) 
 
        size = len(candidates) 
        if size == 0: 
            return [] 
        path = [] 
        res = [] 
        dfs(candidates, 0, size, path, res, target) 
        return res 
 
 
56. 奇偶链表 
给定单链表的头节点 head ，将所有索引为奇数的节点和索引为偶数的节点分别组合
在一起，然后返回重新排序的列表。 
第一个节点的索引被认为是 奇数 ， 第二个节点的索引为 偶数 ，以此类推。 
请注意，偶数组和奇数组内部的相对顺序应该与输入时保持一致。 
你必须在 O(1) 的额外空间复杂度和 O(n) 的时间复杂度下解决这个问题。 
 
class Solution: 
    def oddEvenList(self, head: Optional[ListNode]) -> Optional[ListNode]: 
        pre = odd = ListNode(0) 
        event = cur = ListNode(0) 
        while head: 
            odd.next = head 
            odd = odd.next 
            cur.next = head.next 
            if cur.next: 
                cur = cur.next 
            head = head.next 
            if head: 
                head = head.next 
            else: 
                break 
        odd.next = event.next 
        return pre.next 
 
57. 回文链表 
给定单链表的头节点 head ，将所有索引为奇数的节点和索引为偶数的节点分别组合
在一起，然后返回重新排序的列表。 
第一种方式放到节点的值放到列表中； 
class Solution: 
    def isPalindrome(self, head: Optional[ListNode]) -> bool: 
        if not head: 
            return True 
        fast_pointer = head 
        slow_pointer = head 
 
        def get_half_pointer(fast_pointer, slow_pointer): 
            while fast_pointer.next and fast_pointer.next.next: 
                fast_pointer = fast_pointer.next.next 
                slow_pointer = slow_pointer.next 
            return slow_pointer 
 
        def reverse_linkedlist(head): 
            pre = None 
            cur = head 
            while cur: 
                node = cur.next 
                cur.next = pre 
                pre = cur 
                cur = node 
            return pre 
 
        slow_pointer = get_half_pointer(fast_pointer, slow_pointer) 
        slow_pointer = reverse_linkedlist(slow_pointer.next) 
        while slow_pointer: 
            if slow_pointer.val != head.val: 
                return False 
            slow_pointer = slow_pointer.next 
            head = head.next 
        return True 
 
58. 平衡二叉树 
给定一个二叉树，判断它是否是高度平衡的二叉树。 
本题中，一棵高度平衡二叉树定义为： 
 
一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1 。 
class Solution: 
    def isBalanced(self, root: Optional[TreeNode]) -> bool: 
        if not root: 
            return True 
        def get_depth(root): 
            if not root: 
                return 0 
            return 1 + max(get_depth(root.left), get_depth(root.right)) 
        if -1 <= get_depth(root.left) - get_depth(root.right) <= 1 and \ 
                self.isBalanced(root.left) and self.isBalanced(root.right): 
            return True 
        return False 
 
 
# 59. 二叉树的直径 
给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点
路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。 
示例 : 
给定二叉树 
          1 
         / \ 
        2   3 
       / \      
      4   5     
返回 3, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。 
```python
class Solution: 
    def diameterOfBinaryTree(self, root: TreeNode) -> int: 
        self.ans = 1 
        def depth(node): 
            # 访问到空节点了，返回 0 
            if not node: 
                return 0 
            # 左儿子为根的子树的深度 
            L = depth(node.left) 
            # 右儿子为根的子树的深度 
            R = depth(node.right) 
            # 计算 d_node 即 L+R+1 并更新 ans 
            self.ans = max(self.ans, L + R + 1) 
            # 返回该节点为根的子树的深度 
            return max(L, R) + 1 
 
        depth(root) 
        return self.ans - 1
```

# 60. 最长重复子数组 
给两个整数数组 nums1 和 nums2 ，返回两个数组中公共的、长度最长的子数组的长度。 
```python
class Solution: 
    def findLength(self, A: List[int], B: List[int]) -> int: 
        # 滑动窗口 
         
        def findmax(A,B): 
            ans_max = 0 
            la = len(A)
            lb = len(B)
            # B 进来，从 1 到 la 长度 
            for tlen in range(1,la+1): 
                ans_max = max(ans_max,max_len(A,0,B,lb-tlen,tlen)) 
            # B 出去,长度保持在 la 
            for j in range(lb-la,-1,-1): 
                ans_max = max(ans_max,max_len(A,0,B,j,la)) 
            # B 出去，长度从 la 到 0 -->相当于 A 左移 
            for i in range(1,la): 
                ans_max = max(ans_max,max_len(A,i,B,0,la-i)) 
 
            return ans_max 
 
        def max_len(A,i,B,j,length): 
            count = 0 
            temp_max =0 
            for k in range(length): 
                if A[i+k]==B[j+k]: 
                    count += 1 
                elif count>0: 
                    temp_max = max(temp_max,count) 
                    count = 0 
            temp_max = max(temp_max,count) 
            return temp_max 
         
        if len(A)<=len(B): 
            return findmax(A,B) 
        else: 
            return findmax(B,A)
```
 
61. 反转字符串中的单词 
给你一个字符串 s ，请你反转字符串中单词的顺序。 
 
单词 是由非空格字符组成的字符串。s 中使用至少一个空格将字符串中的单词分隔
开。 
返回单词顺序颠倒且 单词 之间用单个空格连接的结果字符串。 
注意：输入字符串 s 中可能会存在前导空格、尾随空格或者单词间的多个空格。返回
的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。 
class Solution: 
    def reverseWords(self, s: str) -> str: 
        left, right = 0, len(s) - 1 
        # 去掉字符串开头的空白字符 
        while left <= right and s[left] == ' ': 
            left += 1 
         
        # 去掉字符串末尾的空白字符 
        while left <= right and s[right] == ' ': 
            right -= 1 
             
        d, word = collections.deque(), [] 
        # 将单词 push 到队列的头部 
        while left <= right: 
            if s[left] == ' ' and word: 
                d.appendleft(''.join(word)) 
                word = [] 
            elif s[left] != ' ': 
                word.append(s[left]) 
            left += 1 
        d.appendleft(''.join(word)) 
         
        return ' '.join(d) 
或者： 
class Solution: 
    def reverseWords(self, s: str) -> str: 
        return " ".join(reversed(s.split())) 
或 
class Solution: 
    def reverseWords(self, s: str) -> str: 
        return " ".join(s[::-1].split(" ")[::-1]) 
 
 
62. 最小路径和 
给定一个包含非负整数的 m x n 网格 grid ，请找出一条从左上角到右下角的路径，
使得路径上的数字总和为最小。 
 
说明：每次只能向下或者向右移动一步。 
class Solution: 
    def minPathSum(self, grid: [[int]]) -> int: 
        for i in range(len(grid)): 
            for j in range(len(grid[0])): 
                if i == j == 0:  
                    continue 
                elif i == 0:   
                    grid[i][j] = grid[i][j - 1] + grid[i][j] 
                elif j == 0:   
                    grid[i][j] = grid[i - 1][j] + grid[i][j] 
                else:  
                    grid[i][j] = min(grid[i - 1][j], grid[i][j - 1]) + 
grid[i][j] 
        return grid[-1][-1] 
 
63. 不同路径 ok 
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。 
机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标
记为 “Finish” ）。 
问总共有多少条不同的路径？ 
class Solution: 
    def uniquePaths(self, m: int, n: int) -> int: 
        f = [[1] * n] + [[1] + [0] * (n - 1) for _ in range(m - 1)] 
        print(f) 
        for i in range(1, m): 
            for j in range(1, n): 
                f[i][j] = f[i - 1][j] + f[i][j - 1] 
        return f[m - 1][n - 1] 


# 64. 删除排序链表中的重复元素 ok 
给定一个已排序的链表的头 head， 删除所有重复的元素，使每个元素只出现一次 。 返回已排序的链表 。 
```python
class Solution: 
    def deleteDuplicates(self, head: ListNode) -> ListNode: 
        if not head: 
            return head 
        cur = head 
        while cur.next: 
            if cur.val == cur.next.val: 
                cur.next = cur.next.next 
            else: 
                cur = cur.next 
        return head
```

65. 寻找峰值 ok 
峰值元素是指其值严格大于左右相邻值的元素 
给你一个整数数组 nums，找到峰值元素并返回其索引。数组可能包含多个峰值，在这
种情况下，返回 任何一个峰值 所在位置即可。 
你可以假设 nums[-1] = nums[n] = -∞ 。 
你必须实现时间复杂度为 O(log n) 的算法来解决此问题。 
class Solution: 
    def findPeakElement(self, nums: List[int]) -> int: 
        n = len(nums) 
 
        # 辅助函数，输入下标 i，返回 nums[i] 的值 
        # 方便处理 nums[-1] 以及 nums[n] 的边界情况 
        def get(i: int) -> int: 
            if i == -1 or i == n: 
                return float('-inf') 
            return nums[i] 
         
        left, right, ans = 0, n - 1, -1 
        while left <= right: 
            mid = (left + right) // 2 
            if get(mid - 1) < get(mid) > get(mid + 1): 
                ans = mid 
                break 
            if get(mid) < get(mid + 1): 
                left = mid + 1 
            else: 
                right = mid - 1 
         
        return ans 
 
66. 长度最小的子数组 ok 
给定一个含有 n 个正整数的数组和一个正整数 target 。 
找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-
1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。 
 
 
class Solution: 
    def minSubArrayLen(self, s: int, nums: List[int]) -> int: 
        if not nums: 
            return 00 
         
        n = len(nums) 
        ans = n + 1 
        start, end = 0, 0 
        total = 0 
        while end < n: 
            total += nums[end] 
            while total >= s: 
                ans = min(ans, end - start + 1) 
                total -= nums[start] 
                start += 1 
            end += 1 
         
        return 0 if ans == n + 1 else ans 
 
67. 零钱兑换 II  
给你一个整数数组 coins 表示不同面额的硬币，另给一个整数 amount 表示总金额。 
请你计算并返回可以凑成总金额的硬币组合数。如果任何硬币组合都无法凑出总金
额，返回 0 。 
假设每一种面额的硬币有无限个。  
题目数据保证结果符合 32 位带符号整数。 
 
 
用 dp[x] 表示金额之和等于 x 的硬币组合数，目标是求 dp[amount] 
动态规划的边界是 dp[0]=。只有当不选取任何硬币时，金额之和才为 000，因此只有 111 种
硬币组合。 
对于面额为 coin 的硬币，当 coin≤i≤amount 时，如果存在一种硬币组合的金额之和等于 
i−coin，则在该硬币组合中增加一个面额为 coin 的硬币，即可得到一种金额之和等于 iii 的
硬币组合。因此需要遍历 coins，对于其中的每一种面额的硬币，更新数组 dp 中的每个大
于或等于该面额的元素的值。 
 
class Solution: 
    def change(self, amount: int, coins: List[int]) -> int: 
        dp = [0] * (amount+1)  
        dp[0] = 1 
        for coin in coins: 
            i = coin 
            while i <= amount: 
                dp[i] += dp[i - coin] 
                i += 1 
        return dp[amount] 
 
68. 最小覆盖子串  
给你一个字符串 s 、一个字符串 t 。返回 s 中涵盖 t 所有字符的最小子串。如果 s 
中不存在涵盖 t 所有字符的子串，则返回空字符串 "" 。 
输入：s = "ADOBECODEBANC", t = "ABC" 
输出："BANC" 
class Solution: 
    def minWindow(self, s: str, t: str) -> str: 
         
        if len(t) > len(s): 
            return ''         
         
        cnt = collections.Counter(t)    # 哈希表：记录需要匹配到的各个元素的数
目 
        need = len(t)                   # 记录需要匹配到的字符总数【need=0 表示
匹配到了】 
         
        n = len(s) 
        start, end = 0, -1          # 记录目标子串 s[start, end]的起始和结尾 
        min_len = n + 1             # 符合题意的最短子串长度【初始化为一个不可能
的较大值】 
        left = right = 0            # 滑动窗口的左右边界 
         
        for right in range(n): 
             
            # 窗口右边界右移一位 
            ch = s[right]               # 窗口中新加入的字符 
            if ch in cnt:               # 新加入的字符位于 t 中 
                if cnt[ch] > 0:         # 对当前字符 ch 还有需求 
                    need -= 1           # 此时新加入窗口中的 ch 对 need 有影响 
                cnt[ch] -= 1 
             
            # 窗口左边界持续右移 
            while need == 0:            # need=0，当前窗口完全覆盖了 t 
                if right - left + 1 < min_len:      # 出现了更短的子串 
                    min_len = right - left + 1 
                    start, end = left, right 
                 
                ch = s[left]            # 窗口中要滑出的字符 
                if ch in cnt:           # 刚滑出的字符位于 t 中 
                    if cnt[ch] >= 0:    # 对当前字符 ch 还有需求，或刚好无需求
(其实此时只有=0 的情况) 
                        need += 1       # 此时滑出窗口的 ch 会对 need 有影响 
                    cnt[ch] += 1 
                left += 1               # 窗口左边界+1 
         
        return s[start: end+1] 
 
69. 最长有效括号  
给你一个只包含 '(' 和 ')' 的字符串，找出最长有效（格式正确且连续）括号子串的长
度。 
 
class Solution: 
    def longestValidParentheses(self, s: str) -> int: 
        if len(s) < 1: return 0 
        stack, res = [-1], 0 
        for i in range(len(s)): 
            if s[i] == "(": 
                stack.append(i) 
            else: 
                # Stack either store the index of "(" or the last invalide 
bracket ")" index. 
                stack.pop() 
                if not stack: 
                    stack.append(i) # To store the last one that is not a 
valid bracket. 
                else: 
                    # Current index minus the last invalid bracket index. 
                    res = max(res, i - stack[-1]) 
        return res 
 

70. 交替打印奇偶数 ok 
import threading 
 
 
def thread_a(): 
    for i in range(1, 101): 
        if i % 2 != 0: 
            lock_b.acquire() 
            print(i) 
            lock_a.release() 
 
 
def thread_b(): 
    for i in range(1, 101): 
        if i % 2 == 0: 
            lock_a.acquire() 
            print(i)  
            lock_b.release() 
 
 
if __name__ == '__main__': 
    lock_a = threading.Lock() 
    lock_b = threading.Lock() 
 
    t1 = threading.Thread(target=thread_a) 
    t2 = threading.Thread(target=thread_b) 
 
    lock_a.acquire() 
    t1.start() 
    t2.start() 
 
    t1.join() 
 
 
71. 滑动窗口最大值 
给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最
右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。 
 
返回滑动窗口中的最大值。 
 
class Solution: 
    def maxSlidingWindow(self, nums, k: int): 
        n = len(nums) 
        q = [(-nums[i], i) for i in range(k)] 
        heapq.heapify(q) 
        res = [-q[0][0]] 
        for i in range(k, n): 
            heapq.heappush(q, (-nums[i], i)) 
            while i - k >= q[0][1]: 
                heapq.heappop(q) 
            res.append(-q[0][0]) 
        return res 
     
        def max_sliding_window(self, nums, k): 
        n = len(nums) 
        from collections import deque 
        q = deque() 
        for i in range(0, k): 
            while q and nums[i] >= nums[q[-1]]: 
                q.pop() 
            q.append(i) 
        res = [] 
        res.append(nums[q[0]]) 
        for i in range(k, n): 
            while q and nums[i] >= nums[q[-1]]: 
                q.pop() 
            q.append(i) 
            while q[0] <= i - k: 
                q.popleft() 
            res.append(nums[q[0]]) 
        return res 
 
    def max_sliding_window2(self, nums, k): 
        from collections import deque 
        n = len(nums) 
        d, res = deque(), [] 
        for i, j in zip(range(1-k, n+1-k), range(n)): 
            if i > 0 and d[0] == nums[i - 1]: 
                d.popleft() 
                # 保持 deque 递减 
            while d and d[-1] < nums[j]: 
                d.pop() 
            d.append(nums[j]) 
            # 记录窗口最大值 
            if i >= 0: 
                res.append(d[0]) 
        return res 
 
72. 复原 IP 地址 
有效 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 
0），整数之间用 '.' 分隔。 
   
例如："0.1.2.201" 和 "192.168.1.1" 是 有效 IP 地址，但是 "0.011.255.245"、
"192.168.1.312" 和 "192.168@1.1" 是 无效 IP 地址。 
给定一个只包含数字的字符串 s ，用以表示一个 IP 地址，返回所有可能的有效 IP 
地址，这些地址可以通过在 s 中插入 '.' 来形成。你 不能 重新排序或删除 s 中的任
何数字。你可以按 任何 顺序返回答案。 
 
 
class Solution: 
    def restoreIpAddresses(self, s: str) -> List[str]: 
        SEG_COUNT = 4 
        ans = list() 
        segments = [0] * SEG_COUNT 
         
        def dfs(segId: int, segStart: int): 
            # 如果找到了 4 段 IP 地址并且遍历完了字符串，那么就是一种答案 
            if segId == SEG_COUNT: 
                if segStart == len(s): 
                    ipAddr = ".".join(str(seg) for seg in segments) 
                    ans.append(ipAddr) 
                return 
             
            # 如果还没有找到 4 段 IP 地址就已经遍历完了字符串，那么提前回溯 
            if segStart == len(s): 
                return 
 
            # 由于不能有前导零，如果当前数字为 0，那么这一段 IP 地址只能为 0 
            if s[segStart] == "0": 
                segments[segId] = 0 
                dfs(segId + 1, segStart + 1) 
             
            # 一般情况，枚举每一种可能性并递归 
            addr = 0 
            for segEnd in range(segStart, len(s)): 
                addr = addr * 10 + (ord(s[segEnd]) - ord("0")) 
                if 0 < addr <= 0xFF: 
                    segments[segId] = addr 
                    dfs(segId + 1, segEnd + 1) 
                else: 
                    break 
         
 
        dfs(0, 0) 
        return ans 
 
73. 调整数组顺序使奇数位于偶数前面 
输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数在数组
的前半部分，所有偶数在数组的后半部分。 
 
class Solution: 
    def exchange(self, nums: List[int]) -> List[int]: 
        n = len(nums) 
        left, right = 0, n - 1 
        while left < right: 
            while left < right and nums[left] % 2 != 0: 
                left += 1 
            while right > left and nums[right] % 2 == 0: 
                right -= 1 
            if left < right: 
                nums[left], nums[right] = nums[right], nums[left] 
                left += 1 
                right -= 1 
        return nums 
 
 
 
74. 最长公共前缀 
编写一个函数来查找字符串数组中的最长公共前缀。 
如果不存在公共前缀，返回空字符串 ""。 
class Solution: 
    def longestCommonPrefix(self, strs: List[str]) -> str: 
        def lcp(s1, s2): 
            n = min(len(s1), len(s2)) 
            index = 0 
            while index < n and s1[index] == s2[index]: 
                index += 1 
            return s1[0:index] 
        if not strs: 
            return "" 
        prefix = strs[0] 
        for s in strs[1:]: 
            prefix = lcp(prefix, s) 
            if not prefix: 
                return '' 
        return prefix 
 
class Solution: 
    def longestCommonPrefix(self, strs: List[str]) -> str: 
        def lcp(s1, s2): 
            n = min(len(s1), len(s2)) 
            index = 0 
            while index < n and s1[index] == s2[index]: 
                index += 1 
            return s1[0:index] 
        if not strs: 
            return "" 
        strs.sort() 
        return lcp(strs[0], strs[-1]) 
 
class Solution: 
    def longestCommonPrefix(self, strs: List[str]) -> str: 
        ans = '' 
        for l in zip(*strs): 
            seen = set(l) 
            if len(seen) == 1: 
                ans += l[0] 
            else: 
                break 
        return ans


# 75. 字符串转换整数
请你来实现一个 myAtoi(string s) 函数，使其能将字符串转换成一个 32 位有符号整数（类似 C/C++ 中的 atoi 函数）。
函数 myAtoi(string s) 的算法如下：
读入字符串并丢弃无用的前导空格
检查下一个字符（假设还未到字符末尾）为正还是负号，读取该字符（如果有）。 确定最终结果是负数还是正数。 如果两者都不存在，则假定结果为正。
读入下一个字符，直到到达下一个非数字字符或到达输入的结尾。字符串的其余部分将被忽略。
将前面步骤读入的这些数字转换为整数（即，"123" -> 123， "0032" -> 32）。如果没有读入数字，则整数为 0 。必要时更改符号（从步骤 2 开始）。
如果整数数超过 32 位有符号整数范围 [−231,  231 − 1] ，需要截断这个整数，使其保持在这个范围内。具体来说，小于 −231 的整数应该被固定为 −231 ，大于 231 − 1 的整数应该被固定为 231 − 1 。
返回整数作为最终结果。
注意：
本题中的空白字符只包括空格字符 ' ' 。
除前导空格或数字后的其余字符串外，请勿忽略 任何其他字符。
 

示例 1：
输入：s = "42"
输出：42
解释：加粗的字符串为已经读入的字符，插入符号是当前读取的字符。
第 1 步："42"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："42"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："42"（读入 "42"）
           ^
解析得到整数 42 。
由于 "42" 在范围 [-231, 231 - 1] 内，最终结果为 42 。
示例 2：

输入：s = "   -42"
输出：-42
解释：
第 1 步："   -42"（读入前导空格，但忽视掉）
            ^
第 2 步："   -42"（读入 '-' 字符，所以结果应该是负数）
             ^
第 3 步："   -42"（读入 "42"）
               ^
解析得到整数 -42 。
由于 "-42" 在范围 [-231, 231 - 1] 内，最终结果为 -42 。
示例 3：

输入：s = "4193 with words"
输出：4193
解释：
第 1 步："4193 with words"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："4193 with words"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："4193 with words"（读入 "4193"；由于下一个字符不是一个数字，所以读入停止）
             ^
解析得到整数 4193 。
由于 "4193" 在范围 [-231, 231 - 1] 内，最终结果为 4193 。

```python
INT_MAX = 2 ** 31 - 1
INT_MIN = -2 ** 31

class Automaton:
    def __init__(self):
        self.state = 'start'
        self.sign = 1
        self.ans = 0
        self.table = {
            'start': ['start', 'signed', 'in_number', 'end'],
            'signed': ['end', 'end', 'in_number', 'end'],
            'in_number': ['end', 'end', 'in_number', 'end'],
            'end': ['end', 'end', 'end', 'end'],
        }
        
    def get_col(self, c):
        if c.isspace():
            return 0
        if c == '+' or c == '-':
            return 1
        if c.isdigit():
            return 2
        return 3

    def get(self, c):
        self.state = self.table[self.state][self.get_col(c)]
        if self.state == 'in_number':
            self.ans = self.ans * 10 + int(c)
            self.ans = min(self.ans, INT_MAX) if self.sign == 1 else min(self.ans, -INT_MIN)
        elif self.state == 'signed':
            self.sign = 1 if c == '+' else -1

class Solution:
    def myAtoi(self, str: str) -> int:
        automaton = Automaton()
        for c in str:
            automaton.get(c)
        return automaton.sign * automaton.ans
```

# 76. 二叉树的遍历
迭代法遍历树 
前、中、后序遍历通用模板（只需一个栈的空间） 
```python
class Solution: 
    def inorderTraversal(self, root: TreeNode) -> List[int]:  
        res = [] 
        stack = [] 
        cur = root 
        # 中序，模板：先用指针找到每颗子树的最左下角，然后进行进出栈操作 
        while stack or cur: 
            while cur: 
                stack.append(cur) 
                cur = cur.left 
            cur = stack.pop() 
            res.append(cur.val) 
            cur = cur.right 
        return res 
         
        # # 前序，相同模板 
        # while stack or cur: 
        #     while cur: 
        #         res.append(cur.val) 
        #         stack.append(cur) 
        #         cur = cur.left 
        #     cur = stack.pop() 
        #     cur = cur.right 
        # return res 
 
         
        # # 后序，相同模板 
        # while stack or cur: 
        #     while cur: 
        #         res.append(cur.val) 
        #         stack.append(cur) 
        #         cur = cur.right 
        #     cur = stack.pop() 
        #     cur = cur.left 
        # return res[::-1]
```

77. 删除链表的倒数第N个节点
给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。
```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        dummy = ListNode(0, head)
        first = head
        second = dummy
        for i in range(n):
            first = first.next

        while first:
            first = first.next
            second = second.next
        
        second.next = second.next.next
        return dummy.next
```

# 78. 位1的个数
编写一个函数，输入是一个无符号整数（以二进制串的形式），返回其二进制表达式中数字位数为 '1' 的个数（也被称为汉明重量）。
```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        ret = sum(1 for i in range(32) if n & (1 << i)) 
        return ret
```

# 79. 整数反转
给你一个 32 位的有符号整数 x ，返回将 x 中的数字部分反转后的结果。
如果反转后整数超过 32 位的有符号整数的范围 [−231,  231 − 1] ，就返回 0。
假设环境不允许存储 64 位整数（有符号或无符号）。

示例 1：
输入：x = 123
输出：321
示例 2：

输入：x = -123
输出：-321
```python
class Solution:
    def reverse(self, x: int) -> int:
        INT_MIN, INT_MAX = -2**31, 2**31 - 1

        rev = 0
        while x != 0:
            # INT_MIN 也是一个负数，不能写成 rev < INT_MIN // 10
            if rev < INT_MIN // 10 + 1 or rev > INT_MAX // 10:
                return 0
            digit = x % 10
            # Python3 的取模运算在 x 为负数时也会返回 [0, 9) 以内的结果，因此这里需要进行特殊判断
            if x < 0 and digit > 0:
                digit -= 10

            # 同理，Python3 的整数除法在 x 为负数时会向下（更小的负数）取整，因此不能写成 x //= 10
            x = (x - digit) // 10
            rev = rev * 10 + digit
        
        return rev
```

80. 两数相加
给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。
请你将两个数相加，并以相同形式返回一个表示和的链表。
你可以假设除了数字 0 之外，这两个数都不会以 0 开头。
```python
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        num1,num2 = 0,0
        a,b = l1,l2
        tmp = 1
        while a:
            num1 = num1 + a.val * tmp
            tmp *= 10
            a = a.next
        tmp = 1
        while b:
            num2 = num2 + b.val * tmp
            tmp *= 10
            b = b.next
        num3 = num1 + num2
        head = ListNode()
        c = head
        if num3 == 0:
            return head
        while num3 > 0:
            c.next = ListNode(num3 % 10)
            c = c.next
            num3 //=10
        return head.next
```



