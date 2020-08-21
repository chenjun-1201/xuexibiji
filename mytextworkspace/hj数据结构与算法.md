# hj数据结构与算法



## 哈希表



### Hashset

数据结构:哈希表

- jdk1.8之前的数据结构是数组+链表
- jdk1.8之前的数据结构是数组+红黑树或数组+链表
- 





Hashset存储自定义的元素:

原理Hashset的add方法：根据对象的hashcode值比较，如果hashcode值不相等，则添加这个元素，如果hashcode值相等，在调用对象的equals方法,如果是true，则不会添加这个元素，如果是false则添加这个元素。



String 就重写了object的hashcode方法，相同的字符串，hashcode相同，

实现存储自定义的元素：只需要重写对象的hashcode 方法和equals方法即可。

````java

public class Person {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age &&
                name.equals(person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}

````



测试类

```java
package test;

import bean.Person;

import java.util.HashSet;
import java.util.Iterator;

public class Test2 {
    public static void main(String[] args) {
        Person person=new Person("zhangsan",2);
        Person person2=new Person("zhangsan",2);

        Person person3=new Person("zhangsan",18);

        HashSet<Object> hashSet = new HashSet<>();

        hashSet.add(person);

        hashSet.add(person2);

        hashSet.add(person3);

        Iterator<Object> iterator = hashSet.iterator();

        while (iterator.hasNext()){

            Object next = iterator.next();
            System.out.println(next);

        };

    }

}

```

### LinkedHashset

结构为：哈希表+链表

保证了有序性，LinkedHashset对象的元素是有序的



**数据结构**：包含存储结构和逻辑结构，

存储结构：顺序存储结构，链式存储结构



# 一、 概述

1. ## 数据结构

l 什么是数据结构？

​		数据结构是指相互之间存在着一种或多种关系的数据元素的集合和该集合中数据元素之间的关系组成。

## l 数据的存储结构

​		顺序存储结构

顺序存储结构：是把数据元素存放在地址连续的存储单元里，其数据间的逻辑关系和物理关系是一致的。数组就是顺序存储结构的典型代表。

​		链式存储结构

链式存储结构：是把数据元素存放在内存中的任意存储单元里，也就是可以把数据存放在内存的各个位置。这些数据在内存中的地址可以是连续的，也可以是不连续的。

和顺序存储结构不同的是，链式存储结构的数据元素之间是通过指针来连接的，我们可以通使用指针来找到某个数据元素的位置，然后对这个数据元素进行一些操作。

顺序存储结构和链式存储结构的区别



## l 数据的逻辑结构

指反映数据元素之间的逻辑关系的数据结构，其中的逻辑关系是指数据元素之间的前后关系，而与他们在计算机中的存储位置无关。逻辑结构分为以下四类：

1.集合结构

集合结构中的数据元素同属于一个集合，他们之间是并列的关系，除此之外没有其他关系。如下图，可以很好的表示集合结构中的元素之间的关系：

2.线性结构

线性结构中的元素存在一对一的相互关系。如下图，可以很好的表示线性结构中的元素之间的关系：

3.树形结构

树形结构中的元素存在一对多的相互关系。如下图，可以很好的表示树形结构中的元素之间的关系：

4.图形结构

图形结构中的元素存在多对多的相互关系。如下图，可以很好的表示图形结构中的元素之间的关系：

## 顺序存储结构

### 栈

- stack底层是数组

- 取出栈顶元素，先进先出

  

  

```java
//取出栈顶元素
	public int pop() {
		//栈中没有元素
		if(elements.length==0) {
			throw new RuntimeException("stack is empty");
		}
		//取出数组的最后一个元素
		int element = elements[elements.length-1];
		//创建一个新的数组
		int[] newArr = new int[elements.length-1];
		//原数组中除了最后一个元素的其它元素都放入新的数组中
		for(int i=0;i<elements.length-1;i++) {
			newArr[i]=elements[i];
		}
		//替换数组
		elements=newArr;
		//返回栈顶元素
		return element;
	}
```



### 队列

- queue底层是数组
- queue取出栈顶元素是先进先出







## 链式存储结构

### 链表

- 单链表
- 循环链表
- 循环双链表
- 双链表：分为普通双链表和循环双链表



## 递归

- 斐波那契数列

- 汉诺塔问题

  

(1,1,2,3,5,8,13.....)

使用递归方式实现

````java
	
	public static int feibonaq(int i){
		
		if(i==1||i==2){
			return 1;
		}else{
			
			return feibonaq(i-1)+feibonaq(i-2);
		}
		
		
	}
	
	public static void main(String[] args) {
	
		System.out.println(feibonaq(10));;
		
	}
}
````



### 汉诺塔问题

```java
 public static void hano(int n,char from ,char in,char to){
		 
		 if(n==1){
			 System.out.println("第"+n+"个圈从"+from+"移到"+to);
			
			 
		 }else{
			//移动上面所有的盘子到中间位置
			 hano(n-1,from,to,in);
			//移动下面的盘子
			 System.out.println("第"+n+"个圈从"+from+"移到"+to);
			//把上面的所有盘子从中间位置移到目标位置
			 hano(n-1,in,from,to);
		
			
			 
	
		 }
```





## 树

- 二叉树：对于任意一个节点，其子节点的数量不超过二，子节点分为左右两个节点
- 满二叉树，满足节点的总数为2^n-1的二叉树
- 完全二叉树：所有叶子节点的子节点都在倒数第二层和倒数第一层，且满足，倒数一层的节点左边连续，倒数第二层右边连续。从根节点往下数直接数，如果不连续则不是完全二叉树,如下图：





![未命名文件](/Users/mac/Desktop/mytextworkspace/未命名文件.png)

- 二叉树的存储结构：顺序存储结构，链式存储结构



链式存储结构的二叉树：遍历方法：（前序，中序，后序）



![未命名文件 (1)](/Users/mac/Desktop/mytextworkspace/未命名文件 (1).png)

```java
package demo5;

public class TestBinaryTree {

	public static void main(String[] args) {
		//创建一颗树
		BinaryTree binTree = new BinaryTree();
		//创建一个根节点
		Node root = new Node(1);
		//把根节点赋给树
		binTree.setRoot(root);
		//创建一个左节点
		Node rootL = new Node(2);
		//把新创建的节点设置为根节点的子节点
		root.setLeftNode(rootL);
		//创建一个右节点
		Node rootR = new Node(3);
		//把新创建的节点设置为根节点的子节点
		root.setRightNode(rootR);
		//为第二层的左节点创建两个子节点
		rootL.setLeftNode(new Node(4));
		rootL.setRightNode(new Node(5));
		//为第二层的右节点创建两个子节点
		rootR.setLeftNode(new Node(6));
		rootR.setRightNode(new Node(7));
		//前序遍历树
		binTree.frontShow();
		System.out.println("===============");
		//中序遍历
		binTree.midShow();
		System.out.println("===============");
		//后序遍历
		binTree.afterShow();
		System.out.println("===============");
		//前序查找
		Node result = binTree.frontSearch(5);
		System.out.println(result);
		
		System.out.println("===============");
		//删除一个子树
		binTree.delete(4);
		binTree.frontShow();
		
	}

}

```

```java
package demo5;

public class Node {
	//节点的权
	int value;
	//左儿子
	Node leftNode;
	//右儿子
	Node rightNode;
	
	public Node(int value) {
		this.value=value;
	}
	
	//设置左儿子
	public void setLeftNode(Node leftNode) {
		this.leftNode = leftNode;
	}
	//设置右儿子
	public void setRightNode(Node rightNode) {
		this.rightNode = rightNode;
	}
	
	//前序遍历
	public void frontShow() {
		//先遍历当前节点的内容
		System.out.println(value);
		//左节点
		if(leftNode!=null) {
			leftNode.frontShow();
		}
		//右节点
		if(rightNode!=null) {
			rightNode.frontShow();
		}
	}

	//中序遍历
	public void midShow() {
		//左子节点
		if(leftNode!=null) {
			leftNode.midShow();
		}
		//当前节点
		System.out.println(value);
		//右子节点
		if(rightNode!=null) {
			rightNode.midShow();
		}
	}

	//后序遍历
	public void afterShow() {
		//左子节点
		if(leftNode!=null) {
			leftNode.afterShow();
		}
		//右子节点
		if(rightNode!=null) {
			rightNode.afterShow();
		}
		//当前节点
		System.out.println(value);
	}

	//前序查找（其他两个查找方式同理，沈略）
	public Node frontSearch(int i) {
		Node target=null;
		//对比当前节点的值
		if(this.value==i) {
			return this;
		//当前节点的值不是要查找的节点
		}else {
			//查找左儿子
			if(leftNode!=null) {
				//有可能可以查到，也可以查不到，查不到的话，target还是一个null
				target = leftNode.frontSearch(i);
			}
			//如果不为空，说明在左儿子中已经找到
			if(target!=null) {
				return target;
			}
			//查找右儿子
			if(rightNode!=null) {
				target=rightNode.frontSearch(i);
			}
		}
		return target;
	}
	
	//删除一个子树
	public void delete(int i) {
		Node parent = this;
		//判断左儿子
		if(parent.leftNode!=null&&parent.leftNode.value==i) {
			parent.leftNode=null;
			return;
		}
		//判断右儿子
		if(parent.rightNode!=null&&parent.rightNode.value==i) {
			parent.rightNode=null;
			return;
		}
		
		//递归检查并删除左儿子
		parent=leftNode;
		if(parent!=null) {
			parent.delete(i);
		}
		
		//递归检查并删除右儿子
		parent=rightNode;
		if(parent!=null) {
			parent.delete(i);
		}
	}

}

```

```java
package demo5;

public class BinaryTree {

	Node root;
	
	//设置根节点
	public void setRoot(Node root) {
		this.root = root;
	}
	
	//获取根节点
	public Node getRoot() {
		return root;
	}

	public void frontShow() {
		if(root!=null) {
			root.frontShow();
		}
	}

	public void midShow() {
		if(root!=null) {
			root.midShow();
		}
	}

	public void afterShow() {
		if(root!=null) {
			root.afterShow();
		}
	}

	public Node frontSearch(int i) {
		return root.frontSearch(i);
	}

	public void delete(int i) {
		if(root.value==i) {
			root=null;
		}else {
			root.delete(i);
		}
	}
	
}

```

**Tips**





- 二插排序树
- 线索二叉树
- 赫夫曼树
- 二叉查找树
- AVL树
- 多路查找树
- 平衡二叉树
- 二叉树

Node类是基本的节点类，BinaryTree是二叉排序树类,Testbinarytree是测试类

包括了**添加节点**，**遍历**，**删除节点**，**查询节点**。



![未命名文件 (2)](/Users/mac/Desktop/mytextworkspace/未命名文件 (2).png)



## 排序算法



- 







arraylist：

```java
public class DoubleNode {
//上一个节点
	DoubleNode pre=this;
	//下一个节点
	DoubleNode next=this;
	//节点数据
	int data;
	
	public DoubleNode(int data) {
		this.data=data;
	}
	
	//增节点
	public void after(DoubleNode node) {
		//原来的下一个节点
		DoubleNode nextNext = next;
		//把新节点做为当前节点的下一个节点
		this.next=node;
		//把当前节点做新节点的前一个节点
		node.pre=this;
		//让原来的下一个节点作新节点的下一个节点
		node.next=nextNext;
		//让原来的下一个节点的上一个节点为新节点
		nextNext.pre=node;
	}
	
	//下一个节点
	public DoubleNode next() {
		return this.next;
	}
	
	//上一个节点
	public DoubleNode pre() {
		return this.pre;
	}
	
	//获取数据
	public int getData() {
		return this.data;
	}
	
}
```



### Arraylist

Arraylist底层实现：底层是一个数组,因为数组的长度是不可变的，长度一旦定义多少，他就是多少，

Arraylist提供了add方法，set方法实现,都是对数组进行操作。可以看出增加和删除会影响后面的元素，都往下标往后面加一。但是查询快，直接通过下标查询

```java


public class Myarray {
//用于存储数据的数组
	int [] element;
	
	//获取长度的方法
	public int size(){
		return this.element.length;
	}
	
	//初始化数组
	public Myarray(){
		element= new  int[0];
	}
	
	
	//网数组末尾添加一个元素
	public void add(int i){
	int[]el2=new int [this.size()+1];
	
	for(int i1=0;i1<this.size();i1++){
		el2[i1]=element[i1];
		
	}
	el2[element.length]=i;
	element=el2;
	}
	
	//获取元素方法
	public int get(int index){
		
		if(index>element.length|| index<0){
			System.out.println("out of index");
			
		}
		return element[index];
	}
	
	
	// 修改指定位置的元素
	public void set(int index,int value){
		for(int i1=0;i1<this.size();i1++){
			if(i1==index){
				element[i1]=value;
			}
			
		}
	
	}
	
	
	// 插入一个元素到指定位置
	public void insert(int index,int s){
		int[]el2=new int [this.size()+1];
		
		for(int i = 0;i<index;i++){
			el2[i]=element[i];
			
		}
		el2[index]=s;
		for(int i=index+1;i<this.size()+1;i++){
			el2[i]=element[i-1];
			
		}
		element=el2;
		
		
	}
	
}
```





### LinkedList

- 底层是一个双链表
- 有序





