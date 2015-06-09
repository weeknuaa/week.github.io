# 容器 #
- 程序对对象的需求是不可预测的，而且需要妥善的管理这些对象的引用
- 容器可以存放若干引用，而且提供了完善的方法来保存和操作这些引用（从而就能操作对象）

## 数组 ##
- 数组(array)是最常见的数据结构。数组是相同类型元素的有序集合，并有固定的大小(可容纳固定数目的元素)。数组可以根据下标(index)来随机存取(random access)元素。在内存中，数组通常是一段连续的存储单元。
- Java支持数组这一数据结构。我们需要说明每个数组的类型和大小
- 数组是容器吗？它和List、Map这些有啥区别？

## JAVA内部的容器类 ##
- Java容器类包含List、ArrayList、Vector及map、HashTable、HashMap、Hashset
- 最为常用的四个容器：
	- LinkedList ：其数据结构采用的是链表，此种结构的优势是删除和添加的效率很高，但随机访问元素时效率较ArrayList类低。
	- ArrayList：其数据结构采用的是线性表，此种结构的优势是访问和查询十分方便，但添加和删除的时候效率很低。
	- HashSet: Set类不允许其中存在重复的元素（集），无法添加一个重复的元素（Set中已经存在）。HashSet利用Hash函数进行了查询效率上的优化，其contain（）方法经常被使用，以用于判断相关元素是否已经被添加过。
	- HashMap: 提供了key-value的键值对数据存储机制，可以十分方便的通过键值查找相应的元素，而且通过Hash散列机制，查找十分的方便。
- ArrayList和HashMap是异步的，Vector和Hashtable是同步的，所以Vector和Hashtable是线程安全的，而ArrayList和HashMap并不是线程安全的。因为同步需要花费机器时间，所以Vector和Hashtable的执行效率要低于 ArrayList和HashMap。

## 几种容器的特性 ##
- List：
	- ArrayList和数组随即访问性能高
	- LinkedList添加和删除元素性能高
- Set：
	- HashSet各项性能都很高
	- TreeSet有顺序要求时使用
	- LinkedHashSet有特殊操作时使用，如模拟栈
- Map:
	- HashMap、TreeMap、LinkedHashMap的情况和Set类似	


## 迭代器 ##
- 解决问题：不同容器取出元素的方法不同
- Iterator的最大威力：能够将遍历序列的操作与序列底层的结构分离。迭代器统一了对容器的访问方式。
- 迭代器(Iterator)统一了所有容器遍历元素的方式
	- iterator() 要求容器返回一个Iterator
	- hasNext() 检查容器中是否还有元素
	- next() 获得容器中的下一个元素
	- remove() 将迭代器新近返回的元素删除
- sample code

    	Iterator it=hs.keySet().iterator();   
    	while(it.hasNext()){  
	    	String str=(String)it.next();   
	    	if (str.contains('condition')){   
	    		it.remove();   
    		}
    	}   

- ListIterator
	- ListIterator是Iterator的子类接口，只能用于各种List类的访问。Iterator只能单向向前移动，但是ListIterator可以双向移动。
	- 逆向访问
		- 逆向访问用到的两个方法：hasPrevious()、previous()。
		- 注：逆向访问时，初始化ListIterator实例时，要提供一个list.size()参数，这样才能总尾部开始迭代。
- sample code

        List<String> list=new ArrayList<String>();  
        list.add("first");  
        list.add("second");  
        list.add("third");  
        ListIterator it=list.listIterator (list.size());  
        while(it.hasPrevious()){  
        	System.out.println(it.previous());
		}

## 最后看看各个类和接口的关系： ##
![](http://images.cnitblog.com/blog/413416/201304/15203418-c01e434f91fb482b902463bcacbf3f69.png)
