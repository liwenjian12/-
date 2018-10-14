# Python实现典型数据结构

​	python实现常用数据结构，包含属性和动作。属性：结构、value、left、right。动作：count、search、update、new、delete、遍历、判断是否empty。

[TOC]

## 线性结构

1. 有唯一的首元素及尾元素；

2. 数据元素之间是一对一关系 ；
3. 除首元素和尾元素外，每个元素都只有唯一的前段和后端（我词真穷） 

样例：线性表，堆，栈，队列，数组（基本指一维的），链表

顺序表和链式表都可用于实现上述结构

![20180303185957621](C:\Users\wenjian\Desktop\新建文件夹\20180303185957621.png)

### 栈

​	使用python内置List数据类型实现 ；

​	栈（stack），有些地方称为堆栈，是一种容器，可存入数据元素、访问元素、删除元素，它的特点在于只能允许在容器的一端（称为栈顶端指标，英语：top）进行加入数据（英语：push）和输出数据（英语：pop）的运算。没有了位置概念，保证任何时候可以访问、删除的元素都是此前最后存入的那个元素，确定了一种默认的访问顺序。

由于栈数据结构只允许在一端进行操作，因而按照后进先出（LIFO, Last In First Out）的原理运作。

***如何判断元素的进出顺序***

```
# 栈：List实现
stack = [3, 4, 5]

# 动作
stack.append(6) # 顶部添加元素
stack.pop()     # 顶部弹出元素，该操作返回顶部元素
for i in range(len(stack)):  # count 遍历
	print(stack[i])   
```

### 队列

​	队列（queue）是只允许在一端进行插入操作，而在另一端进行删除操作的线性表。队列是一种先进先出的（First In First Out）的线性表，简称FIFO。允许插入的一端为队尾，允许删除的一端为队头。**队列不允许在中间部位进行操作！**

​	使用collections库中内置的deque对象；

​	也可使用List实现；

**deque实现**

```python
# 队列： deque实现
from collections import deque
# 动作
queue = deque(["Eric", "John", "Michael"])   # 新建队列
queue.append("Terry")                        # 队列尾部添加元素
queue.popleft()                              # 队首（弹出）删除元素

```

**List实现**

```python
# 属性
items = []
# 动作
is_empty = (items == [])   # 判断是否为空
items.insert(0,newitem)    # enqueue：进队列
items.pop()                # dequeue: 出队列
size = len(items)          # 返回队列的大小
for i in range(size):      # 遍历
    print(items[i])        

```

### 双端队列

​	双端队列（deque，全名double-ended queue），是一种具有队列和栈的性质的数据结构。双端队列中的元素可以从两端弹出，其限定插入和删除操作在表的两端进行。双端队列可以在队列任意一端入队和出队。![20180611151046442](C:\Users\wenjian\Desktop\新建文件夹\20180611151046442.png)

* Deque() 创建一个空的双端队列
*  add_front(item) 从队头加入一个item元素
* add_rear(item) 从队尾加入一个item元素
* remove_front() 从队头删除一个item元素
* remove_rear() 从队尾删除一个item元素
* is_empty() 判断双端队列是否为空
* size() 返回队列的大小

**List实现**

```python
# 属性
items = []
is_empty = (items == [])        # 是否为空
items.insert(0,newitem)         # 在队头添加元素add_front
items.append(newitem)           # 在队尾添加元素add_rear
items.pop(0)                    # 在队头删除元素remove_front
items.pop()                     # 在队尾删除元素remove_rear
size = len(items)               # 返回队列大小
```



### 链表

#### 单链表

类实现；节点Node类；ChainTable类；

![1539521231525](C:\Users\wenjian\AppData\Roaming\Typora\typora-user-images\1539521231525.png)

```python
class Node(object):
    def __init__(self, data, pnext = None):
        # 属性
        self.data = data
        self._next = pnext
    def __repr__(self):
        return str(self.data)     # 返回节点的值
class ChainTable(object):
    def __init__(self):
        self.head = None
        self.length = 0
        
    def is_empty(self):          # 只有头节点不算为空
        return (self.length==0)
    
    def append(self, dataOrNode):
        item = None
        if isinstance(dataOrNode, Node):   #对象dataOrNode是否为Node类型
            item = dataOrNode
        else:
            item = Node(dataOrNode)

        if not self.head:                 # 若链表的头节点为空    
            self.head = item
            self.length += 1

        else:
            node = self.head
            while node._next:           # while循环一直找到最后一个节点，item 添加到最末尾
                node = node._next
            node._next = item
            self.length += 1
     def delete(self, index):
        if self.is_empty():
            print("this chain table is empty.")
            return
        if index < 0 or index >= self.length:
            print 'error: out of index'
            return

        if index == 0:
            self.head = self.head._next
            self.length -= 1
            return
        j = 0
        node = self.head
        prev = self.head
        while node._next and j < index:
            prev = node
            node = node._next
            j += 1

        if j == index:
            prev._next = node._next
            self.length -= 1
    
    def insert(self, index, dataOrNode):
        if self.isEmpty():
            print "this chain tabale is empty"
            return

        if index < 0 or index >= self.length:
            print "error: out of index"
            return

        item = None
        if isinstance(dataOrNode, Node):
            item = dataOrNode
        else:
            item = Node(dataOrNode)

        if index == 0:
            item._next = self.head
            self.head = item
            self.length += 1
            return

        j = 0
        node = self.head
        prev = self.head
        while node._next and j < index:
            prev = node
            node = node._next
            j += 1

        if j == index:
            item._next = node
            prev._next = item
            self.length += 1
            
    def update(self, index, data):
        if self.isEmpty() or index < 0 or index >= self.length:
            print 'error: out of index'
            return
        j = 0
        node = self.head
        while node._next and j < index:
            node = node._next
            j += 1

        if j == index:
            node.data = data
    
    def getItem(self, index):             #返回value属性
        if self.isEmpty() or index < 0 or index >= self.length:
            print "error: out of index"
            return
        j = 0
        node = self.head
        while node._next and j < index:
            node = node._next
            j += 1

        return node.data


    def getIndex(self, data):              # 返回第一个属性相匹配的节点的序号index
        j = 0
        if self.isEmpty():
            print "this chain table is empty"
            return
        node = self.head
        while node:
            if node.data == data:
                return j
            node = node._next
            j += 1

        if j == self.length:
            print "%s not found" % str(data)
            return
        
    def clear(self):
        self.head = None
        self.length = 0

```

#### 双向链表

双向链表也叫双链表，是链表的一种，它的每个数据结点中都有两个指针，分别指向直接后继和直接前驱。所以，从双向链表中的**任意一个结点**开始，都可以很方便地访问它的前驱结点和后继结点。类实现；

* 初始化双向链表

![20151120150312185](C:\Users\wenjian\Desktop\新建文件夹\20151120150312185.jpg)

```python
"""节点类"""
class Node(object):
    def __init__(self, data=None):
        self.data = data
        self.pre = None
        self.next = None
        
"""初始化双向链表"""        
class LinkedListTwoway(object):
    def __init__(self):
        head = Node()
    	tail = Node()
        self.head = head
        self.tail = tail
        self.head.next = self.tail     # 头节点无pre，为节点无next
        self.tail.pre = self.head   
     
    def __len__(self):              # 获取链表长度
        length = 0
        node = self.head
        while node.next != self.tail:
            length += 1
            node = node.next
        return length
```

* 追加节点

![20151120150212210](C:\Users\wenjian\Desktop\新建文件夹\20151120150212210.jpg)

```python
def append(self, data):
	node = Node(data)
    pre = self.tail.pre
    pre.next = node
    node.pre = pre
    self.tail.pre = node
    node.next = self.tail
    return node
# 获取节点
def get(self, index):
    length = len(self)
    index = index if index >= 0 else length + index #获取第index个值，若index>0正向获取else 反向获取
    if index >= length or index < 0: return None
    node = self.head.next
    while index：
    	node = node.next
        index -= 1
    return node

# 设置节点value
def set(self, index, data):
    node = self.get(index)
    if node:
        node.data = data
    return node
```

* 插入节点

![20151120150250201](C:\Users\wenjian\Desktop\新建文件夹\20151120150250201.jpg)

```python
def insert(self, index, data):
	length = len(self)
    if abs(index + 1) > length:
        return False
    index = index if index >= 0 else index + 1 + length
    
    next_node = self.get(index)
    if next_node:
        node = Node(data)
        pre_node = next_node.pre 
        pre_node.next = node 
        node.pre = pre_node 
        node.next = next_node 
        next_node.pre = node 
        return node
# 删除节点
def delete(self, index):
    node = self.get(index)
    if node:
        node.pre.next = node.next
        node.next.pre = node.pre
        return True
    return False
```

* 反转链表

```python
 def __reversed__(self):
 """1.node.next --> node.pre
      node.pre --> node.next
    2.head.next --> None
      tail.pre --> None
    3.head-->tail
     tail-->head""" 
     pre_head = self.head 
     tail = self.tail 
     def reverse(pre_node, node): 
     	if node: 
            next_node = node.next 
            node.next = pre_node 
            pre_node.pre = node 
            if pre_node is self.head: 
                pre_node.next = None 
            if node is self.tail: 
                node.pre = None 
            return reverse(node, next_node) 
        else: 
            self.head = tail 
            self.tail = pre_head 
     return reverse(self.head, self.head.next)

# 清空链表
def clear(self):
    self.head.next = self.tail
    self.tail.pre = self.head

```



## 非线性结构

元素间有多对一和一对多的状态存在。

1. 没有对应关系的，集合结构

2. 一对多的，树结构（层次结构）

3. 多对多的，图结构或网结构（群结构）

4. 还有一个，多维数组。

### 树（Tree）

#### 二叉树

##### 完全二叉树（满二叉树）

##### 二叉搜索树（BST）

##### 平衡二叉树

##### 线索二叉树

#### 堆

##### 二叉堆

##### 二项堆

##### 斐波那契堆

##### 左偏树

#### B树

##### B+树

##### B*树

#### 字典树（Trie）

### 图（Graph）

### 集合结构

### 多维数组

# 顺序表和链式表

顺序表存储（典型的数组）
​     原理：顺序表存储是将数据元素放到一块连续的内存存储空间，相邻数据元素的存放地址也相邻（逻辑与物理统一）。
​     优点：（1）空间利用率高。（局部性原理，连续存放，命中率高） 
​           （2）存取速度高效，通过下标来直接存储。
​     缺点：（1）插入和删除比较慢，比如：插入或者删除一个元素时，整个表需要遍历移动元素来重新排一次顺序。
​           （2）不可以增长长度，有空间限制,当需要存取的元素个数可能多于顺序表的元素个数时,会出现"溢出"问题.当元素个数远少于预先分配的空间时,空间浪费巨大。  
​     时间性能 :查找 O(1) ,插入和删除O（n）。
链表存储
​    原理：链表存储是在程序运行过程中动态的分配空间，只要存储器还有空间，就不会发生存储溢出问题，相邻数据元素可随意存放，但所占存储空间分两部分，一部分存放结点值，另一部分存放表示结点关系间的指针。
​    优点：（1）存取某个元素速度慢。 
​          （2）插入和删除速度快，保留原有的物理顺序，比如：插入或者删除一个元素时，只需要改变指针指向即可。
​          （3）没有空间限制,存储元素的个数无上限,基本只与内存空间大小有关. 
​    缺点：（1）占用额外的空间以存储指针(浪费空间，不连续存放，malloc开辟，空间碎片多) 
​          （2）查找速度慢，因为查找时，需要循环链表访问，需要从开始节点一个一个节点去查找元素访问。
​    时间性能 :查找 O(n) ,插入和删除O（1）。 
频繁的查找却很少的插入和删除操作可以用顺序表存储，堆排序,二分查找适宜用顺序表.
如果频繁的插入和删除操作很少的查询就可以使用链表存储
顺序表适宜于做查找这样的静态操作；链表适宜于做插入、删除这样的动态操作。

若线性表长度变化不大，如果事先知道线性表的大致长度，比如一年12月，一周就是星期一至星期日共七天，且其主要操作是查找，则采用顺序表；若线性表长度变化较大或根本不知道多大时，且其主要操作是插入、删除，则采用链表，这样可以不需要考虑存储空间的大小问题。











































