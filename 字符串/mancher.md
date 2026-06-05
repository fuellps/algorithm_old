### 模板

$$
\begin{aligned}
&为了能够统一处理奇偶回文串,将s变为扩展串,将s每个字符用\#分割,如s="abc",则s'="\#a\#b\#c". \\
\\
&定义p[i]表示以i为中心的最长回文半径长度。\\
&定义r为能够延申到最远不可达的位置,c为使得r达到最远不可位置的下标\\
&①:当i > r时，没有特殊性质，暴力扩展r即可.\\
&②:当i < r时，此时2c-i位置即为关键点,若i+p[2c-i] < r,此时p[i] = p[2c-i]\\
&③:当i < r时，此时2c-i位置即为关键点,若i+p[2c-i] > r,此时p[i] = r-i\\
&④:当i < r时，此时2c-i位置即为关键点,若i+p[2c-i] = r,此时p[i]长度至少为r-i,暴力扩展即可\\
&每次r都是单调不减，r更新时要更新对应的c位置。\\
\end{aligned}
$$

**相关性质:**

- **p[i]-1 ** 即为当前位置所能扩展出的最长回文半径长度
- 扩展串中  **i/2** 位置对应原串结尾不可达位置
- 对于原串(**下标从0开始**)s[i:j]是回文串当且仅当$p[i+j+1]  -1 \ge   j-i+1$



```c++
//获取字符串s的扩展串对应每个位置的最长回文半径数组
vector<int> get_p(const string& s) {
    string t = "#";
    for (auto c : s) {
        t += c;
        t += '#';
    }
    int n = t.size();
    vector<int> p(n);
    for (int i = 0, r = 0, c = 0; i < n; i++) {
        int len = i < r ? min(p[2 * c - i], r - i) : 1;
        while (i + len < n && i - len >= 0 && t[i + len] == t[i - len])  len++;
        if (i + len > r) {
            r = i + len;
            c = i;
        }
        p[i] = len;
   }
    return p;
}
```



### 习题练习

#### 基础/模板

- [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)  `推荐，练习下标对应`
- [647. 回文子串](https://leetcode.cn/problems/palindromic-substrings/) `推荐。计算回文子数组的个数`





#### 综合

- [3327. 判断 DFS 字符串是否是回文串](https://leetcode.cn/problems/check-if-dfs-strings-are-palindromes/)  `非常推荐。练习对应下标和DFS序`
- 