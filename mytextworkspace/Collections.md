# 	Collections工具类

static <K,V> [SortedMap](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/util/SortedMap.html)<K,V>

static <T extends [Comparable](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/lang/Comparable.html)<? super T>>  

:sort(List<T> list) 

// static <T> boolean addAll(Collection<? super T> c, T... elements) 

给元素中添加任意多个的元素，其中的elements是长度可变的参数，jdk1.5新特性

//打乱集合中的元素
	Static void	Collections.shuffle(list);



```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

public class Mytest {
	public static void main(String[] args) {
		ArrayList<String>list=new ArrayList<>();
		list.add("v");
		list.add("a");
		list.add("b");
		list.add("c");
		list.add("o");
		list.add("p");
		//对集合进行默认排序
		Collections.sort(list);
		
		for (String string : list) {
			System.out.println(string);
		}
		
		//打乱集合中的元素
		Collections.shuffle(list);
		System.out.println("_____________________");
		
		for (String string : list) {
			System.out.println(string);
			
		}
		
		System.out.println("_____________________");
		
		Collections.addAll(list, "j","k","l");
		for (String string : list) {
			System.out.println(string);
		}
		//使用指定规则进行排序
		
		ArrayList<Person>list2=new ArrayList<>();
		list2.add(new Person(19, "a杨幂"));
		list2.add(new Person(20, "李小璐"));
		list2.add(new Person(19, "b迪丽热巴"));
		list2.add(new Person(12, "古力娜扎"));
		
		Collections.sort(list2, new Comparator<Person>() {

			@Override
			public int compare(Person o1, Person o2) {
				// TODO Auto-generated method stub
				int result=o1.age-o2.age;
				if (result==0) {
					//按照年龄升序,修改默认排序规则
					result=o1.getName().charAt(0)-o2.getName().charAt(0);
				}
				return result ;
				
				
			}
		});
		
		for (Person person : list2) {
			System.out.println(person.age+"姓名："+person.name);
		}
		
		
	}

}

```





根据其元素的[natural ordering](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/lang/Comparable.html)对指定的列表进行排序。



Map常用方法

static interface   Map.Entry<K,V>（键值对）。

Entry储存了Map的键值对：相当于一个结婚证

 get(key);通过key获取value

remove(key);

keyset():得到key的set集合。

使用keyset遍历所有的集合元素

```java
   Set<Integer> set=	map.keySet();
   Iterator<Integer>iterator= set.iterator();
   while(iterator.hasNext()){
	   
	  Integer i=iterator.next();
	String value=  map.get(i);
	   System.out.println(value);
   }
	map.entrySet();
	
	System.out.println("````````~");
	for (Integer integer : set) {
		System.out.println(map.get(integer));
	}
```



put ()添加一个



```java
	//map中常用的方法：
	Map<Integer, String>map=new HashMap<>();
	
	map.put(1, "zhangs");
	map.put(2, "lisi");
	//map中不能存int类型只能用Integer
	map.put(3, "wangwu");
	
//	map.remove(3);
//	
//	String u=map.get(3);
//	System.out.println(u);
	
	//
	Set<Entry<Integer, String>>set= map.entrySet();
	
	for (Entry<Integer, String> entry : set) {
		
		//取出entry中的内容
	Integer key=	entry.getKey();
	String value=	entry.getValue();
	System.out.println("key:	"+key+"value: 	"+value);
	}
```



# 迭代器

所有的集合collection有**[iterator](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/util/Collection.html#iterator--)**()方法返回一个迭代器对象，这说明所有的集合对象都可以通过迭代器进行遍历。

使用方法：

集合+.iterator();

```java
public static void main(String[] args) {
		
		ArrayList<String>arrs=new ArrayList<>();
		arrs.add("我");
		arrs.add("爱");
		arrs.add("java");
		arrs.add("666");
		Iterator<String>iterator=arrs.iterator();
		
		while(iterator.hasNext()){
			String value=iterator.next();
			System.out.println(value);
		}
	}

```



原理:Arraylist类内部实现了iterator接口，hasnext判断是否有下一个，

next()使cursor+1，下标往后移,当下标移到最后时hasnext为false

```java
 private class Itr implements Iterator<E> {
        int cursor;  //下标     // index of next element to return
        int lastRet = -1; // index of last element returned; -1 if no such
        int expectedModCount = modCount;

        public boolean hasNext() {
            return cursor != size;//判断下标是否等于元素个数
        }

        @SuppressWarnings("unchecked")
        public E next() {
            checkForComodification();
            int i = cursor;
            if (i >= size)
                throw new NoSuchElementException();
            Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length)
                throw new ConcurrentModificationException();
            cursor = i + 1;
            return (E) elementData[lastRet = i];
        }
 }
```



# Collection



常用方法

**[add](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/util/Collection.html#add-E-)**([E](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/util/Collection.html) e)

**[contains](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/util/Collection.html#contains-java.lang.Object-)**([Object](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/lang/Object.html) o)： 如果此集合包含指定的元素，则返回 `true` 

**[ toArray](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/util/Collection.html#toArray--)**()： 返回一个包含此集合中所有元素的数组。

**[stream](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/util/Collection.html#stream--)**()：返回以此集合作为源的顺序 `Stream` 。	

**[size](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/util/Collection.html#size--)**()：返回此集合中的元素数。

**[isEmpty](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/util/Collection.html#isEmpty--)**()：如果此集合不包含元素，则返回 `true` 。

**[clear](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/util/Collection.html#clear--)**()：从此集合中删除所有元素（可选操作）。

**[remove](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/util/Collection.html#remove-java.lang.Object-)**([Object](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/lang/Object.html) o)：从该集合中删除指定元素的单个实例（如果存在）（可选操作）。



子接口：

 [List](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/util/List.html) <E>

- Arraylist：
- Vector：Arraylist的祖宗，由于是单线程的，速度慢
- LinkedList





   Set<E>

- [HashSet](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/util/HashSet.html) :数据结构是jdk1.8之前是数组加链表，jdk1.8*后*是数组加链表或数组加红黑树
- [TreeSet](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/util/TreeSet.html)
- [LinkedHashSet](chmox-internal://a2b92dc3137a16a8b1cff4568f55815f6c7144/java/util/LinkedHashSet.html)
- 



