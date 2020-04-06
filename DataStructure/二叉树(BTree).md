# 名词解释

![二叉树_1](Images/二叉树(BTree)/二叉树_1.png)

## 二叉树的名词解释:

- 根：树顶端的节点称为根。一棵树只有一个根，如果要把一个节点和边的集合称为树，那么从根到其他任何一个节点都必须有且只有一条路径。A是根节点。
- 父节点：若一个节点含有子节点，则这个节点称为其子节点的父节点；B是D的父节点。
- 子节点：一个节点含有的子树的根节点称为该节点的子节点；D是B的子节点。
- 兄弟节点：具有相同父节点的节点互称为兄弟节点；比如上图的D和E就互称为兄弟节点。
- 叶节点：没有子节点的节点称为叶节点，也叫叶子节点，比如上图的E、H、L、J、G都是叶子节点。
- 子树：每个节点都可以作为子树的根，它和它所有的子节点、子节点的子节点等都包含在子树中。
- 节点的层次：从根开始定义，根为第一层，根的子节点为第二层，以此类推。
- 深度：对于任意节点n,n的深度为从根到n的唯一路径长，根的深度为0；
- 高度：对于任意节点n,n的高度为从n到一片树叶的最长路径长，所有树叶的高度为0

深度与高度的区别在于： 深度为根到节点的距离，而高度是节点到叶的距离(记住根深叶高)。

- 结点的度：结点拥有的子树的数目

- 叶子结点：度为0的结点

- 分支结点：度不为0的结点

- 树的度：树中结点的最大的度

- 层次：根结点的层次为1，其余结点的层次等于该结点的双亲结点的层次加1

- 树的高度：树中结点的最大层次

- 森林：0个或多个不相交的树组成。对森林加上一个根，森林即成为树；删去根，树即成为森林

# 分类

  ## 斜树

  所有的结点都只有左子树（左斜树），或者只有右子树（右斜树）。这就是斜树，应用较少

![二叉树_2](Images\二叉树(BTree)\二叉树_2.png)![二叉树_3](Images\二叉树(BTree)\二叉树_3.png)

## 满二叉树

所有的分支结点都存在左子树和右子树，并且所有的叶子结点都在同一层上，这样就是满二叉树。就是完美圆满的意思，关键在于树的平衡。

![二叉树_4](Images\二叉树(BTree)\二叉树_4.png)

根据满二叉树的定义，得到其特点为：

1. 叶子只能出现在最下一层。
2. 非叶子结点度一定是2.
3. 在同样深度的二叉树中，满二叉树的结点个数最多，叶子树最多。

## 完全二叉树

对一棵具有n个结点的二叉树按层序排号，如果编号为i的结点与同样深度的满二叉树编号为i结点在二叉树中位置完全相同，就是完全二叉树。满二叉树必须是完全二叉树，反过来不一定成立。

其中关键点是按层序编号，然后对应查找。

![二叉树_5](Images\二叉树(BTree)\二叉树_5.png)

在上图中，树1，按层次编号5结点没有左子树，有右子树，10结点缺失。树2由于3结点没有字数，是的6,7位置空挡了。树3中结点5没有子树。

![二叉树_6](Images\二叉树(BTree)\二叉树_6.png)

上图就是一个完全二叉树。

结合完全二叉树定义得到其特点：

1. 叶子结点只能出现在最下一层（满二叉树继承而来）
2. 最下层叶子结点一定集中在左 部连续位置。
3. 倒数第二层，如有叶子节点，一定出现在右部连续位置。
4. 同样结点树的二叉树，完全二叉树的深度最小（满二叉树也是对的）。

根据下图加深理解，什么时候是完全二叉树。

![二叉树_7](Images\二叉树(BTree)\二叉树_7.png)

# 二叉树的性质

## 一般二叉树性质

1. 在非空二叉树的i层上，至多有2i-1个节点(i>=1)。通过归纳法论证。
2. 在深度为K的二叉树上最多有2k-1个结点（k>=1)。通过归纳法论证。
3. 对于任何一棵非空的二叉树,如果叶节点个数为n0，度数为2的节点个数为n2，则有: n0 = n2 + 1

> 证明：在一棵二叉树中，除了叶子结点（度为0）之外，就剩下度为2(n2)和1(n1)的结点了。则树的结点总数为T = n0+n1+n2;在二叉树中结点总数为T，而连线数为T-1.所以有：n0+n1+n2-1 = 2*n2 +n1;最后得到n0 = n2+1;

![二叉树_8](Images\二叉树(BTree)\二叉树_8.png)

上图中结点总数是10，n2为4，n1为1，n0为5。

## 完全二叉树性质

a、具有n的结点的完全二叉树的深度为log2n+1.

> 满二叉树是完全二叉树，对于深度为k的满二叉树中结点数量是2k-1 = n，完全二叉树结点数量肯定最多2k-1,同时完全二叉树倒数第二层肯定是满的（倒数第一层有结点，那么倒是第二层序号和满二叉树相同），所以完全二叉树的结点数最少大于少一层的满二叉树，为2k-1-1。
> 根据上面推断得出： 2k-1-1< n=<2k-1，因为结点数Nn为整数那么n<=2k-1可以推出n<=2k ,n>2k-1-1可以推出 n>=2k-1,所以2k-1<n<=2k  。即可得k-1<=log2n<k 而k作为整数因此k=[log2n]+1。

b、如果有一颗有n个节点的完全二叉树的节点按层次序编号，对任一层的节点i（1<=i<=n）有

1.   如果i=1，则节点是二叉树的根，无双亲，如果i>1，则其双亲节点为[i/2]，向下取整
2.   如果2i>n那么节点i没有左孩子，否则其左孩子为2i
3.   如果2i+1>n那么节点没有右孩子，否则右孩子为2i+1

![二叉树_9](Images\二叉树(BTree)\二叉树_9.png)

在上图中验证

第一条：

当i=1时，为根节点。当i>1时，比如结点为7，他的双亲就是7/2= 3；结点9双亲为4.

第二条：

结点6,6*2 = 12>10，所以结点6无左孩子，是叶子结点。结点5，5*2 = 10，左孩子是10,结点4，为8.

第三条：

结点5，2*5+1>10,没有右孩子，结点4，则有右孩子。

## 二叉搜索树

二叉搜索树是一种特殊的二叉树,除了它的子节点不能超过两个以外,它还拥有如下特点:

- 一个节点的左子节点的关键字的值永远小于该节点的值
- 一个节点的右子节点的关键字的值永远大于等于该节点的值

![二叉树_10](Images\二叉树(BTree)\二叉树_10.png)

二叉搜索树关键字的排序方式

从图还可以看出,二叉搜索树的最小值就是它的最左节点的关键字的值,而最大值则是它的最右节点的值.

二叉搜索树的查找、新增、删除的效率为O(logN)(这是理想状态下,如果树是不平衡的效率会降到O(N),后面会介绍).

二叉搜索树之所以效率高就在于:

1. 它的数据是按照上述的有序的方式排列的.
2. 进行新增、查找、删除的时候使用了二分查找法.

# 二叉树的实现

二叉树中数据是保存在一个个的节点中的,下面是保存数据的节点类:

```java
package BinaryTree1;
 
public class Node {
	// 用来进行排序的关键字数组
	int sortData;
 
	// 其他类型的数据
	int other;
 
	// 该节点的左子节点
	Node leftNode;
 
	// 该节点的右子节点
	Node rightNode;
 
	public static void main(String[] args) {
		Node node = new Node();
		System.out.println("node.leftNode = " + node.leftNode);
		System.out.println(node.leftNode);
	}
 
}
```

在二叉搜索树这个类中新增、修改、删除数据:

```java
public class Tree {
 
    // 根节点
    Node root;
 
    public Tree(Node root) {
        this.root = root;
    }
 
// 新增、查找、删除 暂时省略,下面会一一介绍
```

## 新增数据

在二叉树中插入数据的流程如下:

![二叉树_11](Images\二叉树(BTree)\二叉树_11.png)

![二叉树_12](Images\二叉树(BTree)\二叉树_12.png)

```java
	/* 新增数据 */
	public void insertData(Node node) {
		int currentSortData = root.sortData;
		Node currentNode = root;
		Node currentLeftNode = root.leftNode;
		Node currentRightNode = root.rightNode;
		int insertSortData = node.sortData;
		while (true) {
			if (insertSortData < currentSortData) {
				if (currentLeftNode == null) {
					currentNode.leftNode = node;
					break;
				} else {
					currentNode = currentNode.leftNode;
					currentSortData = currentNode.sortData;
				}
			} else {
				if (currentRightNode == null) {
					currentNode.rightNode = node;
					break;
				} else {
					currentNode = currentNode.rightNode;
					currentSortData = currentNode.sortData;
				}
			}
 
		}
		System.out.println("root = " + root);
	}
```

## 查找方法

流程与插入方法类似.

```java
	public void query(int sortData) {
		Node currentNode = root;
		while (true) {
			if (sortData != currentNode.sortData) {
				if (sortData < currentNode.sortData) {
					if (currentNode.leftNode != null) {
						currentNode = currentNode.leftNode;
					} else {
						System.out.println("对不起没有查询到数据");
					}
				} else {
					if (currentNode.rightNode != null) {
						currentNode = currentNode.rightNode;
					} else {
						System.out.println("对不起没有查询到数据");
					}
				}
			} else {
				System.out.println("二叉树中有该数据");
			}
		}
	}
```

## 删除方法

删除节点要分三种情况.

- 删除节点无子节点的情况
- 删除节点有一个子节点的情况
- 删除节点有两个子节点的情况

删除节点无子节点的情况是最简单的,直接将该节点置为null就可以了:

![二叉树_13](Images\二叉树(BTree)\二叉树_13.png)

删除节点有一个子节点的情况:

![二叉树_14](Images\二叉树(BTree)\二叉树_14.png)

![二叉树_15](Images\二叉树(BTree)\二叉树_15.png)

**最复杂的**删除节点有两个子节点的情况,删除流程如下:

![二叉树_16](Images\二叉树(BTree)\二叉树_16.png)

删除后:

![二叉树_17](Images\二叉树(BTree)\二叉树_17.png)

为什么要以这种方式删除节点呢? 再次回顾一下二叉搜索树的特点:

- 一个节点的左子节点的关键字的值永远**小于**该节点的值
- 一个节点的右子节点的关键字的值永远**大于等于**该节点的值

之所以要找删除节点的右子节点的最后一个左节点,是因为**这个值是删除节点的子节点中最小的值**,为了满足上面的这两个特点,所以删除要以这种算法去实现.

```java
	public boolean delete(int deleteData) {
		Node curr = root;
		Node parent = root;
		boolean isLeft = true;
		while (deleteData != curr.sortData) {
			if (deleteData <= curr.sortData) {
				isLeft = true;
				if (curr.leftNode != null) {
					parent = curr;
					curr = curr.leftNode;
				}
			} else {
				isLeft = false;
				if (curr.rightNode != null) {
					parent = curr;
					curr = curr.rightNode;
				}
			}
			if (curr == null) {
				return false;
			}
		}
		// 删除节点没有子节点的情况
		if (curr.leftNode == null && curr.rightNode == null) {
			if (curr == root) {
				root = null;
			} else if (isLeft) {
				parent.leftNode = null;
			} else {
				parent.rightNode = null;
			}
			// 删除节点只有左节点
		} else if (curr.rightNode == null) {
			if (curr == root) {
				root = root.leftNode;
			} else if (isLeft) {
				parent.leftNode = curr.leftNode;
			} else {
				parent.rightNode = curr.leftNode;
			}
			// 如果被删除节点只有右节点
		} else if (curr.leftNode == null) {
			if (curr == root) {
				root = root.rightNode;
			} else if (isLeft) {
				parent.leftNode = curr.rightNode;
			} else {
				parent.rightNode = curr.rightNode;
			}
		} else {
			Node successor = getSuccessor(curr);
			if (curr == root) {
				root = successor;
			} else if (curr == parent.leftNode) {
				parent.leftNode = successor;
			} else {
				parent.rightNode = successor;
			}
			successor.leftNode = curr.leftNode;
		}
		return true;
 
	}
 
	public Node getSuccessor(Node delNode) {
		Node curr = delNode.rightNode;
		Node successor = curr;
		Node sucParent = null;
		while (curr != null) {
			sucParent = successor;
			successor = curr;
			curr = curr.leftNode;
		}
		if (successor != delNode.rightNode) {
			sucParent.leftNode = successor.rightNode;
			successor.rightNode = delNode.rightNode;
		}
		return successor;
	}
```

# 遍历

遍历二叉树中的数据,有三种遍历方式:

- 前序（先序）
- 中序(最常用)
- 后续

**前序、中序和后序三种遍历方式的步骤是相同的,只是顺序不同**.

前序遍历顺序:

- 先输出当前节点
- 再遍历左子节点
- 再遍历右子节点

中序遍历顺序:

- 先遍历左子节点
- 再输出当前节点
- 再遍历右子节点

后序遍历顺序:

- 先遍历左子节点
- 再遍历又子节点
- 再输出当前节点

什么当前节点?什么左右子节点?太抽象!!!!没关系继续看图.

前序遍历输出顺序图:

![二叉树_18](Images\二叉树(BTree)\二叉树_18.png)

中序遍历输出顺序图:

![二叉树_19](Images\二叉树(BTree)\二叉树_19.png)

后序遍历输出顺序图:

![二叉树_20](Images\二叉树(BTree)\二叉树_20.png)

可以看出所谓的前中后序是输出**当前节点的顺序**,前序是在第一个输出当前节点,中序是第二个输出当前节点,后序是第三个当前节点.

又因为中序遍历是按照关键值由小到大的顺序输出的,所以**中序遍历最为常用**.

前、后序遍历在解析或分析二叉树(不是二叉搜索树)的算术表达式的时候比较有用,用的不太多,看下图:

![二叉树_21](Images\二叉树(BTree)\二叉树_21.png)

# 二叉树的效率

我们用二叉树与数组和链表进行对比,在有100w个数据项的无序数组或链表中,查找数据项平均会比较50w次,**但在有100w个节点的树中,只需要20(或更少)次的比较**.

有序数组可以很快的找到数据项,但插入数据项平均需要移动50w个数据项,**在100w个节点的树中插入数据项需要比较20或更少次的比较,再加上很短的时间来连接数据项.**

同样,从有100w个数据项的数组中删除一个数据项需要平均移动50w个数据项,**而在100w个节点的树中删除节点只需要20次或更少的比较来找到它,再加上(可能的话)一点比较的时间来找到它的后继,一点时间来断开这个节点的链接,以及连接它的后继.**

结论: **树对所有常用的数据存储操作都有很高的效率**

遍历不如其他操作快. 但是,遍历在大型数据库中不是常用的操作.它更长用于程序中的辅助方法来解析算术或其他的表达式,而且表达式一般都不会很长.

如果二叉树是平衡的,它的效率为: O(logN),如果二叉树是不平衡的(最极端的情况,存入树中的数据是升序或降序排列的,那么二叉树就是链表),效率为: O(N)

所以二叉搜索树在保存随机数值的时候,效率才是最高的

# 二叉树的缺点

如果二叉树是极端不平衡的(此时的二叉树就是一个链表),它的效率为O(N),即使数值是随机的,如果数据的量够大,也有可能有一部分的数值是有序的(就像你抛硬币的时间足够长,会有一段时间出现一只抛正面或反面),造成二叉树会变成使局部不平衡的,这样它的效率会介于O(logN)到O(N).

原文：

https://www.cnblogs.com/xisuo/p/11921647.html

https://www.cnblogs.com/polly333/p/4740355.html

https://blog.csdn.net/xiaoquantouer/article/details/65631708
