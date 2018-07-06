---
layout:     post
title:      "Java ArrayList源码阅读"
subtitle:   " \"一步一步来啦\""
date:       2018-07-06 08:42:00
author:     "TenzT"
header-img: "img/post-bg-2015.jpg"
tags:
    - JavaSE
    - List
---

# 数据结构
- ArrayList底层是用`数组`存储元素的，元素默认为空，类型为Object
```
    private static final Object[] EMPTY_ELEMENTDATA = {};

    //默认只制定一个引用， 当第一个元素add进来时才默认用DEFAULT_CAPACITY（官方默认为10）
    private transient Object[] elementData;     //特地查了transient这个关键字，说的是该关键字修饰的变量不参与序列化，

    ....
    //构造方法
    public ArrayList(int initialCapacity) {
        super();
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        this.elementData = new Object[initialCapacity]; //当重新指定Capacity时，新new一个对象数组。
    }

    public ArrayList() {
        super();
        this.elementData = EMPTY_ELEMENTDATA;
    }
    //当输入集合类来创建Arraylist式，调用toArray方法
    public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();
        size = elementData.length;
        // c.toArray可能不返回Object类型，因此再次确认
        if (elementData.getClass() != Object[].class)
            elementData = Arrays.copyOf(elementData, size, Object[].class);
    }

```
- Capacity和size的区别：
    - Capacity：数组的大小
    - size：已存储的元素的大小
- 默认的Capacity为10

# 方法
- public void trimToSize()：比较element的size和capacity的大小，将多余的“空位”去掉，实现方法是Arrays.copyOf
- public int indexOf(Object o)：`遍历所有元素`，用o.equals()找到相同的元素，注意要先判断o是否为null
- public int lastIndexOf(Object o)：与indexOf相似，只不过是反相遍历，因此找到最后一个相同的元素的坐标
- public E get(int index)：返回指定index上元素，注意先要完成rangeCheck（判断index是够大于size）
- public boolean add(E e)：新加元素放到最后，然而不一定之增加一位容量，而是有一定的规则增加：
```
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
- public boolean contains(Object o)：查找是否存在o，底层调用indexOf，看返回index是否>=0
- protected void removeRange(int fromIndex, int toIndex)：移除范围里面的元素，底层实现是把toIndex以后的元素直接copy到fromIndex那里，把多出的空位遍历置为null
- private boolean batchRemove(Collection<?> c, boolean complement)：遍历所有元素查看是否是c里面的元素。complement表示模式如果输入为true，则保留，如果为false，则不保留，retainAll里面调用时complement为true，removeAll时为false
```
    private boolean batchRemove(Collection<?> c, boolean complement) {
        final Object[] elementData = this.elementData;
        int r = 0, w = 0;
        boolean modified = false;v
        try {
            for (; r < size; r++)
                if (c.contains(elementData[r]) == complement)
                    elementData[w++] = elementData[r];
        } finally {
            // 一般情况下是不会运行这一块的********
            // 但当c.contains()抛出异常时，r!=size，此时将未处理的数据保留，w指针移动到最后保证w
            if (r != size) {
                System.arraycopy(elementData, r,
                                 elementData, w,
                                 size - r);
                w += size - r;
            }
            // *******************************

            if (w != size) {
                // clear to let GC do its work
                for (int i = w; i < size; i++)
                    elementData[i] = null;
                modCount += size - w;
                size = w;
                modified = true;
            }
        }
        return modified;
    }

    //去除List和c的交集
    public boolean removeAll(Collection<?> c) {
        return batchRemove(c, false);
    }
    //保留List和c的交集
    public boolean retainAll(Collection<?> c) {
        return batchRemove(c, true);
    }
```

# 备注
- modCount貌似是和线程安全相关的东西，每次对数组的长度或容量的改动都需要modCount++
- 挺多操作都会先ensureCapacityInternal一下，如果确保capacity还够，就不需要新new一个数组对象。
- ArrayList除了查找不得不使用遍历，其他地方如remove等，需要有数组元素移位的操作，都尽可能用arraycopy来完成，应该是会更高效吧。。。
- 留一个小疑问，为什么ArrayList这么执着于将数组转成Object类型呢？