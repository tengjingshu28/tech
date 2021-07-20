# JDK�汾

> JDK 1.8.0_221

# �������

ǰ����Java *ArrayDeque*Ϊ��������*Stack*��*Queue*����ʵ����һ������Ķ��н���*PriorityQueue*�������ȶ��С�**���ȶ��е��������ܱ�֤ÿ��ȡ����Ԫ�ض��Ƕ�����Ȩֵ��С��**��Java�����ȶ���ÿ��ȡ��СԪ�أ�C++�����ȶ���ÿ��ȡ���Ԫ�أ�������ǣ�浽�˴�С��ϵ��**Ԫ�ش�С�����п���ͨ��Ԫ�ر������Ȼ˳��*natural ordering*����Ҳ����ͨ������ʱ����ıȽ���**��*Comparator*��������C++�ķº�������

Java��*PriorityQueue*ʵ����*Queue*�ӿڣ����������`null`Ԫ�أ���ͨ����ʵ�֣�����˵��ͨ����ȫ��������*complete binary tree*��ʵ�ֵ�**С����**������һ����Ҷ�ӽڵ��Ȩֵ�����������������ӽڵ��Ȩֵ����Ҳ����ζ�ſ���ͨ����������Ϊ*PriorityQueue*�ĵײ�ʵ�֡�

![](images/5_Collection_PriorityQueue/PriorityQueue_1.png)

��ͼ�����Ǹ�ÿ��Ԫ�ذ��ղ�������ķ�ʽ�����˱�ţ�������㹻ϸ�ģ��ᷢ�ָ��ڵ���ӽڵ�ı��������ϵ�ģ���ȷ�е�˵���ӽڵ�ı��֮�������¹�ϵ��

```
leftNo = parentNo*2+1
rightNo = parentNo*2+2
parentNo = (nodeNo-1)/2
```

ͨ������������ʽ���������׼����ĳ���ڵ�ĸ��ڵ��Լ��ӽڵ���±ꡣ��Ҳ����Ϊʲô����ֱ�����������洢�ѵ�ԭ��

*PriorityQueue*��`peek()`��`element`�����ǳ���ʱ�䣬`add()`, `offer()`, �޲�����`remove()`�Լ�`poll()`������ʱ�临�Ӷȶ���*log(N)*��

# ��������

## add()��offer()

`add(E e)`��`offer(E e)`��������ͬ�����������ȶ����в���Ԫ�أ�ֻ��`Queue`�ӿڹ涨���߶Բ���ʧ��ʱ�Ĵ���ͬ��ǰ���ڲ���ʧ��ʱ�׳��쳣��������᷵��`false`������*PriorityQueue*������������ʵûʲô���

![](images/5_Collection_PriorityQueue/PriorityQueue_2.png)

�¼����Ԫ�ؿ��ܻ��ƻ�С���ѵ����ʣ������Ҫ���б�Ҫ�ĵ�����

```java
    /**
     * Inserts the specified element into this priority queue.
     *
     * @return {@code true} (as specified by {@link Queue#offer})
     * @throws ClassCastException if the specified element cannot be
     *         compared with elements currently in this priority queue
     *         according to the priority queue's ordering
     * @throws NullPointerException if the specified element is null
     */
    public boolean offer(E e) {
        if (e == null)//���������nullԪ��
            throw new NullPointerException();
        modCount++;
        int i = size;
        if (i >= queue.length)
            grow(i + 1);//�Զ�����
        size = i + 1;
        if (i == 0)//����ԭ��Ϊ�գ����ǲ���ĵ�һ��Ԫ��
            queue[0] = e;
        else
            siftUp(i, e);//����
        return true;
    }
```

���������У����ݺ���`grow()`������`ArrayList`���`grow()`����������������һ����������飬����ԭ�����Ԫ�ظ��ƹ�ȥ�����ﲻ��׸������Ҫע�����`siftUp(int k, E x)`�������÷������ڲ���Ԫ��`x`��ά�ֶѵ����ԡ�

```java
    /**
     * Inserts item x at position k, maintaining heap invariant by
     * promoting x up the tree until it is greater than or equal to
     * its parent, or is the root.
     *
     * To simplify and speed up coercions and comparisons. the
     * Comparable and Comparator versions are separated into different
     * methods that are otherwise identical. (Similarly for siftDown.)
     *
     * @param k the position to fill
     * @param x the item to insert
     */
    private void siftUp(int k, E x) {
        if (comparator != null)
            siftUpUsingComparator(k, x);
        else
            siftUpComparable(k, x);
    }

    @SuppressWarnings("unchecked")
    private void siftUpComparable(int k, E x) {
        Comparable<? super E> key = (Comparable<? super E>) x;
        while (k > 0) {
            int parent = (k - 1) >>> 1; //parentNo = (nodeNo-1)/2
            Object e = queue[parent];
            if (key.compareTo((E) e) >= 0) //���ñȽ����ıȽϷ���
                break;
            queue[k] = e;
            k = parent;
        }
        queue[k] = key;
    }

    @SuppressWarnings("unchecked")
    private void siftUpUsingComparator(int k, E x) {
        while (k > 0) {
            int parent = (k - 1) >>> 1; //parentNo = (nodeNo-1)/2
            Object e = queue[parent];
            if (comparator.compare(x, (E) e) >= 0) //���ñȽ����ıȽϷ���
                break;
            queue[k] = e;
            k = parent;
        }
        queue[k] = x;
    }
```

�¼����Ԫ��`x`���ܻ��ƻ�С���ѵ����ʣ������Ҫ���е����������Ĺ���Ϊ��**��`k`ָ����λ�ÿ�ʼ����`x`����뵱ǰ���`parent`���бȽϲ�������ֱ������`x >= queue[parent]`Ϊֹ**��ע������ıȽϿ�����Ԫ�ص���Ȼ˳��Ҳ�����������Ƚ�����˳��

## element()��peek()

`element()`��`peek()`��������ȫ��ͬ�����ǻ�ȡ����ɾ������Ԫ�أ�Ҳ���Ƕ�����Ȩֵ��С���Ǹ�Ԫ�أ�����Ψһ�������ǵ�����ʧ��ʱǰ���׳��쳣�����߷���`null`������С���ѵ����ʣ��Ѷ��Ǹ�Ԫ�ؾ���ȫ����С���Ǹ������ڶ��������ʾ�������±��ϵ��`0`�±괦���Ǹ�Ԫ�ؼ��ǶѶ�Ԫ�ء�����**ֱ�ӷ�������`0`�±괦���Ǹ�Ԫ�ؼ���**��

![](images/5_Collection_PriorityQueue/PriorityQueue_3.png)

����Ҳ�ͷǳ���ࣺ

```java
    @SuppressWarnings("unchecked")
    public E peek() {
        return (size == 0) ? null : (E) queue[0]; //0�±괦���Ǹ�Ԫ�ؾ�����С���Ǹ�
    }
```

## remove()��poll()

`remove()`��`poll()`����������Ҳ��ȫ��ͬ�����ǻ�ȡ��ɾ������Ԫ�أ������ǵ�����ʧ��ʱǰ���׳��쳣�����߷���`null`������ɾ��������ı���еĽṹ��Ϊά��С���ѵ����ʣ���Ҫ���б�Ҫ�ĵ�����

![](images/5_Collection_PriorityQueue/PriorityQueue_4.png)

�������£�

```java
    public E poll() {
        if (size == 0)
            return null;
        int s = --size;
        modCount++;
        E result = (E) queue[0];//0�±괦���Ǹ�Ԫ�ؾ�����С���Ǹ�
        E x = (E) queue[s];
        queue[s] = null;
        if (s != 0)
            siftDown(0, x);//����
        return result;
    }
```

�����������ȼ�¼`0`�±괦��Ԫ�أ��������һ��Ԫ���滻`0`�±�λ�õ�Ԫ�أ�֮�����`siftDown()`�����Զѽ��е�������󷵻�ԭ��`0`�±괦���Ǹ�Ԫ�أ�Ҳ������С���Ǹ�Ԫ�أ����ص���`siftDown(int k, E x)`�������÷�����������**��`k`ָ����λ�ÿ�ʼ����`x`��������뵱ǰ������Һ����н�С���Ǹ�������ֱ��`x`С�ڻ�������Һ����е��κ�һ��Ϊֹ**��

```java
    /**
     * Inserts item x at position k, maintaining heap invariant by
     * demoting x down the tree repeatedly until it is less than or
     * equal to its children or is a leaf.
     *
     * @param k the position to fill
     * @param x the item to insert
     */
    private void siftDown(int k, E x) {
        if (comparator != null)
            siftDownUsingComparator(k, x);
        else
            siftDownComparable(k, x);
    }

    @SuppressWarnings("unchecked")
    private void siftDownComparable(int k, E x) {
        Comparable<? super E> key = (Comparable<? super E>)x;
        int half = size >>> 1;        // loop while a non-leaf
        while (k < half) {
            //�����ҵ����Һ����н�С���Ǹ�����¼��c�����child��¼���±�
            int child = (k << 1) + 1; // assume left child is least, leftNo = parentNo*2+1
            Object c = queue[child];
            int right = child + 1;
            if (right < size &&
                ((Comparable<? super E>) c).compareTo((E) queue[right]) > 0)
                c = queue[child = right];
            if (key.compareTo((E) c) <= 0)
                break;
            queue[k] = c; //Ȼ����cȡ��ԭ����ֵ
            k = child;
        }
        queue[k] = key;
    }

    @SuppressWarnings("unchecked")
    private void siftDownUsingComparator(int k, E x) {
        int half = size >>> 1;
        while (k < half) {
            int child = (k << 1) + 1; // leftNo = parentNo*2+1
            Object c = queue[child];
            int right = child + 1;
            if (right < size &&
                comparator.compare((E) c, (E) queue[right]) > 0)
                c = queue[child = right];
            if (comparator.compare(x, (E) c) <= 0)
                break;
            queue[k] = c; //Ȼ����cȡ��ԭ����ֵ
            k = child;
        }
        queue[k] = x;
    }
```

## remove(Object o)

`remove(Object o)`��������ɾ�������и�`o`��ȵ�ĳһ��Ԫ�أ�����ж����ȣ�ֻɾ��һ�������÷�������*Queue*�ӿ��ڵķ���������*Collection*�ӿڵķ���������ɾ��������ı���нṹ������Ҫ���е�����������ɾ��Ԫ�ص�λ�ÿ���������ģ����Ե������̱����������Լӷ�����������˵��`remove(Object o)`���Է�Ϊ2�������1. ɾ���������һ��Ԫ�ء�ֱ��ɾ�����ɣ�����Ҫ������2. ɾ���Ĳ������һ��Ԫ�أ���ɾ���㿪ʼ�����һ��Ԫ��Ϊ���յ���һ��`siftDown()`���ɡ��˴�����׸����

![](images/5_Collection_PriorityQueue/PriorityQueue_5.png)

����������£�

```java
    /**
     * Removes a single instance of the specified element from this queue,
     * if it is present.  More formally, removes an element {@code e} such
     * that {@code o.equals(e)}, if this queue contains one or more such
     * elements.  Returns {@code true} if and only if this queue contained
     * the specified element (or equivalently, if this queue changed as a
     * result of the call).
     *
     * @param o element to be removed from this queue, if present
     * @return {@code true} if this queue changed as a result of the call
     */
    public boolean remove(Object o) {
        //ͨ����������ķ�ʽ�ҵ���һ������o.equals(queue[i])Ԫ�ص��±�
        int i = indexOf(o);
        if (i == -1)
            return false;
        else {
            removeAt(i);
            return true;
        }
    }

    /**
     * Removes the ith element from queue.
     *
     * Normally this method leaves the elements at up to i-1,
     * inclusive, untouched.  Under these circumstances, it returns
     * null.  Occasionally, in order to maintain the heap invariant,
     * it must swap a later element of the list with one earlier than
     * i.  Under these circumstances, this method returns the element
     * that was previously at the end of the list and is now at some
     * position before i. This fact is used by iterator.remove so as to
     * avoid missing traversing elements.
     */
    @SuppressWarnings("unchecked")
    private E removeAt(int i) {
        // assert i >= 0 && i < size;
        modCount++;
        int s = --size;
        if (s == i) // removed last element
            queue[i] = null;
        else {
            E moved = (E) queue[s];
            queue[s] = null;
            siftDown(i, moved);
            if (queue[i] == moved) {
                siftUp(i, moved);
                if (queue[i] != moved)
                    return moved;
            }
        }
        return null;
    }
```

# �ο�

https://github.com/CarpenterLee/JCFInternals/blob/master/markdown/8-PriorityQueue.md