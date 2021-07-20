# JDK版本

> JDK 1.8.0_221

# 前言

Java里有一个叫做*Stack*的类，却没有叫做*Queue*的类（它是个接口名字）。当需要使用栈时，Java已不推荐使用*Stack*，而是推荐使用更高效的*ArrayDeque*；既然*Queue*只是一个接口，当需要使用队列时也就首选*ArrayDeque*了（次选是*LinkedList*）。

# 总体介绍

## Queue

在处理元素前用于保存元素的 collection。除了基本的 `Collection` 操作外，队列还提供其他的插入、提取和检查操作。每个方法都存在两种形式：一种抛出异常（操作失败时），另一种返回一个特殊值（`null` 或  `false`，具体取决于操作）。插入操作的后一种形式是用于专门为有容量限制的 `Queue`  实现设计的；在大多数实现中，插入操作不会失败。 

|          | *抛出异常* | *返回特殊值*|
| -------- | ---------- | ----------- |
| **插入** | `add(e)`    | `offer(e)` |
| **移除** | `remove()`  | `poll()`   |
| **检查** | `element()` | `peek()`   |

## Deque

一个线性 collection，支持在两端插入和移除元素。名称 *deque* 是“double ended  queue（双端队列）”的缩写，通常读为“deck”。大多数 `Deque`  实现对于它们能够包含的元素数没有固定限制，但此接口既支持有容量限制的双端队列，也支持没有固定大小限制的双端队列。 

此接口定义在双端队列两端访问元素的方法。提供插入、移除和检查元素的方法。每种方法都存在两种形式：一种形式在操作失败时抛出异常，另一种形式返回一个特殊值（`null`  或 `false`，具体取决于操作）。插入操作的后一种形式是专为使用有容量限制的 `Deque`  实现设计的；在大多数实现中，插入操作不能失败。 

|          | **第一个元素（头部）**|           | **最后一个元素（尾部）**|     |
| -------- | --------------- | --------------- |--------------- |--------------- |
|          | *抛出异常*      | *特殊值*        | *抛出异常*     | *特殊值*       |
| **插入** | `addFirst(e)`   | `offerFirst(e)` | `addLast(e)`   | `offerLast(e)` |
| **移除** | `removeFirst()` | `pollFirst()`   | `removeLast()` | `pollLast()`   |
| **检查** | `getFirst()`    | `peekFirst()`   | `getLast()`    | `peekLast()`   |

此接口扩展了 `Queue` 接口。在将双端队列用作队列时，将得到  FIFO（先进先出）行为。将元素添加到双端队列的末尾，从双端队列的开头移除元素。从 `Queue` 接口继承的方法完全等效于  `Deque` 方法，如下表所示：

| **`Queue` 方法**| **等效 `Deque` 方法**| 说明                                   |
| --------------- | --------------------- | -------------------------------------- |
| `add(e)`        | `addLast(e)`          | 向队尾插入元素，失败则抛出异常         |
| `offer(e)`      | `offerLast(e)`        | 向队尾插入元素，失败则返回`false`      |
| `remove()`      | `removeFirst()`       | 获取并删除队首元素，失败则抛出异常     |
| `poll()`        | `pollFirst()`         | 获取并删除队首元素，失败则返回`null`   |
| `element()`     | `getFirst()`          | 获取但不删除队首元素，失败则抛出异常   |
| `peek()`        | `peekFirst()`         | 获取但不删除队首元素，失败则返回`null` |


下表列出了*Deque*与*Stack*对应的接口：

| Stack Method | Equivalent Deque Method | 说明                                   |
| ------------ | ----------------------- | -------------------------------------- |
| `push(e)`    | `addFirst(e)`           | 向栈顶插入元素，失败则抛出异常         |
| 无           | `offerFirst(e)`         | 向栈顶插入元素，失败则返回`false`      |
| `pop()`      | `removeFirst()`         | 获取并删除栈顶元素，失败则抛出异常     |
| 无           | `pollFirst()`           | 获取并删除栈顶元素，失败则返回`null`   |
| `peek()`     | `peekFirst()`           | 获取但不删除栈顶元素，失败则抛出异常   |
| 无           | `peekFirst()`           | 获取但不删除栈顶元素，失败则返回`null` |

上面两个表共定义了*Deque*的12个接口。添加，删除，取值都有两套接口，它们功能相同，区别是对失败情况的处理不同。**一套接口遇到失败就会抛出异常，另一套遇到失败会返回特殊值（`false`或`null`）**。除非某种实现对容量有限制，大多数情况下，添加操作是不会失败的。**虽然*Deque*的接口有12个之多，但无非就是对容器的两端进行操作，或添加，或删除，或查看**。明白了这一点讲解起来就会非常简单。

*ArrayDeque*和*LinkedList*是*Deque*的两个通用实现，由于官方更推荐使用*AarryDeque*用作栈和队列，加之上一篇已经讲解过*LinkedList*，本文将着重讲解*ArrayDeque*的具体实现。

从名字可以看出*ArrayDeque*底层通过数组实现，为了满足可以同时在数组两端插入或删除元素的需求，该数组还必须是循环的，即**循环数组（circular array）**，也就是说数组的任何一点都可能被看作起点或者终点。*ArrayDeque*是非线程安全的（not thread-safe），当多个线程同时使用的时候，需要程序员手动同步；另外，该容器不允许放入`null`元素。

![](images/4_Collection_Stack_&_Queue/Stack_&_Queue_1.png)

上图中我们看到，**`head`指向首端第一个有效元素，`tail`指向尾端第一个可以插入元素的空位**。因为是循环数组，所以`head`不一定总等于0，`tail`也不一定总是比`head`大。

# 方法剖析

## addFirst()

`addFirst(E e)`的作用是在*Deque*的首端插入元素，也就是在`head`的前面插入元素，在空间足够且下标没有越界的情况下，只需要将`elements[--head] = e`即可。

![](images/4_Collection_Stack_&_Queue/Stack_&_Queue_2.png)

实际需要考虑：1.空间是否够用，以及2.下标是否越界的问题。上图中，如果`head`为`0`之后接着调用`addFirst()`，虽然空余空间还够用，但`head`为`-1`，下标越界了。下列代码很好的解决了这两个问题。

```java
//addFirst(E e)
public void addFirst(E e) {
    if (e == null)//不允许放入null
        throw new NullPointerException();
    elements[head = (head - 1) & (elements.length - 1)] = e;//2.下标是否越界
    if (head == tail)//1.空间是否够用
        doubleCapacity();//扩容
}
```

上述代码我们看到，**空间问题是在插入之后解决的**，因为`tail`总是指向下一个可插入的空位，也就意味着`elements`数组至少有一个空位，所以插入元素的时候不用考虑空间问题。

下标越界的处理解决起来非常简单，`head = (head - 1) & (elements.length - 1)`就可以了，**这段代码相当于取余，同时解决了`head`为负值的情况**。因为`elements.length`必需是`2`的指数倍，`elements - 1`就是二进制低位全`1`，跟`head - 1`相与之后就起到了取模的作用，如果`head - 1`为负数（其实只可能是-1），则相当于对其取相对于`elements.length`的补码。

下面再说说扩容函数`doubleCapacity()`，其逻辑是申请一个更大的数组（原数组的两倍），然后将原数组复制过去。过程如下图所示：

![](images/4_Collection_Stack_&_Queue/Stack_&_Queue_3.png)

图中我们看到，复制分两次进行，第一次复制`head`右边的元素，第二次复制`head`左边的元素。

```java
//doubleCapacity()
private void doubleCapacity() {
    assert head == tail;
    int p = head;
    int n = elements.length;
    int r = n - p; // head右边元素的个数
    int newCapacity = n << 1;//原空间的2倍
    if (newCapacity < 0)
        throw new IllegalStateException("Sorry, deque too big");
    Object[] a = new Object[newCapacity];
    System.arraycopy(elements, p, a, 0, r);//复制右半部分，对应上图中绿色部分
    System.arraycopy(elements, 0, a, r, p);//复制左半部分，对应上图中灰色部分
    elements = (E[])a;
    head = 0;
    tail = n;
}
```

## addLast()

`addLast(E e)`的作用是在*Deque*的尾端插入元素，也就是在`tail`的位置插入元素，由于`tail`总是指向下一个可以插入的空位，因此只需要`elements[tail] = e;`即可。插入完成后再检查空间，如果空间已经用光，则调用`doubleCapacity()`进行扩容。

![](images/4_Collection_Stack_&_Queue/Stack_&_Queue_4.png)

```java
public void addLast(E e) {
    if (e == null)//不允许放入null
        throw new NullPointerException();
    elements[tail] = e;//赋值
    if ( (tail = (tail + 1) & (elements.length - 1)) == head)//下标越界处理
        doubleCapacity();//扩容
}
```

下标越界处理方式`addFirt()`中已经讲过，不再赘述。

## pollFirst()

`pollFirst()`的作用是删除并返回*Deque*首端元素，也即是`head`位置处的元素。如果容器不空，只需要直接返回`elements[head]`即可，当然还需要处理下标的问题。由于`ArrayDeque`中不允许放入`null`，当`elements[head] == null`时，意味着容器为空。

```java
public E pollFirst() {
    E result = elements[head];
    if (result == null)//null值意味着deque为空
        return null;
    elements[h] = null;//let GC work
    head = (head + 1) & (elements.length - 1);//下标越界处理
    return result;
}
```

## pollLast()

`pollLast()`的作用是删除并返回*Deque*尾端元素，也即是`tail`位置前面的那个元素。

```java
public E pollLast() {
    int t = (tail - 1) & (elements.length - 1);//tail的上一个位置是最后一个元素
    E result = elements[t];
    if (result == null)//null值意味着deque为空
        return null;
    elements[t] = null;//let GC work
    tail = t;
    return result;
}
```

## peekFirst()

`peekFirst()`的作用是返回但不删除*Deque*首端元素，也即是`head`位置处的元素，直接返回`elements[head]`即可。

```java
public E peekFirst() {
    return elements[head]; // elements[head] is null if deque empty
}
```

## peekLast()

`peekLast()`的作用是返回但不删除*Deque*尾端元素，也即是`tail`位置前面的那个元素。

```java
public E peekLast() {
    return elements[(tail - 1) & (elements.length - 1)];
}
```

# 参考

https://github.com/CarpenterLee/JCFInternals/blob/master/markdown/4-Stack%20and%20Queue.md

https://www.pdai.tech/md/java/collection/java-collection-Queue&Stack.html