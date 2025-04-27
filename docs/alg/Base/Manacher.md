# Manacher算法 - 最长回文子串

- Manacher算法的预处理
    - Manacher算法对偶数字符串做了预处理，这个预处理可以巧妙的让所有（包括奇和偶）字符串都变为奇数回文串。
    - 例如：abba --> #a#b#b#a#
    - 从预处理后的字符串得到的最长回文字符串的长度除以 2 就是原字符串的最长回文子串长度，也就是我们想要得到的结果。
- Manacher算法核心概念
    - 回文半径 p ：经过处理后的字符串的长度一定是奇数，回文半径就是以回文中心字符的回文子串长度的一半。
    - 回文半径数组 P[] ,该数组记录以每个字符为中心，对应的最长回文半径。
    - 最右回文边界 R ：遍历字符串时，每个字符的最长回文子串都会有个右边界，而 R 则是所有已知右边界中最右的位置。R值保持单增。
    - 回文中心 C ： R 对应的回文中心
- 算法流程
    - 将原字符串转换为 Manacher String，定义为 S
    - 初始化 R 、C、P[] ：R = -1 ； C = -1，P[] = new int[]; **实际是最右边界位置的右一位**
    - 遍历数组 arr[i]
      - 当 i <= R 时，可以参考 i 相对于 C 的映射 i' = 2*C - i, 其中 i' - L ==  R - i
        - P[i'] < i' - L 时，P[i] = P[i']; 原因：以 C 为中心的最大回文子串的边界是 L 和 R，如果 P[i] != P[i'] 则违反这个前提
        - P[i'] > i' - L 时，P[i] = i' - L; 原因：以 C 为中心的最大回文子串的边界是 L 和 R，如果 P[i] != i' - L 则违反这个前提
        - P[i'] == i' - L 时，P[i] >=  i' - L; 原因：P[i] ==  i' - L 后，可能还可以往外扩
      - 当 i > R 时，P[i'] 没有参考价值，直接计算

    ![img.png](./../../resources/image/algoruthm/manacher-1.png)

- 参考
    - [Manacher](https://liuchang.men/2020/12/29/%E6%9C%80%E9%95%BF%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B2%E7%AE%97%E6%B3%95%E2%80%94%E2%80%94Manacher%E7%AE%97%E6%B3%95/)
    - [leetcode](https://leetcode.wang/leetCode-5-Longest-Palindromic-Substring.html)

