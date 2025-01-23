# Python cheat sheet

## **语法**

### 字典

from collections import defaultdict 与 dict.get(键，默认值)

用于创建dict且不限元素是否存在于dict

**用法：**

dict_name=defaultdict(（数据类型，如int，list，dict）)

修改时与dict一样

 

from collections import Counter 一个字典子类，用于计数可哈希对象

**用法：**

dict_name=Counter(（哈希结构，如str，list）)

 

### set

**添加元素：**my_set.add(元素)

**移除元素：**my_set.discard(元素)

**清空集合：**my_set.clear()

**并集（Union）:**my_set=set1 | set2

**交集（Intersection）:**my_set=set1 & set2

**差集（Difference）:**my_set=set1 - set2

**对称差集（Symmetric Difference）:**my_set=set1 ^ set2

**子集和超集检查：**使用 issubset() 和 issuperset() 方法。

例：subset_check = set1.issubset({1, 2, 3, 4, 5}) 最终return true或false

 

 

### list

my_list.append(4)      **# 添加一个元素**

my_list.extend([5, 6])    **# 添加多个元素**

my_list.insert(0, 'start')  **# 在开始处插入元素**

my_list.remove(3)      **# 移除第一个值为3的元素**

popped_element = my_list.pop()   **# 移除并返回最后一个元素**

my_list.clear()       **# 清空列表**

index_of_two = my_list.index(2)  **# 找到2的位置**

count_of_three = my_list.count(3)  **# 数一数3出现了几次**

my_list.sort()  **# 排序**

my_list.reverse()  **# 反转**

new_list = my_list.copy()  **# 浅拷贝**

 

 

### **拷贝**

import copy

**浅拷贝：**

代码：list2=copy.copy(list1)

适用于不需要复制内部对象的情况，或者内部对象是不可变类型（如整数、字符串、元组等）。对于包含可变类型的复杂结构，浅拷贝可能导致意外的行为。

 

**深拷贝：**

代码：list2=copy.deepcopy(list1)

则确保了完全独立的副本，但代价是更高的内存消耗和更长的复制时间，特别是当对象层次结构很深或很大时。

 

 

### **增加默认的递归深度限制**

Import sys

sys.setrecursionlimit(10000)

 



### 同时访问元素及其索引的情况

enumerate()

例：for index, value in enumerate(my_list):

 

 

### 输出n位小数

代码：f"{number:.3f}" 或output=round(number, 3)

 

 

### 生成全排列

代码：from itertools import permutations

for perm in permutations(my_list,(int即排列总长度)):

 

 

### 将字符转化为ASCII码

ord(‘A’)





### 将ASCII码转化为字符

chr(65)

 

 

### Math函数

math.ceil(x): **返回不小于 x 的最小整数。**

math.floor(x): **返回不大于 x 的最大整数。**

math.sqrt(x): **返回 x 的平方根。**

math.pow(x, y): **返回 x 的 y 次幂。**

math.exp(x): **返回 e 的 x 次幂。**

math.log(x[, base]): **返回 x 的自然对数，默认底数为 e。**如果指定了可选参数 base，则返回以 base 为底的对数。

math.sin(x), math.cos(x), math.tan(x): **分别返回 x 弧度的正弦、余弦、正切值。**

math.asin(x), math.acos(x), math.atan(x): **分别返回 x 的反正弦、反余弦、反正切值（结果为弧度）。**

math.degrees(x): **将弧度转换为角度。**

math.radians(x): **将角度转换为弧度。**

math.pi: **数学常数 π (pi)，即圆周率。**

math.e: **数学常数 e，自然对数的底。**

math.fabs(x): **返回 x 的绝对值。**

math.factorial(x): **返回 x 的阶乘。**

math.gcd(a, b): **返回整数 a 和 b 的最大公约数。**

math.comb(n, k)：**组合数**

math.perm(n, k)：**排列数**

 

 

### Bisect函数

```python
import bisect
# 假设我们有一个已经排序的列表
sorted_list = [1, 3, 4, 7, 9]
# 查找插入位置
print(bisect.bisect_left(sorted_list, 5))  # 输出: 3
print(bisect.bisect_right(sorted_list, 5)) # 输出: 3
# 插入元素并保持列表有序
bisect.insort_left(sorted_list, 6)
print(sorted_list)  # 输出: [1, 3, 4, 6, 7, 9]
bisect.insort_right(sorted_list, 2)
print(sorted_list)  # 输出: [1, 2, 3, 4, 6, 7, 9]
```

 

## 算法

### 冒泡排序

```python
def optimized_bubble_sort(arr):
	n = len(arr)
	for i in range(n):
		swapped = False
		for j in range(0, n-i-1):
			if arr[j] > arr[j+1]:
				arr[j], arr[j+1] = arr[j+1], arr[j]
				swapped = True
			# 如果没有发生交换，说明列表已经排好序了
			if not swapped:
				break
	return arr
```



 

### 联通结构（并查集）

```python
def __init__(self, size):
    # 初始化父结点数组，每个结点指向自己
    self.parent = list(range(size))
    # 初始化秩数组，所有结点的初始秩为0
    self.rank = [0] * size
 

	def find(self, p):
    	"""查找p所在的集合，并进行路径压缩"""
    	if self.parent[p] != p:
      		self.parent[p] = self.find(self.parent[p])  # 路径压缩
    	return self.parent[p]
 

  	def union(self, x, y):
    	"""合并x和y所在的集合"""
    	rootX = self.find(x)
    	rootY = self.find(y)
    	if rootX != rootY:
    	  	\# 按秩合并
    	  	if self.rank[rootX] > self.rank[rootY]:
        		self.parent[rootY] = rootX
      		elif self.rank[rootX] < self.rank[rootY]:
        		self.parent[rootX] = rootY
      		else:
        		self.parent[rootY] = rootX
        	self.rank[rootX] += 1
 

	def connected(self, x, y):
    	"""检查x和y是否在同一集合中"""
    	return self.find(x) == self.find(y)
```