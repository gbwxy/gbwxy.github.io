## 回溯算法

### 算法思想

回溯算法实际上一个类似枚举的搜索尝试过程，主要是在搜索尝试过程中寻找问题的解，当发现已不满足求解条

件时，就“回溯”返回，尝试别的路径。其本质就是穷举。

经典题目：8 皇后

### 一般解题步骤

- 针对所给问题，定义问题的解空间，它至少包含问题的一个（最优）解。
- 确定易于搜索的解空间结构,使得能用回溯法方便地搜索整个解空间 。
- 以深度优先的方式搜索解空间，并且在搜索过程中用剪枝函数避免无效搜索。

### leetcode

77.组合：<https://leetcode.cn/problems/combinations/>

39.组合总和：<https://leetcode.cn/problems/combination-sum/>

40.组合总和 II：<https://leetcode.cn/problems/combination-sum-ii/>

78.子集：<https://leetcode.cn/problems/subsets/>

90.子集 II：<https://leetcode.cn/problems/subsets-ii/>

51.N 皇后：<https://leetcode.cn/problems/n-queens/>