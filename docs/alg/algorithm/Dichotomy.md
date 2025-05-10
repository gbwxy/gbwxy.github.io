# 二分&分治

## 二分法、二分查找

### 局部最小值问题 Code001

**问题描述**

定义何为局部最小值：

- arr[0] < arr[1]，0位置是局部最小；
- arr[N-1] < arr[N-2]，N-1位置是局部最小；
- arr[i-1] > arr[i] < arr[i+1]，i位置是局部最小；

给定一个数组arr，已知任何两个相邻的数都不相等，找到随便一个局部最小位置返回


## 分治算法

### 算法思想

将一个规模为 N 的问题分解为 K 个规模较小的子问题，这些子问题相互独立且与原问题性质相同。求出子问题的解，就可得到原问题的解。

经典题目：二分查找、汉诺塔问题

### 一般解题步骤

- 将原问题分解为若干个规模较小，相互独立，与原问题形式相同的子问题；
- 若子问题规模较小而容易被解决则直接解，否则递归地解各个子问题
- 将各个子问题的解合并为原问题的解。

### LeetCode

108.将有序数组转换成二叉搜索数：<https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/>

148.排序列表：<https://leetcode.cn/problems/sort-list/>

23.合并 k 个升序链表：<https://leetcode.cn/problems/merge-k-sorted-lists/>
