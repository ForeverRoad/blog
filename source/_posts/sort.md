
---
title: 排序
date: 2022-02-21 18:08:09
tags: 排序
categories: 算法
---

#### 插入排序

> 通过N-1趟排序，保证0-i之间元素都为已排序

``` Java
	/**
	 * 通过比较，把大于[i]的往后移，直到当前最小位插入
	 * @param list [description]
	 */
	public static void insertionSort(List<Integer> list){
		int j;
        for (int i = 1;i<list.size(); i++) {
            Integer tmp = list.get(i);
            for (j=i; j>0&&list.get(j-1)>tmp;j-- ) {
                list.set(j, list.get(j - 1));
            }
            list.set(j, tmp);
        }

	}
```

#### 希尔排序

> 插入排序的一种，也叫缩减增量排序，通过分组进行插入排序，直到增量为1，全部都排序完成

``` Java

	public static void shellSort(List<Integer> list){
		int j;
        for (int gap = list.size()/2;gap >0; gap/=2) {
            for (int i=gap; i<list.size(); i++) {
                Integer tmp = list.get(i);
                for (j=i; j>=gap && list.get(j-gap)>tmp;j-=gap ) {
                    list.set(j, list.get(j - gap));
                }
                list.set(j, tmp);
            }

        }
	}

```


#### 归并排序

> 利用分治的四项，通过对不同的小分组进行排序，然后再合并成大分组，递归进行。


```Java 

	public static void mergeSort(Integer[] list){
        Integer[] tmpArr = new Integer[list.length];
        mergeSort(list,tmpArr,0,list.length-1);
    }

    public static void mergeSort(Integer[] list ,Integer[]  tmpArr,int left,int right){
        if (left < right) {
            int center = (left +right)/2;
            mergeSort(list,tmpArr,left,center);
            mergeSort(list,tmpArr,center+1,right);
            merge(list,tmpArr,left,center+1,right);
        }
    }

    /**
     * 把排序过的分组合并到一个分组中
     * @param list     原始分组
     * @param tmpArr   临时分组
     * @param leftPos  左边边界
     * @param rightPos 中间节点
     * @param rightEnd 右边界
     */
    public static void merge(Integer[] list,Integer[] tmpArr,int leftPos,int rightPos,int rightEnd){
        // 左边的结界
        int leftEnd = rightPos -1;
        // 临时数组当前索引
        int tmpPos = leftPos;
        int numElement = rightEnd - leftPos +1;
        // 把左右两边数组有序的合并到tmp
        while(leftPos <= leftEnd && rightPos <= rightEnd){
            if (list[leftPos]<=list[rightPos]) {
                tmpArr[tmpPos++] = list[leftPos++];
            }else{
                tmpArr[tmpPos++] = list[rightPos++];
            }
        }
        // 把一边有剩余的部分全部填充到tmp中
        while(leftPos <= leftEnd){
            tmpArr[tmpPos++] = list[leftPos++];
        }
        // 把一边有剩余的部分全部填充到tmp中
        while(rightPos <= rightEnd){
            tmpArr[tmpPos++] = list[rightPos++];
        }
        // 复制临时数组到list中
        for (int i=0; i<numElement;i++,rightEnd-- ) {
            list[rightEnd] = tmpArr[rightEnd];
        }
    }



```

#### 快排


> 利用分治的思想，通过基准值的判断，让左边的数组小于基准，右边都大于基准，递归下去，最终得到排序号的数组



``` Java
public static void quickSort(List<Integer> arr) {
        if (arr.size() < 2) {
            return;
        } else {
            int temp = arr.get(0);
            List<Integer> left = new ArrayList<>();
            List<Integer> right = new ArrayList<>();
            for (int i = 0; i < arr.size(); i++) {
                if (arr.get(i) < temp) {
                    left.add(arr.get(i));
                }
                if (arr.get(i) > temp) {
                    right.add(arr.get(i));
                }
            }
            List<Integer> list = quickSort(left);
            list.add(temp);
            list.addAll(quickSort(right));
            
        }
    }
```


