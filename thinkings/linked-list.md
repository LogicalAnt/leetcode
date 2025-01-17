# 几乎刷完了力扣所有的链表题，我发现了这些东西。。。

![](https://tva1.sinaimg.cn/large/0081Kckwly1gki5nbjcgqj31be0u0q5w.jpg)

先上下本文的提纲，这个是我用 mindmap 画的一个脑图，之后我后继续完善，将其他专题逐步完善起来。

> 大家也可以使用 vscode blink-mind 打开源文件查看，里面有一些笔记可以点开查看。源文件可以去我的公众号《力扣加加》回复脑图获取，以后脑图也会持续更新更多内容。vscode 插件地址：https://marketplace.visualstudio.com/items?itemName=awehook.vscode-blink-mind

大家好，我是 lucifer。今天给大家带来的专题是《链表》。很多人觉得链表是一个很难的专题。实际上，只要你掌握了诀窍，它并没那么难。接下来，我们展开说说。

[链表标签](https://leetcode-cn.com/tag/linked-list/ "链表标签")在 leetcode 一共有 **54 道题**。 为了准备这个专题，我花了几天时间将 leetcode 几乎所有的链表题目都刷了一遍。

![](https://tva1.sinaimg.cn/large/0081Kckwly1gki5vhm12jj310y0raadh.jpg)

可以看出，除了六个上锁的，其他我都刷了一遍。而实际上，这六个上锁的也没有什么难度，甚至和其他 48 道题差不多。

通过集中刷这些题，我发现了一些有趣的信息，今天就分享给大家。

<!-- more -->

## 简介

各种数据结构，不管是队列，栈等线性数据结构还是树，图的等非线性数据结构，从根本上底层都是数组和链表。不管你用的是数组还是链表，用的都是计算机内存，物理内存是一个个大小相同的内存单元构成的，如图：

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gfqt71jt4cj30kg0b4wfl.jpg)

（图 1. 物理内存）

而数组和链表虽然用的都是物理内存，都是两者在对物理的使用上是非常不一样的，如图：

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gfqtbpbwmrj31gu0qmtej.jpg)

（图 2. 数组和链表的物理存储图）

不难看出，数组和链表只是使用物理内存的两种方式。

数组是连续的内存空间，通常每一个单位的大小也是固定的，因此可以按下标随机访问。而链表则不一定连续，因此其查找只能依靠别的方式，一般我们是通过一个叫 next 指针来遍历查找。链表其实就是一个结构体。 比如一个可能的单链表的定义可以是：

```ts
interface ListNode<T> {
  data: T;
  next: ListNode<T>;
}
```

data 是数据域，存放数据，next 是一个指向下一个节点的指针。

链表是一种物理存储单元上非连续、非顺序的存储结构，数据元素的逻辑顺序是通过链表中的指针链接次序实现的。链表由一系列结点（链表中每一个元素称为结点）组成，结点可以在运行时动态生成。

从上面的物理结构图可以看出数组是一块连续的空间，数组的每一项都是紧密相连的，因此如果要执行插入和删除操作就很麻烦。对数组头部的插入和删除时间复杂度都是$O(N)$，而平均复杂度也是$O(N)$，只有对尾部的插入和删除才是$O(1)$。简单来说”数组对查询特别友好，对删除和添加不友好“。为了解决这个问题，就有了链表这种数据结构。链表适合在数据需要有一定顺序，但是又需要进行频繁增删除的场景，具体内容参考后面的《链表的基本操作》小节。

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gfigmeqc3xj316o094jt6.jpg)

（图 3. 一个典型的链表逻辑表示图）

> 后面所有的图都是基于逻辑结构，而不是物理结构

链表只有一个后驱节点 next，如果是双向链表还会有一个前驱节点 pre。

> 有没有想过为啥只有二叉树，而没有一叉树。实际上链表就是特殊的树，即一叉树。

## 链表的基本操作

要想写出链表的题目， 熟悉链表的各种基本操作和复杂度是必须的。

### 插入

插入只需要考虑要插入位置前驱节点和后继节点（双向链表的情况下需要更新后继节点）即可，其他节点不受影响，因此在给定指针的情况下插入的操作时间复杂度为<code>O(1)</code>。这里给定指针中的指针指的是插入位置的前驱节点。

伪代码：

```

temp = 待插入位置的前驱节点.next
待插入位置的前驱节点.next = 待插入指针
待插入指针.next = temp

```

如果没有给定指针，我们需要先遍历找到节点，因此最坏情况下时间复杂度为 <code>O(N)</code>。

> 提示 1: 考虑头尾指针的情况。

> 提示 2: 新手推荐先画图，再写代码。等熟练之后，自然就不需要画图了。

### 删除

只需要将需要删除的节点的前驱指针的 next 指针修正为其下下个节点即可，注意考虑**边界条件**。

伪代码：

```
待删除位置的前驱节点.next = 待删除位置的前驱节点.next.next
```

> 提示 1: 考虑头尾指针的情况。

> 提示 2: 新手推荐先画图，再写代码。等熟练之后，自然就不需要画图了。

### 遍历

遍历比较简单，直接上伪代码。

迭代伪代码：

```
当前指针 =  头指针
while 当前节点不为空 {
   print(当前节点)
   当前指针 = 当前指针.next
}

```

一个前序遍历的递归的伪代码：

```jsx
dfs(cur) {
    if 当前节点为空 return
    print(cur.val)
    return dfs(cur.next)
}
```

## 链表和数组到底有多大的差异？

熟悉我的小伙伴应该经常听到我说过一句话，那就是**数组和链表同样作为线性的数组结构，二者在很多方便都是相同的，只在细微的操作和使用场景上有差异而已**。而使用场景，很难在题目中直接考察。

> 实际上，使用场景是可以死记硬背的。

因此，对于我们做题来说，**二者的差异通常就只是细微的操作差异**。这么说大家可能感受不够强烈，我给大家举几个例子。

数组的遍历：

```java

for(int i = 0; i < arr.size();i++) {
    print(arr[i])
}

```

链表的遍历：

```java
for (ListNode cur = head; cur != null; cur = cur.next) {
    print(cur.val)
}
```

是不是很像？

**可以看出二者逻辑是一致的，只不过细微操作不一样。**比如：

- 数组是索引 ++
- 链表是 cur = cur.next

如果我们需要逆序遍历呢？

```java
for(int i = arr.size() - 1; i > - 1;i--) {
    print(arr[i])
}
```

如果是链表，通常需要借助于双向链表。而双向链表在力扣的题目很少，因此大多数你没有办法拿到前驱节点，这也是为啥很多时候会自己记录一个前驱节点 pre 的原因。

```java
for (ListNode cur = tail; cur != null; cur = cur.pre) {
    print(cur.val)
}
```

如果往数组末尾添加一个元素就是：

```java
arr.push(1)
```

链表的话，很多语言没有内置的数组类型。比如力扣通常使用如下的类来模拟。

```java
 public class ListNode {
      int val;
      ListNode next;
      ListNode() {}
      ListNode(int val) { this.val = val; }
      ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 }
```

我们是不能直接调用 push 方法的。想一下，如果让你实现这个，你怎么做？你可以先自己想一下，再往下看。

3...2...1

ok，其实很简单。

```java
// 假设 tail 是链表的尾部节点
tail.next = new ListNode('lucifer')
tail = tail.next
```

经过上面两行代码之后， tail 仍然指向尾部节点。是不是很简单，你学会了么？

这有什么用？比如有的题目需要你复制一个新的链表， 你是不是需要开辟一个新的链表头，然后不断拼接（push）复制的节点？这就用上了。

对于数组的底层也是类似的，一个可能的数组 push 底层实现：

```java
arr.length += 1
arr[arr.length - 1] = 'lucifer'
```

总结一下， 数组和链表逻辑上二者有很多相似之处，不同的只是一些使用场景和操作细节，对于做题来说，我们通常更关注的是操作细节。关于细节，接下来给大家介绍，这一小节主要让大家知道二者在思想和逻辑的**神相似**。

有些小伙伴做链表题先把链表换成数组，然后用数组做，本人不推荐这种做法，这等于是否认了链表存在的价值，小朋友不要模仿。

## 链表题难度几何？

链表题真的不难。说链表不难是有证据的。就拿 LeetCode 平台来说，处于困难难度的题目只有两个。

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhptfjewrj310c0fajt1.jpg)

其中 第 23 题基本没有什么链表操作，一个常规的“归并排序”即可搞定，而合并两个有序链表是一个简单题。如果你懂得数组的归并排序和合并两个有序链表，应该轻松拿下这道题。

> 合并两个有序数组也是一个简单题目，二者难度几乎一样。

而对于第 25 题， 相信你看完本节的内容，也可以做出来。

不过，话虽这么说，但是还是有很多小朋友给我说 ”指针绕来绕去就绕晕了“， ”老是死循环“ 。。。。。。链表题目真的那么难么？我们又该如何破解？ lucifer 给大家准备了一个口诀 **一个原则， 两种题型，三个注意，四个技巧**，让你轻松搞定链表题，再也不怕手撕链表。 我们依次来看下这个口诀的内容。

## 一个原则

一个原则就是 **画图**，尤其是对于新手来说。不管是简单题还是难题一定要画图，这是贯穿链表题目的一个准则。

画图可以减少我们的认知负担，这其实和打草稿，备忘录道理是一样的，将存在脑子里的东西放到纸上。举一个不太恰当的例子就是你的脑子就是 CPU，脑子的记忆就是寄存器。寄存器的容量有限，我们需要把不那么频繁使用的东西放到内存，把寄存器用在真正该用的地方，这个内存就是纸或者电脑平板等一切你可以画图的东西。

画的好看不好看都不重要，能看清就行了。用笔随便勾画一下， 能看出关系就够了。

## 两个考点

我把力扣的链表做了个遍。发现一个有趣的现象，那就是链表的考点很单一。除了设计类题目，其考点无法就两点：

- 指针的修改
- 链表的拼接

### 指针的修改

其中指针修改最典型的就是链表反转。其实链表反转不就是修改指针么？

对于数组这种支持随机访问的数据结构来说， 反转很容易， 只需要头尾不断交换即可。

```js
function reverseArray(arr) {
  let left = 0;
  let right = arr.length - 1;
  while (left < right) {
    const temp = arr[left];
    arr[left++] = arr[right];
    arr[right--] = temp;
  }
  return arr;
}
```

而对于链表来说，就没那么容易了。力扣关于反转链表的题简直不要太多了。

今天我给大家写了一个最完整的链表反转，以后碰到可以直接用。当然，前提是大家要先理解再去套。

接下来，我要实现的一个反转**任意一段链表**

```py
reverse(self, head: ListNode, tail: ListNode)。
```

其中 head 指的是需要反转的头节点，tail 是需要反转的尾节点。 不难看出，如果 head 是整个链表的头，tail 是整个链表的尾，那就是反转整个链表，否则就是反转局部链表。接下来，我们就来实现它。

首先，我们要做的就是画图。这个我在**一个原则**部分讲过了。

如下图，是我们需要反转的部分链表：

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhy98pp5fj31d40am3zx.jpg)

而我们期望反转之后的长这个样子：

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhs3rsde4j31cc09o75p.jpg)

不难看出， **最终返回 tail 即可**。

由于链表的递归性，实际上，我们只要反转其中相邻的两个，剩下的采用同样的方法完成即可。

> 链表是一种递归的数据结构，因此采用递归的思想去考虑往往事半功倍，关于递归思考链表将在后面《三个注意》部分展开。

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhs8pccboj30ku09u0td.jpg)

对于两个节点来说，我们只需要下修改一次指针即可，这好像不难。

```java
cur.next = pre
```

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhsaoodvrj30yu0h8761.jpg)

就是这一个操作，不仅硬生生有了环，让你死循环。还让不应该一刀两断的它们分道扬镳。

关于分道扬镳这个不难解决， 我们只需要反转前，记录一下下一个节点即可：

```java
next = cur.next
cur.next = pre

cur = next
```

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhsft0cyuj30wa0s80ux.jpg)

那么环呢？ 实际上， 环不用解决。因为如果我们是从前往后遍历，那么实际上，前面的链表已经被反转了，因此上面我的图是错的。正确的图应该是：

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhsi5tiorj311y0gcwg1.jpg)

至此为止，我们可以写出如下代码：

```py
    # 翻转一个子链表，并返回新的头与尾
    def reverse(self, head: ListNode, tail: ListNode):
        cur = head
        pre = None
        while cur != tail:
            # 留下联系方式
            next = cur.next
            # 修改指针
            cur.next = pre
            # 继续往下走
            pre = cur
            cur = next
        # 反转后的新的头尾节点返回出去
        return tail, head
```

如果你仔细观察，会发现，我们的 tail 实际上是没有被反转的。解决方法很简单，将 tail 后面的节点作为参数传进来呗。

```py
class Solution:
    # 翻转一个子链表，并且返回新的头与尾
    def reverse(self, head: ListNode, tail: ListNode, terminal:ListNode):
        cur = head
        pre = None
        while cur != terminal:
            # 留下联系方式
            next = cur.next
            # 修改指针
            cur.next = pre

            # 继续往下走
            pre = cur
            cur = next
         # 反转后的新的头尾节点返回出去
        return tail, head
```

相信你对反转链表已经有了一定的了解。后面我们还会对这个问题做更详细的讲解，大家先留个印象就好。

### 链表的拼接

大家有没有发现链表总喜欢穿来穿去（拼接）的？比如反转链表 II，再比如合并有序链表等。

为啥链表总喜欢穿来穿去呢？实际上，这就是链表存在的价值，这就是设计它的初衷呀！

链表的价值就在于其**不必要求物理内存的连续性，以及对插入和删除的友好**。这在文章开头的链表和数组的物理结构图就能看出来。

因此链表的题目很多拼接的操作。如果上面我讲的链表基本操作你会了，我相信这难不倒你。除了环，边界 等 。。。 ^\_^。 这几个问题我们后面再看。

## 三个注意

链表最容易出错的地方就是我们应该注意的地方。链表最容易出的错 90 % 集中在以下三种情况：

- 出现了环，造成死循环。
- 分不清边界，导致边界条件出错。
- 搞不懂递归怎么做

接下来，我们一一来看。

### 环

环的考点有两个：

- 题目就有可能环，让你判断是否有环，以及环的位置。
- 题目链表没环，但是被你操作指针整出环了。

这里我们只讨论第二种，而第一种可以用我们后面提到的**快慢指针算法**。

避免出现环最简单有效的措施就是画图，如果两个或者几个链表节点构成了环，通过图是很容易看出来的。因此一个简单的**实操技巧就是先画图，然后对指针的操作都反应在图中**。

但是链表那么长，我不可能全部画出来呀。其实完全不用，上面提到了链表是递归的数据结构， 很多链表问题天生具有递归性，比如反转链表，因此**仅仅画出一个子结构就可以了。**这个知识，我们放在后面的**前后序**部分讲解。

### 边界

很多人错的是没有考虑边界。一个考虑边界的技巧就是看题目信息。

- 如果题目的头节点可能被移除，那么考虑使用虚拟节点，这样**头节点就变成了中间节点**，就不需要为头节点做特殊判断了。
- 题目让你返回的不是原本的头节点，而是尾部节点或者其他中间节点，这个时候要注意指针的变化。

以上两者部分的具体内容，我们在稍后讲到的虚拟头部分讲解。老规矩，大家留个印象即可。

### 前后序

ok，是时候填坑了。上面提到了链表结构天生具有递归性，那么使用递归的解法或者递归的思维都会对我们解题有帮助。

在 [二叉树遍历](https://github.com/azl397985856/leetcode/blob/master/thinkings/binary-tree-traversal.md) 部分，我讲了二叉树的三种流行的遍历方法，分别是前序遍历，中序遍历和后序遍历。

前中后序实际上是指的当前节点相对子节点的处理顺序。如果先处理当前节点再处理子节点，那么就是前序。如果先处理左节点，再处理当前节点，最后处理右节点，就是中序遍历。后序遍历自然是最后处理当前节点了。

实际过程中，我们不会这么扣的这么死。比如：

```py
def traverse(root):
    print('pre')
    traverse(root.left)
    traverse(root.righ)
    print('post')

```

如上代码，我们既在**进入左右节点前**有逻辑， 又在**退出左右节点之后**有逻辑。这算什么遍历方式呢？一般意义上，我习惯只看主逻辑的位置，如果你的主逻辑是在后面就是后序遍历，主逻辑在前面就是前序遍历。 这个不是重点，对我们解题帮助不大，对我们解题帮助大的是接下来要讲的内容。

> 绝大多数的题目都是单链表，而单链表只有一个后继指针。因此只有前序和后序，没有中序遍历。

还是以上面讲的经典的反转链表来说。 如果是前序遍历，我们的代码是这样的：

```py
def dfs(head, pre):
    if not head: return pre
    next = head.next
    # # 主逻辑（改变指针）在后面
    head.next = pre
    dfs(next, head)

dfs(head, None)
```

后续遍历的代码是这样的：

```py

def dfs(head):
    if not head or not head.next: return head
    res = dfs(head.next)
    # 主逻辑（改变指针）在进入后面的节点的后面，也就是递归返回的过程会执行到
    head.next.next = head
    head.next = None

    return res
```

可以看出，这两种写法不管是边界，入参，还是代码都不太一样。为什么会有这样的差异呢？

回答这个问题也不难，大家只要记住一个很简单的一句话就好了，那就是**如果是前序遍历，那么你可以想象前面的链表都处理好了，怎么处理的不用管**。相应地**如果是后序遍历，那么你可以想象后面的链表都处理好了，怎么处理的不用管**。这句话的正确性也是毋庸置疑。

如下图，是前序遍历的时候，我们应该画的图。大家把注意力集中在中间的框（子结构）就行了，同时注意两点。

1. 前面的已经处理好了
2. 后面的还没处理好

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhvdo54mpj31ly0ikjvp.jpg)

据此，我们不难写出以下递归代码，代码注释很详细，大家看注释就好了。

```py
def dfs(head, pre):
    if not head: return pre
    # 留下联系方式（由于后面的都没处理，因此可以通过 head.next 定位到下一个）
    next = head.next
    # 主逻辑（改变指针）在进入后面节点的前面（由于前面的都已经处理好了，因此不会有环）
    head.next = pre
    dfs(next, head)

dfs(head, None)
```

如果是后序遍历呢？老规矩，秉承我们的一个原则，**先画图**。

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhvf05u10j31n20ikdk6.jpg)

不难看出，我们可以通过 head.next 拿到下一个元素，然后将下一个元素的 next 指向自身来完成反转。

用代码表示就是:

```py
head.next.next = head
```

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhvhje71pj31ji0k2gq0.jpg)

画出图之后，是不是很容易看出图中有一个环？ 现在知道画图的好处了吧？就是这么直观，当你很熟练了，就不需要画了，但是在此之前，请不要偷懒。

因此我们需要将 head.next 改为不会造成环的一个值，比如置空。

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhvj9pa2oj31j40k0n1h.jpg)

```py
def dfs(head):
    if not head or not head.next: return head
    # 不需要留联系方式了，因为我们后面已经走过了，不需走了，现在我们要回去了。
    res = dfs(head.next)
    # 主逻辑（改变指针）在进入后面的节点的后面，也就是递归返回的过程会执行到
    head.next.next = head
    # 置空，防止环的产生
    head.next = None

    return res
```

值得注意的是，**前序遍历很容易改造成迭代，因此推荐大家使用前序遍历**。我拿上面的迭代和这里的前序遍历给大家对比一下。

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhur25rl6j317i0iiq7b.jpg)

那么为什么**前序遍历很容易改造成迭代**呢？实际上，这句话我说的不准确，准确地说应该是**前序遍历容易改成不需要栈的递归，而后续遍历需要借助栈来完成**。这也不难理解，由于后续遍历的主逻辑在函数调用栈的弹出过程，而前序遍历则不需要。

> 这里给大家插播一个写递归的技巧，那就是想象我们已经处理好了一部分数据，并把他们用手挡起来，但是还有一部分等待处理，接下来思考”如何根据已经处理的数据和当前的数据来推导还没有处理的数据“就行了。

## 四个技巧

针对上面的考点和注意点，我总结了四个技巧来应对，这都是在平时做题中非常实用的技巧。

### 虚拟头

来了解虚拟头的意义之前，先给大家做几个小测验。

Q1: 如下代码 ans.next 指向什么？

```py
ans = ListNode(1)
ans.next = head
head = head.next
head = head.next
```

A1: 最开始的 head。

Q2：如下代码 ans.next 指向什么？

```py
ans = ListNode(1)
head = ans
head.next = ListNode(3)
head.next = ListNode(4)
```

A2: ListNode(4)

似乎也不难，我们继续看一道题。

Q3: 如下代码 ans.next 指向什么？

```py
ans = ListNode(1)
head = ans
head.next = ListNode(3)
head = ListNode(2)
head.next = ListNode(4)
```

A3: ListNode(3)

如果三道题你都答对了，那么恭喜你，这一部分可以跳过。

如果你没有懂也没关系，我这里简单解释一下你就懂了。

**ans.next 指向什么取决于最后切断 ans.next 指向的地方在哪**。比如 Q1，ans.next 指向的是 head，我们假设其指向的内存编号为 `9527`。

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhw2lifs8j30rs0d275t.jpg)

之后执行 `head = head.next` （ans 和 head 被切断联系了），此时的内存图：

> 我们假设头节点的 next 指针指向的节点的内存地址为 10200

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhw3pzl7xj30wa0nmwh7.jpg)

不难看出，ans 没变。

对于第二个例子。一开始和上面例子一样，都是指向 9527。而后执行了：

```py
head.next = ListNode(3)
head.next = ListNode(4)
```

ans 和 head 又同时指向 ListNode(3) 了。如图：

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhwb3y2yfj30tc0g4aca.jpg)

`head.next = ListNode(4)` 也是同理。因此最终的指向 ans.next 是 ListNode(4)。

我们来看最后一个。前半部分和 Q2 是一样的。

```py
ans = ListNode(1)
head = ans
head.next = ListNode(3)
```

按照上面的分析，此时 head 和 ans 的 next 都指向 ListNode(3)。关键是下面两行：

```py
head = ListNode(2)
head.next = ListNode(4)
```

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhwgh3gkfj311q0qadj0.jpg)

指向了 `head = ListNode(2)` 之后， head 和 ans 的关系就被切断了，**当前以及之后所有的 head 操作都不会影响到 ans**，因此 ans 还指向被切断前的节点，因此 ans.next 输出的是 ListNode(3)。

花了这么大的篇幅讲这个东西的原因就是，指针操作是链表的核心，如果这些基础不懂， 那么就很难做。接下来，我们介绍主角 - 虚拟头。

相信做过链表的小伙伴都听过这么个名字。为什么它这么好用？它的作用无非就两个：

- 将头节点变成中间节点，简化判断。
- 通过在合适的时候断开链接，返回链表的中间节点。

我上面提到了链表的三个注意，有一个是边界。头节点是最常见的边界，那如果**我们用一个虚拟头指向头节点，虚拟头就是新的头节点了，而虚拟头不是题目给的节点，不参与运算，因此不需要特殊判断**，虚拟头就是这个作用。

如果题目需要返回链表中间的某个节点呢？实际上也可借助虚拟节点。由于我上面提到的指针的操作，实际上，你可以新建一个虚拟头，然后让虚拟头在恰当的时候（刚好指向需要返回的节点）断开连接，这样我们就可以返回虚拟头的 next 就 ok 了。[25. K 个一组翻转链表](https://github.com/azl397985856/leetcode/blob/master/problems/25.reverse-nodes-in-k-groups.md) 就用到了这个技巧。

不仅仅是链表， 二叉树等也经常用到这个技巧。 比如我让你返回二叉树的最左下方的节点怎么做？我们也可以利用上面提到的技巧。新建一个虚拟节点，虚拟节点 next 指向当前节点，并跟着一起走，在递归到最左下的时候断开链接，最后返回 虚拟节点的 next 指针即可。

### 快慢指针

判断链表是否有环，以及环的入口都是使用快慢指针即可解决。这种题就是不知道不会，知道了就不容易忘。不多说了，大家可以参考我之前的题解 https://github.com/azl397985856/leetcode/issues/274#issuecomment-573985706 。

除了这个，求链表的交点也是快慢指针，算法也是类似的。不这都属于不知道就难，知道了就容易。且下次写不容易想不到或者出错。

这部分大家参考我上面的题解理一下， 写一道题就可以掌握。接下来，我们来看下**穿针引线**大法。

另外由于链表不支持随机访问，因此如果想要获取数组中间项和倒数第几项等特定元素就需要一些特殊的手段，而这个手段就是快慢指针。比如要找链表中间项就**搞两个指针，一个大步走（一次走两步），一个小步走（一次走一步）**，这样快指针走到头，慢指针刚好在中间。 如果要求链表倒数第 2 个，那就**让快指针先走一步，慢指针再走**，这样快指针走到头，慢指针刚好在倒数第二个。这个原理不难理解吧？这种技巧属于**会了就容易，且不容易忘。不会就很难想出的类型**，因此大家学会了拿几道题练一下就可以放下了。

### 穿针引线

这是链表的第二个考点 - **拼接链表**。我在 [25. K 个一组翻转链表](https://github.com/azl397985856/leetcode/blob/master/problems/25.reverse-nodes-in-k-groups.md)，[61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/) 和 [92. 反转链表 II](https://github.com/azl397985856/leetcode/blob/master/problems/92.reverse-linked-list-ii.md) 都用了这个方法。穿针引线是我自己起的一个名字，起名字的好处就是方便记忆。

这个方法通常不是最优解，但是好理解，方便书写，不易出错，推荐新手用。

还是以反转链表为例，只不过这次是`反转链表的中间一部分`，那我们该怎么做？

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhx2qi4r3j31y80jsq5s.jpg)

反转前面我们已经讲过了，于是我假设链表已经反转好了，那么如何将反转好的链表拼后去呢？

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhx496ge5j31w60gywh1.jpg)

我们想要的效果是这样的：

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhx5vpcg2j31lg0u0afd.jpg)

那怎么达到图上的效果呢？我的做法是从左到右给断点编号。如图有两个断点，共涉及到四个节点。于是我给它们依次编号为 a，b，c，d。

其实 a，d 分别是需要反转的链表部分的前驱和后继（不参与反转），而 b 和 c 是需要反转的部分的头和尾（参与反转）。

因此除了 cur， 多用两个指针 pre 和 next 即可找到 a，b，c，d。

找到后就简单了，直接**穿针引线**。

```
a.next = c
b.next = d
```

![](https://tva1.sinaimg.cn/large/0081Kckwly1gkhyqypiltj31b40oy77h.jpg)

这不就好了么？我记得的就有 25 题，61 题 和 92 题都是这么做的，清晰不混乱。

### 先穿再排后判空

这是四个技巧的最后一个技巧了。虽然是最后讲，但并不意味着它不重要。相反，它的实操价值很大。

继续回到上面讲的链表反转题。

```py
cur = head
pre = None
while cur != tail:
    # 留下联系方式
    next = cur.next
    # 修改指针
    cur.next = pre
    # 继续往下走
    pre = cur
    cur = next
# 反转后的新的头尾节点返回出去
```

什么时候需要判断 next 是否存在，上面两行代码先写哪个呢？

是这样？

```py
    next = cur.next
    cur.next = pre
```

还是这样？

```py
    cur.next = pre
    next = cur.next
```

#### 先穿

我给你的建议是：先穿。这里的穿是修改指针，包括反转链表的修改指针和穿针引线的修改指针。**先别管顺序，先穿**。

#### 再排

穿完之后，代码的总数已经确定了，无非就是排列组合让代码没有 bug。

因此第二步考虑顺序，那上面的两行代码哪个在前？应该是先 next = cur.next ，原因在于后一条语句执行后 cur.next 就变了。由于上面代码的作用是反转，那么其实经过 cur.next = pre 之后链表就断开了，后面的都访问不到了，也就是说此时你**只能返回头节点这一个节点**。

实际上，有假如有十行**穿**的代码，我们很多时候没有必要全考虑。我们**需要考虑的仅仅是被改变 next 指针的部分**。比如 cur.next = pre 的 cur 被改了 next。因此下面用到了 cur.next 的地方就要考虑放哪。其他代码不需要考虑。

#### 后判空

和上面的原则类似，穿完之后，代码的总数已经确定了，无非就是看看哪行代码会空指针异常。

和上面的技巧一样，我们很多时候没有必要全考虑。我们**需要考虑的仅仅是被改变 next 指针的部分**。

比如这样的代码

```py

while cur:
    cur = cur.next
```

我们考虑 cur 是否为空呢？ 很明显不可能，因为 while 条件保证了，因此不需判空。

那如何是这样的代码呢？

```py
while cur:
    next = cur.next
    n_next = next.next
```

如上代码有两个 next，第一个不用判空，上面已经讲了。而第二个是需要的，因为 next 可能是 null。如果 next 是 null ，就会引发空指针异常。因此需要修改为类似这样的代码：

```py
while cur:
    next = cur.next
    if not next: break
    n_next = next.next

```

以上就是我们给大家的四个技巧了。相信有了这四个技巧，写链表题就没那么艰难啦~ ^\_^

## 题目推荐

最后推荐几道题给大家，用今天学到的知识解决它们吧~

- [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
- [82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)
- [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)
- [86. 分隔链表](https://leetcode-cn.com/problems/partition-list/)
- [92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)
- [138. 复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)
- [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)
- [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
- [143. 重排链表](https://leetcode-cn.com/problems/reorder-list/)
- [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)
- [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)
- [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

## 总结

数组和栈从逻辑上没有大的区别，你看基本操作都是差不多的。如果是单链表，我们无法在 $O(1)$ 的时间拿到前驱节点，这也是为什么我们遍历的时候老是维护一个前驱节点的原因。但是本质原因其实是链表的增删操作都依赖前驱节点。这是链表的基本操作，是链表的特性天生决定的。

可能有的同学有这样的疑问”考点你只讲了指针的修改和链表拼接，难道说链表就只会这些就够了？那我做的题怎么还需要我会前缀和啥的呢？你是不是坑我呢？“

我前面说了，所有的数据结构底层都是数组和链表中的一种或两种。而我们这里讲的链表指的是考察链表的基本操作的题目。因此如果题目中需要你使用归并排序去合并链表，那其实归并排序这部分已经不再本文的讨论范围了。

实际上，你去力扣或者其他 OJ 翻链表题会发现他们的链表题大都指的是入参是链表，且你需要对链表进行一些操作的题目。再比如树的题目大多数是入参是树，你需要在树上进行搜索的题目。也就是说需要操作树（比如修改树的指针）的题目很少，比如有一道题让你给树增加一个 right 指针，指向同级的右侧指针，如果已经是最右侧了，则指向空。

链表的基本操作就是增删查，牢记链表的基本操作和复杂度是解决问题的基本。有了这些基本还不够，大家要牢记我的口诀”一个原则，两个考点，三个注意，四个技巧“。

做链表的题，要想入门，无它，唯画图尔。能画出图，并根据图进行操作你就入门了，甭管你写的代码有没有 bug 。

而链表的题目核心的考察点只有两个，一个是指针操作，典型的就是反转。另外一个是链表的拼接。这两个既是链表的精髓，也是主要考点。

知道了考点肯定不够，我们写代码哪些地方容易犯错？要注意什么？ 这里我列举了三个容易犯错的地方，分别是环，边界和前后序。

其中环指的是节点之间的相互引用，环的题目如果题目本身就有环， 90 % 双指针可以解决，如果本身没有环，那么环就是我们操作指针的时候留下的。如何解决出现环的问题？那就是**画图，然后聚焦子结构，忽略其他信息。**

除了环，另外一个容易犯错的地方往往是边界的条件， 而边界这块链表头的判断又是一个大头。克服这点，我们需要认真读题，看题目的要求以及返回值，另外一个很有用的技巧是虚拟节点。

如果大家用递归去解链表的题， 一定要注意自己写的是前序还是后序。

- 如果是前序，那么**只思考子结构即可，前面的已经处理好了，怎么处理的，不用管。非要问，那就是同样方法。后面的也不需考虑如何处理，非要问，那就是用同样方法**
- 如果是后续，那么**只思考子结构即可，后面的已经处理好了，怎么处理的，不用管。非要问，那就是同样方法。前面的不需考虑如何处理。非要问，那就是用同样方法**

如果你想递归和迭代都写， 我推荐你用前序遍历。因为前序遍历容易改成不用栈的递归。

以上就是链表专题的全部内容了。大家对此有何看法，欢迎给我留言，我有时间都会一一查看回答。更多算法套路可以访问我的 LeetCode 题解仓库：https://github.com/azl397985856/leetcode 。 目前已经 37K star 啦。大家也可以关注我的公众号《力扣加加》带你啃下算法这块硬骨头。

我整理的 1000 多页的电子书已经开发下载了，大家可以去我的公众号《力扣加加》后台回复电子书获取。

![](https://cdn.jsdelivr.net/gh/azl397985856/cdn/2020-10-17/1602928846461-image.png)

![](https://cdn.jsdelivr.net/gh/azl397985856/cdn/2020-10-17/1602928862442-image.png)
