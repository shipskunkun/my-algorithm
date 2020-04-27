## 2章，数组

### 2.2.1 创建数组
var numbers = new Array(10).fill(0);

### 2.2.3 由字符串生成数组
调用字符串对象的 split() 方法也可以生成数组

### 2.3.1 查找元素
indexOf() 如果目标数组包含该参数，就返回该元素在数组中的索引;
如果不包含，就返回 -1。

### 2.3.2 数组的字符串表示
有两个方法可以将数组转化为字符串:join() 和 toString()

### 2.3.3 由已有数组创建新数组
concat() 和 splice() 方法允许通过已有数组创建新数组。

concat 方法可以合并多个数组	
创建一个新数组，splice() 方法截取一个数组的子集创建一个新数组。

我们先来看看 concat() 方法的工作原理。该方法的发起者是一个数组，参数是另一个数 组。
splice() 方法从现有数组里截取一个新数组。该方法的第一个参数是截取的起始索引，第 二个参数是截取的长度。


slice()：返回从原数组中指定开始下标到结束下标之间的项组成的新数组。slice()方法可以接受一或两个参数，即要返回项的起始和结束位置。在只有一个参数的情况下， slice()方法返回从该参数指定位置开始到当前数组末尾的所有项。如果有两个参数，该方法返回起始和结束位置之间的项——但不包括结束位置的项。

### 2.4.1 为数组添加元素
有两个方法可以为数组添加元素:push() 和 unshift()。  
push() 方法会将一个元素添加到数组末尾。unshift() 方法可以将元素添加在数组的开头

### 2.4.2 从数组中删除元素
使用 pop() 方法可以删除数组末尾的元素。shift() 方法可以删除数组的第一个元素  
pop() 和 shift() 方法都将删掉的元素作为方法的 返回值返回，因此可以使用一个变量来保存删除的元素


### 2.4.3 从数组中间位置添加和删除元素 

删除数组中的第一个元素和在数组开头添加一个元素存在同样的问题——两种操作都需要将
数组中的剩余元素向前或向后移，然而 splice() 方法可以帮助我们执行其中任何一种操作。 使用 splice() 方法为数组添加元素，需提供如下参数:  

+ 起始索引(也就是你希望开始添加元素的地方);
+ 需要删除的元素个数(添加元素时该参数设为 0); 
+ 想要添加进数组的元素。
看一个简单的例子。下面的程序在数组中间插入元素:

```javascript
var nums = [1,2,3,7,8,9];
var newElements = [4,5,6];
nums.splice(3,0,newElements);
print(nums); // 1,2,3,4,5,6,7,8,9
     
nums.splice(3,0,4,5,6);
splice() 方法从数组中删除元素的例子
nums.splice(3,4);
```

### 2.5 迭代器方法
最后一组方法是迭代器方法。这些方法对数组中的每个元素应用一个函数，可以返回一个 值、一组值或者一个新数组。

### 2.5.1 不生成新数组的迭代器方法 

我们要讨论的第一组迭代器方法不产生任何新数组，相反，它们要么对于数组中的每个元
素执行某种操作，要么返回一个值。
这组中的第一个方法是 forEach()，该方法接受一个函数作为参数，对数组中的每个元素 使用该函数。

另一个迭代器方法是 every()，该方法接受一个返回值为布尔类型的函数，对数组中的每 个元素使用该函数。如果对于所有的元素，该函数均返回 true，则该方法返回 true。下面 是一个例子:

```javascript
function isEven(num) {
    return num % 2 == 0;
}
var nums = [2,4,6,8,10];
var even = nums.every(isEven);
```    
 
some() 方法也接受一个返回值为布尔类型的函数，只要有一个元素使得该函数返回 true，
该方法就返回 true

### 2.5.2 生成新数组的迭代器方法
有两个迭代器方法可以产生新数组:map() 和 filter()。

map() 和 forEach() 有点儿像，对 数组中的每个元素使用某个函数。两者的区别是 map() 返回一个新的数组，该数组的元素 是对原有元素应用某个函数得到的结果。

```javascript
function curve(grade) {
   return grade += 5;
}
var grades = [77, 65, 81, 92, 83];
var newgrades = grades.map(curve);
print(newgrades); // 82, 70, 86, 97, 88
```

filter() 和 every() 类似，传入一个返回值为布尔类型的函数。和 every() 方法不同的是， 当对数组中的所有元素应用该函数，结果均为 true 时，该方法并不返回 true，而是返回 一个新数组，该数组包含应用该函数后结果为 true 的元素。

```javascript
function passing(num) {
	return num >= 60;
}

var grades = [];
for (var i = 0; i < 20; ++i) {
 grades[i] = Math.floor(Math.random() * 101);
}
var passGrades = grades.filter(passing);
```

## 10章，二叉树，109


有三种遍历 BST 的方式:中序、先序和后序。中序遍历按照节点上的键值，以升序访问 BST 上的所有节点。先序遍历先访问根节点，然后以同样方式访问左子树和右子树。后序 遍历先访问叶子节点，从左子树到右子树，再到根节点。


中序： 左，根，右	
先序：根，左，右	
后续：左，右，根	


## 12章，排序算法

插入、冒泡、选择， 平均都是 O（n^2）

快排，堆，归并，平均都是 O(nlogn)


快速排序的算法和伪代码  
快速排序的算法如下:  
(1) 选择一个基准元素，将列表分隔成两个子序列;  
(2) 对列表重新排序，将所有小于基准值的元素放在基准值的前面，所有大于基准值的元
素放在基准值的后面;  
(3) 分别对较小元素的子序列和较大元素的子序列重复步骤 1 和 2。  
这个算法的 JavaScript 程序如下所示:  

```javascript
 function qSort(list) {
     if (list.length == 0) {
		return []; 
	  }
     var lesser = [];
     var greater = [];
     var pivot = list[0];
     for (var i = 1; i < list.length; i++) {
         if (list[i] < pivot) {
            lesser.push(list[i]);
         } else {
            greater.push(list[i]);
} }
     return qSort(lesser).concat(pivot, qSort(greater));
 }
```
## 13章 检索算法


### 13.2 二分查找算法
我们可以将这个策略实现为二分查找算法。这个算法只对有序的数据集有效。算法描述
如下。  
(1) 将数组的第一个位置设置为下边界(0)。  
(2) 将数组最后一个元素所在的位置设置为上边界(数组的长度减 1)。   
(3) 若下边界等于或小于上边界，则做如下操作。  
a. 将中点设置为(上边界加上下边界)除以 2。  
b. 如果中点的元素小于查询的值，则将下边界设置为中点元素所在下标加 1。 c. 如果中点的元素大于查询的值，则将上边界设置为中点元素所在下标减 1。 d. 否则中点元素即为要查找的数据，可以进行返回。

```javascript
function binSearch(arr, data) {
    var upperBound = arr.length-1;
    var lowerBound = 0;
    while (lowerBound <= upperBound) {
		var mid = Math.floor((upperBound + lowerBound) / 2); 
		if (arr[mid] < data) {
          lowerBound = mid + 1;
      }
      else if (arr[mid] > data) {
          upperBound = mid - 1;
		}
		else {
			return mid; 
		}
	}
	return -1; 
}
```

计算重复次数
当 binSearch() 函数找到某个值时，如果在数据集中还有其他相同的值出现，那么该函数 会定位在类似值的附近。换句话说，其他相同的值可能会出现已找到值的左边或右边。

```javascript
 function count(arr, data) {
    var count = 0;
    var position = binSearch(arr, data);
    if (position > -1) {
       ++count;
       for (var i = position-1; i > 0; --i) {
          if (arr[i] == data) {
             ++count;
			}
			else {
				break; 
			}
		}
		for (var i = position+1; i < arr.length; ++i) {
			if (arr[i] == data) {
				++count;
			}
			else {
				break; 
			}
		} 
	}
	return count;
}
```
## 14章，高级算法，187





todo

14.1.2 寻找最长公共子串