---
title: 数据结构--Java描述
categories:
	- Algorithms
tags:
	- 算法
	- 数据结构
date: 2019-09-24
---

本篇文章是为了记录自己在学习数据结构时的笔记，会对常见的数据结构做基本的介绍以及使用Java语言进行实现。包括

- 动态数组
- 栈
- 队列
- 链表
- 二分搜索树
- 优先队列和堆
- 线段树
- `Trie`
- 并查集
- `AVL` 树
- 红黑树
- 哈希表

## 动态数组

### API介绍

数组是一种根据下标操作的数据结构，它的查询速度很快，但是它有缺点，那就是数组的容量一旦在创建时确定，就不能进行更改，所以为了克服这一缺点，我们实现一个自己的数组，并除此以外，还会实现一些方法，包括以下

- `add(int index, E e)`
  - 向指定 `index` 添加元素 `e`
- `get(int index)`
  - 获得指定 `index` 的元素
- `remove(int index)`
  - 删除指定 `index` 的元素并返回该元素
- `set(int index, E e)`
  - 更改 `index` 处的元素为 `e`
- `getSize()`
  - 返回数组中元素的个数
- `contains(E e)`
  - 查询数组是否包含元素 `e`
- `isEmpty()`
  - 查看数组是否为空(是否有元素)
- `find(E e)`
  - 返回数组中元素 `e` 第一次出现的 `index`，若没有元素 `e`，则返回 `-1`

新建一个 `Array` 类，它含有两个私有成员变量

- `E[] data`
  - 用以保存数据
- `int size`
  - 用以记录数组中元素的个数

除此以外还有两个构造方法

- `Array(int capacity)`
  - 设定数组的容量
- `Array()`
  - 容量默认为 `10`

```java
public class Array<E> {
    private E[] data;
    private int size;

    public Array(int capacity) {
        data = (E[]) new Object[capacity];
        size = 0;
    }

    public Array() {
        this(10);
    }
}
```

现在我们来实现上面提到的方法。

### 方法实现

首先来实现 `getSize()` 方法，这个是返回数组元素的个数的，我们直接返回 `size` 即可

```java
public int getSize() {
    return size;
}
```

`isEmpty()` 是为了查看数组中是否还有元素，如果 `size` 为 `0` 的话说明数组为空，所以我们返回 `size == 0` 即可

```java
public boolean isEmpty() {
    return size == 0;
}
```

现在来实现 `add(int index, E e) ` 方法，该方法的实现是将 `index` 后面的元素都向后移动一位，然后在 `index` 处插入元素 `e`

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703004832.png" width="70%"/>
</center>

```java
public void add(int index, E e) {
    //对inex进行验证 如果不符合规范则抛出异常
    if (index < 0 || index > size) {
        throw new IllegalArgumentException("参数错误");
    }
    //将元素向后移动
    for (int i = size; i > index; i--) {
        data[i] = data[i - 1];
    }
    
    //在index处插入元素e
    data[index] = e;
    //数组中元素个数+1
    size++;
}
```

根据这个方法，我们可以很快的实现 `addFirst(E e)` 和 `addLast(E e)` 方法，这两个方法一个是在数组头添加元素，一个是在数组的末尾添加一个元素

```java
public void addLast(E e) {
    //在index = size处添加元素 即在数组末尾添加一个元素
    add(size,e);
}
public void addFirst(E e) {
    //在index = 0处添加一个元素 即在数组头添加一个元素
    add(0,e);
}
```

下面来实现 `remove(int index)` 方法，该方法是删除 `index` 处的元素，并将该元素返回，以添加的操作相反，删除是将后面的元素向前移动，覆盖掉 `index` 处的元素即可删除

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703004936.png" width="70%"/>
</center>

```java
public E remove(int index) {
    //参数检查
    if (index < 0 || index >= size) {
        throw new IllegalArgumentException("参数错误");
    }
    //获得index处的元素用以返回
    E e = data[index];
    //将元素从后向前移一个
    for (int i = index; i < size - 1; i++) {
        data[i] = data[i+1];
    }
    //数组中元素个数-1
    size --;
    
    //返回删除的元素
    return e;
}
```

同理，根据这个方法我们可以快速的实现 `removeLast()` 和 `removeFirst()` 方法

```java
public E removeLast() {
    return remove(size -1);
}
public E removeFirst() {
    return remove(0);
}
```

我们可以添加一个删除指定元素的方法 `removeElement(E e)`，我们会遍历数组，如果发现有元素等于该元素，那么删除该元素并退出方法，所以这个方法只删除第一个元素 `e`，并不是数组所有的元素 `e`

```java
public void removeElement(E e) {
    //遍历数组
    for (int i = 0; i < size; i++) {
        //如果找到等于该元素的元素
        if (e.equals(data[i])) {
            //删除该元素
            remove(i);
            //退出方法
            return;
        }
    }
}
```

下面实现 `contains(E e)` 方法，这个方法的思路同删除指定元素相似，遍历数组，如果找到元素与指定元素相同，那么返回 `true`，如果遍历完数组还没有找到与之相等的元素，那么返回 `false`

```java
public boolean contains(E e) {
    //遍历数组
    for (int i = 0; i < size; i++) {
        //如果找到元素，那么返回true
        if (data[i].equals(e)) {
            return true;
        }
    }
    //如果遍历完所有数组没有找到，那么返回false
    return false;
}
```

`find(E e)` 方法的实现也是遍历数组，如果找到了元素，那么返回下标，如果遍历完数组都没有找到，那么返回 `-1`

```java
public int find(E e) {
    //遍历数组
    for (int i = 0; i < size; i++) {
        //找到元素则返回下标
        if (data[i].equals(e)) {
            return i;
        }
    }
    //如果遍历完数组都没有找到，返回-1
    return -1;
}
```

下面实现 `get(int index)` 和 `set(int index, E e)`，这两个方法的实现及其简单，直接上代码

```java
public E get(int index) {
    if (index < 0 || index >= size) {
        throw new IllegalArgumentException("参数错误");
    }
    
    return data[index];
}

public void set(int index, E e) {
    if (index < 0 || index >= size) {
        throw new IllegalArgumentException("参数错误");
    }
    
    data[index] = e;
}
```

我们可以根据 `get` 方法实现 `getLast()`和 `getFirst()` 方法

```java
public E getFirst() {
    return get(0);
}
public E getLast() {
    return get(size - 1);
}
```

现在我们已经实现了 `API` 中提到的所有的方法，但是我们还是没有解决数组容量固定的问题，为了解决这个问题，我们需要实现一个 `resize(int newCapacity)`，它的作用是改变数组的容量大小，这样当数组的容量不足时，我们调用该方法就可以将数组进行扩容，或者当数组中有大量空间空闲时，我们可以缩小数组的容量，代码如下

```java
private void resize(int newCapacity) {
    //创建一个新容量的数组
    E[] temp = (E[]) new Object[newCapacity];
    //将数组中的数据全部放入新数组中
    for (int i =0; i < size; i++) {
        temp[i] = data[i];
    }
    //改变数组指针指向
    data = temp;
}
```

现在我们改变 `add(int index, E e)和remove(int index)` 方法，我们会在添加元素和删除元素时检查数组的容量，以便对数组进行扩容或者缩容

```java
public void add(int index, E e) {
    if (index < 0 || index > size) {
        throw new IllegalArgumentException("参数错误");
    }
    //如果数组容量满了 那么将数组的容量扩为原来的两倍
    if (size == data.length) {
        resize(data.length * 2);
    }
    for (int i = size; i > index; i--) {
        data[i] = data[i - 1];
    }
    data[index] = e;
    size++;
}
```

```java
public E remove(int index) {
    if (index < 0 || index >= size) {
        throw new IllegalArgumentException("参数错误");
    }
    E e = data[index];
    for (int i = index; i < size - 1; i++) {
        data[i] = data[i+1];
    }
    size --;
    //如果数组中的元素个数为数组容量的1/4，那么容量变为原来的1/2
    //思考一下为什么是1/4 提示：复杂度震荡
    if (size == data.length/4) {
        resize(data.length/2);
    }
    return e;
}
```

为了方便的打印 `Array` 类，我们重写 `toString()` 方法如下

```java
public String toString() {
    StringBuilder str = new StringBuilder();
    str.append("size " + size);
    str.append(" capacity " + data.length);
    str.append("\n[");
    for (int i = 0; i < size; i++) {
        if (i == size - 1) {
            str.append(data[i].toString());
        } else {
            str.append(data[i].toString() + ", ");
        }
    }
    str.append("]");
    return str.toString();
}
```

至此，我们已经完全实现了 `Array`，它的容量没有限制，并且提供了很多的方法供用户调用，我们将使用该类来实现其它的基本的数据结构。下面贴出完整的代码

```java
public class Array<E> {
    private E[] data;
    private int size;

    public Array(int capacity) {
        data = (E[]) new Object[capacity];
        size = 0;
    }

    public Array() {
        this(10);
    }

    public int getSize() {
        return size;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public void addLast(E e) {
        add(size,e);
    }
    public void addFirst(E e) {
        add(0,e);
    }

    public void add(int index, E e) {

        if (index < 0 || index > size) {
            throw new IllegalArgumentException("参数错误");
        }

        if (size == data.length) {
            resize(data.length * 2);
        }

        for (int i = size; i > index; i--) {
            data[i] = data[i - 1];
        }

        data[index] = e;
        size++;
    }

    public E removeLast() {
        return remove(size -1);
    }
    public E removeFirst() {
        return remove(0);
    }

    public E remove(int index) {
        if (index < 0 || index >= size) {
            throw new IllegalArgumentException("参数错误");
        }

        E e = data[index];
        for (int i = index; i < size - 1; i++) {
            data[i] = data[i+1];
        }
        size --;

        if (size == data.length/4) {
            resize(data.length/2);
        }

        return e;
    }

    public void removeElement(E e) {
        for (int i = 0; i < size; i++) {
            if (e.equals(data[i])) {
                remove(i);
                return;
            }
        }
    }

    public boolean contains(E e) {
        for (int i = 0; i < size; i++) {
            if (data[i].equals(e)) {
                return true;
            }
        }

        return false;
    }

    public int find(E e) {
        for (int i = 0; i < size; i++) {
            if (data[i].equals(e)) {
                return i;
            }
        }
        return -1;
    }

    private void resize(int newCapacity) {
        E[] temp = (E[]) new Object[newCapacity];
        for (int i =0; i < size; i++) {
            temp[i] = data[i];
        }
        data = temp;
    }

    public E get(int index) {
        if (index < 0 || index >= size) {
            throw new IllegalArgumentException("参数错误");
        }

        return data[index];
    }

    public E getFirst() {
        return get(0);
    }

    public E getLast() {
        return get(size - 1);
    }

    public void set(int index, E e) {
        if (index < 0 || index >= size) {
            throw new IllegalArgumentException("参数错误");
        }

        data[index] = e;
    }

    public String toString() {
        StringBuilder str = new StringBuilder();

        str.append("size " + size);
        str.append(" capacity " + data.length);
        str.append("\n[");

        for (int i = 0; i < size; i++) {
            if (i == size - 1) {
                str.append(data[i].toString());
            } else {
                str.append(data[i].toString() + ", ");
            }
        }

        str.append("]");

        return str.toString();
    }
}
```

## 栈

栈是一种先进后出的结构，比如你放书会把书放在最上面，最先放的书在最下面，而你拿书却是从最上面拿，最后放的最先拿到，栈正是怎么一种结构，我们规定最上面的位置叫做栈顶，我们向栈中添加元素是添加到栈顶，向栈中取出元素是从栈顶取出的，我们先来定义一个 `Stack` 接口，里面规定了一个栈包含的操作

```java
public interface Stack<E> {
    //向栈中压入一个元素
    void push(E e);
    //将栈顶元素弹出
    E pop();
    //栈是否为空
    boolean isEmpty();
    //获得栈中元素的个数
    int getSize();
    //获得栈顶元素
    E peek();
}

```

下面我们将使用上面实现的 `Array` 来实现一个 `ArrayStack`，我们把数组的最后位置定义为栈顶

```java
public class ArrayStack<E> implements Stack<E> {
    private Array<E> data;

    public ArrayStack(int capacity) {
        data = new Array<>(capacity);
    }

    public ArrayStack() {
        data = new Array<>();
    }

    @Override
    public void push(E e) {
        data.addLast(e);
    }

    @Override
    public E pop() {
        return data.removeLast();
    }

    @Override
    public boolean isEmpty() {
        return data.isEmpty();
    }

    @Override
    public int getSize() {
        return data.getSize();
    }

    @Override
    public E peek() {
        return data.getLast();
    }

    public String toString() {
        StringBuilder res = new StringBuilder();
        res.append("Stack: ");
        res.append("[");

        for (int i = 0; i < data.getSize(); i++) {
            res.append(data.get(i));
            if (i != data.getSize()-1) {
                res.append(", ");
            }
        }
        res.append("] top");

        return res.toString();
    }
}
```

上面的代码极其的简单，只要仔细的阅读就可以完全的理解，这里不多做解释。

下面介绍一个有关于栈的题目，此题来自于[LeetCode第20题](https://leetcode-cn.com/problems/valid-parentheses)

>给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串，判断字符串是否有效。有效字符串需满足：
>1. 左括号必须用相同类型的右括号闭合。
>2. 左括号必须以正确的顺序闭合。
>
>注意空字符串可被认为是有效字符串。

这道题的解题思路是，如果遇到左括号`'(', '[', '{'`，那么将左括号压入栈中，如果遇到右括号，那么将栈顶的左括号弹出，判断两个括号是否匹配，如果不匹配返回 `fasle`，如果匹配进行下一轮，最后如果字符串遍历完毕，如果栈为空说明匹配成功，如果栈不为空，所以左边的括号多匹配失败，代码如下

```java
import java.util.Stack;

class Solution {
    public boolean isValid(String s) {
        //创建一个空栈
        Stack<Character> stack = new Stack<>();
		
        //遍历字符串
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            //如果是左括号，则压入栈中
            if (c == '(' || c == '[' || c == '{') {
                stack.push(c);
            } else {
                //如果是右括号 先判断栈是否为空
                if (stack.isEmpty()) {
                    return false;
                }
				
                //获得栈顶的左括号
                char charTop = stack.pop();
                //下面三种皆为不匹配的情况
                if (c == ')' && charTop != '(') {
                    return false;
                }
                if (c == ']' && charTop != '[') {
                    return false;
                }
                if (c == '}' && charTop != '{') {
                    return false;
                }
            }
        }
		
        //这里不能直接返回true 要根据栈是否为空决定返回值
        return stack.isEmpty();
    }
}
```

## 队列

队列是一种先进先出的结构，假设你在排队，那么最先排队的人最先得到服务。我们只能从队尾添加元素，从队首取出元素。老规矩，我们首先规定一下队列 `Queue` 的 `API`

```java
public interface Queue<E> {
    //向队列中添加一个元素
    void enqueue(E e);
    //从队列中取出一个元素
    E dequeue();
    //获得队首的元素
    E getFront();
    //获取队列中元素的个数
    int getSize();
    //判断队列是否为空
    boolean isEmpty();
}
```

### 数组队列

 现在我们将使用动态数组 `Array` 类来实现队列，实现的逻辑也十分的简单，如下

```java
public class ArrayQueue<E> implements Queue<E> {
    private Array<E> array;

    public ArrayQueue() {
        array = new Array<>();
    }

    public ArrayQueue(int capacity) {
        array = new Array<>(capacity);
    }

    @Override
    public void enqueue(E e) {
        array.addLast(e);
    }

    @Override
    public E dequeue() {
        return array.removeFirst();
    }

    @Override
    public E getFront() {
        return array.getFirst();
    }

    @Override
    public int getSize() {
        return array.getSize();
    }

    @Override
    public boolean isEmpty() {
        return array.isEmpty();
    }

    @Override
    public String toString() {
        StringBuilder res = new StringBuilder();
        res.append("Queue: ");
        res.append("front [");

        for (int i = 0; i < array.getSize(); i++) {
            res.append(array.get(i));
            if (i != array.getSize()-1) {
                res.append(", ");
            }
        }
        res.append("] tail");

        return res.toString();
    }
}
```

注意上面我们的 `dequeue` 操作是调用了动态数组的 `removeFirst` 操作，这个操作需要遍历整个数组将元素向前移动，所以该操作是 `O(n)` 的。

### 循环队列

上面队列的 `dequeue` 操作是 `O(n)` 级别的，这是因为上面会将数组整体向前移一位，但是如果我们不这么做，而是增加一个变量 `front` 来记录队首的位置，这样我们只要将 `front` 向前移一位即可，这样的操作就是 `O(1)` 级别的

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703005059.png" width="70%"/>
</center>



这样做的同时，我们发现，如果当 `tail` 来到数组的末尾，按道理应该将数组进行扩容，但是 `front` 前面还有空间

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703005127.png" width="70%"/>
</center>



这个时候我们应当将 `tail` 移动到数组头去

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703005156.png" width="70%"/>
</center>

这时 `tail` 的计算公式不再是简单的 `tail = tail + 1`，而是 `tail = (tail + 1) % data.length`，如果不理解这个式子，就想象一下时钟，11 点向前一步就是 12 点，也可以称为是 0 点，这个时候时钟的计算公式为 `(11 + 1) % 12`。因为这种循环的特性，我们把这种实现方式称为循环队列。这次我们实现队列不在使用上面的动态数组，有了上面实现栈和队列的经验，想必可以容易理解下面的代码(在关键的步骤给予注释)

```java
public class LoopQueue<E> implements Queue<E> {
    private int front;
    private int tail;
    //队列中元素的个数
    private int size;
    //底层实现的数组
    private E[] data;
	
    //构造方法初始化
    public LoopQueue(int capacity) {
        data = (E[]) new Object[capacity];
        size = 0;
        front = 0;
        tail = 0;
    }
    //默认容量为10
    public LoopQueue() {
        this(10);
    }
	
    @Override
    public void enqueue(E e) {
        //首先判断数组是不是满了，如果是那么就进行扩容
        if (size == data.length) {
            resize(2 * data.length);
        }
		
        //向队尾添加元素
        data[tail] = e;
        //tail向后移动 不是简单的+1 上面已有解释
        tail = (tail +1) % data.length;
        size++;
    }
	
    //数组伸缩操作，已接触过
    private void resize(int newCapacity) {
        E[] temp = (E[]) new Object[newCapacity];
        for (int i =0; i < size; i++) {
            //这里我们将队列的头对应到新数组的开头
            temp[i] = data[(front + i)%data.length];
        }
        //重新记录front和tail的位置
        front = 0;
        tail = size;
        data = temp;
    }

    @Override
    public E dequeue() {
        //如果队列为空，抛出异常
        if (size == 0) {
            throw new IllegalArgumentException("队列为空");
        }
		
        //获得出队的元素
        E e = data[front];
        data[front] = null;
		
        //front向前移动(带循环)
        front = (front + 1) % data.length;
        size--;
		
        //缩容操作，不做解释
        if (size == data.length / 4) {
            resize(data.length / 2);
        }

        return e;
    }

    @Override
    public E getFront() {
        if (size == 0) {
            throw new IllegalArgumentException("队列为空");
        }

        return data[front];
    }

    @Override
    public int getSize() {
        return size;
    }

    @Override
    public boolean isEmpty() {
        return size == 0;
    }

    @Override
    public String toString() {
        StringBuilder str = new StringBuilder();
        str.append("Queue: size " + size);
        str.append(" capacity " + data.length);
        str.append("\nfront [");
        for (int i = 0; i < size; i++) {
            if (i == size - 1) {
                str.append(data[(front + i) % data.length].toString());
            } else {
                str.append(data[(front + i) % data.length].toString() + ", ");
            }
        }
        str.append("] tail");
        return str.toString();
    }
}
```

这次我们得到的 `dequeue` 操作就是 `O(1)` 的了(严格的讲均摊复杂度为 `O(1)`，因为里面 `resize()` 复杂度是 `O(n)` 的)。

## 链表

链表是一种非常重要的线性数据结构，我们在实现栈和队列时使用的是动态数组实现的，这个动态数组是针对用户而言是动态的，实际上底层是静态的，是通过 `resize()` 操作去解决容量问题的。而链表则是一种真正的动态数据结构，它是这么一种数据结构，我们把数据存储在一个节点(Node)中，一个节点一般包含两部分的内容，一个是存储的数据，一个是它要指向的下一个节点

```java
class Node {
    private E e;
    private Node next;
}
```

一个节点指向一个节点，所以最后看起来就像是一个链，我们把这种数据结构称为链表

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703005355.png" width="50%"/>
</center>
最后一个节点的下一个节点为 `NULL`，表示后面没有节点了。它是一个真正的动态的数据结构，不需要处理容量的问题。但是它也有缺点，它没有数组那样快的查询能力，它要查询某个节点的数据，只能通过头结点一直寻找下来(后面我们将看到)，所以它的查询速度比数组慢。

### 链表实现

现在我们将实现这么一个结构，首先设计好节点类

```java
public class LinkedList<E> {
    //我们将Node设置为LinkedList的私有内部类
    private class Node<E> {
        public E e;
        public Node next;

        public Node(E e, Node next) {
            this.e = e;
            this.next = next;
        }

        public Node(E e) {
            this(e, null);
        }

        public Node() {
            this(null, null);
        }
        
        @Override
        public String toString() {
            return e.toString();
        }
    }
}
```

我们要想向链表中添加(或其他操作)元素，不可避免的要遍历链表(因为链表不能通过索引访问，只能通过前面的节点找到后面的节点)，而要遍历链表，我们就要将链表的头存储起来，这样才能遍历链表，我们将链表的头称为 `head`

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703082352.png" width="50%"/>
</center>
同时我们使用变量 `size` 来记录链表中元素的个数

```java
public class LinkedList<E> {
    //为了节省篇幅，Node类不再展示，下同

    //头结点
    private Node head;
    //链表中元素的个数
    private int size;

    public LinkedList() {
        head = null;
        size = 0;
    }
}

```

现在我们实现两个简单的方法 `getSize()` 和 `isEmpty()`

```java
public int getSize() {
    return size;
}
public boolean isEmpty() {
    return size == 0;
}
```

#### 添加元素

##### 向链表头添加元素

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703082745.png" width="65%"/>
</center>
首先将要插入的新节点指向 `head`，然后将 `head` 设置为新节点，实现如下

```java
public void addFirst(E e) {
    //体会一下这条语句的意思
    head = new Node(e,head);
    size++;
}
```

##### 在链表的中间添加一个元素

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703082943.png" width="70%"/>
</center>
比如现在往节点 `1` 后面插入一个元素，首先将新节点指向节点 `2`，然后节点 `1` 指向新节点，实现如下

```java
public void add(int index, E e) {
    if (index < 0 || index > size) {
        throw new IllegalArgumentException("参数错误");
    }
    //如果是头结点需要单独处理
    if (index == 0) {
        addFirst(e);
    }
    //prev代表要插入位置的前一个节点
    Node prev = head;
    for (int i = 0; i < index - 1; i++) {
        prev = prev.next;
    }
    prev.next = new Node(e, prev.next);
    size++;
}
```

##### 向链表的尾部添加一个元素

直接复用上面的代码

```java
public void addLast(E e) {
    add(size, e);
}
```

#### 虚拟头结点

我们在向链表中添加元素时，因为 `head` 前面没有节点，所以我们在添加元素时会对 `head` 进行单独的处理，为了不使 `head` 具有特殊性，我们在链表的最头部添加一个虚拟头结点，里面不存储元素，它的存在是为了使得操作链表方便

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703083049.png" width="70%"/>
</center>
现在我们修改上面的 `head` 为 `dummyHead`

```java
public class LinkedList<E> {
    

    //虚拟结点
    private Node dummyHead;
    private int size;

    public LinkedList() {
        //这里修改了
        dummyHead = new Node(null, null);
        size = 0;
    }

    public int getSize() {
        return size;
    }
    public boolean isEmpty() {
        return size == 0;
    }

    //直接调用add方法
    public void addFirst(E e) {
        add(0,e);
    }

    public void add(int index, E e) {
        if (index < 0 || index > size) {
            throw new IllegalArgumentException("参数错误");
        }
		
        //不需要对head进行单独的处理了
        //index - 1修改为了index
        Node prev = dummyHead;
        for (int i = 0; i < index; i++) {
            prev = prev.next;
        }
        prev.next = new Node(e, prev.next);
        size++;
    }

    public void addLast(E e) {
        add(size, e);
    }
}
```

#### 获得某个索引的值

实现的思路同 `add` 很像，不过这里我们找的不是前一个节点，而是当前的节点

```java
public E get(int index) {
    if (index < 0 || index >= size) {
        throw new IllegalArgumentException("参数错误");
    }
    //cur代表当前节点
    Node cur = dummyHead.next;
    for (int i = 0; i < index; i++) {
        cur = cur.next;
    }
    return (E) cur.e;
}
```

基于这个方法，我们可以很快的实现 `getFirst()` 和 `getLast()`

```java
public E getFirst() {
    return get(0);
}
public E getLast() {
    return get(size - 1);
}
```

#### 更新某个索引的值

实现的思路完全是同 `get()` 方法，直接上代码

```java
public void set(int index, E e) {
    if (index < 0 || index >= size) {
        throw new IllegalArgumentException("参数错误");
    }
    Node cur = dummyHead.next;
    for (int i = 0; i < index; i++) {
        cur = cur.next;
    }
    cur.e = e;
}
```

#### 查找链表是否存在元素e

```java
public boolean contains(E e) {
    //从当前节点开始，一直遍历到最后一个节点
    for (Node cur = dummyHead.next; cur != null; cur = cur.next) {
        if (cur.e.equals(e)) {
            return true;
        }
    }
    return false;
}
```

#### 删除链表中的元素

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703083204.png" width="70%"/>
</center>
上图已详细说明了操作的步骤，这里直接贴上代码实现

```java
public E remove(int index) {
    if (index < 0 || index >= size) {
        throw new IllegalArgumentException("参数错误");
    }
    
    //获得要删除节点的前一个节点
    Node prev = dummyHead;
    for (int i = 0; i < index; i++) {
        prev = prev.next;
    }
    
    //图示的操作
    Node delNode = prev.next;
    prev.next = delNode.next;
    delNode.next = null;
    size--;
    return (E) delNode.e;
}
```

根据上面的方法，可以很快的实现 `removeFirst()` 和 `removeLast()` 方法

```java
public E removeFirst() {
    return remove(0);
}
public E removeLast() {
    return remove(size - 1);
}
```

#### toString()

```java
@Override
public String toString() {
    StringBuilder res = new StringBuilder();
    
    Node cur = dummyHead.next;
    //你可以使用上面的for循环
    while (cur != null) {
        res.append(cur + "->");
        cur = cur.next;
    }
    res.append("NULL");
    
    return res.toString();
}
```

#### 全部代码

```java
public class LinkedList<E> {
    private class Node<E> {
        public E e;
        public Node next;

        public Node(E e, Node next) {
            this.e = e;
            this.next = next;
        }

        public Node(E e) {
            this(e, null);
        }

        public Node() {
            this(null, null);
        }

        @Override
        public String toString() {
            return e.toString();
        }
    }

    private Node dummyHead;
    private int size;

    public LinkedList() {
        dummyHead = new Node(null, null);
        size = 0;
    }

    public int getSize() {
        return size;
    }
    public boolean isEmpty() {
        return size == 0;
    }

    public void addFirst(E e) {
        add(0,e);
    }

    public void add(int index, E e) {
        if (index < 0 || index > size) {
            throw new IllegalArgumentException("参数错误");
        }

        Node prev = dummyHead;
        for (int i = 0; i < index; i++) {
            prev = prev.next;
        }
        prev.next = new Node(e, prev.next);
        size++;
    }

    public void addLast(E e) {
        add(size, e);
    }

    public E get(int index) {
        if (index < 0 || index >= size) {
            throw new IllegalArgumentException("参数错误");
        }

        Node cur = dummyHead.next;
        for (int i = 0; i < index; i++) {
            cur = cur.next;
        }

        return (E) cur.e;
    }

    public E getFirst() {
        return get(0);
    }
    public E getLast() {
        return get(size - 1);
    }

    public void set(int index, E e) {
        if (index < 0 || index >= size) {
            throw new IllegalArgumentException("参数错误");
        }

        Node cur = dummyHead.next;
        for (int i = 0; i < index; i++) {
            cur = cur.next;
        }

        cur.e = e;
    }

    public boolean contains(E e) {
        for (Node cur = dummyHead.next; cur != null; cur = cur.next) {
            if (cur.e.equals(e)) {
                return true;
            }
        }

        return false;
    }

    public E remove(int index) {
        if (index < 0 || index >= size) {
            throw new IllegalArgumentException("参数错误");
        }

        Node prev = dummyHead;
        for (int i = 0; i < index; i++) {
            prev = prev.next;
        }

        Node delNode = prev.next;
        prev.next = delNode.next;
        delNode.next = null;

        size--;

        return (E) delNode.e;
    }

    public E removeFirst() {
        return remove(0);
    }
    public E removeLast() {
        return remove(size - 1);
    }

    @Override
    public String toString() {
        StringBuilder res = new StringBuilder();

        Node cur = dummyHead.next;
        while (cur != null) {
            res.append(cur + "->");
            cur = cur.next;
        }
        res.append("NULL");

        return res.toString();
    }
}
```

### 使用链表实现栈

由于链表的 `addFirst()` 和 `removeFirst()` 的操作都是 `O(1)`，所以我们使用链表头作为栈顶，具体的实现逻辑如下

```java
public class LinkedListStack<E> implements Stack<E> {
    private LinkedList<E> linkedList;

    public LinkedListStack() {
        linkedList = new LinkedList<>();
    }

    @Override
    public void push(E e) {
        linkedList.addFirst(e);
    }

    @Override
    public E pop() {
        return linkedList.removeFirst();
    }

    @Override
    public boolean isEmpty() {
        return linkedList.isEmpty();
    }

    @Override
    public int getSize() {
        return linkedList.getSize();
    }

    @Override
    public E peek() {
        return linkedList.getFirst();
    }

    @Override
    public String toString() {
        StringBuilder res = new StringBuilder();
        res.append("Stack: top ");
        res.append(linkedList);

        return res.toString();
    }
}
```

### 使用链表实现队列

我们之前使用数组实现队列，由于它的 `dequeue` 操作是 `O(n)` 级别的，所以我们使用 `front` 来标记队首，使用循环队列设计，同样的在链表中从链表尾部删除或增加元素都是 `O(n)` 级别的，为了解决这一个问题，我们决定在链表的尾部增加一个 `tail` 变量来标记，从而使得在尾部增加元素是 `O(1)` 级别的。


另外考虑在尾部删除一个元素是 `O(1)` 的吗? 答案是不是。因为我们删除一个节点需要知道该节点的前一个节点，而知道 `tail` 节点是无法知道 `tail` 的前一个节点的，我们还是要遍历。所以我们在 `head` 端删除元素，在 `tail` 端添加元素，并且由于只涉及到头部和尾部的操作，所以我们也不需要添加虚拟头结点了

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703083306.png" width="50%"/>
</center>
下面就是实现的代码

```java
public class LinkedListQueue<E> implements Queue<E> {
    private class Node<E> {
        public E e;
        public Node next;

        public Node(E e, Node next) {
            this.e = e;
            this.next = next;
        }

        public Node(E e) {
            this(e, null);
        }

        public Node() {
            this(null, null);
        }

        @Override
        public String toString() {
            return e.toString();
        }
    }

    private Node head;
    private Node tail;
    private int size;

    public LinkedListQueue() {
        head = null;
        tail = null;
        size = 0;
    }

    @Override
    public void enqueue(E e) {
        //队列为空时，tail和head都为null 添加元素后二者都指向第一个元素
        if (size == 0) {
            tail = new Node(e);
            head = tail;
        } else {
            tail.next = new Node(e);
            tail = tail.next;
        }
        size++;
    }

    @Override
    public E dequeue() {
        if (size == 0) {
            throw new IllegalArgumentException("队列为空");
        }

        Node delNode = head;
        head = head.next;
        delNode.next = null;

        size--;
        //如果队列为空了，此时tail指向的是delNode，此时应该让tail为null
        if (size == 0) {
            tail = null;
        }
        return (E) delNode.e;
    }

    @Override
    public E getFront() {
        if (head == null) {
            throw new IllegalArgumentException("队列为空");
        }

        return (E) head.e;
    }

    @Override
    public int getSize() {
        return size;
    }

    @Override
    public boolean isEmpty() {
        return size == 0;
    }

    @Override
    public String toString() {
        StringBuilder res = new StringBuilder();

        res.append("Queue: front ");

        Node cur = head;
        while (cur != null) {
            res.append(cur + "->");
            cur = cur.next;
        }
        res.append("NULL tail");

        return res.toString();
    }
}
```

## 二分搜索树

什么是树结构

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703083447.png" width="60%"/>
</center>
当你把上面的图倒过来看，就像是一棵树，所以我们把这种结构称为是树结构。那为什么要使用树结构，因为树结构在生活中很常见，如文件夹的组织方式，又或者如公司职能的组织方式，这些都是树结构的例子。为什么会使用树结构呢? 原因就是因为高效。

### 概念

同链表一样，它也是一种动态的数据结构，链表中的节点是指向一个节点，而二叉树是指向两个节点，我们把这两个节点称为左子树和右子树，又或者称为左孩子和右孩子。如下图表示的就是二叉树

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703083447.png" width="60%"/>
</center>
```java
class Node {
    E e;
    Node left;
    Node right;
}
```

- 根节点
  - 最顶部的那个节点，如上图中 28 就是根节点
  - 二叉树具有唯一的一个根节点
- 叶子节点
  - 没有孩子的节点，如上图的最后一行都是叶子节点
- 二叉树的每个节点最多有两个孩子，最多有一个父亲

那是什么是二分搜索树，首先二分搜索树是二叉树，它满足这样的特点，对于每个节点

- 大于左子树所有节点的值
- 小于右子树所有节点的值

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703083447.png" width="60%"/>
</center>
可以验算，上面的这棵树满足二分搜索树的性质，所以这棵树是二分搜索树。下面我们来实现二分搜索树中节点有关代码

```java
public class BST<E extends Comparable<E>> {
    private class Node {
        public E e;
        public Node left;
        public Node right;

        public Node(E e) {
            this.e = e;
            left = null;
            right = null;
        }

        @Override
        public String toString() {
            return e.toString();
        }
    }

    //根节点
    private Node root;
    //树中元素的个数
    private int size;

    public BST() {
        root = null;
        size = 0;
    }
    
    public int size() {
        return size;
    }
    public boolean isEmpty() {
        return size == 0;
    }
}
```

### 实现

#### 添加元素

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703083820.png" width="80%"/>
</center>
上图想必已经将添加元素的规则说的很详细了，所以这里直接上代码

```java
public void add(E e) {
    if (root == null) {
        root = new Node(e);
        size++;
    } else {
        add(root, e);
    }
}
private void add(Node node, E e) {
    //递归终止条件
    if (e.equals(node.e)) {
        return;
    } else if (e.compareTo(node.e) < 0 && node.left == null) {
        node.left = new Node(e);
        size++;
        return;
    } else if (e.compareTo(node.e) > 0 && node.right == null) {
        node.right = new Node(e);
        size++;
        return;
    }
    if (e.compareTo(node.e) < 0) {
        add(node.left, e);
    }
    if (e.compareTo(node.e) > 0) {
        add(node.right, e);
    }
}
```

其实上面的代码还可以改进，因为我们在 `add(E e)` 中对 `root` 为根节点进行了单独的考虑，其实可以不再这里考虑，因为通过上面的规则知道，当一个节点为 `null` 时，不管它是根节点还是左右孩子，新加入的节点都将取代这个 `null` 节点

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703084129.png" width="70%"/>
</center>
所以我们优化上面的代码如下

```java
public void add(E e) {
    root = add(root, e);
}
private Node add(Node node, E e) {
    //这里返回的Node所指的语义是node所代表的根节点
    if (node == null) {
        size++;
        return new Node(e);
    }
    if (e.compareTo(node.e) < 0) {
        //如果比根节点小，对左子树进行更新
        node.left = add(node.left, e);
    } else if (e.compareTo(node.e) > 0) {
        //如果比根节点大，对右子树进行更新
        node.right = add(node.right,e);
    }
    //等于的话什么都不做
    return node;
}
```

#### 查询操作

```java
public boolean contains(E e) {
    return contains(root, e);
}
private boolean contains(Node node, E e) {
    if (node == null) {
        return false;
    }
    
    if (e.equals(node.e)) {
        return true;
    } else if (e.compareTo(node.e) < 0) {
        return contains(node.left, e);
    } else {
        return contains(node.right,e);
    }
}
```

#### 二叉树的遍历

对于某个数据结构的遍历就是将该数据结构中所有的元素都访问一遍，分为三类

- 前序遍历
  - 父节点在访问左子树之前访问
- 中序遍历
  - 父节点在访问左子树之后，在访问右子树之前访问
- 后序遍历
  - 父节点在访问右子树之后访问

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703084349.png" width="40%"/>
</center>


##### 前序遍历

```java
public void preOrder() {
    preOrder(root);
}
private void preOrder(Node node) {
    if (node == null) {
        return;
    }
    //先访问父节点
    System.out.println(node);
    preOrder(node.left);
    preOrder(node.right);
}
```

##### 中序遍历

```java
public void inOrder() {
    inOrder(root);
}
private void inOrder(Node node) {
    if (node == null) {
        return;
    }
    
    inOrder(node.left);
    System.out.println(node);
    inOrder(node.right);
}
```

中序遍历的结果是元素从小到大排序。

##### 后序遍历

```java
public void postOrder() {
    postOrder(root);
}
private void postOrder(Node node) {
    if (node == null) {
        return;
    }
    
    postOrder(node.left);
    postOrder(node.right);
    System.out.println(node);
}
```

后序遍历的一个应用是内存释放，我们必须先把左右孩子的内存释放完才能释放该节点的内存。

##### 前序遍历的非递归实现

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703084649.png" width="80%"/>
</center>
代码实现

```java
public void preOrderNR() {
    //非递归写法
    if (root == null) {
        return;
    }
    //这里的Stack是上面自己写的Stack
    Stack<Node> stack = new ArrayStack<>();
    stack.push(root);
    
    while (!stack.isEmpty()) {
        Node top = stack.pop();
        System.out.println(top);
        if (top.right != null) {
            stack.push(top.right);
        }
        if (top.left != null) {
            stack.push(top.left);
        }
    }
}
```

##### 层序遍历

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703085018.png" width="90%"/>
</center>
代码实现

```java
public void levelOrder() {
    if (root == null) {
        return;
    }
    Queue<Node> queue = new LoopQueue<>();
    queue.enqueue(root);
    while (!queue.isEmpty()) {
        Node front = queue.dequeue();
        System.out.println(front);
        if (front.left != null) {
            queue.enqueue(front.left);
        }
        if (front.right != null) {
            queue.enqueue(front.right);
        }
    }
}
```

#### 删除元素

在二分搜索树中删除一个节点是比较复杂的，我们首先从最简单的情况开始，删除二分搜索树中的最小值和最大值，首先是如何找到最大值和最小值

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703085309.png" width="90%"/>
</center>
```java
public E minimum() {
    if (size == 0) {
        throw new IllegalArgumentException("树为空");
    }
    return minimum(root).e;
}
private Node minimum(Node node) {
    if (node.left == null) {
        return node;
    }
    return minimum(node.left);
}
public E maximum() {
    if (size == 0) {
        throw new IllegalArgumentException("树为空");
    }
    return maximum(root).e;
}
private Node maximum(Node node) {
    if (node.right == null) {
        return node;
    }
    return maximum(node.right);
}
```

找到了之后如何删除呢

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703085650.png" width="90%"/>
</center>
```java
public E removeMin() {
    E ret = minimum();
    root = removeMin(root);
    return ret;
}
private Node removeMin(Node node) {
    if (node.left == null) {
        Node rightNode = node.right;
        node.right = null;
        size--;
        return rightNode;
    }
    node.left = removeMin(node.left);
    return node;
}
```

```java
public E removeMax() {
    E ret = maximum();
    root = removeMax(root);
    return ret;
}
private Node removeMax(Node node) {
    if (node.right == null) {
        Node leftNode = node.left;
        node.left = null;
        size--;
        return leftNode;
    }
    
    node.right = removeMax(node.right);
    return node;
}
```

现在讲解如何删除二分搜搜数中的任意一个节点

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703090142.png" width="90%"/>
</center>
```java
public void remove(E e) {
    root = remove(root, e);
}
private Node remove(Node node, E e) {
    if (node == null) {
        return null;
    }
    if (e.equals(node.e)) {
        if (node.right == null) {
            Node leftNode = node.left;
            node.left = null;
            size--;
            return leftNode;
        } else if (node.left == null) {
            Node rightNode = node.right;
            node.right = null;
            size--;
            return rightNode;
        } else {
            Node successor = minimum(node.right);
            successor.right = removeMin(node.right);//为什么这条语句必须在前面???
            successor.left = node.left;
            node.left = node.right = null;
            //size--; 在removeMin中已经维护size了
            return successor;
        }
    } else if (e.compareTo(node.e) < 0) {
        node.left = remove(node.left, e);
    } else {
        node.right = remove(node.right, e);
    }
    return node;
}
```

上面说的是取右子树中的最小值，你也可以考虑取左子树中的最大值，道理都是一样的。

#### 完整代码

```java
public class BST<E extends Comparable<E>> {
    private class Node {
        public E e;
        public Node left;
        public Node right;

        public Node(E e) {
            this.e = e;
            left = null;
            right = null;
        }

        @Override
        public String toString() {
            return e.toString();
        }
    }

    //根节点
    private Node root;
    //树中元素的个数
    private int size;

    public BST() {
        root = null;
        size = 0;
    }

    public int size() {
        return size;
    }
    public boolean isEmpty() {
        return size == 0;
    }

    public void add(E e) {
        root = add(root, e);
    }


    private Node add(Node node, E e) {
        //这里返回语义指的的是node所代表的根节点
        if (node == null) {
            size++;
            return new Node(e);
        }

        if (e.compareTo(node.e) < 0) {
            //如果比根节点小，对左子树进行更新
            node.left = add(node.left, e);
        } else if (e.compareTo(node.e) > 0) {
            //如果比根节点大，对右子树进行更新
            node.right = add(node.right,e);
        }
        //等于的话什么都不做
        return node;
    }

    public boolean contains(E e) {
        return contains(root, e);
    }

    private boolean contains(Node node, E e) {
        if (node == null) {
            return false;
        }

        if (e.equals(node.e)) {
            return true;
        } else if (e.compareTo(node.e) < 0) {
            return contains(node.left, e);
        } else {
            return contains(node.right,e);
        }
    }

    //前序遍历
    public void preOrder() {
        preOrder(root);
    }
    private void preOrder(Node node) {
        if (node == null) {
            return;
        }

        System.out.println(node);
        preOrder(node.left);
        preOrder(node.right);
    }

    public void preOrderNR() {
        //非递归写法
        if (root == null) {
            return;
        }
        Stack<Node> stack = new ArrayStack<>();
        stack.push(root);

        while (!stack.isEmpty()) {
            Node top = stack.pop();
            System.out.println(top);
            if (top.right != null) {
                stack.push(top.right);
            }
            if (top.left != null) {
                stack.push(top.left);
            }
        }
    }

    //中序遍历
    public void inOrder() {
        inOrder(root);
    }
    private void inOrder(Node node) {
        if (node == null) {
            return;
        }

        inOrder(node.left);
        System.out.println(node);
        inOrder(node.right);
    }

    //后序遍历
    public void postOrder() {
        postOrder(root);
    }
    private void postOrder(Node node) {
        if (node == null) {
            return;
        }

        postOrder(node.left);
        postOrder(node.right);
        System.out.println(node);
    }

    public void levelOrder() {
        if (root == null) {
            return;
        }
        Queue<Node> queue = new LoopQueue<>();
        queue.enqueue(root);

        while (!queue.isEmpty()) {
            Node front = queue.dequeue();
            System.out.println(front);
            if (front.left != null) {
                queue.enqueue(front.left);
            }
            if (front.right != null) {
                queue.enqueue(front.right);
            }
        }
    }

    public E minimum() {
        if (size == 0) {
            throw new IllegalArgumentException("树为空");
        }
        return minimum(root).e;
    }

    private Node minimum(Node node) {
        if (node.left == null) {
            return node;
        }
        return minimum(node.left);
    }

    public E maximum() {
        if (size == 0) {
            throw new IllegalArgumentException("树为空");
        }
        return maximum(root).e;
    }

    private Node maximum(Node node) {
        if (node.right == null) {
            return node;
        }
        return maximum(node.right);
    }

    public E removeMin() {
        E ret = minimum();
        root = removeMin(root);
        return ret;
    }

    private Node removeMin(Node node) {
        if (node.left == null) {
            Node rightNode = node.right;
            node.right = null;
            size--;
            return rightNode;
        }

        node.left = removeMin(node.left);
        return node;
    }

    public E removeMax() {
        E ret = maximum();
        root = removeMax(root);
        return ret;
    }
    private Node removeMax(Node node) {
        if (node.right == null) {
            Node leftNode = node.left;
            node.left = null;
            size--;
            return leftNode;
        }

        node.right = removeMax(node.right);
        return node;
    }

    public void remove(E e) {
        root = remove(root, e);
    }
    private Node remove(Node node, E e) {
        if (node == null) {
            return null;
        }
        if (e.equals(node.e)) {
            if (node.right == null) {
                Node leftNode = node.left;
                node.left = null;
                size--;
                return leftNode;
            } else if (node.left == null) {
                Node rightNode = node.right;
                node.right = null;
                size--;
                return rightNode;
            } else {
                Node successor = minimum(node.right);
                successor.right = removeMin(node.right);//为什么这条语句必须在前面???
                successor.left = node.left;
                node.left = node.right = null;
                //size--; 在removeMin中已经维护size了
                return successor;
            }
        } else if (e.compareTo(node.e) < 0) {
            node.left = remove(node.left, e);
        } else {
            node.right = remove(node.right, e);
        }
        return node;
    }
}
```

## 优先队列和堆

普通队列：先进先出，就像是我们在银行办业务或者是在超市买东西，但是考虑在医院，有病人有突发情况，这个时候容不得他去排队挂号了，这时他的优先级是比较高的，所以他需要得到优先的处理，像这种队列中的元素具有优先级的队列，我们把它称之为优先队列。在游戏中我们也会设置优先攻击血量最低的怪或者距离最近的怪，这时候血量和距离就成为了判断优先级的标准；在操作系统的任务调度，我们为程序分配 CPU，内存等等资源，并不是先到先得的，也是根据程序的优先级来进行分配的。

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703090345.png" width="80%"/>
</center>
### 堆的结构

这里的堆指的是二叉堆，它满足以下的性质

- 二叉堆是一棵完全二叉树
  - 把元素顺序排列成树的形状

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703090438.png" width="70%"/>
</center>
- 堆中某个节点的值总是不大于其父亲节点的值(最大堆，相应也可以定义最小堆)

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703090533.png" width="40%"/>
</center>


如果我们使用数组去实现堆

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703090633.png" width="50%"/>
</center>
上面的序号表示的是在数组中的下标，我们发现如果父节点的下标为 `i`，那么左孩子的下标就为 `2i + 1`，右孩子的下标为 `2i + 2`，所以可以很快的根据父节点的下标得到左右孩子的下标，如果知道左右孩子的下标 `i`，那么 `(i - 1)/2` 就可以得到父节点的下标(整数除法，小数部分会被舍去)。这个结论可以使用数学归纳法进行证明，但不是这里的重点，所以不多做阐述。

```java
public class MaxHeap<E extends Comparable<E>> {
    private Array<E> data;

    public MaxHeap(int capacity) {
        data = new Array<>(capacity);
    }
    public MaxHeap() {
        data = new Array<>();
    }

    public int size() {
        return data.getSize();
    }
    public boolean isEmpty() {
        return data.isEmpty();
    }
    
    //根据左右孩子的下标获得父亲节点的下标
    private int parent(int index) {
        return (index - 1) / 2;
    }
    //根据父节点的下标获得左孩子的下标
    private int leftChild(int index) {
        return 2 * index + 1;
    }
    //根据父节点的下标获得右孩子的下标
    private int rightChild(int index) {
        return 2 * index + 2;
    }
}
```

### 堆的实现

#### 向堆中添加元素

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703090845.png"/>
</center>
```java
public void swap(int i, int j) {
    if (i < 0 || i >= size() || j < 0 || j >= size()) {
        throw new IllegalArgumentException("参数错误");
    }
    E temp = data.get(i);
    data.set(i, data.get(j));
    data.set(j, temp);
}
public void add(E e) {
    data.addLast(e);
    siftUp(data.getSize() - 1);
}
private void siftUp(int index) {
    //index不是根节点(根节点不要上浮了) 并且孩子比父亲大
    while (index != 0 && data.get(index).compareTo(data.get(parent(index))) > 0) {
        swap(index, parent(index));
        index = parent(index);
    }
}
```

#### 向堆中取出最大元素

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703091047.png" width="95%"/>
</center>
```java
public E findMax() {
    if (isEmpty()) {
        throw new IllegalArgumentException("堆为空");
    }
    return data.get(0);
}
public E extractMax() {
    E ret = findMax();
    swap(0,data.getSize() - 1);
    data.removeLast();
    siftDown(0);
    return ret;
}
private void siftDown(int index) {
    //没有孩子时，下沉结束
    while (leftChild(index) < size()) {
        int max = leftChild(index);
        int rightIndex = rightChild(index);
        if (rightIndex < size()) {
            max = data.get(max).compareTo(data.get(rightIndex)) > 0 ? max : rightIndex;
        }
        //最大孩子比父节点小时，下沉结束
        if (data.get(max).compareTo(data.get(index)) <= 0) {
            break;
        }
        swap(max,index);
        index = max;
    }
}
```

#### replace

`replace` 操作指的是从堆中取出元素，并向堆中添加一个元素，实现的方法为

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703091202.png" width="80%"/>
</center>
```java
//取出堆中的最大元素，并添加一个新元素e
public E replace(E e) {
    E ret = findMax();
    data.set(0,e);
    siftDown(0);
    return ret;
}
```

#### heapify

`heapify` 是指将任意一个数组整理成堆的形状，

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703091332.png" width="90%"/>
</center>
我们把这个方法做成一个构造函数

```java
public MaxHeap(E[] arr) {
    data = new Array<>(arr.length);
    for (int i = 0; i < arr.length; i++) {
        data.addLast(arr[i]);
    }
    for (int i = parent(data.getSize() -1); i >=0; i--) {
        siftDown(i);
    }
}
```

#### 完整代码

```java
public class MaxHeap<E extends Comparable<E>> {
    private Array<E> data;

    public MaxHeap(int capacity) {
        data = new Array<>(capacity);
    }
    public MaxHeap() {
        data = new Array<>();
    }
    public MaxHeap(E[] arr) {
        data = new Array<>(arr.length);
        for (int i = 0; i < arr.length; i++) {
            data.addLast(arr[i]);
        }

        for (int i = parent(data.getSize() -1); i >=0; i--) {
            siftDown(i);
        }
    }

    public int size() {
        return data.getSize();
    }
    public boolean isEmpty() {
        return data.isEmpty();
    }

    //根据左右孩子的下标获得父亲节点的下标
    private int parent(int index) {
        return (index - 1) / 2;
    }
    //根据父节点的下标获得左孩子的下标
    private int leftChild(int index) {
        return 2 * index + 1;
    }
    //根据父节点的下标获得右孩子的下标
    private int rightChild(int index) {
        return 2 * index + 2;
    }

    public void swap(int i, int j) {
        if (i < 0 || i >= size() || j < 0 || j >= size()) {
            throw new IllegalArgumentException("参数错误");
        }

        E temp = data.get(i);
        data.set(i, data.get(j));
        data.set(j, temp);
    }

    public void add(E e) {
        data.addLast(e);
        siftUp(data.getSize() - 1);
    }
    private void siftUp(int index) {
        //index不是根节点(根节点不要上浮了) 并且孩子比父亲大
        while (index != 0 && data.get(index).compareTo(data.get(parent(index))) > 0) {
            swap(index, parent(index));
            index = parent(index);
        }
    }
    public E findMax() {
        if (isEmpty()) {
            throw new IllegalArgumentException("堆为空");
        }

        return data.get(0);
    }

    public E extractMax() {
        E ret = findMax();
        swap(0,data.getSize() - 1);
        data.removeLast();
        siftDown(0);
        return ret;
    }

    private void siftDown(int index) {
        //没有孩子时，下沉结束
        while (leftChild(index) < size()) {
            int max = leftChild(index);
            int rightIndex = rightChild(index);
            if (rightIndex < size()) {
                max = data.get(max).compareTo(data.get(rightIndex)) > 0 ? max : rightIndex;
            }
            //最大孩子比父节点小时，下沉结束
            if (data.get(max).compareTo(data.get(index)) <= 0) {
                break;
            }
            swap(max,index);
            index = max;
        }
    }

    //取出堆中的最大元素，并添加一个新元素e
    public E replace(E e) {
        E ret = findMax();

        data.set(0,e);
        siftDown(0);

        return ret;
    }
}
```

### 基于堆的优先队列

```java
public class PriorityQueue<E extends Comparable<E>> implements Queue<E> {
    private MaxHeap<E> maxHeap;
    
    public PriorityQueue() {
        maxHeap = new MaxHeap<>();
    }
    
    @Override
    public void enqueue(E e) {
        maxHeap.add(e);
    }

    @Override
    public E dequeue() {
        return maxHeap.extractMax();
    }

    @Override
    public E getFront() {
        return maxHeap.findMax();
    }

    @Override
    public int getSize() {
        return maxHeap.size();
    }

    @Override
    public boolean isEmpty() {
        return maxHeap.isEmpty();
    }
}
```

## 线段树

对于有一类的问题，我们主要关心的是线段(区间)，比如说查询一个区间 `[i, j]` 内的最大值，最小值等等。假设你有一个网站，你想查询某年(或某年以后)的用户访问量，消费最多的用户等等，这些都是在某个区间内进行查询，一般线段树的区间是固定的，不包含删除和添加的操作，只有查询和更新的操作

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703091505.png" width="80%"/>
</center>
### 线段树的表示

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703091616.png"/>
</center>
现在如果假设有n个元素，用数组存储的话，需要多少空间呢

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703093701.png" width="95%"/>
</center>
```java
public class SegmentTree<E> {
    private E[] tree;
    private E[] data;

    public SegmentTree(E[] arr) {
        data = (E[]) new Object[arr.length];
        for (int i = 0; i < arr.length; i++) {
            data[i] = arr[i];
        }

        tree = (E[]) new Object[4 * data.length];
    }

    public int getSize() {
        return data.length;
    }
    public E get(int index) {
        if (index < 0 || index >= data.length) {
            throw new IllegalArgumentException("参数错误");
        }
        return data[index];
    }
    
    private int leftChild(int index) {
        return 2 * index + 1;
    }
    private int rightChild(int index) {
        return 2 * index + 2;
    }
}
```

### 实现

#### 创建线段树

下面就要根据数组来创建一棵线段树，我们的方法先创建下面的子线段树，然后由这些子线段树合并成大的线段树，以此类推

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703093915.png" width="90%"/>
</center>
在合并左右子树的过程中，我们不能写死合并的过程，具体怎么合并应该由业务决定，由用户去决定如何合并，所以合并的过程我们写一个接口，具体的实现由用户去实现

```java
public interface Merger<E> {
    public E merge(E a, E b);
}
```

然后我们在构造方法中添加创建线段树的过程(为了创建线段树，增加了一个辅助方法)

```java
private Merger<E> merger;
//merger由用户传入 用户决定如何合并
public SegmentTree(E[] arr, Merger<E> merger) {
    this.merger = merger;
    
    data = (E[]) new Object[arr.length];
    for (int i = 0; i < arr.length; i++) {
        data[i] = arr[i];
    }
    
    tree = (E[]) new Object[4 * data.length];
    //构造线段树 创建根节点为0，范围为[0,data.length - 1]的线段树
    buildSegmentTree(0, 0, data.length - 1);
}

//在treeIndex创建一棵[l,r]的线段树
private void buildSegmentTree(int treeIndex, int l, int r) {
    if (l == r) {
        tree[treeIndex] = data[l];
        return;
    }
    
    //l != r 那么就要创建子树的线段树
    int leftTreeIndex = leftChild(treeIndex);
    int rightTreeIndex = rightChild(treeIndex);
    int mid = l + (r - l) / 2; //(l +r) / 2中l + r可能会大于int表示的范围从而溢出
    buildSegmentTree(leftTreeIndex, l, mid);
    buildSegmentTree(rightTreeIndex, mid + 1, r);
    
    //融合的方法由用户传入
    tree[treeIndex] = merger.merge(tree[leftTreeIndex],tree[rightTreeIndex]);
}
```

为了方便我们打印出线段树，我们实现一个 `toString()` 方法

```java
@Override
public String toString() {
    StringBuilder res = new StringBuilder();
    res.append("[");
    for (int i = 0; i < tree.length; i++) {
        if (tree[i] != null) {
            res.append(tree[i]);
        } else {
            res.append("null");
        }
        if (i != tree.length - 1) {
            res.append(", ");
        }
    }
    res.append("]");
    return res.toString();
}
```

#### 查询

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703094104.png" width="80%"/>
</center>
实现代码

```java
public E query(int queryL, int queryR) {
    if (queryL < 0 || queryL >= data.length
            || queryR < 0 || queryR >= data.length
            || queryL > queryR) {
        throw new IllegalArgumentException("参数错误");
    }
    return query(0, 0, data.length - 1, queryL, queryR);
}
private E query(int treeIndex, int l, int r, int queryL, int queryR) {
    if (l == queryL && r == queryR) {
        return tree[treeIndex];
    }
    int leftChildIndex = leftChild(treeIndex);
    int rightChildIndex = rightChild(treeIndex);
    int mid = l + (r - l) / 2;
    if (queryL >= mid + 1) {
        return query(rightChildIndex, mid+1, r, queryL, queryR);
    } else if (queryR <= mid) {
        return query(leftChildIndex, l, mid, queryL, queryR);
    }
    E leftResult = query(leftChildIndex, l, mid, queryL, mid);
    E rightResult = query(rightChildIndex, mid + 1, r, mid + 1, queryR);
    return merger.merge(leftResult, rightResult);
}
```

#### 更新

```java
public void set(int index, E e) {
    if (index < 0 || index >= data.length) {
        throw new IllegalArgumentException("参数错误");
    }
    
    set(0, 0, data.length - 1, index, e);
}
private void set(int treeIndex, int l, int r, int index, E e) {
    if (l == r) {
        tree[treeIndex] = e;
        return;
    }
    int leftChildIndex = leftChild(treeIndex);
    int rightChildIndex = rightChild(treeIndex);
    int mid = l + (r - l) / 2;
    if (index >= mid + 1) {
        set(rightChildIndex, mid+1, r, index, e);
    } else {
        set(leftChildIndex, l, mid, index, e);
    }
    
    tree[treeIndex] = merger.merge(tree[leftChildIndex], tree[rightChildIndex]);
}
```

#### 完整代码

```java
public class SegmentTree<E>{
    private E[] tree;
    private E[] data;
    private Merger<E> merger;

    public SegmentTree(E[] arr, Merger<E> merger) {
        this.merger = merger;

        data = (E[]) new Object[arr.length];
        for (int i = 0; i < arr.length; i++) {
            data[i] = arr[i];
        }

        tree = (E[]) new Object[4 * data.length];
        buildSegmentTree(0, 0, data.length - 1);
    }

    //在treeIndex创建一棵[l,r]的线段树
    private void buildSegmentTree(int treeIndex, int l, int r) {
        if (l == r) {
            tree[treeIndex] = data[l];
            return;
        }

        int leftTreeIndex = leftChild(treeIndex);
        int rightTreeIndex = rightChild(treeIndex);

        int mid = l + (r - l) / 2; //(l +r) / 2中l + r可能会大于int表示的范围从而溢出
        buildSegmentTree(leftTreeIndex, l, mid);
        buildSegmentTree(rightTreeIndex, mid + 1, r);

        tree[treeIndex] = merger.merge(tree[leftTreeIndex],tree[rightTreeIndex]);
    }

    public E query(int queryL, int queryR) {
        if (queryL < 0 || queryL >= data.length
                || queryR < 0 || queryR >= data.length
                || queryL > queryR) {
            throw new IllegalArgumentException("参数错误");
        }

        return query(0, 0, data.length - 1, queryL, queryR);
    }

    private E query(int treeIndex, int l, int r, int queryL, int queryR) {
        if (l == queryL && r == queryR) {
            return tree[treeIndex];
        }

        int leftChildIndex = leftChild(treeIndex);
        int rightChildIndex = rightChild(treeIndex);
        int mid = l + (r - l) / 2;

        if (queryL >= mid + 1) {
            return query(rightChildIndex, mid+1, r, queryL, queryR);
        } else if (queryR <= mid) {
            return query(leftChildIndex, l, mid, queryL, queryR);
        }

        E leftResult = query(leftChildIndex, l, mid, queryL, mid);
        E rightResult = query(rightChildIndex, mid + 1, r, mid + 1, queryR);

        return merger.merge(leftResult, rightResult);
    }


    public void set(int index, E e) {
        if (index < 0 || index >= data.length) {
            throw new IllegalArgumentException("参数错误");
        }

        set(0, 0, data.length - 1, index, e);
    }

    private void set(int treeIndex, int l, int r, int index, E e) {
        if (l == r) {
            tree[treeIndex] = e;
            return;
        }
        int leftChildIndex = leftChild(treeIndex);
        int rightChildIndex = rightChild(treeIndex);
        int mid = l + (r - l) / 2;
        if (index >= mid + 1) {
            set(rightChildIndex, mid+1, r, index, e);
        } else {
            set(leftChildIndex, l, mid, index, e);
        }

        tree[treeIndex] = merger.merge(tree[leftChildIndex], tree[rightChildIndex]);
    }

    public int getSize() {
        return data.length;
    }
    public E get(int index) {
        if (index < 0 || index >= data.length) {
            throw new IllegalArgumentException("参数错误");
        }
        return data[index];
    }

    private int leftChild(int index) {
        return 2 * index + 1;
    }
    private int rightChild(int index) {
        return 2 * index + 2;
    }

    @Override
    public String toString() {
        StringBuilder res = new StringBuilder();
        res.append("[");
        for (int i = 0; i < tree.length; i++) {
            if (tree[i] != null) {
                res.append(tree[i]);
            } else {
                res.append("null");
            }

            if (i != tree.length - 1) {
                res.append(", ");
            }
        }
        res.append("]");
        return res.toString();
    }
}
```

## Trie

`Trie` 树又称为字典树、前缀树。如果我们使用一般树结构去查询一个数据集里的单词，它的复杂度是 `O(log n)`，但是如果我们使用 `Trie` 去查询单词的话，查询的复杂度只与单词的长度有关，与数据的规模无关。比如对于一个 $2^{20}$ 规模的数据集，我们去查一个单词 `"word"`，一般树的复杂度为 `O(20)`，而 `Trie` 树的复杂度为 `O(4)`，其中 4 是单词的长度，所以 `Trie` 树是一种很高效的查询字符串的树结构。

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703100017.png" width="80%"/>
</center>
```java
class Node {
    char c;
    Node next[26];
}
```

但是这样考虑忽略了大小写，并且没有考虑一些特殊的字符，如 `@` 等符号或标点符号。所以我们每个节点不再是静态的指向 26 个节点，而是动态的指向若干个节点

```java
class Node {
    char c;
    Map<Character,Node> next;
}
```

另外我们通过某个字符来到一个节点，可以通过 `Map `已经知道了，所以我们不必存储这个字符

```java
class Node {
    Map<Character,Node> next;
}
```

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703094429.png" width="50%"/>
</center>
另外通过叶子节点是无法区别单词的结尾的，因为有的单词可能为某个单词的前缀，如 `"pan"` 为 `"panda"` 的前缀，所以我们要增加一个变量 `isWord` 来表示是否是单词的结尾

```java
import java.util.TreeMap;

public class Trie {
    private class Node {
        public boolean isWord;
        public TreeMap<Character,Node> next;

        public Node(boolean isWord) {
            this.isWord = isWord;
            next = new TreeMap<>();
        }

        public Node() {
            this(false);
        }
    }

    private Node root;
    private int size;

    public Trie() {
        root = new Node();
        size = 0;
    }

    public int getSize() {
        return size;
    }
}
```

### 实现

#### 添加单词

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703094613.png" width="90%"/>
</center>
```java
public void add(String word) {
    Node cur = root;
    for (int i = 0; i < word.length(); i++) {
        char c = word.charAt(i);
        //判断是否有指向这个字符的节点
        if (cur.next.get(c) == null) {
            //没有则新建一个节点
            cur.next.put(c, new Node());
        }
        //移动到这个节点
        cur = cur.next.get(c);
    }
    //遍历完毕，判断这个节点是否被标记为单词的结尾 如果没有则标记并且维护size++
    if (!cur.isWord) {
        cur.isWord = true;
        size++;
    }
}
```

#### 查询单词

查询单词的逻辑与添加单词的逻辑高度重复，如果在查询过程中遇到没有指向该字符的节点，则直接返回 `false`，如果遍历完毕都没有发生上面的情况，则判断该节点是否被标记为单词的结尾，如果没有则返回 `false`，否则返回 `true`。

```java
public boolean contains(String word) {
    Node cur = root;
    for (int i = 0; i < word.length(); i++) {
        char c = word.charAt(i);
        if (cur.next.get(c) == null) {
            return false;
        }
        cur = cur.next.get(c);
    }
    
    return cur.isWord;
}
```

#### 前缀搜索

查询是否包含某个前缀，与 `contains()` 方法几乎一样，不过最后不用判断是否是单词结尾，直接返回 `true`

```java
public boolean isPrefix(String prefix) {
    Node cur = root;
    for (int i = 0; i < prefix.length(); i++) {
        char c = prefix.charAt(i);
        if (cur.next.get(c) == null) {
            return false;
        }
        cur = cur.next.get(c);
    }
    return true;
}
```

### 简单字符匹配

对于字符串中的字符 `.` 规定它可以匹配任意的字符，那么这样的一个匹配算法如何写，如果我们遇到的字符不是 `.` 的话，逻辑和上面一样，如果遇到的是`.`的话，我们就要去搜索该节点中所有的分叉(子树)

```java
public boolean match(String word) {
    return match(root, word, 0);
}
private boolean match(Node node, String word, int index) {
    //递归终止条件
    if (index == word.length()) {
        return node.isWord;
    }
    
    char c = word.charAt(index);
    //如果不是.
    if (c != '.') {
        //没有指向该字符的节点 返回false
        if (node.next.get(c) == null) {
            return false;
        } else {
            //否则继续匹配
            return match(node.next.get(c), word, index + 1);
        }
    } else {
        //如果是. 去该节点的所有分叉中搜索
        for (char nextChar : node.next.keySet()) {
            //如果有任一个分叉匹配到了，则返回true
            if (match(node.next.get(nextChar), word, index + 1)) {
                return true;
            }
        }
        //说明上面的没有一个匹配成功了，返回fasle
        return false;
    }
}
```

### 全部代码

```java
import java.util.TreeMap;

public class Trie {
    private class Node {
        public boolean isWord;
        public TreeMap<Character,Node> next;

        public Node(boolean isWord) {
            this.isWord = isWord;
            next = new TreeMap<>();
        }

        public Node() {
            this(false);
        }
    }

    private Node root;
    private int size;

    public Trie() {
        root = new Node();
        size = 0;
    }

    public int getSize() {
        return size;
    }

    public void add(String word) {
        Node cur = root;
        for (int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            if (cur.next.get(c) == null) {
                cur.next.put(c, new Node());
            }
            cur = cur.next.get(c);
        }
        if (!cur.isWord) {
            cur.isWord = true;
            size++;
        }
    }

    public boolean contains(String word) {
        Node cur = root;
        for (int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            if (cur.next.get(c) == null) {
                return false;
            }
            cur = cur.next.get(c);
        }

        return cur.isWord;
    }

    public boolean isPrefix(String prefix) {
        Node cur = root;
        for (int i = 0; i < prefix.length(); i++) {
            char c = prefix.charAt(i);
            if (cur.next.get(c) == null) {
                return false;
            }
            cur = cur.next.get(c);
        }
        return true;
    }

    public boolean match(String word) {
        return match(root, word, 0);
    }
    private boolean match(Node node, String word, int index) {
        //递归终止条件
        if (index == word.length()) {
            return node.isWord;
        }

        char c = word.charAt(index);
        if (c != '.') {
            if (node.next.get(c) == null) {
                return false;
            } else {
                return match(node.next.get(c), word, index + 1);
            }
        } else {
            for (char nextChar : node.next.keySet()) {
                if (match(node.next.get(nextChar), word, index + 1)) {
                    return true;
                }
            }
            //说明上面的没有一个匹配成功了
            return false;
        }
    }
}
```

## 并查集

我们之前遇到的树结构都是由父亲指向孩子，但是并查集不一样，它是由孩子指向父亲的一种结构，并查集结构可以非常高效的回答连接问题(Connectivity Problem)，它可以很快的判断网络中节点的连接状态。并查集主要支持两个动作

- `union(p, q)`
  - 将元素 `p, q` 连接起来
- `isConnected(p, q)`
  - 判断元素 `p, q` 是否是连接的，即是否所属一个集合

这里先给出并查集的接口，后面我们将实现多个版本的并查集

```java
public interface UF {
    public int getSize();
    public boolean isConnected(int p, int q);
    public void unionElements(int p, int q);
}
```

### Quick Find

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703100212.png" width="80%"/>
</center>
```java
//第一版的并查集
public class UnionFind1 implements UF{
    private int[] id;

    public UnionFind1(int size) {
        id = new int[size];

        //这时id全部为0，相当于在一个集合中 一开始应该全部不在一个集合中
        for (int i = 0; i < id.length; i++) {
            id[i] = i;
        }
    }

    @Override
    public int getSize() {
        return id.length;
    }

    //找到元素p所属的集合
    private int find(int p) {
        if (p < 0 || p >= id.length) {
            throw new IllegalArgumentException("参数错误");
        }

        return id[p];
    }

    @Override
    public boolean isConnected(int p, int q) {
        return find(p) == find(q);
    }

    @Override
    public void unionElements(int p, int q) {
        if (find(p) == find(q)) {
            return;
        }

        for (int i = 0; i < id.length; i++) {
            if (id[i] == find(p)) {
                id[i] = find(q);
            }
        }
    }
}
```

### Quick Union

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703100443.png" width="95%"/>
</center>
```java
//第二版的并查集
public class UnionFind2 implements UF{
    private int[] parent;

    public UnionFind2(int size) {
        parent = new int[size];

        for (int i = 0; i < parent.length; i++) {
            parent[i] = i;
        }
    }

    @Override
    public int getSize() {
        return parent.length;
    }

    private int find(int index) {
        if (index < 0 || index >= parent.length) {
            throw new IllegalArgumentException("参数错误");
        }

        while (index != parent[index]) {
            index = parent[index];
        }
        return index;
    }

    @Override
    public boolean isConnected(int p, int q) {
        return find(p) == find(q);
    }

    @Override
    public void unionElements(int p, int q) {
        int pRoot = find(p);
        int qRoot = find(q);
        if (pRoot == qRoot) {
            return;
        } else {
            parent[pRoot] = parent[qRoot];
        }
    }
}
```

### 基于rank的优化

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703100629.png"/>
</center>
```java
//第三版的并查集
public class UnionFind3 implements UF{
    private int[] parent;
    //记录根节点的高度
    private int[] rank;

    public UnionFind3(int size) {
        parent = new int[size];
        rank = new int[size];

        for (int i = 0; i < parent.length; i++) {
            parent[i] = i;
            rank[i] = 1;
        }
    }

    @Override
    public int getSize() {
        return parent.length;
    }

    private int find(int index) {
        if (index < 0 || index >= parent.length) {
            throw new IllegalArgumentException("参数错误");
        }

        while (index != parent[index]) {
            index = parent[index];
        }
        return index;
    }

    @Override
    public boolean isConnected(int p, int q) {
        return find(p) == find(q);
    }

    @Override
    public void unionElements(int p, int q) {
        int pRoot = find(p);
        int qRoot = find(q);
        if (pRoot == qRoot) {
            return;
        }

        if (rank[pRoot] <= rank[qRoot]){
            parent[pRoot] = parent[qRoot];
            //只要在两个数的高度相等的时候 树的高度才会增加
            if (rank[pRoot] == rank[qRoot]) {
                rank[qRoot]++;
            }
        } else {
            parent[qRoot] = parent[pRoot];
        }
    }
}
```

### 路径压缩

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703100827.png" width="80%"/>
</center>
```java
//第四版的并查集
public class UnionFind4 implements UF{
    private int[] parent;
    private int[] rank;

    public UnionFind4(int size) {
        parent = new int[size];
        rank = new int[size];

        for (int i = 0; i < parent.length; i++) {
            parent[i] = i;
            rank[i] = 1;
        }
    }

    @Override
    public int getSize() {
        return parent.length;
    }

    private int find(int index) {
        if (index < 0 || index >= parent.length) {
            throw new IllegalArgumentException("参数错误");
        }

        while (index != parent[index]) {
            //只添加了这一行代码
            parent[index] = parent[parent[index]];
            index = parent[index];
        }
        return index;
    }

    @Override
    public boolean isConnected(int p, int q) {
        return find(p) == find(q);
    }

    @Override
    public void unionElements(int p, int q) {
        int pRoot = find(p);
        int qRoot = find(q);
        if (pRoot == qRoot) {
            return;
        }
		
        //这时rank不代表高度 因为在路径压缩时没有维护rank
        //但是整体上rank还是能够表示大小关系的
        if (rank[pRoot] <= rank[qRoot]){
            parent[pRoot] = parent[qRoot];
            
            if (rank[pRoot] == rank[qRoot]) {
                rank[qRoot]++;
            }
        } else {
            parent[qRoot] = parent[pRoot];
        }
    }
}
```

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703100940.png" width="80%"/>
</center>
```java
//第五版的并查集
public class UnionFind5 implements UF{
    private int[] parent;
    private int[] rank;

    public UnionFind5(int size) {
        parent = new int[size];
        rank = new int[size];

        for (int i = 0; i < parent.length; i++) {
            parent[i] = i;
            rank[i] = 1;
        }
    }

    @Override
    public int getSize() {
        return parent.length;
    }

    private int find(int index) {
        if (index < 0 || index >= parent.length) {
            throw new IllegalArgumentException("参数错误");
        }
		
        //修改了这里
        if (index != parent[index]) {
            parent[index] = find(parent[index]);
        }
        return parent[index];
    }

    @Override
    public boolean isConnected(int p, int q) {
        return find(p) == find(q);
    }

    @Override
    public void unionElements(int p, int q) {
        int pRoot = find(p);
        int qRoot = find(q);
        if (pRoot == qRoot) {
            return;
        }

        if (rank[pRoot] <= rank[qRoot]){
            parent[pRoot] = parent[qRoot];
            
            if (rank[pRoot] == rank[qRoot]) {
                rank[qRoot]++;
            }
        } else {
            parent[qRoot] = parent[pRoot];
        }
    }
}
```

第五版的效率不一定比第四版的好，因为第四版最后也可能做到"扁平化"，并且第五版的递归操作比较耗时。

## AVL树

### 概念及实现

我们在研究二分搜索树时发现，如果我们将数据顺序添加进树中时，它有会退化成一棵链表，即所有的元素都添加到一个孩子上，这样树结构的优势就体现不出来，为了不使左右孩子的高度相差太大，我们需要对树进行调整，使树达到平衡，成为一棵平衡二叉树，`AVL` 就是一种经典的平衡二叉树

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703110331.png" width="60%"/>
</center>
在 `AVL` 中，我们定义的平衡二叉树为，对于任意一个节点，左子树和右子树的高度相差不能超过 `1`。

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703110510.png" width="70%"/>
</center>


我们为每一个节点标注好高度值，计算方法为取左右子树高度较高的高度，然后 `+1`

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703110604.png" width="70%"/>
</center>
然后我们还有记录节点左右子树的高度差，我们称之为平衡因子(规定用左子树的高度-右子树的高度)

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703110648.png" width="70%"/>
</center>
由于我们只是在添加元素和删除元素时对树进行调整，其余的代码同二分搜索树是相同的，所以就不贴出所有的代码，只给出不同的代码，首先我们需要在 `Node` 类中添加一个 `height` 变量来记录高度

```java
private class Node {
    public E e;
    public Node left;
    public Node right;
    //高度
    public int height;
    public Node(E e) {
        this.e = e;
        left = null;
        right = null;
        //高度初始为1
        height = 1;
    }
    @Override
    public String toString() {
        return e.toString();
    }
}
```

新增加一个获得某节点高度的函数和平衡因子的函数

```java
private int getHeight(Node node) {
    if (node == null) {
        return 0;
    }
    return node.height;
}
private int getBalanceFactor(Node node) {
    if (node == null) {
        return 0;
    }
    
    return getHeight(node.left) - getHeight(node.right);
}
```

有了这些因素，我们一般需要在添加元素时进行维护，重新计算高度和平衡因子，从而进行调整

```java
private Node add(Node node, E e) {
    if (node == null) {
        size++;
        return new Node(e);
    }
    if (e.compareTo(node.e) < 0) {
        node.left = add(node.left, e);
    } else if (e.compareTo(node.e) > 0) {
        node.right = add(node.right,e);
    }
    
    //更新高度
    node.height = Math.max(getHeight(node.left),getHeight(node.right)) + 1;
    //计算平衡因子
    int balanceFactor = getBalanceFactor(node);
    if (Math.abs(balanceFactor) > 1) {
        //进行调整
    }
    return node;
}
```

我们后面的内容主要是如何调整，后面所以只给出如何调整的代码，在学如何调整之前，我们来写两个辅助函数来判断这棵树是不是二分搜索树和 `AVL` 树，因为如果我们的代码有问题的话，有可能破坏二分搜索树的性质，这样有利于我们检查，那怎么检查一棵树是不是二分搜索树，我们根据二分搜索树的性质，它的中序遍历的结果是从小到大的特性，我们重写中序遍历为

```java
public boolean isBST() {
    ArrayList<E> arrayList = new ArrayList<>();
    inOrder(root, arrayList);
    
    for (int i = 1; i < arrayList.size(); i++) {
        if (arrayList.get(i-1).compareTo(arrayList.get(i)) > 0) 
            return false;
        }
    }
    return true;
}
private void inOrder(Node node, ArrayList<E> arrayList) {
    if (node == null) {
        return;
    }
    
    inOrder(node.left, arrayList);
    arrayList.add(node.e);
    inOrder(node.right, arrayList);
}
```

现在我们判断这棵树是不是平衡二叉树

```java
public boolean isBalanced() {
    return isBalanced(root);
}
//判断某个节点是不是平衡
private boolean isBalanced(Node node) {
    if (node == null) {
        return true;
    }
    int balanceFactor = getBalanceFactor(node);
    if (Math.abs(balanceFactor) > 1) {
        return false;
    }
    return isBalanced(node.left) && isBalanced(node.right);
}
```

下面对不平衡的四种情形进行讨论，并给出调整方法

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703101655.png"/>
</center>
```java
// 对节点y进行向右旋转操作，返回旋转后新的根节点x
//        y                              x
//       / \                           /   \
//      x   T4     向右旋转 (y)        z     y
//     / \       - - - - - - - ->    / \   / \
//    z   T3                       T1  T2 T3 T4
//   / \
// T1   T2
private Node rightRotate(Node y) {
    Node x = y.left;
    Node T3 = x.right;
    x.right = y;
    y.left = T3;
    
    //更新x和y的高度值 先更新y的，因为y是x的右孩子，x的更新取决于y
    y.height = Math.max(getHeight(y.left), getHeight(y.right)) + 1;
    x.height = Math.max(getHeight(x.left), getHeight(x.right)) + 1;
    
    return x;
}
// 对节点y进行向左旋转操作，返回旋转后新的根节点x
//    y                             x
//  /  \                          /   \
// T4   x      向左旋转 (y)       y     z
//     / \   - - - - - - - ->   / \   / \
//   T3  z                     T4 T3 T1 T2
//      / \
//     T1 T2
private Node leftRotate(Node y) {
    Node x = y.right;
    Node T3 = x.left;
    x.left = y;
    y.right = T3;
    
    y.height = Math.max(getHeight(y.left), getHeight(y.right)) + 1;
    x.height = Math.max(getHeight(x.left), getHeight(x.right)) + 1;
    
    return x;
}

public void add(E e) {
    root = add(root, e);
}
private Node add(Node node, E e) {
    if (node == null) {
        size++;
        return new Node(e);
    }
    if (e.compareTo(node.e) < 0) {
        node.left = add(node.left, e);
    } else if (e.compareTo(node.e) > 0) {
        node.right = add(node.right,e);
    }
    //更新高度
    node.height = Math.max(getHeight(node.left),getHeight(node.right)) + 1;
    //计算平衡因子
    int balanceFactor = getBalanceFactor(node);
    //调整
    if (balanceFactor > 1 && getBalanceFactor(node.left) >= 0) {
        return rightRotate(node);
    }
    if (balanceFactor < -1 && getBalanceFactor(node.right) <= 0) {
        return leftRotate(node);
    }
    if (balanceFactor > 1 && getBalanceFactor(node.left) < 0) {
        node.left = leftRotate(node.left);
        return rightRotate(node);
    }
    if (balanceFactor < -1 && getBalanceFactor(node.right) > 0) {
        node.right = rightRotate(node.right);
        return leftRotate(node);
    }
    return node;
}
public void remove(E e) {
    root = remove(root, e);
}
private Node remove(Node node, E e) {
    if (node == null) {
        return null;
    }
    Node retNode;
    if (e.equals(node.e)) {
        if (node.right == null) {
            Node leftNode = node.left;
            node.left = null;
            size--;
            retNode = leftNode;
        } else if (node.left == null) {
            Node rightNode = node.right;
            node.right = null;
            size--;
            retNode = rightNode;
        } else {
            Node successor = minimum(node.right);
            ////由于removeMin没有维持balance，所以我们复用remove
            successor.right = remove(node.right,successor.e);
            successor.left = node.left;
            node.left = node.right = null;
            
            retNode = successor;
        }
    } else if (e.compareTo(node.e) < 0) {
        node.left = remove(node.left, e);
        retNode = node;
    } else {
        node.right = remove(node.right, e);
        retNode = node;
    }
    
    //否则retNode.height会有空指针异常
    if (retNode == null) {
        return null;
    }
    
    //更新高度
    retNode.height = Math.max(getHeight(retNode.left),getHeight(retNode.right)) + 1;
    //计算平衡因子
    int balanceFactor = getBalanceFactor(retNode);
    if (balanceFactor > 1 && getBalanceFactor(retNode.left) >= 0) {
        return rightRotate(retNode);
    }
    if (balanceFactor < -1 && getBalanceFactor(retNode.right) <= 0) {
        return leftRotate(retNode);
    }
    if (balanceFactor > 1 && getBalanceFactor(retNode.left) < 0) {
        retNode.left = leftRotate(retNode.left);
        return rightRotate(retNode);
    }
    if (balanceFactor < -1 && getBalanceFactor(retNode.right) > 0) {
        retNode.right = rightRotate(retNode.right);
        return leftRotate(retNode);
    }
    return retNode;
}
```

### 完整代码

```java
import java.util.ArrayList;

public class AVLTree<E extends Comparable<E>> {
    private class Node {
        public E e;
        public Node left;
        public Node right;
        public int height;

        public Node(E e) {
            this.e = e;
            left = null;
            right = null;
            height = 1;
        }

        @Override
        public String toString() {
            return e.toString();
        }
    }

    //根节点
    private Node root;
    //树中元素的个数
    private int size;

    public AVLTree() {
        root = null;
        size = 0;
    }

    public int size() {
        return size;
    }
    public boolean isEmpty() {
        return size == 0;
    }
    private int getHeight(Node node) {
        if (node == null) {
            return 0;
        }

        return node.height;
    }

    private int getBalanceFactor(Node node) {
        if (node == null) {
            return 0;
        }

        return getHeight(node.left) - getHeight(node.right);
    }

    public boolean isBST() {
        ArrayList<E> arrayList = new ArrayList<>();
        inOrder(root, arrayList);

        for (int i = 1; i < arrayList.size(); i++) {
            if (arrayList.get(i-1).compareTo(arrayList.get(i)) > 0) {
                return false;
            }
        }
        return true;
    }
    private void inOrder(Node node, ArrayList<E> arrayList) {
        if (node == null) {
            return;
        }

        inOrder(node.left, arrayList);
        arrayList.add(node.e);
        inOrder(node.right, arrayList);
    }

    public boolean isBalanced() {
        return isBalanced(root);
    }
    //判断某个节点是不是平衡
    private boolean isBalanced(Node node) {
        if (node == null) {
            return true;
        }
        int balanceFactor = getBalanceFactor(node);
        if (Math.abs(balanceFactor) > 1) {
            return false;
        }
        return isBalanced(node.left) && isBalanced(node.right);
    }

    private Node rightRotate(Node y) {
        Node x = y.left;
        Node T3 = x.right;
        x.right = y;
        y.left = T3;

        //更新x和y的高度值 先更新y的，因为y是x的右孩子，x的更新取决于y
        y.height = Math.max(getHeight(y.left), getHeight(y.right)) + 1;
        x.height = Math.max(getHeight(x.left), getHeight(x.right)) + 1;

        return x;
    }

    private Node leftRotate(Node y) {
        Node x = y.right;
        Node T3 = x.left;
        x.left = y;
        y.right = T3;

        y.height = Math.max(getHeight(y.left), getHeight(y.right)) + 1;
        x.height = Math.max(getHeight(x.left), getHeight(x.right)) + 1;

        return x;
    }

    public void add(E e) {
        root = add(root, e);
    }


    private Node add(Node node, E e) {
        if (node == null) {
            size++;
            return new Node(e);
        }

        if (e.compareTo(node.e) < 0) {
            node.left = add(node.left, e);
        } else if (e.compareTo(node.e) > 0) {
            node.right = add(node.right,e);
        }

        //更新高度
        node.height = Math.max(getHeight(node.left),getHeight(node.right)) + 1;
        //计算平衡因子
        int balanceFactor = getBalanceFactor(node);

        if (balanceFactor > 1 && getBalanceFactor(node.left) >= 0) {
            return rightRotate(node);
        }
        if (balanceFactor < -1 && getBalanceFactor(node.right) <= 0) {
            return leftRotate(node);
        }
        if (balanceFactor > 1 && getBalanceFactor(node.left) < 0) {
            node.left = leftRotate(node.left);
            return rightRotate(node);
        }
        if (balanceFactor < -1 && getBalanceFactor(node.right) > 0) {
            node.right = rightRotate(node.right);
            return leftRotate(node);
        }

        return node;
    }

    public boolean contains(E e) {
        return contains(root, e);
    }

    private boolean contains(Node node, E e) {
        if (node == null) {
            return false;
        }

        if (e.equals(node.e)) {
            return true;
        } else if (e.compareTo(node.e) < 0) {
            return contains(node.left, e);
        } else {
            return contains(node.right,e);
        }
    }

    public E minimum() {
        if (size == 0) {
            throw new IllegalArgumentException("树为空");
        }
        return minimum(root).e;
    }

    private Node minimum(Node node) {
        if (node.left == null) {
            return node;
        }
        return minimum(node.left);
    }

    public void remove(E e) {
        root = remove(root, e);
    }
    private Node remove(Node node, E e) {
        if (node == null) {
            return null;
        }
        Node retNode;
        if (e.equals(node.e)) {
            if (node.right == null) {
                Node leftNode = node.left;
                node.left = null;
                size--;
                retNode = leftNode;
            } else if (node.left == null) {
                Node rightNode = node.right;
                node.right = null;
                size--;
                retNode = rightNode;
            } else {
                Node successor = minimum(node.right);
                successor.right = remove(node.right,successor.e);//由于removeMin没有维持balance，所以我们用remove
                successor.left = node.left;
                node.left = node.right = null;
                //size--; 在removeMin中已经维护size了
                retNode = successor;
            }
        } else if (e.compareTo(node.e) < 0) {
            node.left = remove(node.left, e);
            retNode = node;
        } else {
            node.right = remove(node.right, e);
            retNode = node;
        }

        //否则retNode.height会有空指针异常
        if (retNode == null) {
            return null;
        }

        //更新高度
        retNode.height = Math.max(getHeight(retNode.left),getHeight(retNode.right)) + 1;
        //计算平衡因子
        int balanceFactor = getBalanceFactor(retNode);
        if (balanceFactor > 1 && getBalanceFactor(retNode.left) >= 0) {
            return rightRotate(retNode);
        }
        if (balanceFactor < -1 && getBalanceFactor(retNode.right) <= 0) {
            return leftRotate(retNode);
        }
        if (balanceFactor > 1 && getBalanceFactor(retNode.left) < 0) {
            retNode.left = leftRotate(retNode.left);
            return rightRotate(retNode);
        }
        if (balanceFactor < -1 && getBalanceFactor(retNode.right) > 0) {
            retNode.right = rightRotate(retNode.right);
            return leftRotate(retNode);
        }
        return retNode;
    }
}
```

## 红黑树

### 2-3树

`2-3` 树的节点它可以有一个元素，也可以有两个元素，它也满足二分搜索树的性质

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703110752.png" width="75%"/>
</center>
我们把含有两个孩子的节点称为 `2` 节点，含有 `3` 个孩子的节点称为 `3` 节点

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703111306.png" width="70%"/>
</center>
`2-3` 树是一种绝对平衡的树，所谓绝对平衡的树指的是从根节点到任意一个叶子节点，所经过的节点是都是相同的。那么 `2-3` 树是怎么做到的呢? 

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703111521.png"/>
</center>
### 红黑树与2-3树的等价性

由于我们一般每个节点都是表示一个数据的，`2-3` 树有点难以实现，所以有人发明一种树叫做红黑树，它可以说是 `2-3` 树的等价，那么它树如何等价的呢?

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703111718.png" width="70%"/>
</center>
上图想必很清楚的描述了等价的过程

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703111829.png"/>
</center>
现在我们来实现一下上面描述的红黑树，大部分的代码都是和二分搜索树是重合的，只是在添加时有调整，另外这里我们不牵涉到从红黑树中删除元素，因为太复杂了(其实是我不会)

```java
public class RedBlackTree<E extends Comparable<E>> {
    //规定红色为true 黑色为false
    private static final boolean RED = true;
    private static final boolean BLACK = false;

    private class Node {
        public E e;
        public Node left;
        public Node right;
        public boolean color;

        public Node(E e) {
            this.e = e;
            left = null;
            right = null;
            //我们在2-3树中添加节点时 永远是和别的节点融合 所以默认为红色
            color = RED;
        }

        @Override
        public String toString() {
            return e.toString();
        }
    }

    private Node root;
    private int size;

    public RedBlackTree() {
        root = null;
        size = 0;
    }

    public int size() {
        return size;
    }
    public boolean isEmpty() {
        return size == 0;
    }

    public void add(E e) {
        root = add(root, e);
    }
	
    // 判断节点node的颜色
    private boolean isRed(Node node){
        if(node == null)
            return BLACK;
        return node.color;
    }

    private Node add(Node node, E e) {
        if (node == null) {
            size++;
            return new Node(e);
        }

        if (e.compareTo(node.e) < 0) {
            node.left = add(node.left, e);
        } else if (e.compareTo(node.e) > 0) {
            node.right = add(node.right,e);
        }

        return node;
    }

    public boolean contains(E e) {
        return contains(root, e);
    }

    private boolean contains(Node node, E e) {
        if (node == null) {
            return false;
        }

        if (e.equals(node.e)) {
            return true;
        } else if (e.compareTo(node.e) < 0) {
            return contains(node.left, e);
        } else {
            return contains(node.right,e);
        }
    }
}
```

### 红黑树的性质

在了解了红黑树与 `2-3` 等价以后，我们来看红黑树满足哪些性质

- 每个节点或者是红色的，或者的是黑色的
- 根节点是黑色的

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703111918.png" width="80%"/>
</center>
- 每一个叶子节点(最后的空节点是黑色的)
  - 因为红色节点只存在于 3 节点中，而所有的叶子节点都是 2 节点
- 如果一个节点是红色的，那么它的所有孩子节点都是黑色的

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703112042.png" width="70%"/>
</center>
- 从任意一个节点到黑色节点，经过的黑色节点是一样的
  - 因为 `2-3` 树到所有叶子节点的距离都是一样的，而经过的节点，不管是 `2` 节点还是 `3` 节点，都包括一个黑色节点，所以经过的黑色节点是一样的

### 向红黑树中添加元素

因为根节点是黑色的，所以我们在添加完元素后需要将根节点变为黑色

```java
public void add(E e) {
    root = add(root, e);
    
    root.color = BLACK;
}
```

在添加元素到红黑树中时，可能会破坏红黑树的规则，这时就需要红黑树进行自我调整，我们就来看一下添加过程会碰到的所有情形，以及处理方法

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703112214.png"/>
</center>
```java
//   node                     x
//  /   \     左旋转         /  \
// T1   x   --------->   node   T3
//     / \              /   \
//    T2 T3            T1   T2
private Node leftRotate(Node node){
    Node x = node.right;
    // 左旋转
    node.right = x.left;
    x.left = node;
    x.color = node.color;
    node.color = RED;
    return x;
}
```

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703112337.png" width="90%"/>
</center>
```java
// 颜色翻转
private void flipColors(Node node){
    node.color = RED;
    node.left.color = BLACK;
    node.right.color = BLACK;
}
```

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703112718.png" width="90%"/>
</center>
```java
//     node                   x
//    /   \     右旋转       /  \
//   x    T2   ------->   y   node
//  / \                       /  \
// y  T1                     T1  T2
private Node rightRotate(Node node){
    Node x = node.left;
    // 右旋转
    node.left = x.right;
    x.right = node;
    x.color = node.color;
    node.color = RED;
    return x;
}
```

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703112846.png"/>
</center>
对上面的情况进行总结

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703112959.png" width="80%"/>
</center>
```java
private Node add(Node node, E e) {

    if (node == null) {
        size++;
        return new Node(e);
    }
    
    if (e.compareTo(node.e) < 0) {
        node.left = add(node.left, e);
    } else if (e.compareTo(node.e) > 0) {
        node.right = add(node.right,e);
    }
    
    if (isRed(node.right) && !isRed(node.left))
        node = leftRotate(node);
    if (isRed(node.left) && isRed(node.left.left))
        node = rightRotate(node);
    if (isRed(node.left) && isRed(node.right))
        flipColors(node);

    return node;
}
```

### 完整代码

```java
public class RedBlackTree<E extends Comparable<E>> {
    //规定红色为true 黑色为false
    private static final boolean RED = true;
    private static final boolean BLACK = false;

    private class Node {
        public E e;
        public Node left;
        public Node right;
        public boolean color;

        public Node(E e) {
            this.e = e;
            left = null;
            right = null;
            //我们在2-3树中添加节点时 永远是和别的节点融合 所以默认为红色
            color = RED;
        }

        @Override
        public String toString() {
            return e.toString();
        }
    }
    
    private Node root;
    private int size;

    public RedBlackTree() {
        root = null;
        size = 0;
    }

    public int size() {
        return size;
    }
    public boolean isEmpty() {
        return size == 0;
    }

    private boolean isRed(Node node) {
        if (node == null) {
            return BLACK;
        }
        return node.color;
    }

    //   node                     x
    //  /   \     左旋转         /  \
    // T1   x   --------->   node   T3
    //     / \              /   \
    //    T2 T3            T1   T2
    private Node leftRotate(Node node) {
        Node x = node.right;
        node.right = x.left;
        x.left = node;

        x.color = node.color;
        node.color = RED;

        return x;
    }

    //     node                   x
    //    /   \     右旋转       /  \
    //   x    T2   ------->   y   node
    //  / \                       /  \
    // y  T1                     T1  T2
    private Node rightRotate(Node node) {
        Node x = node.left;
        node.left = x.right;
        x.right = node;

        x.color = node.color;
        node.color = RED;

        return x;
    }

    private void flipColors(Node node) {
        node.color = RED;
        node.left.color = BLACK;
        node.right.color = BLACK;
    }

    public void add(E e) {
        root = add(root, e);

        root.color = BLACK;
    }


    private Node add(Node node, E e) {
        if (node == null) {
            size++;
            return new Node(e);
        }

        if (e.compareTo(node.e) < 0) {
            node.left = add(node.left, e);
        } else if (e.compareTo(node.e) > 0) {
            node.right = add(node.right,e);
        }

        if (isRed(node.right) && !isRed(node.left))
            node = leftRotate(node);
        if (isRed(node.left) && isRed(node.left.left))
            node = rightRotate(node);
        if (isRed(node.left) && isRed(node.right))
            flipColors(node);
        
        return node;
    }

    public boolean contains(E e) {
        return contains(root, e);
    }

    private boolean contains(Node node, E e) {
        if (node == null) {
            return false;
        }

        if (e.equals(node.e)) {
            return true;
        } else if (e.compareTo(node.e) < 0) {
            return contains(node.left, e);
        } else {
            return contains(node.right,e);
        }
    }
}
```

## 哈希表

我们通过将我们要查找的某种数据类型转化为一个索引 `index`，然后通过索引去数组中查找，这时它的复杂度就是 `O(1)` 级别的。而将某个数据类型转化为索引的函数我们就称为是哈希函数，比如说将 26 个小写字母转化为索引，我们可以这么写

```java
index = ch - 'a';
```

这样就建立起了一一对应的关系，但是并不是所有的对应关系都是一一对应的，因为数组的容量是有限的，而输入的范围可能是无穷的，所以很有可能不同的键对应着同一个索引，比如说键是字符串，因为字符串的组合方式是非常的多，可以看做是无穷的，我们不可能去开辟一个无穷的空间去与这些字符串一一对应，所以不同的字符串生成的索引很有可能会有冲突，我们称这种情况为哈希冲突。由于上面讲到的哈希冲突，所以我们要设计好哈希函数(`hashCode()`)使得发生哈希冲突的可能性小，即使哈希函数产生的哈希值均匀的分布在数组中。

### 哈希函数的设计

哈希函数应该满足上面提到的：哈希函数产生的哈希值均匀的分布在数组中。数据的类型五花八门，对于特殊的领域有特殊领域的哈希函数的设计方式，甚至还有专门的论文，说这么多就是想说哈希函数的设计十分的复杂，在这里我们只提最简单的一种，哈希函数的设计应该满足

- 一致性
  - 如果 `a == b`，那么 `hashCode(a) == hashCode(b)`
- 高效性
  - 计算迅速
- 均匀性
  - 输出尽可能均匀

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703113145.png" width="80%"/>
</center>
由于 `Java` 中基本数据类型和字符串类型有默认的 `hashCode()` 计算，所以我们就用 `Java` 自带的 `hashCode` 计算基本数据类型和字符串的哈希值，而对于引用类型 `Java` 是根据地址计算的哈希值，所以可能会出现问题，需要我们自己自定义规则，比如对于一个 `Student` 类，我们规定学号以及姓名相同(不区分大小写)就是同一个学生，所以根据一致性原则，它们应该产生相同的哈希值，但是由于 `Java` 默认是根据地址产生哈希值，由于二者的地址是不同的，所以产生的哈希值有极大的概率是不同的，所以我们需要自己创建哈希函数。

### 链地址法

现在我们来演示往哈希表中添加元素的步骤

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703113346.png" width="90%"/>
</center>
```java
import java.util.TreeMap;

public class HashTable<K, V> {
    //数组中存储的是TreeMap这种查找表
    private TreeMap<K, V>[] hashTable;
    private int M;
    private int size;

    public HashTable(int M) {
        this.M = M;
        size = 0;
        hashTable = new TreeMap[M];

        for (int i = 0; i < hashTable.length; i++) {
            hashTable[i] = new TreeMap<>();
        }
    }

    public HashTable() {
        this(97);
    }

    public int getSize() {
        return size;
    }

    //得到在数组中的索引
    private int hash(K key) {
        //与0x7fffffff是为了消除负数
        return (key.hashCode() & 0x7fffffff) % M;
    }

    public void add(K key, V value) {
        TreeMap<K, V> map = hashTable[hash(key)];
        //先查看已经是否有这个键了
        if (map.containsKey(key)) {
            //有则更新
            map.put(key, value);
        } else {
            //没有则进行添加，并维护size
            map.put(key, value);
            size++;
        }
    }

    public V remove(K key, V value) {
        V ret = null;
        TreeMap<K, V> map = hashTable[hash(key)];
        //如果包含键则删除，没有返回null
        if (map.containsKey(key)) {
            ret = map.remove(key);
            size--;
        }

        return ret;
    }
    
    public void set(K key, V value) {
        TreeMap<K, V> map = hashTable[hash(key)];
        //没有该键抛出异常
        if (!map.containsKey(key)) {
            throw new IllegalArgumentException("键不存在");
        }
        
        map.put(key,value);
    }
    
    //直接得到相应的TreeMap，然后去查，TreeMap有检查步骤
    public V get(K key) {
        return hashTable[hash(key)].get(key);
    }
}
```

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20200703113437.png" width="80%"/>
</center>
```java
import java.util.TreeMap;

public class HashTable<K, V> {
    private static final int upperTol = 10;
    private static final int lowerTol = 2;
    private static final int initCapacity = 7;

    //数组中存储的是TreeMap这种查找表
    private TreeMap<K, V>[] hashTable;
    private int M;
    private int size;

    public HashTable(int M) {
        //只显示改变的内容
        //...
        hashTable = new TreeMap[initCapacity];
    }

    public void add(K key, V value) {
        //...

        if (size >= upperTol * M) {
            resize(2 * M);
        }
    }

    public V remove(K key, V value) {
        //...

        if (size < M * lowerTol && M / 2 >= initCapacity) {
            resize(M / 2);
        }

        return ret;
    }

    private void resize(int newM) {
        TreeMap<K,V>[] newHashTable = new TreeMap[newM];

        //后面要更新M，但是还需要旧M遍历数组
        int oldM = M;
        //由于后面要重新计算下标，所以这里要更新M
        M = newM;

        for (int i = 0; i < oldM; i++) {
            TreeMap<K, V> map = hashTable[i];
            for (K key: map.keySet()) {
                //重新计算下标并赋值
                newHashTable[hash(key)].put(key, map.get(key));
            }
        }

        hashTable = newHashTable;
    }
}
```

但是我们发现每次我们都扩容为 `2 \* M`，这时 `M` 就不是一个素数了，为了解决这一个问题，我们准备一个素数表，让 `M` 取素数表中的值，每次扩容 `M` 在素数表中的索引 `+1`，缩容 `-1`

```java
import java.util.TreeMap;

public class HashTable<K, V> {
    //素数表
    private static final int[] capacity = {};
    private static final int upperTol = 10;
    private static final int lowerTol = 2;
    private  int capacityIndex = 0;

    //数组中存储的是TreeMap这种查找表
    private TreeMap<K, V>[] hashTable;
    private int M;
    private int size;

    public HashTable() {
        this.M = capacity[capacityIndex];
        size = 0;
        hashTable = new TreeMap[M];

        for (int i = 0; i < hashTable.length; i++) {
            hashTable[i] = new TreeMap<>();
        }
    }

    public void add(K key, V value) {
        //...
        if (size >= upperTol * M && capacityIndex + 1 < size) {
            capacityIndex++;
            resize(capacity[capacityIndex]);
        }
    }

    public V remove(K key, V value) {
        //...

        if (size < M * lowerTol && capacityIndex - 1 >= 0) {
            capacityIndex--;
            resize(capacity[capacityIndex]);
        }

        return ret;
    }
}
```

### 完整代码

```java
import java.util.TreeMap;

public class HashTable<K, V> {
    private static final int[] capacity = {};
    private static final int upperTol = 10;
    private static final int lowerTol = 2;
    private  int capacityIndex = 0;

    //数组中存储的是TreeMap这种查找表
    private TreeMap<K, V>[] hashTable;
    private int M;
    private int size;

    public HashTable() {
        this.M = capacity[capacityIndex];
        size = 0;
        hashTable = new TreeMap[M];

        for (int i = 0; i < hashTable.length; i++) {
            hashTable[i] = new TreeMap<>();
        }
    }

    public int getSize() {
        return size;
    }

    //得到在数组中的索引
    private int hash(K key) {
        //与0x7fffffff是为了消除负数
        return (key.hashCode() & 0x7fffffff) % M;
    }

    public void add(K key, V value) {
        TreeMap<K, V> map = hashTable[hash(key)];
        //先查看已经是否有这个键了
        if (map.containsKey(key)) {
            //有则更新
            map.put(key, value);
        } else {
            //没有则进行添加，并维护size
            map.put(key, value);
            size++;
        }

        if (size >= upperTol * M && capacityIndex + 1 < capacity.length) {
            capacityIndex++;
            resize(capacity[capacityIndex]);
        }
    }

    public V remove(K key, V value) {
        V ret = null;
        TreeMap<K, V> map = hashTable[hash(key)];
        //如果包含键则删除，没有返回null
        if (map.containsKey(key)) {
            ret = map.remove(key);
            size--;
        }

        if (size < M * lowerTol && capacityIndex - 1 >= 0) {
            capacityIndex--;
            resize(capacity[capacityIndex]);
        }

        return ret;
    }

    public void set(K key, V value) {
        TreeMap<K, V> map = hashTable[hash(key)];
        //没有该键抛出异常
        if (!map.containsKey(key)) {
            throw new IllegalArgumentException("键不存在");
        }

        map.put(key,value);
    }

    //直接得到相应的TreeMap，然后去查，TreeMap有检查步骤
    public V get(K key) {
        return hashTable[hash(key)].get(key);
    }

    private void resize(int newM) {
        TreeMap<K,V>[] newHashTable = new TreeMap[newM];

        //后面要更新M，但是还需要旧M遍历数组
        int oldM = M;
        //由于后面要重新计算下标，所以这里要更新M
        M = newM;

        for (int i = 0; i < oldM; i++) {
            TreeMap<K, V> map = hashTable[i];
            for (K key: map.keySet()) {
                //重新计算下标并赋值
                newHashTable[hash(key)].put(key, map.get(key));
            }
        }

        hashTable = newHashTable;
    }
}
```

## 参考链接

- [玩转数据结构](https://coding.imooc.com/class/chapter/207.html)