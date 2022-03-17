
---
title: 排序
date: 2022-02-21 18:08:09
tags: 排序
categories: 算法
---




#### 快排


> 利用分治的思想，通过基准值的判断，让左边的数组小于基准，右边都大于基准，递归下去，最终得到排序号的数组



```
public static List<Integer> quickSort(List<Integer> arr) {
        if (arr.size() < 2) {
            return arr;
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
            return list;
        }
    }---
title: 二叉树
date: 2020-07-08 18:08:09
tags: 二叉树
categories: 数据结构
---
```