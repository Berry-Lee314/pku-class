# Cheatsheet

## 归并排序

```python
def merge_sort(arr):
    if len(arr) > 1:
        # 找到中间点，分割数组
        mid = len(arr) // 2
        left_half = arr[:mid]
        right_half = arr[mid:]

        # 递归排序左右两部分
        merge_sort(left_half)
        merge_sort(right_half)

        # 合并两个有序数组
        i = j = k = 0
        while i < len(left_half) and j < len(right_half):
            if left_half[i] < right_half[j]:
                arr[k] = left_half[i]
                i += 1
            else:
                arr[k] = right_half[j]
                j += 1
            k += 1

        # 处理剩余元素
        while i < len(left_half):
            arr[k] = left_half[i]
            i += 1
            k += 1

        while j < len(right_half):
            arr[k] = right_half[j]
            j += 1
            k += 1

# 示例
arr = [38, 27, 43, 3, 9, 82, 10]
merge_sort(arr)
print("排序后的数组:", arr)
```



## 二叉树的前、中、后序排序

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



## Topological Order

```python
def has_cycle_dfs(graph):
    visited = set()
    rec_stack = set()

    def dfs(u):
        if u in rec_stack:
            return True  # 发现环
        if u in visited:
            return False  # 已经访问过，无需重复处理

        # 标记当前节点为正在访问
        visited.add(u)
        rec_stack.add(u)

        # 递归遍历相邻节点
        for v in graph[u]:
            if dfs(v):
                return True

        # 当前节点访问完成，移出递归栈
        rec_stack.remove(u)
        return False

    # 对每个未访问的节点调用 DFS
    for u in graph:
        if u not in visited:
            if dfs(u):
                return True  # 发现环

    return False  # 无环
```



## 打印列表

```python
my_list=[1,2,3]
print(*my_list, sep="-")
#输出：1-2-3
print(*my_list, sep="\n")
#输出：
#1
#2
#3
print(*my_list, sep="")
#输出：123
```



## 如何处理Global出现的Compile Error

```python
# pylint:skip-file
```



## 建立二叉树并获得根

```python
class TreeNode:
    def __init__(self,val=0,left=None,right=None):
        self.val=val
        self.left=left
        self.right=right

n=int(input())
Tree=list(TreeNode(i) for i in range(n))
findroot=[True for i in range(n)]
for i in range(n):
	left,right=map(int,input().split())
	if left!=-1:
		Tree[i].left=Tree[left]
		findroot[left]=False
	if right!=-1:
		Tree[i].right=Tree[right]
		findroot[right]=False
for i in range(n):
	if findroot[i]:
		root=i
```



## 二叉树最近公共祖先

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if root in (None,p,q):
            return root
        left=self.lowestCommonAncestor(root.left,p,q)
        right=self.lowestCommonAncestor(root.right,p,q)
        if left and right:
            return root
        return left or right   
```



## 3.3.3 哈夫曼编码实现

要构建一个最优的哈夫曼编码树，首先需要对给定的字符及其权值进行排序。然后，通过重复合并权值最小的两个节点（或子树），直到所有节点都合并为一棵树为止。

下面是用 Python 实现的代码：

```python
import heapq

class Node:
    def __init__(self, char, freq):
        self.char = char
        self.freq = freq
        self.left = None
        self.right = None

    def __lt__(self, other):
        return self.freq < other.freq

def huffman_encoding(char_freq):
    heap = [Node(char, freq) for char, freq in char_freq.items()]
    heapq.heapify(heap)

    while len(heap) > 1:
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        merged = Node(None, left.freq + right.freq) # note: 合并之后 char 字典是空
        merged.left = left
        merged.right = right
        heapq.heappush(heap, merged)

    return heap[0]

def external_path_length(node, depth=0):
    if node is None:
        return 0
    if node.left is None and node.right is None:
        return depth * node.freq
    return (external_path_length(node.left, depth + 1) +
            external_path_length(node.right, depth + 1))

def main():
    char_freq = {'a': 3, 'b': 4, 'c': 5, 'd': 6, 'e': 8, 'f': 9, 'g': 11, 'h': 12}
    huffman_tree = huffman_encoding(char_freq)
    external_length = external_path_length(huffman_tree)
    print("The weighted external path length of the Huffman tree is:", external_length)

if __name__ == "__main__":
    main()

# Output:
# The weighted external path length of the Huffman tree is: 169 
```

这段代码首先定义了一个 `Node` 类来表示哈夫曼树的节点。然后，使用最小堆来构建哈夫曼树，每次从堆中取出两个频率最小的节点进行合并，直到堆中只剩下一个节点，即哈夫曼树的根节点。接着，使用递归方法计算哈夫曼树的带权外部路径长度（weighted external path length）。最后，输出计算得到的带权外部路径长度。

你可以运行这段代码来得到该最优二叉编码树的带权外部路径长度。



## KMP算法

```python
# 得到字符串s的前缀值列表
def kmp_next(s):
 # kmp算法计算最⻓相等前后缀
	next = [0] * len(s) j = 0
	for i in range(1, len(s)):
		while s[i] != s[j] and j > 0:
            j = next[j - 1]
		if s[i] == s[j]:
			j += 1
	next[i] = j
	return next
def main():
	case = 0
	while True:
        n = int(input().strip())
        if n == 0:
            break
	s = input().strip()
	case += 1
	print("Test case #{}".format(case))
	next = kmp_next(s)
	for i in range(2, len(s) + 1):
		k = i - next[i - 1] # 可能的重复⼦串的⻓度
		if (i % k == 0) and i // k > 1:
			print(i, i // k)
	print()
```



## dict用法

```python
for key in d:
    print(key)

# 遍历键（显式）
for key in d.keys():
    print(key)

# 遍历值
for value in d.values():
    print(value)

# 遍历键值对
for key, value in d.items():
    print(f"{key}: {value}")
```

