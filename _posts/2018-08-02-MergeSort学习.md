---
layout:     post
title:      "MergeSort学习笔记"
subtitle:   "归并排序"
date:       2018-08-02 21:45:00
author:     "TenzT"
header-img: "img/post-bg-2015.jpg"
tags:
    - 数据结构与算法
---
# 归并的核心思想？
二分的方式将数列一层层分裂成多对子序列，然后再倒着一层层合并
# 时间复杂度
- O(nlogn)

# 时间复杂度
- O(n)：记得需要一个数组来做辅助。

# Java代码
```
import java.util.Arrays;

public class MergeSort {
	public static void sort(int[] arr) {
		//关键是记得创建辅助空间
		int[] tmp = new int[arr.length];
		sort(arr,tmp,0,arr.length-1);
	}
	public static void sort(int[] arr, int[] tmp, int low, int high) {
		if(low==high) {
			tmp[low] = arr[low];
		} else {
			int mid = (low + high)/2;
			sort(arr, tmp, low, mid);		//左边的子序列
			sort(arr, tmp, mid+1, high);	//右边的子序列
			merge(arr, tmp, low, high, mid);//合并左右子序列
		}
	}
	
	public static void merge(int[] arr, int[] tmp, 
			int low, int high, int mid) {
		int pHigh = mid + 1;
		int pLow = low;
		int pTmp = low;
		for(;pTmp<=mid && pHigh<=high;pTmp++) {
			if(arr[pLow]<arr[pHigh]) {
				tmp[pTmp] = arr[pLow++];
			} else {
				tmp[pTmp] = arr[pHigh++];
			}
		}
		while(pLow<=mid) {
			tmp[pTmp++] = arr[pLow++];
		}
		
		while(pHigh<=high) {
			tmp[pTmp++] = arr[pHigh++];
		}
		for(int i=low;i<=high;i++) {
			arr[i] = tmp[i];
		}
	}
	
	public static void main(String[] args) {
		int[] arr = {9,8,7,6,5,4,3,2,1};
		sort(arr);
		System.out.println(Arrays.toString(arr));
	}
}
```

# 优化方案：
使用非递归的方式：由于同一层的操作可以变成顺序化执行（递归底层运行是顺序的，但其实各个子序列对排序时可看成先并行再联系），且处理的子序列的长度为2、4、8....，因此可以用迭代来表示,从最小的序列开始归并：
```
public static void sort2(int[] arr) {
	int[] tmp = new int[arr.length];
	int k = 1;
	while(k < arr.length) {
		MergePass(arr, tmp, k, arr.length);
		k *= 2;

		MergePass(tmp, arr, k, arr.length);
		k *= 2;
	}
}

public static void MergePass(int[] arr, int[] tmp, int lenList, int length) {
	int i=0;
	int j;
	while(i<= length-2*lenList) {
		merge(arr, tmp, i, i+lenList-1, i+2*lenList-1);
		i += 2 * lenList;
	}
	if(i < length-lenList) {
		merge(arr, tmp, i, i+lenList-1, length-1);
	} else {
		for(j = i;j < length;j++) {
			tmp[j] = arr[j];
		}
	}
}
```