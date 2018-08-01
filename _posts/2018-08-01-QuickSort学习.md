---
layout:     post
title:      "QuickSort学习笔记"
subtitle:   "快速排序"
date:       2018-08-01 15:15:00
author:     "TenzT"
header-img: "img/post-bg-2015.jpg"
tags:
    - 数据结构与算法
---
# 快排的核心思想？
`从列表中选取一个值，称作pivot值，指定左右两个指针分别向中间遍历，最终达到的效果是比pivot小的都在列表左半部分，大的在右半部分`,然后继续对左子表和右字表重复相同的操作
# 时间复杂度
- 最好:O(nlogn)
- 最坏:O(n^2)

# Java代码
```
public class QuickSort {
	public static void QSort(int[] list, int low, int high) {
		int pivot;  
		if(low < high) {
			pivot = Partition(list, low, high); //划分出左右字表
			QSort(list, low,pivot-1);
			QSort(list, pivot+1, high);
		}
	}
	public static int Partition(int[] list, int low, int high) {
		int pivotKey = list[low];
		while(low<high) {
			while(low<high && list[high]>=pivotKey) {   //从high指针开始扫
				high--;                                 //描直到找到比pivot小的值
			}
			swap(list, low, high);  //比pivot大的值放右边
			while(low<high && list[low]<=pivotKey) {    //从low指针开始扫
				low++;                                  //描直到找到比pivot大的值
			}
			swap(list, low, high);  //比pivot小的值放左边
		}
		return low;
	}
	public static void swap(int[] list, int low, int high) {
		int tmp = list[low];
		list[low] = list[high];
		list[high] = tmp;
	}
	
	public static void main(String[] args) {
		int[] list = {50, 10, 90, 30, 70, 40, 80, 60, 20};
		for(int i :list) {
			System.out.print(i + " ");
		}
		System.out.println();

		long startTime=System.currentTimeMillis();
		QSort1(list, 0, list.length-1);
		System.out.println("After:");
		for(int i :list) {
			System.out.print(i + " ");
		}
	}
}
```

# 优化：
1. 三数取中：当遇到正序或倒序的列表时，由于pivot在最左/右端，导致每次都只处理少了一个元素的字表，此时时间复杂度为O(n^2)。因此将pivot设定成列表前中后三个数的中间值
```
int m = (high + low)/2;
if(list[low]>list[high]) {
    swap(list, low, high);
}
if(list[m]>list[high]) {
    swap(list, m, high);
}
if(list[m]>list[low]) {
    swap(list, low, m);
}
pivot = list[low];
```
2. 优化小数组时的排序方案：如果数组中只有几个记录需要排序时，使用直接插入排序
```
if((high - low)>MAX_LENGTH_INSERT_SORT) {
        pivot = Partition(list, low, high); //划分出左右字表
        QSort(list, low,pivot-1);
        QSort(list, pivot+1, high);
} else {
    Insert(list);
}
```
3. 尾递归优化：减少递归
```
while(low < high) {
    pivot = Partition(list, low, high); //划分出左右字表
    QSort(list, low,pivot-1);
    pivot = low + 1;
}
```
