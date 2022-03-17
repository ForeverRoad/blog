---
title: 回溯算法
date: 2022-02-17 16:06:09
tags: 回溯算法
categories: 数据结构
---


### 回溯算法


#### 基本了解

> 利用递归解决组合排序等穷举类的问题，相当于从集合中寻找符合条件的子集，相当于遍历树，集合的大小就相当于树的宽度，横向 for循环遍历孩子数，再通过递归调用自己，来进行深度遍历，递归的深度表示树的深度




#### 回溯法解决的问题

- 组合问题：N个数里面按一定规则找出k个数的集合
- 切割问题：一个字符串按一定规则有几种切割方式
- 子集问题：一个N个数的集合里有多少符合条件的子集
- 排列问题：N个数按一定规则全排列，有几种排列方式
- 棋盘问题：N皇后，解数独等等

> 组合是不考虑顺序，{1,2}，{2,1}是一种情况，对于排序就是两种



#### 回溯模板


``` Java
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果 // 让回退到上一层的时候继续下一个孩子数的处理
    }
}

```




#### 组合例题

给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。

示例:
输入: n = 4, k = 2
输出:
[
[2,4],
[3,4],
[2,3],
[1,2],
[1,3],
[1,4],
]


##### 思路

两层for循环，第一层选一个数，第二层遍历孩子数


``` Java
	
	for (int i = 1;i<=n ;i++ ) {
		for (int j=i+1;j<=n ;j++ ) {
			// 记录
		}
	}

```

所以k就是递归深度，所有当k很大的时候就没办法手动写for了，就需要运用到递归

``` Java

	static List<List<Integer>> result = new ArrayList();
    static List<Integer> path = new ArrayList();

    public static List<List<Integer>> combine(int n, int k) {
        backtracking(n, k, 1);
        return result;
    }

    public static void backtracking(int n, int k, int startIndex) {
        // 当子集长度达标后退出递归
        if (path.size() == k) {
            //存放结果;
            result.add(new ArrayList<>(path));
            return;
        }

        //选择：本层集合中元素（树中节点孩子的数量就是集合的大小）
        for (int i = startIndex; i <= n; i++) {
        // 剪枝优化，当当前层已选择元素个数 已经达到k个之后就不必再递归了
        // for (int i = startIndex; i <= n - (k - path.size()) + 1; i++) {
            //处理节点;
            path.add(i);
            //路径，选择列表
            // 递归
            backtracking(n, k, startIndex + 1);
            // 回溯，撤销处理结果 // 让回退到上一层的时候继续下一个孩子数的处理
            path.remove(path.size() - 1);
        }
    }

```



