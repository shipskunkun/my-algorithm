### 4.12-4.18 四月第三周


1. 合并排序的数组
2. 找出数组中重复的数字
3. 删除排序数组中的重复项，在原数组上操作
4. 青蛙跳台阶问题
5. 斐波那契数列
6. 有序数组两数之和
7. 链表中倒数第k个节点  
8. 移除数组中所有值为val的元素
9. 调整数组顺序使奇数位于偶数前面
10. 数组中出现次数超过一半的数字








### 1. 合并排序的数组


给定两个排序后的数组 A 和 B，其中 A 的末端有足够的缓冲空间容纳 B。 编写一个方法，将 B 合并入 A 并排序。

初始化 A 和 B 的元素数量分别为 m 和 n。

示例:

输入:
A = [1,2,3,0,0,0], m = 3
B = [2,5,6],       n = 3

输出: [1,2,2,3,5,6]
说明:

A.length == n + m

### 解题思路


对于数组的操作，效率比较高的是，数组头部或者尾部操作	
已知合并后的数组长度，那么我们从数组后面操作	
注意边界值，k>=0，m 可能为0， n 可能为0 的情况	
for循环执行步骤	

时间复杂度：O(m+n)	,两个指针分别移动 mm 和 nn 的距离。	
空间复杂度：O(1), 对原数组原地修改

```
代码
/**
 * @param {number[]} A
 * @param {number} m
 * @param {number[]} B
 * @param {number} n
 * @return {void} Do not return anything, modify A in-place instead.
 */
var merge = function(A, m, B, n) {
    var i  = m - 1;
    var j = n - 1;
    //从A尾部操作，每次确定最后一个数
    for(var k = m + n - 1; k >= 0; k--) {
        if(i > -1 && j > -1) {
            if(A[i] < B[j]) {
                A[k] = B[j--];
            }else {
                A[k] = A[i--];
            }
        }else if(i > -1) {
            A[k] = A[i--];
        }else {
            A[k] = B[j--]
        }
    }
    return A;
};

```

### 2. 数组中重复的数字

找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

示例 1：

输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
 

限制：

2 <= n <= 100000

### 解题思路

map, key不重复的，值-值，数据结构		
然后注意，for的边境，i=nums.length - 1 要执行
		
时间复杂度：O(n),遍历数组一遍。使用哈希集合（HashSet），添加元素的时间复杂度为 O(1)，故总的时间复杂度是 O(n)。

空间复杂度： O(n),不重复的每个元素都可能存入集合，因此占用 O(n) 额外空间。

```
代码
/**
 * @param {number[]} nums
 * @return {number}
 */
var findRepeatNumber = function(nums) {
    var map = new Map();
    for(var i = 0; i < nums.length ; i++) {
        if(!map.has(nums[i])) {
            map.set(nums[i], true)
        }else{
            return nums[i]
        }
    }
};

```

### 3.  删除排序数组中的重复项

给定一个排序数组，你需要在 原地 删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

 

示例 1:

给定数组 nums = [1,1,2], 

函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 

你不需要考虑数组中超出新长度后面的元素。
示例 2:

给定 nums = [0,0,1,1,1,2,2,3,3,4],

函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。

你不需要考虑数组中超出新长度后面的元素。

### 解题思路
显然，由于数组已经排序，所以重复的元素一定连在一起，找出它们并不难，但如果毎找到一个重复元素就立即删除它，就是在数组中间进行删除操作，整个时间复杂度是会达到 O(N^2)。而且题目要求我们原地修改，也就是说不能用辅助数组，空间复杂度得是 O(1)

其实，对于数组相关的算法问题，有一个通用的技巧：要尽量避免在中间删除元素，那我就想先办法把这个元素换到最后去。这样的话，最终待删除的元素都拖在数组尾部，一个一个 pop 掉就行了，每次操作的时间复杂度也就降低到 O(1) 了。

按照这个思路呢，又可以衍生出解决类似需求的通用方式：双指针技巧。具体一点说，应该是快慢指针。

我们让慢指针 slow 走左后面，快指针 fast 走在前面探路，找到一个不重复的元素就告诉 slow 并让 slow 前进一步。这样当 fast 指针遍历完整个数组 nums 后，nums[0..slow] 就是不重复元素，之后的所有元素都是重复元素。


时间复杂度：O(n)	
空间复杂度： O(1)



```
代码
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function(nums) {
    if(nums.length == 0) return 0;
    var i = 0;
    var j = 1;
    while(j < nums.length) {
        if(nums[i] != nums[j]) {
            i++
            nums[i] = nums[j]
        }
        j++
    }
    return i + 1
};
```


### 4. 青蛙跳台阶问题

一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

示例 1：

输入：n = 2
输出：2
示例 2：

输入：n = 7
输出：21
提示：

0 <= n <= 100



### 解题思路
啥叫「自顶向下」？递归树（或者说图），是从上向下延伸，都是从一个规模较大的原问题比如说 f(20)，向下逐渐分解规模，直到 f(1) 和 f(2) 触底，然后逐层返回答案，这就叫「自顶向下」。

啥叫「自底向上」？反过来，我们直接从最底下，最简单，问题规模最小的 f(1) 和 f(2) 开始往上推，直到推到我们想要的答案 f(20)，这就是动态规划的思路，这也是为什么动态规划一般都脱离了递归，而是由循环迭代完成计算。

注意取模，要在循环里面取

时间复杂度： O(n),计算 f(n)f(n) 需循环 nn 次，每轮循环内计算操作使用 O(1)O(1)  		
空间复杂度：O(n)

也有空间复杂度为 O(1)的解法

### 代码

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var numWays = function(n) {
    if(n <= 1) return 1;
    var arr = new Array( n + 1 );
    arr[0] = 1;
    arr[1] = 1;
    for(var i = 2; i< n+1; i++) {
        arr[i] = (arr[i-1] + arr[i-2])%1000000007
    }
    return arr[n]
};
```

### 5. 斐波那契数列

写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项。斐波那契数列的定义如下：

F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

 

示例 1：

输入：n = 2
输出：1
示例 2：

输入：n = 5
输出：5
 

提示：

0 <= n <= 100



### 解题思路
啥叫「自顶向下」？递归树（或者说图），是从上向下延伸，都是从一个规模较大的原问题比如说 f(20)，向下逐渐分解规模，直到 f(1) 和 f(2) 触底，然后逐层返回答案，这就叫「自顶向下」。

啥叫「自底向上」？反过来，我们直接从最底下，最简单，问题规模最小的 f(1) 和 f(2) 开始往上推，直到推到我们想要的答案 f(20)，这就是动态规划的思路，这也是为什么动态规划一般都脱离了递归，而是由循环迭代完成计算。

注意取模，要在循环里面取

时间复杂度： O(n)		
空间复杂度： O(1)

### 代码

```
/**
 * @param {number} n
 * @return {number}
 */
var fib = function(n) {
    var dp = [];
    dp[0] = 0;
    dp[1] = 1;
    if(n <= 1) { return dp[n] }
    for(var i = 2; i <= n; i++){
        dp[2] = (dp[0] + dp[1])%1000000007;
        dp[0] = dp[1];
        dp[1] = dp[2];
    }
    return dp[2];
};
```


### 6. 两数之和 II - 输入有序数组

给定一个已按照升序排列 的有序数组，找到两个数使得它们相加之和等于目标数。

函数应该返回这两个下标值 index1 和 index2，其中 index1 必须小于 index2。

说明:

返回的下标值（index1 和 index2）不是从零开始的。
你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。
示例:

输入: numbers = [2, 7, 11, 15], target = 9
输出: [1,2]
解释: 2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。


### 解题思路
我把双指针技巧再分为两类，一类是「快慢指针」，一类是「左右指针」。前者解决主要解决链表中的问题，比如典型的判定链表中是否包含环；后者主要解决数组（或者字符串）中的问题，比如二分查找。

只要数组有序，就应该想到双指针技巧。这道题的解法有点类似二分查找，通过调节 left 和 right 可以调整 sum 的大小：

### 代码

```javascript
/**
 * @param {number[]} numbers
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(numbers, target) {
    var i = 0;
    var j = numbers.length - 1;
    while(i < j) {
        var sum = numbers[i] + numbers[j]
        if(sum == target) {
            return [i+1, j+1] 
        }else if( sum < target) {
            i++
        }else {
            j--
        }
    }
    return [-1, -1]
};
```





### 7. 链表中倒数第k个节点

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。

 

示例：

给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.



### 解题思路
快慢指针主要解决链表中的问题，让快指针先走 k 步，然后快慢指针开始同速前进。
这样当快指针走到链表末尾 null 时，慢指针所在的位置就是倒数第 k 个链表节点（为了简化，假设 k 不会超过链表长度）

注意边界测试用例

### 代码

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} k
 * @return {ListNode}
 */
var getKthFromEnd = function(head, k) {
    var low = head;
    var high = head;
    for(var i = 0; i < k; i++) {
        high = high.next
    }
    while( high ) {
        low = low.next;
        high = high.next;
    }
    return low
};
```


### 8. 移除数组中所有值为val的元素
给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

 

示例 1:

给定 nums = [3,2,2,3], val = 3,

函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。

你不需要考虑数组中超出新长度后面的元素。
示例 2:

给定 nums = [0,1,2,2,3,0,4,2], val = 2,

函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。

注意这五个元素可为任意顺序。

你不需要考虑数组中超出新长度后面的元素。


### 错误原因：

[2] 3, 输出 []，实际要求输出 [2] ???

特殊案例怎么搞？

```javascript
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
var removeElement = function(nums, val) {
    var i = 0 ;
    var j = nums.length - 1;
    while(i < j) {
        while(nums[j] == val ) {
            j--
        }
        while(nums[i] != val) {
            i++
        }
        if(i < j) {
            [nums[i],nums[j]] = [nums[j],nums[i]] 
        }
    }
    return i ;
};
```

没考虑 起始条件 i = j 的情况


### 解题思路

这道题目，和删除排序数组中的重复项有点类似

我们使用两个指针，第一个指针用于维护，一个数组中所有值 不等于 val 
第二个指针用于寻找原数组中，剩下的，不等于 val 的值，如果有，把它放到第一个数组中

删除排序数组中的重复项有点类似，只不过j是从1开始的，这里j 从0 开始。

时间复杂度：O(n)	
空间复杂度: O(1)



### 代码

```javascript
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
var removeElement = function(nums, val) {
    var i = 0 ;
    for(var j = 0 ; j < nums.length; j++) {
        if(nums[j] != val) {
            nums[i++] = nums[j]
        }
    }
    return i
};
```





### 9.调整数组顺序使奇数位于偶数前面

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

 

示例：

输入：nums = [1,2,3,4]
输出：[1,3,2,4] 
注：[3,1,2,4] 也是正确的答案之一。
 

提示：

1 <= nums.length <= 50000
1 <= nums[i] <= 10000




### 自己的错误写法		
错误原因：每次 i 也是递归的，不是说只走了一步，要一直向右，直到找到第一个偶数为止。


	/**
	 * @param {number[]} nums
	 * @return {number[]}
	 */
	var exchange = function(nums) {
	    var i = 0;
	    var j = nums.length - 1;
	    while(i < j) {
	        if(i%2 != 0) {   // 这里写错！注意
	            i++
	        }
	        if(j%2 ==0) { 	// 这里写错！注意
	            j--
	        }
	        if(i < j) {
	            var temp = nums[i];
	            nums[i] = nums[j];
	            nums[j] =  temp;
	        }
	    }
	    return nums;
	};


第二次又写错了，？？

超时原因：  是nums[i]  不是  i


	/**
	 * @param {number[]} nums
	 * @return {number[]}
	 */
	var exchange = function(nums) {
	    var i = 0;
	    var j = nums.length - 1;
	    while(i < j) {
	        while(i%2 != 0 && i<j) {
	            i++
	        }
	        while(j%2 != 0 && i<j) {
	            j--
	        }
	        var temp = nums[i];
	        nums[i] = nums[j];
	        nums[j] =  temp;
	    }
	    return nums;
	};



### 解题思路

找到左边第一个偶数，右边第一个奇数，交换

自己写错误原因：每次 i 也是递归的，不是说只走了一步，要一直向右，直到找到第一个偶数为止。


### 代码

```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var exchange = function(nums) {
    let i = 0;
    let j = nums.length - 1;
    while(i < j) {
        while( nums[i]%2 ) {
            i++
        }
        while( nums[j]%2 == 0 ) {
            j--
        }
        if (i < j) {
            [nums[i], nums[j]] = [nums[j], nums[i]];
        }
    } 
    return nums
};
```


### 10. 数组中出现次数超过一半的数字

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

示例 1:

输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
输出: 2
 
限制：

1 <= 数组长度 <= 50000


### 解题思路

如果一个数在一个数组中长度超过一半，那么用这个数出现的次数，减去其他数字出现的次数，是大于0的


### 代码

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var majorityElement = function(nums) {
    var major = nums[0];
    var count = 1;
    for(var i = 1; i < nums.length; i++) {
        if(major == nums[i]) {
            count++
        }
        else {
            count--
        }
        if(count == 0) {
            major = nums[i]
            count = 1;
        }
    }
    return major
};
```






