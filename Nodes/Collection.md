Collection: List, Set
List
- 特点：有序；可重复；可通过索引值操作元素
- 分类：
	- 底层时数组查询快，增删慢
		- ArrayList:线程不安全，效率高
		- Vector：线程安全，效率低
	- 底层是链表，查询慢，增删快
		- LinkedList：线程不安全，效率高

Set
- 特点：无序（存储和取出的顺序）；元素唯一
- 分类：
	- 底层是哈希表：HashSet》保证元素唯一性》hashCode();equals()
	- 底层是二叉树：TreeSet》保证元素排序
		- 自然顺序，让对象所属的类去实现comparable接口，无参构造
		- 比较器接口comparator，带参构造








