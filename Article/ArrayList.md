#### ArrayList

#### 关于扩容

- 如果没有指定初始容量，则设置为10

``

```java
/**
 * Default initial capacity.
 */
private static final int DEFAULT_CAPACITY = 10;
```



- ArrayList的扩容比较简单，容量扩为之前的1.5倍

``

```java
/**
 * Increases the capacity to ensure that it can hold at least the
 * number of elements specified by the minimum capacity argument.
 *
 * @param minCapacity the desired minimum capacity
 */
private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    // minCapacity is usually close to size, so this is a win:
    elementData = Arrays.copyOf(elementData, newCapacity);
}
```



#### 关于拷贝

源码中用到的数组复制的两个方法分别是：System.arraycopy()和Arrays.copyOf()

一句话总结二者区别：Arrays.copyOf()可以看做是受限制的System.arraycopy()

Arrays.copyOf()是系统自己创建一个数组，再调用System.arraycopy()复制

``

```java
public static <T,U> T[] copyOf(U[] original, int newLength, Class<? extends T[]> newType) {
    @SuppressWarnings("unchecked")
    T[] copy = ((Object)newType == (Object)Object[].class)
        ? (T[]) new Object[newLength]
        : (T[]) Array.newInstance(newType.getComponentType(), newLength);
    
    
    System.arraycopy(original, 0, copy, 0,
                     Math.min(original.length, newLength));
    return copy;
}
```

System.arraycopy()需要传入一个目标数组作为参数，同时可以指定拷贝的起点和拷贝的长度

``

```java
public static native void arraycopy(Object src,  int  srcPos,
                                    Object dest, int destPos,
                                    int length);
```

> 各个参数的含义：
>
> src - 源数组。
> srcPos - 源数组中的起始位置。
> dest - 目标数组。
> destPos - 目标数据中的起始位置。
> length - 要复制的数组元素的数量。



同时要注意的是，上述两个拷贝方法都是浅拷贝，关于深拷贝和浅拷贝，后续会做详细说明。



####关于Fail-Fast

Fail-Fast是非线程安全的集合，实现的一种错误机制。但不能百分百得到保证，只是尽最大努力抛出ConcurrentModificationException。

- 什么时候产生Fail-Fast
- ArrayList中如何实现Fail-Fast
  1. 两个变量：modCount和expectedModCount
  2. 只要涉及到数组个数改变的方法，都会导致modCount的改变（add、remove、clear）
  3. 当发现expectedModCount和modCount不一致，就会抛出ConcurrentModificationException
  4. 所以，Iterator在遍历时，是不允许被迭代的对象被改变的

``

```java
final void checkForComodification() {
    if (modCount != expectedModCount)
        throw new ConcurrentModificationException();
}
```

​	

- 如何避免Fail-Fast：用CopyOnWriteArrayList代替ArrayList





#### 关于ArrayList中删除元素

- 错误的删除方式一：for循环遍历删除

``

```java
public void testRemove() {
   List<Integer> integers = new ArrayList<>(5);
   integers.add(1);
   integers.add(2);
   integers.add(2);
   integers.add(4);
   integers.add(5);

   for (int i = 0; i < integers.size(); i++) {
      if (integers.get(i) % 2 == 0) {
         integers.remove(i);
      }
   }

   System.out.println(integers);
}
```

​		这段代码的输出是 [1,2,5]

​		因为在remove方法执行的时候，删除第一个“2”，会更新后面的索引值，数组变为[1,2,4,5]，这样会导致第二个“2”不会被删除



- 错误的删除方式二：使用Iterator遍历，但仍用ArrayList的remove方法

``

```java
public void testRemove(){
   List<String> strings = new ArrayList<>();
   strings.add("a");
   strings.add("b");
   strings.add("c");
   strings.add("d");

   Iterator<String> iterator = strings.iterator();
   while (iterator.hasNext()){
      String next = iterator.next();
      strings.remove(next);
   }

   System.out.println(strings);
}
```

​		会抛出ConcurrentModificationException，参见上述的Fail-Fast机制



- 正确的删除方法：使用Iterator的remove方法

``

```java
public static void main(String[] args) {
   List<Integer> intList = new ArrayList<Integer>();
   Collections.addAll(intList, 1, 2, 3, 5, 6);
   Iterator<Integer> it = intList.iterator();
   while(it.hasNext()) {
      Integer value = it.next();
      if(value == 3 || value == 5) {
         it.remove();
      }
   }
   System.out.println(intList);
}
```



