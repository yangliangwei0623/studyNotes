# 算法学习 #









**“道阻且长，行则将至”**



































> **学习方法**：先背过模板，通过模板题完成默写
> 一个题目通过后可以删掉再写，重复3-5次来提高熟练度.

### 应试技巧

- 一般ACM或者笔试题的时间限制是1秒或2秒。
  在这种情况下，C++代码中的操作次数控制在 10^7∼10^8 为最佳。
  下面给出在不同数据范围下，代码的时间复杂度和算法该如何选择：

[由数据范围反推算法复杂度以及算法内容](https://www.acwing.com/file_system/file/content/whole/index/content/3074/)

- 做题时，我门一般可以先想出一个暴力的算法，再考虑有无数据结构可以优化算法的的时间复杂度
- 若我们的代码会用到f[i-1]这种我们就可以让i从1开始，给f[0]设定一个值为初值，就可以减少代码量

**关于输入输出的一些说明**

**1**.getline(cin,s)可以完整的读入一行并且读入换行符，但是换行符不会被放入s中，也就是在换行符已经被读掉了
而正常的cin 会读到遇到空格或者换行符，即换行符未被读掉
简而言之，**cin(scanf)之后使用getline需要在cin之后读掉换行符**

```c++
str="\n";
getline(cin,str);//将回车符作为输入流cin以清除缓存
```

**2**.下面关于sstream（主要用于类型转换）的用法做单简单说明

- 題目：输入的第一行有一个数字 N 代表接下來有 N 行资料，每一行资料里有不固定个数的整数(最多20个，每行最大200个字元)，编程將每行的总和打印出來。

- 输入：

  3
  1 2 3
  20 17 23 54 77 60
  111 222 333 444 555 666 777 888 999

  输出：

  6
  251
  4995

```c++
#include <iostream>
#include <string>
#include <sstream>
 
using namespace std;
 
int main()
{
    string s;
    stringstream ss;
    int n;
    cin >> n;
    getline(cin, s);  //读取换行
 
    for (int i = 0; i < n; i++)
 
    {
        getline(cin, s);
        ss.clear();//多次转换时，需要先对ss清空
        ss.str(s);
        int p;
        int sum = 0;
       while(ss>>p)sum += p;
        cout << sum << endl;
    }
    return 0;
}
```

3.sscanf函数(从已有字符串读入)的使用

[航班时间](https://www.acwing.com/problem/content/1233/)
对于该题的数据读入，我们可以用getline将一整行的数据读入s中，由于这一行数据的格式可以通过变化使得他们一样，然后我们再从s中读入相应的时分秒

```c++
	string line;
    getline(cin, line);
    //保持一致:
    if (line.back() != ')') line += "(+0)";
    //把字符串中的数字信息读入到变量之中
    int h1, m1, s1, h2, m2, s2, d;
    sscanf(line.c_str(), "%d:%d:%d %d:%d:%d (+%d)", &h1, &m1, &s1, &h2, &m2, &s2, &d);
```







## 1.基础算法

### 排序

- 快排

  ```c++
  void quick_sort(int q[], int l, int r)
  {
      if (l >= r) return;//最小规模子问题
      //分
      int i = l - 1, j = r + 1, x = q[l + r >> 1];//x = q[(l + r)/2]
      while (i < j)
      {
          do i ++ ; while (q[i] < x);
          do j -- ; while (q[j] > x);
          if (i < j) swap(q[i], q[j]);
      }//退出循环时，i>j,q[i]>x的第一个数,q[j]<x的最后一个数;
      //治
      quick_sort(q, l, j), quick_sort(q, j + 1, r);
      
      //qSort 不用合
  }
  ```

[快速排序]([785. 快速排序 - AcWing题库](https://www.acwing.com/problem/content/787/))
[第k个数]([786. 第k个数 - AcWing题库](https://www.acwing.com/problem/content/788/))

- 归并排序

  ```c++
  void merge_sort(int q[], int l, int r)
  {
      if (l >= r) return;
  	//分
      int mid = l + r >> 1;
      //治
      merge_sort(q, l, mid);
      merge_sort(q, mid + 1, r);
  	//合
      int k = 0, i = l, j = mid + 1;
      while (i <= mid && j <= r)
          if (q[i] <= q[j]) tmp[k ++ ] = q[i ++ ];
          else tmp[k ++ ] = q[j ++ ];
  
      while (i <= mid) tmp[k ++ ] = q[i ++ ];
      while (j <= r) tmp[k ++ ] = q[j ++ ];
  
      for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];
  }
  ```

[归并排序]([787. 归并排序 - AcWing题库](https://www.acwing.com/problem/content/789/))

[逆序对的数量]([788. 逆序对的数量 - AcWing题库](https://www.acwing.com/problem/content/790/))

### 二分

- 整数二分

> 使用的情况：一段区间，左边一部分满足一个性质，右边一部分满足另一个性质，找出左边的端点和右边的端点

```c++
bool check(int x) {/* ... */} // 检查x是否满足某种性质,我们找到的是满足这个性质的端点
//如何确定check函数呢? -- 我们要找的是哪个段的端点，我们就让那个段的点check全为真，另一段check全为假

// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
int bsearch_1(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;    // check()判断mid是否满足性质
        else l = mid + 1;
    }
    return l;
}
// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_2(int l, int r)
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;//+1是为了处理边界情况
        //考虑r=l-1的情况，若不+1，这时mid = l，更新后l = l，进入死循环
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}


```

[数的范围]([789. 数的范围 - AcWing题库](https://www.acwing.com/problem/content/791/))

- 浮点二分

```c++
double bsearch_3(double l, double r)
{
    const double eps = 1e-6;   // eps 表示精度，取决于题目对精度的要求
    while (r - l > eps)
    {
        double mid = (l + r) / 2;//不用位运算
        if (check(mid)) r = mid;
        else l = mid;
    }
    return l;
}
```

[数的三次方根]([790. 数的三次方根 - AcWing题库](https://www.acwing.com/problem/content/submission/792/))

### 高精度算数

- 高精度加法

```c++
// C = A + B, A >= 0, B >= 0
vector<int> add(vector<int> &A, vector<int> &B)
{
    if (A.size() < B.size()) return add(B, A);//保证A的位数一定大于B

    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size(); i ++ )
    {
        t += A[i];
        if (i < B.size()) t += B[i];
        C.push_back(t % 10);
        t /= 10;
    }

    if (t) C.push_back(t);
    return C;
}
```

[高精度加法]([791. 高精度加法 - AcWing题库](https://www.acwing.com/problem/content/793/))

- 高精度减法

```c++
// C = A - B, 满足A >= B, A >= 0, B >= 0
//因为要满足A>=B,在调用前要先判断下大小
vector<int> sub(vector<int> &A, vector<int> &B)
{
    vector<int> C;
    for (int i = 0, t = 0; i < A.size(); i ++ )
    {
        t = A[i] - t;
        if (i < B.size()) t -= B[i];
        C.push_back((t + 10) % 10);
        if (t < 0) t = 1;
        else t = 0;
    }

    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}

```

[高精度减法]([792. 高精度减法 - AcWing题库](https://www.acwing.com/problem/content/794/))

- 高精度乘法

```c++
vector<int> mul(vector<int> &A, int b)
{
    vector<int> C;

    int t = 0;
    for (int i = 0; i < A.size() || t; i ++ )
    {
        if (i < A.size()) t += A[i] * b;
        C.push_back(t % 10);
        t /= 10;
    }

    while (C.size() > 1 && C.back() == 0) C.pop_back();//乘一个0，就会出现前导0，所以需要除去

    return C;
}
```



- 高精度除法

```c++
// A / b = C ... r, A >= 0, b > 0
vector<int> div(vector<int> &A, int b, int &r)
{
    vector<int> C;
    r = 0;
    for (int i = A.size() - 1; i >= 0; i -- )
    {
        r = r * 10 + A[i];
        C.push_back(r / b);
        r %= b;
    }
    //C[0]是的高位，C[1]是低位，需要翻转。
    reverse(C.begin(), C.end());
    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}
```

### 前缀和

> 用于解决对区间和频繁查询的问题<br>画图解决相关推导问题会更清晰
>
> 一般从1开始，方便写代码

一维前缀和：

S[i] = a[1] + a[2] + ... a[i]
a[l] + ... + a[r] = S[r] - S[l - 1]

[前缀和]([795. 前缀和 - AcWing题库](https://www.acwing.com/problem/content/797/))

二维前缀和

S[i, j] = 第i行j列格子左上部分所有元素的和
以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵的和为：
S[x2, y2] - S[x1 - 1, y2] - S[x2, y1 - 1] + S[x1 - 1, y1 - 1]（即子矩阵的和）

[子矩阵的和]([796. 子矩阵的和 - AcWing题库](https://www.acwing.com/problem/content/798/))

### 差分

> 给区间[l, r]中的每个数加上c：B[l] += c, B[r + 1] -= c
>
> 用于多次给某个区间的数进行加减一个数后求操作后的数组

![](D:\study\md笔记图片\屏幕截图(288).png)

![](D:\study\md笔记图片\屏幕截图(289).png)

> 如果数组a是数组b的前缀和我们就称数组b是数组a的差分数组

- 一维差分 

[差分]([797. 差分 - AcWing题库](https://www.acwing.com/problem/content/799/))

- 二维差分

  > 给以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵中的所有元素加上c：
  > S[x1, y1] += c, S[x2 + 1, y1] -= c, S[x1, y2 + 1] -= c, S[x2 + 1, y2 + 1] += c

  ![](D:\study\md笔记图片\屏幕截图 2024-04-26 102001.png)

  [差分矩阵](https://www.acwing.com/problem/content/800/)

### 双指针

```c++
for (int i = 0, j = 0; i < n; i ++ )
{
    while (j < i && check(i, j)) j ++ ;//i,j满足某种性质

    // 具体问题的逻辑
}
常见问题分类：
    (1) 对于一个序列，用两个指针维护一段区间
    (2) 对于两个序列，维护某种次序，比如归并排序中合并两个有序序列的操作
    
//核心思想
对于一般要用两层for循环的遍历(整个区间选一个区间，再对选的这个区间的这种遍历)，可将时间复杂度从O(n^2)降到O(n);
for(int i = 0; i < n; i++){
    for(int j = 0; j < i; j++)
        ...
}

//做题时：
我们可以先考虑暴力的做法，然后观察其中i，j的关系（往往会出现j单增的情况）来优化成双指针算法
```

 [最长连续不重复子序列](https://www.acwing.com/activity/content/problem/content/833/)

[数组元素的目标和](https://www.acwing.com/problem/content/802/)

[判断子序列](https://www.acwing.com/problem/content/2818/)

### 位运算

```c++
求n的第k位数字: n >> k & 1
返回n的最后一位1：lowbit(n) = n & -n//返回x的最后一位1
    eg: 1010 -> 10
        1001010 -> 10
```

### 离散化

```c++

vector<int> alls; // 存储所有待离散化的值
sort(alls.begin(), alls.end()); // 将所有值排序
//unique函数，把vector所有重复过的元素放到最后（保留一个放在前面），返回重复的这些元素的起始迭代器
alls.erase(unique(alls.begin(), alls.end()), alls.end());   // 去掉重复元素

// 二分求出x对应的离散化的值
int find(int x) // 找到第一个大于等于x的位置，这里相当于一个二分查找，就是找到a[?] == x，所以使用bSearch也自然是ok的。
{
    int l = 0, r = alls.size() - 1;
    while (l < r)
    {
        int mid = l + r >> 1;
        if (alls[mid] >= x) r = mid;
        else l = mid + 1;
    }
    return r + 1; // 映射到1, 2, ...n
    //如果return r; 映射到0，1，2, ...n;
}
```

不要求保序的离散化可以用map简单实现



[区间和](https://www.acwing.com/activity/content/problem/content/836/)

### 区间合并

```c++
// 将所有存在交集的区间合并
void merge(vector<PII> &segs)
{
    vector<PII> res;

    sort(segs.begin(), segs.end());

    int st = -2e9, ed = -2e9;
    for (auto seg : segs)
        if (ed < seg.first)//维护的区间与考察的区间没有交集
        {
            if (st != -2e9) res.push_back({st, ed});//找到一个区间
            st = seg.first, ed = seg.second;//更新维护的区间
        }
        else ed = max(ed, seg.second);//维护的区间与考察的区间有交集，合并
    
    
    if (st != -2e9) res.push_back({st, ed});//这条if语句在for循环之外

    segs = res;
}
```

[区间合并](https://www.acwing.com/problem/content/805/)

## 2.数据结构





### 并查集

```c++
(1)朴素并查集：

    int p[N]; //存储每个点的祖宗节点

    // 返回x的祖宗节点
    int find(int x)
    {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ ) p[i] = i;

    // 合并a和b所在的两个集合：
    p[find(a)] = find(b);


(2)维护size的并查集：

    int p[N], size[N];
    //p[]存储每个点的祖宗节点, size[]只有祖宗节点的有意义，表示祖宗节点所在集合中的点的数量

    // 返回x的祖宗节点
    int find(int x)
    {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ )
    {
        p[i] = i;
        size[i] = 1;
    }

    // 合并a和b所在的两个集合：
    size[find(b)] += size[find(a)];
    p[find(a)] = find(b);


(3)维护到祖宗节点距离的并查集：

    int p[N], d[N];
    //p[]存储每个点的祖宗节点, d[x]存储x到p[x]的距离

    // 返回x的祖宗节点
    int find(int x)
    {
        if (p[x] != x)
        {
            int u = find(p[x]);
            d[x] += d[p[x]];
            p[x] = u;
        }
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ )
    {
        p[i] = i;
        d[i] = 0;
    }

    // 合并a和b所在的两个集合：
    p[find(a)] = find(b);
    d[find(a)] = distance; // 根据具体问题，初始化find(a)的偏移量
```



### 链表

> 用数组模拟，动态链表太慢，因为new node（）太慢

- 单链表

```c++
// head存储链表头，e[]存储节点的值，ne[]存储节点的next指针，idx表示当前用到了哪个节点
//head的next就是head的值
//也可以说这是一个带有哨位结点的链表
int head, e[N], ne[N], idx;

// 初始化
void init()
{
    head = -1;
    idx = 0;
}

// 在链表头插入一个数a，即把a插入head的后面
void insert(int a)
{
    e[idx] = a, ne[idx] = head, head = idx ++ ;
}
//在“下标”为k的结点后插入一个x
void insert(int k,int x){
    e[idx] = x,ne[idx] = ne[k],ne[k] = idx++;
}
//将k结点后面的那个节点删掉
void remove(int k){
    ne[k] = ne[ne[k]];
}
// 将头结点删除，需要保证头结点存在
void remove()
{
    head = ne[head];
}
```

[单链表](https://www.acwing.com/problem/content/828/)

- 双链表

```c++
// e[]表示节点的值，l[]表示节点的左指针，r[]表示节点的右指针，idx表示当前用到了哪个节点
int e[N], l[N], r[N], idx;

// 初始化
void init()
{
    //0是左端点，1是右端点
    r[0] = 1, l[1] = 0;
    idx = 2;
}

// 在节点a的右边插入一个数x
void insert(int a, int x)
{
    e[idx] = x;
    l[idx] = a, r[idx] = r[a];
    l[r[a]] = idx, r[a] = idx ++ ;
}
//在结点a的左边插入一个数就相当于在了l[a]的右边插入一个数
我们直接调用 insert(l[a],x)就好

// 删除节点a
void remove(int a)
{
    l[r[a]] = l[a];
    r[l[a]] = r[a];
}

```

[双链表](https://www.acwing.com/problem/content/829/)

### 栈与队列

> 略去

### 单调栈与单调队列

> 即保证栈和队列里的元素是单调的（不单调就扔出去而非对其排序）
> 虽然技巧性很强，但几乎只有几种特定的题型

- 单调栈

  > 考虑一个序列，求出这个序列中每个数字左边离他最近且比他小的数字，当然还有其他与之对称的情况（左or右，大or小）

```c++
常见模型：找出每个数左边离它最近的比它大/小的数
int tt = 0;//初始，栈为空
for (int i = 1; i <= n; i ++ )
{
    while (tt && check(stk[tt], i)) tt -- ;//栈不为空且栈顶元素>=当前元素，弹出
   //这时stk[tt]即为a[i]左边离他最近满足~check的数
    stk[ ++ tt] = i;//a[i]入栈
}
//所有元素最多进栈一次，出栈一次，故最多有2n次操作没时间复杂度为O（n);
```

[单调栈](https://www.acwing.com/problem/content/832/)

- 单调队列

  > 求滑动窗口的最大或最小值

  ```c++
  常见模型：找出滑动窗口中的最大值/最小值
      //单调队列一般用双端队列保持单调性
  int hh = 0, tt = -1;//因为要时刻判断此时窗口的大小，队列中存储的是数组下标
  for (int i = 0; i < n; i ++ )
  {
      while (hh <= tt && check_out(q[hh])) hh ++ ;  // 判断队头是否滑出窗口,若是则滑出队头,考虑的是这个元素入了队之后的情况
      while (hh <= tt && check(q[tt], i)) tt -- ;//
      q[ ++ tt] = i;
  }
  ```

[滑动窗口](https://www.acwing.com/problem/content/156/)

### KMP算法

我们先看暴力匹配算法，s[N]是原串，p[M]是模式串，这里我们考虑下标从1开始
```c++
for (int i = 1; i <= n; i ++ )//从第s的第i位开始匹配，看看能不能匹配成功
{
    bool flag = true;
    for (int j = 1; j <= m; j ++ )
    {
        if (s[i + j - 1] != p[j])//不匹配了
        {
            flag=false;
            break;//从第i+1位开始看匹不匹配
        }
    }
```

下面我们来看看怎么优化

```c++
for (int i = 1; i <= n; i ++ )//我们必须依次寻找可能成功的匹配位置，此步不可优化（反正没人提出这步的优化我也想不出来。。。）
{
    bool flag = true;
    for (int j = 1; j <= m; j ++ )//匹配失败后，下次匹配还是要从模式串的第一位开始，可不可以不从第一位？
 //为了不漏掉可能的答案，我们思考如何j最小移动到哪儿才能让匹配可能成功，值得一提的是，这里的最小是限定在我们已获得的信息的条件下的 
    
    {
        if (s[i + j - 1] != p[j])
        {
            flag=false;
            break;
        }
    }
```


我们这样移动j

![b8a4aabcb469802ae89b89b6a2f6858](D:\wechat data\WeChat Files\wxid_83vo2i5sidcp22\FileStorage\Temp\b8a4aabcb469802ae89b89b6a2f6858.jpg)

下一步就是求next数组了，具体逻辑见下面的kmp算法

给出kmp算法
```c++
// s[]是长文本，p[]是模式串，n是s的长度，m是p的长度
//next[i]:以p[i]结尾的这个串的最长相等前后缀长度
求模式串的Next数组：//next这个名字在某个头文件中被定义过，所以我们用ne表示next数组
for (int i = 2, j = 0; i <= m; i ++ )//next[1] = 0，因为我们下一次匹配的位置实际是next[j]+1。
    //next[0]我们不会用到，也没有什么实际意义，我们在代码中也保证了不会访问next[0]
{	//相当于从2开始填充next数组
    while (j && p[i] != p[j + 1]) j = ne[j];
    if (p[i] == p[j + 1]) j ++ ;
    ne[i] = j;
}

// 匹配
for (int i = 1, j = 0; i <= n; i ++ )
{
    while (j && s[i] != p[j + 1]) j = ne[j];//0<=j<=m,每次最少减一，最多减m次
    if (s[i] == p[j + 1]) j ++ ;//每次循环最多加一
    if (j == m)
    {
         // 匹配成功后的逻辑
        j = ne[j];//假装失败，找下一个位置
    }
}
//时间复杂度是O(n+m),我们可以优化到O(n)
```

[我们给出一个严格完成kmp算法相关论述的证明](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/solutions/732236/shi-xian-strstr-by-leetcode-solution-ds6y)

[KMP字符串](https://www.acwing.com/problem/content/833/)

### stl简介

```c++
vector, 变长数组，倍增的思想
    size()  返回元素个数//时间复杂度O(1)
    empty()  返回是否为空
    clear()  清空
    front()/back()
    push_back()/pop_back()
    begin()/end()//end()是最后一个数后面那个数
    []//随机访问
    支持比较运算，按字典序

pair<int, int>
    first, 第一个元素
    second, 第二个元素
    支持比较运算，以first为第一关键字，以second为第二关键字（字典序）
 p = make_pari{ , };
p = { , };
//两个pari嵌套可以存储三个属性
string，字符串
    append(const string& str)在string的末尾增加一个字符串
    append(const char c)在string的末尾增加一个字符
    重载"+"
    size()/length()  返回字符串长度
    empty()
    clear()
    substr(起始下标，(子串长度))  返回子串//起始下标从0开始，若超出了字符串长度，就输出从起始位置开始的整个字符串
    c_str()  返回字符串所在字符数组的起始地址//printf("%s/n",s.c_str())
	int find(const string& str, int pos = 0) const;//查找str第一次出现位置,从pos开始查找
	int find(const char* s, int pos = 0) const;//查找s第一次出现位置,从pos开始查找
queue, 队列
    //没有clear函数 q = queue<T>();通过重新构造达到清空的目的
    size()
    empty()
    push()  向队尾插入一个元素
    front()  返回队头元素
    back()  返回队尾元素
    pop()  弹出队头元素

priority_queue, 优先队列，默认是大根堆
    size()
    empty()
    push()  插入一个元素
    top()  返回堆顶元素
    pop()  弹出堆顶元素
    定义成小根堆的方式：priority_queue<int, vector<int>, greater<int>> q;

stack, 栈
    size()
    empty()
    push()  向栈顶插入一个元素
    top()  返回栈顶元素
    pop()  弹出栈顶元素

deque, 双端队列
    //功能强大，效率低
    size()
    empty()
    clear()
    front()/back()
    push_back()/pop_back()
    push_front()/pop_front()
    begin()/end()
    []

set, map, multiset, multimap, 基于平衡二叉树（红黑树），动态维护有序序列
    size()
    empty()
    clear()
    begin()/end()
    ++, -- 返回前驱和后继，时间复杂度 O(logn)

    set/multiset
        insert()  插入一个数
        find()  查找一个数//不存在返回end迭代器
        count()  返回某一个数的个数
        erase()
            (1) 输入是一个数x，删除所有x   O(k + logn)
            (2) 输入一个迭代器，删除这个迭代器，就只删除这一个数
        lower_bound()/upper_bound()
            lower_bound(x)  返回大于等于x的最小的数的迭代器
            upper_bound(x)  返回大于x的最小的数的迭代器
    		//不存在均返回end
    map/multimap
        insert()  插入的数是一个pair
        erase()  输入的参数是pair或者迭代器
        find()
        []  注意multimap不支持此操作。 时间复杂度是 O(logn)//像数组一样用map
        lower_bound()/upper_bound()

unordered_set, unordered_map, unordered_multiset, unordered_multimap, 哈希表
    和上面类似，增删改查的时间复杂度是 O(1)
    不支持 lower_bound()/upper_bound()， 迭代器的++，--

bitset, 圧位
    bitset<10000> s;
    ~, &, |, ^
    >>, <<
    ==, !=
    []

    count()  返回有多少个1

    any()  判断是否至少有一个1
    none()  判断是否全为0

    set()  把所有位置成1
    set(k, v)  将第k位变成v
    reset()  把所有位变成0
    flip()  等价于~
    flip(k) 把第k位取反
```

### trie树

> 用来高效存储字符串集合并查找每个字符串及其出现次数的数据结构
> Trie这个名字取自“retrieval”，因为Trie可以只用一个前缀便可以在一部字典中找到想要的单词。

```c++
int son[N][26], cnt[N], idx;//idx代表这个节点的身份
// 0号点既是根节点，又是空节点
// son[][]存储树中每个节点的子节点
// cnt[]存储以每个节点结尾的单词数量

// 插入一个字符串
void insert(char *str)
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int u = str[i] - 'a';
        if (!son[p][u]) son[p][u] = ++ idx;
        p = son[p][u];
    }
    cnt[p] ++ ;
}

// 查询字符串出现的次数
int query(char *str)
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int u = str[i] - 'a';
        if (!son[p][u]) return 0;
        p = son[p][u];
    }
    return cnt[p];
}
```

[Trie字符串统计](https://www.acwing.com/problem/content/837/)

[最大异或对](https://www.acwing.com/problem/content/description/145/)

### 哈希表

> eg: 把0-1e9 映射到 0-1e5,与离散化不同的是这里真的有这么多数要用，而非要用的数的范围很大而用到的个数很少，
> 某种意义上，我们可以把离散化看作一种特殊的哈希，哈希表一般只考虑吧添加和查找这两个操作，若要实现删除，也是在该点上做个标记，如bool flag，来表示其是否被删除，hash表的随意删除很容易导致查找异常。

哈希取模时我们一般取一个质数，且这个数要离2的某次幂最远，这样产生哈希碰撞的概率最小

- 拉链法

```c++
int h[N], e[N], ne[N], idx;
	memset(h,-1,sizeof h);//-1视作空指针
    // 向哈希表中插入一个数
    void insert(int x)
    {
        int k = (x % N + N) % N;//加N为了使其余数变成正数，c++中负数mod上一个数余数是负数
        e[idx] = x;
        ne[idx] = h[k];
        h[k] = idx ++ ;
    }

    // 在哈希表中查询某个数是否存在
    bool find(int x)
    {
        int k = (x % N + N) % N;
        for (int i = h[k]; i != -1; i = ne[i])
            if (e[i] == x)
                return true;

        return false;
    }
```

- 开放寻址

  ```c++
  int h[N];//N为大于数据范围2-3倍的第一个质数，将h全初始化为0x3f3f3f3f，可用memset(h,0x3f,sizeof h),这个函数在cstring里
  int null = 0x3f3f3f3f;//null是一个我们数据范围内不会出现的数
      // 如果x在哈希表中，返回x的下标；如果x不在哈希表中，返回x应该插入的位置
      int find(int x)
      {
          int t = (x % N + N) % N;
          while (h[t] != null && h[t] != x)
          {
              t ++ ;
              if (t == N) t = 0;//t达到数组最右端，由于已经+1，这里是判断条件是t == N;
          }
          return t;
      }
  ```

[模拟散列表](https://www.acwing.com/problem/content/842/)

- 字符串哈希

  > 此处介绍字符串前缀哈希法

  我们先把字符串看作一个p进制的数，从而把字符串转化成了一个数字，再对这个数字对一个较小的数字q取模
  一般情况下，不能把某个字母映射为0，经验告诉我们当p = 131 or 13331,q = 2^64时，我们假定没有哈希冲突产生（概率低于万十分之一）。我们先计算这个字符串的每个前缀的哈希值，从而便能得到任意一个字串的哈希值。

  ```c++
  核心思想：将字符串看成P进制数，P的经验值是131或13331，取这两个值的冲突概率低//26个字符中ascii码最大z为122，若只涉及到字母，取 p = 131就可，对与数字而言他们实际他们的ASCII码就是几
  小技巧：取模的数用2^64，这样直接用unsigned long long存储，溢出的结果就是取模的结果
  
  typedef unsigned long long ULL;
  ULL h[N], p[N]; // h[k]存储字符串前k个字母的哈希值, p[k]存储 P^k mod 2^64
  
  // 初始化
  p[0] = 1;
  h[0] = 0;
  for (int i = 1; i <= n; i ++ )
  {
      h[i] = h[i - 1] * P + str[i];//预处理得到字符串前缀的哈希值
      p[i] = p[i - 1] * P;//预处理得到所有p^k的值
  }
  
  // 计算子串 str[l ~ r] 的哈希值
  ULL get(int l, int r)
  {
      return h[r] - h[l - 1] * p[r - l + 1];//公式，推导见图
  }
  ```

  [字符串哈希](https://www.acwing.com/problem/content/843/)


### 简单树状数组
```c++

int lowbit(int x)
{
    return x&-x;
}

//第x个数加上v
int add(int x,int v)
{
    //因为树状数组的性质，加一个数，只影响logn个数，所有不用全加完
    //从当前位置开始加，每个间隔是lowbit(i)，一直加到最后
    for(int i=x;i<=n;i+=lowbit(i))
        tr[i]+=v;
}

//返回x的前缀和
int qurry(int x)
{
    //因为树状数组的性质，求前缀和，只用加logn个数，所有不用全加完
    //从当前位置开始累加，每个间隔是lowbit(i)，一直加到i==0停止
    int cnt=0;
    for(int i=x;i!=0;i-=lowbit(i))
        cnt+=tr[i];
    return cnt;
}
//初始化，即建立a数组的树状数组tr数组
for(int i = 0; i < n; i++)add(i,a[i]);
```
### 简单线段树
```c++
#include<iostream>
#include<cstring>
#include<cstdio>
#include<algorithm>

using namespace std;

const int N=100010;

int n,m;

int w[N];//记录一下权重

struct node{
    int l,r;//左右区间

    int sum;//总和
}tr[N*4];//记得开 4 倍空间

void push_up(int u)//利用它的两个儿子来算一下它的当前节点信息
{
    tr[u].sum=tr[u<<1].sum+tr[u<<1|1].sum;//左儿子 u<<1 ,右儿子 u<<1|1  
}

void build(int u,int l,int r)/*第一个参数，根节点编号，第二个参数，左边界，第三个参数，右边界*/
{
    if(l==r)tr[u]={l,r,w[r]};//如果当前已经是叶节点了，那我们就直接赋值就可以了
    else//否则的话，说明当前区间长度至少是 2 对吧，那么我们需要把当前区间分为左右两个区间，那先要找边界点
    {
        tr[u]={l,r};//这里记得赋值一下左右边界的初值

        int mid=l+r>>1;//边界的话直接去计算一下 l + r 的下取整

        build(u<<1,l,mid);//先递归一下左儿子

        build(u<<1|1,mid+1,r);//然后递归一下右儿子

        push_up(u);//做完两个儿子之后的话呢 push_up 一遍u，更新一下当前节点信息
    }
}

int query(int u,int l,int r)//查询的过程是从根结点开始往下找对应的一个区间
{
    if(l<=tr[u].l&&tr[u].r<=r)return tr[u].sum;//如果当前区间已经完全被包含了，那么我们直接返回它的值就可以了
    //否则的话我们需要去递归来算
    int mid=tr[u].l+tr[u].r>>1;//计算一下我们 当前 区间的中点是多少
    //先判断一下和左边有没有交集

    int sum=0;//用 sum 来表示一下我们的总和

    if(mid>=l)sum+=query(u<<1,l,r);//看一下我们当前区间的中点和左边有没有交集
    if(r>=mid+1)//看一下我们当前区间的中点和右边有没有交集
    sum+=query(u<<1|1,l,r);

    return sum;

}

void modify(int u,int x,int v)//第一个参数也就是当前节点的编号,第二个参数是要修改的位置，第三个参数是要修改的值
{
    if(tr[u].l==tr[u].r)tr[u].sum+=v; //如果当前已经是叶节点了，那我们就直接让他的总和加上 v 就可以了

    //否则
    else
    {

      int mid=tr[u].l+tr[u].r>>1;
      //看一下 x 是在左半边还是在右半边
      if(x<=mid)modify(u<<1,x,v);//如果是在左半边，那就找左儿子
      else modify(u<<1|1,x,v);//如果在右半边，那就找右儿子

      //更新完之后当前节点的信息就要发生变化对吧，那么我们就需要 pushup 一遍

      push_up(u);
    }

}

int main()
{
    scanf("%d%d",&n,&m);

    for(int i=1;i<=n;i++)scanf("%d",&w[i]);

    build(1,1,n);/*第一个参数是根节点的下标，根节点是一号点，然后初始区间是 1 到 n */

    //后面的话就是一些修改操作了

    while(m--)
    {
        int k,a,b;

        scanf("%d%d%d",&k,&a,&b);

        if(!k)printf("%d\n",query(1,a,b));//求和的时候，也是传三个参数，第一个的话是根节点的编号 ，第二个的话是我们查询的区间 
        //第一个参数也就是当前节点的编号
        else
        modify(1,a,b);//第一个参数是根节点的下标,第二个参数是要修改的位置，第三个参数是要修改的值

    }


    return 0;
}
```

## 3.搜索与图论

### DFS与BFS

- DFS

> DFS回溯时切记恢复现场

```c++
int dfs(int u)
{
    st[u] = true; // st[u] 表示点u已经被遍历过

    for (int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!st[j]) dfs(j);
    }
}
```

[排列数字]([842. 排列数字 - AcWing题库](https://www.acwing.com/problem/content/description/844/))
[n-皇后问题](https://www.acwing.com/problem/content/845/)

- BFS

```c++
queue<int> q;
st[1] = true; // 表示1号点已经被遍历过
q.push(1);

while (q.size())
{
    int t = q.front();
    q.pop();
 
    for (int i = h[t]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!st[j])
        {
            st[j] = true; // 表示点j已经被遍历过
            q.push(j);
        }
    }
}
```

[走迷宫](https://www.acwing.com/problem/content/description/846/)
[八数码](https://www.acwing.com/problem/content/847/)

### 树与图的存储

> 1.邻接矩阵（略）
>
> 2.邻接表

```c++
// 对于每个点k，开一个单链表，存储k所有可以走到的点。h[k]存储这个单链表的头结点
int h[N], e[N], ne[N], idx;

// 添加一条边a->b
void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

// 初始化
idx = 0;
memset(h, -1, sizeof(h));
```

### 树与图的遍历

- BFS

```c++
queue<int> q;
st[1] = true; // 表示1号点已经被遍历过
q.push(1);
while (q.size())
{
    int t = q.front();
    q.pop();

    for (int i = h[t]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!st[j])
        {
            st[j] = true; // 表示点j已经被遍历过
            q.push(j);
        }
    }
}
```

[图中点的层次](https://www.acwing.com/problem/content/849/)

- DFS

```c++
int dfs(int u)
{
    st[u] = true; // st[u] 表示点u已经被遍历过

    for (int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!st[j]) dfs(j);
    }
}
```

[树的重心](https://www.acwing.com/activity/content/problem/content/909/)
DFS并不直接得到答案，而是在DFS的过程中不断更新答案

- ToPoOrder

```c++
#include<iostream>
#include<algorithm>
#include<queue>
#include<cstring>
using namespace std;
const int N  = 1e5+10;
int e[N],ne[N],h[N],idx;
int d[N];
queue<int> q;
queue<int> ans;
int n,m;

void add(int a,int b){
    e[idx] = b,ne[idx] = h[a],h[a] = idx++;
}

bool topoSort(){
    for(int i = 1; i<= n; i++){
        if(!d[i]){
            q.push(i);
            ans.push(i);
        }
    }
    while(!q.empty()){
        int t = q.front();q.pop();
        for(int i  =h[t]; i!= -1; i = ne[i]){
            int j = e[i];
            d[j]--;
            if(d[j] == 0){
                q.push(j);
                ans.push(j);
            }
        }
    }
    if(ans.size()!=n)return false;
    return true;
}

int main(){
    idx = 0;
    memset(h,-1,sizeof(h));
    cin>>n>>m;
    while(m--){
        int x,y;
        cin>>x>>y;
        add(x,y);
        d[y]++;
    }
  if(topoSort()){
        while(!ans.empty()){
            cout<<ans.front()<<' ';
            ans.pop();
        }
    }
    else cout<<-1;
    return 0;
}
```

[有向图的拓扑序列](https://www.acwing.com/problem/content/850/)

### 最短路算法

![](D:\study\md笔记图片\屏幕截图(283).png)

- 朴素的dijkstra算法

> 时间复杂度位O（n^2）,应在稠密图中使用
>
> 算法概述<br>1.初始化dist[1] = 0，dist[else] = 无穷大<br>2.找到不在s中的dist最小的点t，放入s<br>3.通过t更新相关点的dist<br> 重复上述过程n次（n为结点数）  

```c++
int g[N][N];  // 存储每条边
int dist[N];  // 存储1号点到每个点的最短距离
bool st[N];   // 存储每个点的最短路是否已经确定

// 求1号点到n号点的最短路，如果不存在则返回-1
int dijkstra()
{
    //初始化
    memset(dist, 0x3f, sizeof dist);//填充的是二进制的0x3f，相当于int的0x3f3f3f3f3，常在图中表示无穷大
    dist[1] = 0;
	//第一个for循环的作用是执行n次
    for (int i = 0; i < n - 1; i ++ )
    {
        //找t
        int t = -1;     // 在还未确定最短路的点中，寻找距离最小的点
        for (int j = 1; j <= n; j ++ )
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;
        //把t放入s
		st[t] = true;
        // 用t更新其他点的距离
        for (int j = 1; j <= n; j ++ )
            dist[j] = min(dist[j], dist[t] + g[t][j]);
    }

    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}
```

[Dijkstra求最短路 I]([AcWing 849. Dijkstra求最短路 I - AcWing](https://www.acwing.com/activity/content/problem/content/918/))

- 堆优化的dijkstra算法

> 时间复杂度O（mlogn）,应在稀疏图中使用

```c++
typedef pair<int, int> PII;

int n;      // 点的数量
int h[N], w[N], e[N], ne[N], idx;       // 邻接表存储所有边
int dist[N];        // 存储所有点到1号点的距离
bool st[N];     // 存储每个点的最短距离是否已确定

// 求1号点到n号点的最短距离，如果不存在，则返回-1
int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    heap.push({0, 1});      // first存储距离，second存储节点编号
    //插入只能按照，dist[i]，i的顺序插入
    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();
        int ver = t.second, distance = t.first;//不在s中的dist最小的点ver，distance表示其dist值

        if (st[ver]) continue;//我们每次更新dist的方法是直接将更小的dist插入，而未修改之前堆中的dist，可能会存在冗余
        st[ver] = true;
        //更新与ver相关的点的dist
        for (int i = h[ver]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > distance + w[i])
            {
                dist[j] = distance + w[i];
                heap.push({dist[j], j});
            }
        }
    }

    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}
```

[Dijkstra求最短路 II]([850. Dijkstra求最短路 II - AcWing题库](https://www.acwing.com/problem/content/852/))

- Bellman-Flord算法
  O（n*m）

```c++
int n, m;       // n表示点数，m表示边数
int dist[N];        // dist[x]存储1到x的最短路距离

struct Edge     // 边，a表示出点，b表示入点，w表示边的权重
{
    int a, b, w;
}edges[M];

// 求1到n的最短路距离，如果无法从1走到n，则返回-1。
int bellman_ford()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    // 如果第n次迭代仍然会松弛三角不等式，就说明存在一条长度是n+1的最短路径，由抽屉原理，路径中至少存在两个相同的点，说明图中存在负权回路。
    for (int i = 0; i < n; i ++ )//第k次迭代代表经过不超过k条边的最小dist，但若要求，需要增加一个备份数组
    {
        for (int j = 0; j < m; j ++ )
        {
            int a = edges[j].a, b = edges[j].b, w = edges[j].w;
            if (dist[b] > dist[a] + w)
                dist[b] = dist[a] + w;
        }
    }

    if (dist[n] > 0x3f3f3f3f / 2) return -1;
    return dist[n];
}
//
```

- spfa算法
  ```c++
  int n;      // 总点数
  int h[N], w[N], e[N], ne[N], idx;       // 邻接表存储所有边
  int dist[N];        // 存储每个点到1号点的最短距离
  bool st[N];     // 存储每个点是否在队列中
  
  // 求1号点到n号点的最短路距离，如果从1号点无法走到n号点则返回-1
  int spfa()
  {
      memset(dist, 0x3f, sizeof dist);
      dist[1] = 0;
  
      queue<int> q;
      q.push(1);
      st[1] = true;
  
      while (q.size())
      {
          auto t = q.front();
          q.pop();
  
          st[t] = false;
  
          for (int i = h[t]; i != -1; i = ne[i])
          {
              int j = e[i];
              if (dist[j] > dist[t] + w[i])
              {
                  dist[j] = dist[t] + w[i];
                  if (!st[j])     // 如果队列中已存在j，则不需要将j重复插入
                  {
                      q.push(j);
                      st[j] = true;
                  }
              }
          }
      }
  
      if (dist[n] == 0x3f3f3f3f) return -1;
      return dist[n];
  }
  
  ```

  [spfa求最短路](https://www.acwing.com/problem/content/description/853/)

- Floyd算法

```c++
初始化：
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= n; j ++ )
            if (i == j) d[i][j] = 0;
            else d[i][j] = INF;

// 算法结束后，d[a][b]表示a到b的最短距离
void floyd()
{
    for (int k = 1; k <= n; k ++ )
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= n; j ++ )
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
}
```

[ Floyd求最短路]([854. Floyd求最短路 - AcWing题库](https://www.acwing.com/problem/content/856/))

### 最小生成树算法

![](D:\study\md笔记图片\屏幕截图(284).png)

> 稠密图用朴素版Prim算法，稀疏图用Kruscal算法

- Prim算法(稠密图时使用)

```c++
int n;      // n表示点数
int g[N][N];        // 邻接矩阵，存储所有边
int dist[N];        // 存储其他点到当前最小生成树的距离
bool st[N];     // 存储每个点是否已经在生成树中


// 如果图不连通，则返回INF(值是0x3f3f3f3f), 否则返回最小生成树的树边权重之和
int prim()
{
    memset(dist, 0x3f, sizeof dist);
    int res = 0;
    dist[1] = 0;
    for (int i = 0; i < n; i ++ )//起到迭代n次的作用
    {
        int t = -1;
        for (int j = 1; j <= n; j ++ )
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;//第一次运行结束t = 1,dist[t] = INF
        if (dist[t] == INF) return INF;//其他点到该集合的距离都是无穷大，此图不连通
  		res += dist[t];
        st[t] = true;
		//用t更新其他点到集合的距离(此步与dijkstra算法不同)
        for (int j = 1; j <= n; j ++ ) dist[j] = min(dist[j], g[t][j]);//思考，此处为什么不需要保证j不在集合中
    }

    return res;
}
```

- krusacl算法
  用于稀疏图

```c++
int n, m;       // n是点数，m是边数
int p[N];       // 并查集的父节点数组

struct Edge     // 存储边
{
    int a, b, w;

    bool operator< (const Edge &W)const
    {
        return w < W.w;
    }
}edges[M];

int find(int x)     // 并查集核心操作
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int kruskal()
{
    sort(edges, edges + m);

    for (int i = 1; i <= n; i ++ ) p[i] = i;    // 初始化并查集

    int res = 0, cnt = 0;
    for (int i = 0; i < m; i ++ )
    {
        int a = edges[i].a, b = edges[i].b, w = edges[i].w;

        a = find(a), b = find(b);
        if (a != b)     // 如果两个连通块不连通，则将这两个连通块合并
        {
            p[a] = b;
            res += w;
            cnt ++ ;
        }
    }

    if (cnt < n - 1) return INF;
    return res;
}
```



[Prim算法求最小生成树]([858. Prim算法求最小生成树 - AcWing题库](https://www.acwing.com/problem/content/860/))

- Kruscal算法

> 1.将边按权值从小到大进行排序<br>2.枚举边a,b，如果两点不连通就放入集合（运用并查集实现）

```c++
int n, m;       // n是点数，m是边数
int p[N];       // 并查集的父节点数组

struct Edge     // 存储边
{
    int a, b, w;

    bool operator< (const Edge &W)const
    {
        return w < W.w;
    }
}edges[M];

int find(int x)     // 并查集核心操作
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int kruskal()
{
    sort(edges, edges + m);

    for (int i = 1; i <= n; i ++ ) p[i] = i;    // 初始化并查集

    int res = 0, cnt = 0;
    for (int i = 0; i < m; i ++ )
    {
        int a = edges[i].a, b = edges[i].b, w = edges[i].w;

        a = find(a), b = find(b);
        if (a != b)     // 如果两个连通块不连通，则将这两个连通块合并
        {
            p[a] = b;
            res += w;
            cnt ++ ;
        }
    }

    if (cnt < n - 1) return INF;
    return res;
}
```

[Kruskal算法求最小生成树]([859. Kruskal算法求最小生成树 - AcWing题库](https://www.acwing.com/problem/content/861/))



### 二分图

> 一个图是二分图当且仅当该图可以被二染色
> 当且仅当不含奇数环

- 用染色法判断二分图

```c++
#include <algorithm>

using namespace std;

const int N = 100010, M = 200010;

int n, m;
int h[N], e[M], ne[M], idx;
int color[N];//用0表示未染色，1表示一种颜色，2表示另一种颜色

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

bool dfs(int u, int c)
{
    color[u] = c;

    for (int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!color[j])
        {
            if (!dfs(j, 3 - c)) return false;
        }
        else if (color[j] == c) return false;
    }

    return true;
}

int main()
{
    scanf("%d%d", &n, &m);

    memset(h, -1, sizeof h);

    while (m -- )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        add(a, b), add(b, a);
    }

    bool flag = true;
    for (int i = 1; i <= n; i ++ )//图可能不连通，一次DFS不一定能全部染色
        if (!color[i])//没有染色才进行染色，不然会改变已染色点的状态
        {
            if (!dfs(i, 1))//每个联通块第一个点染上1色，可以证明，染哪个颜色都一样
            {
                flag = false;
                break;
            }
        }

    if (flag) puts("Yes");
    else puts("No");

    return 0;
}

```

- 匈牙利算法

> 解决二分图的最大匹配问题
> 二分图的匹配：给定一个二分图 G，在 G 的一个子图 M 中，M 的边集 {E} 中的任意两条边都不依附于同一个顶点，则称 M 是一个匹配。（找对象，按照边一人只能找一个）
>
> 二分图的最大匹配：所有匹配中包含边数最多的一组匹配被称为二分图的最大匹配，其边数即为最大匹配数

**时间复杂度为O(n*m)**

```c++
int n1, n2;     // n1表示第一个集合中的点数，n2表示第二个集合中的点数
int h[N], e[M], ne[M], idx;     // 邻接表存储所有边，匈牙利算法中只会用到从第一个集合指向第二个集合的边，所以这里只用存一个方向的边
int match[N];       // 存储第二个集合中的每个点当前匹配的第一个集合中的点是哪个
bool st[N];     // 表示在每一轮寻找中第二个集合中的每个点是否已经被遍历过，用来确保每次都会考虑每个可能的女生

bool find(int x)
{
    for (int i = h[x]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!st[j])
        {
            st[j] = true;
            if (match[j] == 0 || find(match[j]))
            {
                match[j] = x;
                return true;
            }
        }
    }

    return false;
}

// 求最大匹配数，依次枚举第一个集合中的每个点能否匹配第二个集合中的点
int res = 0;
for (int i = 1; i <= n1; i ++ )
{
    memset(st, false, sizeof st);//保证此轮寻找中，会找遍i所有边的女生是否有可能成功
    if (find(i)) res ++ ;
}
```



## 4.数学知识

### 质数

> 大于等于二的整数中，只有1和自己这两个因子的数

**可用试除法判断是否为质数**

```c++
bool is_prime(int x)
{
    if (x < 2) return false;//x<2时不是质数
    for (int i = 2; i <= x / i; i ++ )//i<=sqrt(x)就行，但sqrt函数计算需要时间，这样写更快
        if (x % i == 0)
            return false;
    return true;
}
//时间复杂度为O(sqrt(n))
```



**可用试除法分解质因数**

```c++
void divide(int x)
{
    for (int i = 2; i <= x / i; i ++ )
        if (x % i == 0)
        {
            int s = 0;
            while (x % i == 0) x /= i, s ++ ;//此处的i必定是质数
            cout << i << ' ' << s << endl;
        }
    if (x > 1) cout << x << ' ' << 1 << endl;//注意此步
    cout << endl;
}
//时间复杂度为O(sqrt(n))
```

**筛数法，这里直接讲解算法复杂度最低的线性筛**

```c++
const int N = 1000010;

//primes数组用来存放质数
int primes[N], cnt;
//st[i], i为质数则为false否则为true
bool st[N];// false表示是质数

void get_primes(int n)
{
    for(int i = 2; i <= n; i++)//筛掉所有以i为最大因子的合数，这样就不会重复标记一个合数
        //假设筛掉的数为 j = i*p,要保 证i是其最大因子，则要满足
		//1.p为质数 2.p<i的最小质因子
    {
        if(!st[i]) primes[cnt++] = i;
        for(int j = 0; primes[j] <= n / i; j ++)//j则为上文的p
        {
            st[primes[j]*i] = true;
            if(i % primes[j] == 0) break;//j已经增大到与i的最小质因子相等了，再筛下去i就不是筛掉的数字的最大因子了，增大i
        }
    }
}
//时间复杂度为O(nloglogn)
```



### 约数

**理论准备：**
$$
N = \prod_{i=1}^{k} p_i^{a_i} = p_1^{a_1} \cdot p_2^{a_2} \cdots p_k^{a_k}
\\ \text{约数个数} : \prod_{i=1}^{k} (a_i + 1) = (a_1 + 1)(a_2 + 1) \cdots (a_k + 1)
\\ \text{约数之和} : \prod_{i=1}^{k} \sum_{j=0}^{a_i} p_i^j = \prod_{i=1}^{k} (p_i^0 + p_i^1 + \cdots + p_i^{a_i})
$$
**试除法求约数**(求出从小到大的所有约数)

```c++
vector<int> get_divisors(int x)
{
    vector<int> res;
    for (int i = 1; i <= x / i; i ++ )
        if (x % i == 0)
        {
            res.push_back(i);
            if (i != x / i) res.push_back(x / i);//i = sqrt(x)时，防止压入2个i
        }
    sort(res.begin(), res.end());
    return res;
}

```



**约数个数**

```c++
int f(int x){
    ll res = 1;
    for(int i = 2; i <= x/i; i++){
        if(x%i==0){
            int s = 0;
            while(x&i==0){
                x /= i;
                s++;
            }
            res *= (s+1;)
        }
    }
    if(x>1)res *= 2;
    return res%(1e9+7);
}
```



**约数求和**

```c++
#include<iostream>
#include<unordered_map>
using namespace std;
typedef long long ll;
unordered_map<int,int> h;
const int mod = 1e9+7;
void f(int x){
    for(int i = 2; i <= x/i; i++){
        int s = 0;
        if(x%i==0){
            while(x%i==0){
                s++;
                x /= i;
            }
            h[i] += s;
        }
    }
    if(x>1)h[x] += 1;
}

int main(){
    int n;
    cin>>n;
    while(n--){
        int a;
        cin>>a;
        f(a);
    }
    ll ans = 1;
    for(auto i : h){
        ll t = 1;
        ll a = i.first;
        ll b = i.second;
        while(b--) t = (t*a+1)%mod;//求 t = a^0+a^1...+a^b
        ans = (ans*t)%mod;
    }
    cout<<ans;
    return 0;
}
```



- **最大公约数**

> **辗转相除法**(欧几里得算法)，计算两个数的最大公约数

(a,b) = (b,a mod b)
$$
d|a,d|b \Rightarrow d|ax+by
\\ amodb = a - \left \lfloor \frac{a}{b} \right \rfloor b
\\ let\ c = \left \lfloor \frac{a}{b} \right \rfloor
\\ amodb = a-cb
\\ d|a,d|b \Rightarrow d|b,d|a-cb
\\ d|b,d|a-cb\Rightarrow d|b,d|a-cb+cb,d|a
\\Q.E.D
$$

```c++
int gcd(int a,int b){
    if(!b)return a;//处理b一开始就为0的情况
    return gcd(b,a%b);//不能return gcd(a%b,b),当a%b<b时就会无限递归，导致系统栈爆掉
}
//float point exception 错误 是/ or % 的操作数为0
```



### 欧拉函数

原理证明略去，此处给出结论
$$
1∼N中与 N互质的数的个数被称为欧拉函数，记为 ϕ(N)
\\ N= \prod_{i=1}^{k} p_i^{a_i} = p_1^{a_1} \cdot p_2^{a_2} \cdots p_k^{a_k}
\\则：ϕ(N)= N\prod_{i=1}^{i=m} \frac{p_i - 1}{p_i}
$$

- 求某个数的euler函数
  ```c++
   int phi(int x)
  {
      int res = x;
      for (int i = 2; i <= x / i; i ++ )
          if (x % i == 0)
          {
              res = res / i * (i - 1);
              while (x % i == 0) x /= i;
          }
      if (x > 1) res = res / x * (x - 1);//保证每一步的运算都是整数
  
      return res;
  }
  //时间复杂度为O(sqrt(n));
  ```

- 求前n个数的euler函数

```c++
//用筛法求euler 函数
int primes[N], cnt;     // primes[]存储所有素数
int euler[N];           // 存储每个数的欧拉函数
bool st[N];         // st[x]存储x是否被筛掉


void get_eulers(int n)
{
    euler[1] = 1;
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i])
        {
            primes[cnt ++ ] = i;
            euler[i] = i - 1;//i为质数 Φ(i) = i-1
        }
        for (int j = 0; primes[j] <= n / i; j ++ )
        {
            int t = primes[j] * i;
            st[t] = true;
            if (i % primes[j] == 0)
            {   //若t为质数则euler[t]为t-1，若t为合数，则其必定已经背筛掉，在筛的过程中euler[t]也被算出来了
                //故euler[t]必定已被算出来了
                euler[t] = euler[i] * primes[j];//i不是质数，primse[j]是t的质因子
                break;
            }
            euler[t] = euler[i] * (primes[j] - 1);//t不是质数，primes[j]也不是t的质因子
        }
    }
}

//时间复杂度为O(nloglogn)

```

- euler 定理

$$
对任意两个正整数 a, n，如果两者互质，那么 a^{φ(n)}≡1(modn)。
\\ 取n为质数\ 得a^{n-1}\equiv1(mod n)\ 此即为费马小定理
$$



### 快速幂

- 

```c++
//求 m^k mod p，时间复杂度 O(logk)。

LL qmi(int a, int b, int p)
{
    LL res = 1 % p;//处理a=1，b=0的边界情况
    while (b)
    {
        if (b & 1) res = res * a % p;
        a = (LL)a * a % p;//确保a*a这一步是以long long 乘法进行的，long * int 按照long乘法进行运算
        b >>= 1;
    }
    return res;
}
//容易超出范围，可将相应数的类型调整为long long 
```

**对于快速幂的进一步理解**

- 定义一个运算 + 和一个元素 a，如果我们要算a+a+a+a...  做n次+运算，如果+运算具有结合律，那我们就可以使用快速幂的技巧。

  ```c++
  LL qmi(int a, int b)//队a做b次上述意义的运算并求其结果
  {
      LL res = 1 ; //这里的1即是在上述运算所定义的群上的零元
      while (b)//做b次运算
      {
          if (b & 1) res = res * a;
          a = (LL)a * a ;
          b >>= 1;
      }
      return res;
  }
  ```

### 扩展欧几里得算法

> 引理：
>
> 对于 x , y 的二元一次不定方程 a x + b y = c  ，其有解的充要条件为 gcd ⁡ ( a , b ) ∣ c（证明略），这里的x，y是任意整数

```c++
// 求x, y，使得ax + by = gcd(a, b)
int exgcd(int a, int b, int &x, int &y)
{
    if (!b)
    {
        x = 1; y = 0;
        return a;
    }
    int d = exgcd(b, a % b, y, x);
    y -= (a/b) * x;
    return d;
}
```

原理如下

![](D:\study\md笔记图片\Snipaste_2024-08-28_22-23-57.png)

更一般地
![](D:\study\md笔记图片\Snipaste_2024-08-28_22-55-43.png)

### 中国剩余定理

![image-20240829085930480](C:\Users\32765\AppData\Roaming\Typora\typora-user-images\image-20240829085930480.png)





### 高斯消元

```c++

const double eps = 1e-6;//用于判断一浮点数是否为0
//返回代表有无数个解，0代表有唯一解，-1代表无解
int gauss()
{
    int c, r;// c 代表 列 col ， r 代表 行 row
    for (c = 0, r = 0; c < n; c ++ )
    {
        int t = r;// 先找到当前这一列，绝对值最大的一个数字所在的行号
        for (int i = r; i < n; i ++ )
            if (fabs(a[i][c]) > fabs(a[t][c]))
                t = i;

        if (fabs(a[t][c]) < eps) continue;// 如果当前这一列的最大数都是 0 ，处理下一列就好
		//把当前这一行，换到最上面（不是第一行，是第 r 行）去，这主要是为了运算精度考虑
        for (int i = c; i < n + 1; i ++ ) swap(a[t][i], a[r][i]);
        
        for (int i = n; i >= c; i -- ) a[r][i] /= a[r][c];// 把当前这一行的第一个数，变成 1， 方程两边同时除以 第一个数，必须要到着算，不然第一个数直接变1，系数就被篡改，后面的数字没法算
        for (int i = r + 1; i < n; i ++ )// 把当前列下面的所有数，全部消成 0
            if (fabs(a[i][c]) > eps)// 如果非0 再操作，已经是 0就没必要操作了
                for (int j = n; j >= c; j -- )// 从后往前，当前行的每个数字，都减去对应列 * 行首非0的数字，这样就能保证第一个数字是 a[i][0] -= 1*a[i][0]，由于每次都要用到a[i][c],顺着来会导致这个数据被篡改
                    a[i][j] -= a[r][j] * a[i][c];

        r ++ ;// 这一行的工作做完，换下一行
    }

    if (r < n)// 说明剩下方程的个数是小于 n 的，说明不是唯一解，判断是无解还是无穷多解
    {// 因为已经是阶梯型，所以 r ~ n-1 的值应该都为 0
        for (int i = r; i < n; i ++ )// 
            if (fabs(a[i][n]) > eps)// a[i][n] 代表 b_i ,即 左边=0，右边=b_i,0 != b_i, 所以无解。
                return -1;
        return 1;// 否则， 0 = 0，就是r ~ n-1的方程都是多余方程
    }
    // 唯一解 ↓，从下往上回代，得到方程的解
    for (int i = n - 1; i >= 0; i -- )
        for (int j = i + 1; j < n; j ++ )
            a[i][n] -= a[j][n] * a[i][j];//因为只要得到解，所以只用对 b_i 进行操作，中间的值，可以不用操作，因为不用输出

    return 0;
}
```



### 求组合数

$$
理论依据： C_a^b = C_{a-1}^{b-1}+C_{a-1}^{b}
$$







### 容斥原理



## 5.动态规划

构造一个DP，我们先考虑需要用几个参数来表示状态，也就是“几维”，最后再思考如何进行状态计算，也就是求状态转移方程。
一个状态也就是满足参数要求的一个集合。而dp(i,j)存储的就是这个集合的属性，一般就是这些状态的最大值，最小值或者数量

状态计算一般对应着集合的“划分",划分要满足一下几个原则：1.不重（不一定需要满足，如属性为最大值时） 2.不漏

DP的优化就是对状态转移方程进行等价变形，我们要先考虑一 般的情况，再考虑优化

动态规划的时间复杂度一般为O(状态数量 * 转移计算的时间复杂度)



动态规划要想得到是怎样达到最佳状态的话，我们只需记录一下每个状态是怎样转移得来的就好

### 背包问题



- 0-1背包

  ![](D:\study\md笔记图片\屏幕截图 2024-04-29 182545.png)

  ```c++
  const int MAXN = 1005;
  int v[MAXN];    // 体积
  int w[MAXN];    // 价值 
  int f[MAXN][MAXN];  // f[i][j], j体积下前i个物品的最大价值 
  
  int main() 
  {
      int n, m;   
      cin >> n >> m;
      for(int i = 1; i <= n; i++) 
          cin >> v[i] >> w[i];
  
      for(int i = 1; i <= n; i++) 
          for(int j = 1; j <= m; j++)
          {
              //  当前背包容量装不进第i个物品，则价值等于前i-1个物品
              if(j < v[i]) 
                  f[i][j] = f[i - 1][j];
              // 能装，需进行决策是否选择第i个物品
              else    
                  f[i][j] = max(f[i - 1][j], f[i - 1][j - v[i]] + w[i]);
          }           
  
      cout << f[n][m] << endl;
  
      return 0;
  }
  ```
  
  我们考虑如何优化
  
  ```c++
  const int MAXN = 1005;
  int v[MAXN];    // 体积
  int w[MAXN];    // 价值 
  int f[MAXN];  //优化
  
  int main() 
  {
      int n, m;   
      cin >> n >> m;
      for(int i = 1; i <= n; i++) 
          cin >> v[i] >> w[i];
  
      for(int i = 1; i <= n; i++) //每次只用到了i-1，我们可以用滚动数组优化，故此处不再需要f[i]这一维度
         //删去f[i]这一维度
          for(int j = m; j>=v[i];j--)
          {
             	//此语句无需实现，优化掉 f[j] = f[j];
              // 能装，需进行决策是否选择第i个物品
             // else //j从v[i]枚举起，可以省掉判断    
              f[i][j] = max(f[j], f[j - v[i]] + w[i]);//用的是f[i-1]的状态，所以我们让j从大到小计算
              //这样计算f[i][j]要用到f[i - 1][j]和f[i - 1][j - v[i]]时的发f[j]就是f[i-1][j]
          }           
  
      cout << f[n][m] << endl;
  
      return 0;
  }
  ```
  
  
  
  [01背包问题](https://www.acwing.com/problem/content/2/)

- 完全背包
  ![](D:\study\md笔记图片\屏幕截图 2024-05-04 115530.png)

暴力思路：

```c++
#include<iostream>
using namespace std;
const int N = 1010;
int f[N][N];
int v[N],w[N];
int main(){
    int n,m;
    cin>>n>>m;
    for(int i = 1; i <= n; i++)cin>>v[i]>>w[i];
    for(int i = 1;i<=n ;i++){
        for(int j = 1;j<=m;j++){
            for(int k = 0;k*v[i]<=j;k++){
                f[i][j] = max(f[i][j],f[i-1][j-k*v[i]]+k*w[i]);
            }
        }
    }
    cout<<f[n][m];
}
```

观察可发现：

f[i , j ] = max( f[i-1,j] , f[i-1,j-v]+w ,  f[i-1,j-2*v]+2*w , f[i-1,j-3*v]+3*w , .....)
f[i , j-v]= max(            f[i-1,j-v]   ,  f[i-1,j-2*v] + w , f[i-1,j-3*v]+2*w , .....)
由上两式，可得出如下递推关系： 
                        f[i,j] = max(f[i-1,j],f[i,j-v]);
故可对转移方程进行优化，时间复杂度由O(m*nk)优化到O(mn);

再类似0-1背包进行优化，得到最终代码,这一步优化在物品总数仅为1e3时，可以快将近一倍
```c++
#include<iostream>
using namespace std;
const int N = 1010;
int f[N];
int v[N],w[N];
int main(){
    int n,m;
    cin>>n>>m;
    for(int i = 1; i <= n; i++)cin>>v[i]>>w[i];
    for(int i = 1;i<=n ;i++)
        for(int j = v[i];j<=m;j++)
            f[j] = max(f[j],f[j-v[i]]+w[i]);
    cout<<f[m];
}
```

[完全背包问题](https://www.acwing.com/problem/content/description/3/)

- 多重背包问题
  暴力思路：

```c++
#include<iostream>
using namespace std;
const int N = 110;
int v[N],w[N],s[N],f[N][N];
int main(){
    int n,m;
    cin>>n>>m;
    for(int i = 1;i<=n; i++)cin>>v[i]>>w[i]>>s[i];
    for(int i = 1; i <=n ;i++){
        for(int j = 1;j<=m; j++){
            for(int k = 0;k*v[i]<=j && k<=s[i];k++)
                f[i][j] = max(f[i][j],f[i-1][j-k*v[i]]+k*w[i]);
        }
    }
    cout<<f[n][m];
}
```

进行优化：本题的优化思路较难想到，我们对于k的枚举进行二进制优化

我们将物品i按照2^0,2^1,...,2^k(他们的和加起来<=s,和记为sum，s-sum我们单独打包为一组，记为第k+1组)分组打包，这样就得到了新的k+1种物品，他们的v和w分别为k*v[i],k*w[i],，不难证明，我们有0--k+1组的物品数量可以组合成任意一个<=s的整数,且无论如何组合也不会得到>s的数，那么这个问题就等价于在这0-k+1种物品当中做0-1背包问题。再对得到的结果进行类似0-1背包的优化。

代码如下：

```c++
#include<iostream>
using namespace std;

const int N = 12010;//要考虑到拆分后物品数量会增加，相关数组要开大
int v[N], w[N], f[N];
int cnt = 0;

int main() {
    int n, m;
    cin >> n >> m;
    int cnt = 0;
    for(int i = 1;i<=n;i++){
        int a,b,c;
        cin>>a>>b>>c;
        int k = 1;
        while(k<=c){
            cnt++;
            v[cnt] = k*a;
            w[cnt] = k*b;
            c -= k;
            k *= 2;
        }
        if(c>0){
            cnt++;
            v[cnt] = c*a;
            w[cnt] = c*b;
        }
    }
    n = cnt;

    for (int i = 1; i <= n; ++i) {
        for (int j = m; j >= v[i]; j--) {
           f[j] = max(f[j], f[j - v[i]] + w[i]);
        }
    }

    cout << f[m];
    return 0;
}

```

时间复杂度由O(mns)降到了O(mnlogs);

[ 多重背包问题 II](https://www.acwing.com/problem/content/5/)

用单调队列进行优化(谢谢，已经难死了)

记r = jmodv，即是把这个物品装到最大数量时，体积为j的背包剩余的容积，为了说明的方便
**记f(i-1,x) = fx**

f(i,r)=fr  
f(i,r+v)=max(fr+v,fr+w)  
f(i,r+2v)=max(fr+2v,fr+v+w,fr+2w)  
⋯  
f(i,r+(s−1)v)=max(fr+(s−1)v,fr+(s−2)v+w,⋯,fr+(s−1)w) 
f(i,r+sv)=max(fr+sv,fr+(s−1)v+w,⋯,fr+sw)(滑动窗口已满)  这个物品已经装到其最大数量s个了
f(i,r+(s+1)v)=max(fr+(s+1)v,fr+sv+w,⋯,fr+v+sw)(滑动窗口已满)  
⋯ 
f(i,j−2v)=max(fj−2v,fj−3v+w,⋯,fj−(s+2)v+sw)(滑动窗口已满)  
f(i,j−v)=max(fj−v,fj−2v+w,⋯,fj−(s+1)v+sw)(滑动窗口已满)  
f(i,j)=max(fj,fj−v+w,⋯,fj−sv+sw)(滑动窗口已满) 

不难发现，所有 j = r(modvi)的f(i,j)的最大值就是fj,fj−v+w,⋯,fj−sv+sw这个窗口内的最大值
我们暂时去掉其后加的w的倍数

**我们就是要求fj,fj−v,⋯,fj−sv，fj−(s+1)v,⋯fr上面窗口尺寸为s(当然,物品体积和也不能超过背包容量)的最大值**，实现时，我们还需加上前面略去的w的倍数

所以可以变成一个滑动窗口求最大值的问题

**我们使用单调递减的单调队列来优化**

```c++
#include <iostream>
using namespace std;
const int N = 1010, M = 20010;

int n, m;
int v[N], w[N], s[N];
int f[N][M];
int q[M];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; ++ i) cin >> v[i] >> w[i] >> s[i];
    for (int i = 1; i <= n; ++ i)
    {
        for (int r = 0; r < v[i]; ++ r)//枚举余数
        {
            int hh = 0, tt = -1;//模拟队列
            for (int j = r; j <= m; j += v[i])
            {
                //滑动窗口的处理方式
                while (hh <= tt && j - q[hh] > s[i] * v[i]) hh ++ ;//队列非空，且装的物品数量超过s个，滑出队头
                //因为物品数量装满s个的体积也就是S[i]*v[i]
                while (hh <= tt && f[i - 1][q[tt]] + (j - q[tt]) / v[i] * w[i] <= f[i - 1][j]) -- tt;
                q[ ++ tt] = j;
                f[i][j] = f[i - 1][q[hh]] + (j - q[hh]) / v[i] * w[i];
            }
        }
    }
    cout << f[n][m] << endl;
    return 0;
}
```

时间复杂度优化到 O(n*m)

类似其他背包问题，我们还可以对空间复杂度进行优化。
虽然每次f(i,j)只需要f(i-1,k)的值，但是k与j的大小关系并不固定，所以我们不能简单地通过让j从大到小or从小到大循环来解决这个问题，我们可以采用拷贝数组或者滚动数组的方式来实现，这里我们选择拷贝数组

```c++
#include<iostream>
#include<cstring>
using namespace std;

const int M = 20010, N = 1010;
int v[N],w[N],s[N];
int f[M],g[M];
int q[M];
int m,n;

int main() {
    cin>>n>>m;
    for(int i = 1; i <= n; i++)cin>>v[i]>>w[i]>>s[i];

    for(int i = 1; i <= n; i++) {
        for(int r = 0; r < v[i]; r++) {
            int hh = 0, tt = -1;
            memcpy(g,f,sizeof g);
            for(int j = r; j <= m; j += v[i]) {
                while(hh<=tt && j-q[hh]>s[i]*v[i])hh++;
                while (hh<=tt && g[q[tt]]+(j-q[tt])/v[i]*w[i]<=g[j])--tt;
                q[++tt] = j;
                f[j] = g[q[hh]]+(j-q[hh])/v[i]*w[i];
            }
        }
    }
    cout<<f[m];
    return 0;
}
```







- 分组背包

暴力做法：

```c++
#include<iostream>
using namespace std;
const int N = 110;
int f[N][N],v[N][N],w[N][N],s[N];
int main(){
    int n,m;
    cin>>n>>m;
    for(int i = 1; i <= n; i++){
        cin>>s[i];
        for(int j = 1; j <=s[i]; j++){
            cin>>v[i][j]>>w[i][j];
        }
    }
    for(int i = 1; i <= n; i++){
        for(int j = 1; j<=m ;j++){
            f[i][j] = f[i-1][j];
            for(int k = 1; k<=s[i]; k++){
                if(v[i][k]<=j)
                    f[i][j] = max(f[i][j],f[i-1][j-v[i][k]]+w[i][k]);
            }
        }
    }
    cout<<f[n][m];
}
```

用滚动数组优化

```c++
#include<iostream>
using namespace std;
const int N = 110;
int f[N],v[N][N],w[N][N],s[N];
int main(){
    int n,m;
    cin>>n>>m;
    for(int i = 1; i <= n; i++){
        cin>>s[i];
        for(int j = 1; j <=s[i]; j++){
            cin>>v[i][j]>>w[i][j];
        }
    }
    for(int i = 1; i <= n; i++){
        for(int j = m; j>=1 ;j--){
            for(int k = 1; k<=s[i]; k++){
                if(v[i][k]<=j)
                    f[j] = max(f[j],f[j-v[i][k]]+w[i][k]);
            }
        }
    }
    cout<<f[m];
}
```

[分组背包问题](https://www.acwing.com/problem/content/9/)

### 线性dp

> 所谓线性DP，就是递推方程是有一个明显的线性关系的，一维线性和二维线性甚至多维都有可能。
> 动态规划里的每一个状态都是一个多维(1-n维)的状态。
> 比如说背包问题就是一个二维的问题，如果我们把它画出来的话会是一个二维矩阵的形式。
> 而我们在求的时候，有一个明显的求的顺序：即一行一行，或者一列一列来求。这样的有线性顺序的叫做线性DP

- 数字三角形
  编码方式注意一下，其他没啥好说的

```c++

//自底向上
#include<iostream>
using namespace std;
const int N = 510;
int a[N][N],f[N][N];

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int n;
    cin>>n;
    for(int i = 1; i<=n; i++){
        for(int j = 1; j<=i; j++)cin>>a[i][j];
    }
    for(int j = 1; j <= n; j++)
        f[n][j] = a[n][j];
    for(int i = n-1; i>0; i--){
        for(int j = 1; j <= i ; j++)
            f[i][j] = max(f[i+1][j],f[i+1][j+1])+a[i][j];
    }
    cout<<f[1][1];
}
```



```c++
//自顶向下，需要考虑边界情况，也更复杂一点
#include<bits/stdc++.h>
using namespace std;

const int N=510,INF=0x3f3f3f3f;
int f[N][N];
int a[N][N];

int main(){
    int n;
    cin>>n;

    for(int i=1;i<=n;i++){
        for(int j=1;j<=i;j++){
            cin>>a[i][j];
        }
    }

    for(int i=1;i<=n;i++){             
        for(int j=0;j<=i+1;j++){//因为有负数，所以应该将两边也设为-INF
            f[i][j]=-INF;
        }
    }

    f[1][1]=a[1][1];
    for(int i=2;i<=n;i++){
        for(int j=1;j<=i;j++){
            f[i][j]=a[i][j]+max(f[i-1][j-1],f[i-1][j]);
        }
    }

    int res=-INF;
    for(int i=1;i<=n;i++) res=max(res,f[n][i]);
    cout<<res<<endl;
}
```

[数字三角形](https://www.acwing.com/problem/content/900/)

- 最长上升子序列

没啥好说的，找好状态转移方程就好，同时注意f初状态及边界的值

暴力做法：

```c++
#include<iostream>
#include<algorithm>
using namespace std;
const int N = 1010;
int a[N],f[N];

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int n;
    cin>>n;
    for(int i = 1; i<= n; i++){
        cin>>a[i];
        f[i] = 1;
    }
    for(int i = 1; i<= n; i++){
        for(int j = 1; j<=i; j++){
            if(a[i]>a[j])f[i] = max(f[i],f[j]+1);
        }
    }
    int res = 0;
    for(int i = 1; i<=n ;i++)res = max(res,f[i]);
    cout<<res;
}
```

我们不难得到该算法的时间复杂度为O(n^2)，我们思考如何优化

优化后的写法其实已经不是dp，这也告诉我们有时候dp并不是最优做法，我们的优化思路如下：

​	首先数组a中存输入的数（原本的数），开辟一个数组f用来存结果，最终数组f的长度就是最终的答案；假如数组f现在存了数，当到了数组a的第i个位置时，首先判断a[i] > f[cnt] ？ 若是大于则直接将这个数添加到数组f中，即f[++cnt] = a[i]（这个时候就有一个问题，我也许不选择这个数放入我们的数组而是选择后面某个比这个小的数但是又大于a[cnt]的数放入f中不是更好码，这样放的话才会让我们f的长度尽量大嘛，我们后面的操作会解决这个问题）
当a[i] <= f[cnt] 的时,我们就用a[i]去替代数组f中的第一个**大于等于**a[i]的数（也就是大于等于他的数中最小的那个，这不就解决掉前面的问题了吗，这样替换后会导致后面原来一些不能替换的数又能替换了，这个替换也许会替换掉数组中最后一个数，这样就能得到更长的上升子序列，也就是撤销我们前面错误的选择，但替换了也不影响我数组的长度，这就是这个做法正确性的理论根据），因为在整个过程中我们维护的数组f 是一个递增的数组，所以我们可以用二分查找在 logn 的时间复杂的的情况下直接找到对应的位置，然后替换，即f[l] = a[i]。

**为什么是大于等于而不是大于:** 如果是大于的话，那我们二分找到的就是大于他的数字中最小的那个，假定序列为 1，2，3，4，这时如果a[i] = 2，那我们就会找到3并把他替换掉，序列就变为了 1，2，2，4，这显然是不合理的

这样当我们遍历完整个数组a后就可以得到最终的结果。

**值得注意的是：** 我们f数组中得到的并不是最长上升子序列，f[i]的值实际上是长度为i的上升子序列的结尾最小是多少



理论是抽象的，而生活之树常青，模拟几组数据便能了解其中真意。

我们可以注意到，时间复杂降为了O(logn);

```c++
#include<iostream>
#include<algorithm>
using namespace std;
const int N = 1e5+10;
int a[N],f[N];

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int n;
    cin>>n;
    for(int i = 1; i<= n; i++){
        cin>>a[i];
    }
    int cnt = 0;
    f[++cnt] = a[1];
    for(int i = 2; i<=n ;i++){
        if(a[i]>f[cnt])f[++cnt] = a[i];
        else{
            int l = 1,r = cnt;
            while(l<r){
                int mid = l+r>>1;
                if(f[mid]>=a[i]) r =mid;
                else l = mid+1;
            }
            f[l] = a[i];
        }
    }
    cout<<cnt;
}
```

[最长上升子序列 II](https://www.acwing.com/problem/content/description/898/)

- 最长公共子序列

状态转移方程有点儿难想，考虑一下有多少种方式能够从以前的状态到代求状态（也就是所谓集合划分），再求出这些方式中的最大值，值得一提的是，在属性为最大值或者最小值的时候，我们这些划分是可以重合的，及这个集合可以在划分a中也可以在划分b中，这是不影响我们求最大值的，但是若属性是数量那么集合划分则不可重合

```c++
#include<iostream>
using namespace std;
const int N  = 1010;
char a[N],b[N];
int n,m,f[N][N];
int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    cin>>n>>m;
    cin>>a+1>>b+1;
    for(int i = 1;i<=n; i++){
        for(int j = 1; j<=m; j++){
            if(a[i] == b[j])f[i][j] = f[i-1][j-1]+1;
            else f[i][j] = max(f[i-1][j],f[i][j-1]);
        }
    }
    cout<<f[n][m];
}
```

[最长公共子序列](https://www.acwing.com/problem/content/899/)

- 最短编辑距离

> 不是哥们儿，这题也能dp啊

搞明白为什么这样划分满足无后效性就好

想清楚状态转移方程

```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

int n, m;
char a[N], b[N];
int f[N][N];

int main()
{
    scanf("%d%s", &n, a + 1);
    scanf("%d%s", &m, b + 1);

    for (int i = 0; i <= m; i ++ ) f[0][i] = i;
    for (int i = 0; i <= n; i ++ ) f[i][0] = i;

    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
        {
            f[i][j] = min(f[i-1][j]+1,f[i][j-1]+1);//删a，增a
           	if(a[i]!=b[j]) f[i][j] = min(f[i][j],f[i-1][j-1]+1);//不相等就改a
            else f[i][j] = min(f[i][j],f[i-1][j-1]);//相等就不改
        }

    printf("%d\n", f[n][m]);

    return 0;
}
```

[最短编辑距离](https://www.acwing.com/problem/content/904/)

- 编辑距离

没啥好说的，就是用最短编辑距离处理多个数据而已

```c++
    #include <iostream>
    #include <algorithm>
    #include <string.h>

    using namespace std;

    const int N = 15, M = 1010;

    int n, m;
    int f[N][N];
    char str[M][N];

    int edit_distance(char a[], char b[])
    {
        int la = strlen(a + 1), lb = strlen(b + 1);

        for (int i = 0; i <= lb; i ++ ) f[0][i] = i;
        for (int i = 0; i <= la; i ++ ) f[i][0] = i;

        for (int i = 1; i <= la; i ++ )
            for (int j = 1; j <= lb; j ++ )
            {
                f[i][j] = min(f[i - 1][j] + 1, f[i][j - 1] + 1);
                f[i][j] = min(f[i][j], f[i - 1][j - 1] + (a[i] != b[j]));
            }

        return f[la][lb];
    }

    int main()
    {
        scanf("%d%d", &n, &m);
        for (int i = 0; i < n; i ++ ) scanf("%s", str[i] + 1);

        while (m -- )
        {
            char s[N];
            int limit;
            scanf("%s%d", s + 1, &limit);

            int res = 0;
            for (int i = 0; i < n; i ++ )
                if (edit_distance(str[i], s) <= limit)
                    res ++ ;

            printf("%d\n", res);
        }

        return 0;
    }
```

[编辑距离](https://www.acwing.com/problem/content/901/)

### 区间dp ###

> 就是f(i)(j) 表示从i到j的某个性质，这样的dp递推时也有他的特点

```c++
for(int len = 1; len<=n; len++){
    for(int i = 1; i+len-1<=n; i++){
        int j = i+len-1;
        ......//后续操作
    }
}
```

[石子合并](https://www.acwing.com/problem/content/284/)

### 数位dp

重点是：**分类讨论 **

### 计数类dp

> f的属性是计数的值，思考好初值条件

### 记忆化搜索

以滑雪问题为例
```c++
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 310;

int n, m;
int g[N][N];
int f[N][N];

int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

int dp(int x, int y)
{
    int &v = f[x][y];//简化写代码而已
    if (v != -1) return v;//v不为-1则已经计算了，直接返回其值
//否则计算
    v = 1;
    for (int i = 0; i < 4; i ++ )
    {
        int a = x + dx[i], b = y + dy[i];
        if (a >= 1 && a <= n && b >= 1 && b <= m && g[x][y] > g[a][b])
            v = max(v, dp(a, b) + 1);
    }

    return v;
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            scanf("%d", &g[i][j]);

    memset(f, -1, sizeof f);

    int res = 0;
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            res = max(res, dp(i, j));

    printf("%d\n", res);

    return 0;
}

```

### 状态压缩dp

就是用一串二进制数表示某种状态，需要一些常用的二进制运算技巧（看得懂写不出来。。。。。

### 树形dp

还没掌握到精髓。。。



### 以题型为划分再论动态规划

#### **1.数字三角形模型**

[摘花生](https://www.acwing.com/problem/content/1017/)
[**最低通行费**](https://www.dotcpp.com/oj/problem3054.html)
[**传纸条**](https://www.dotcpp.com/oj/problem1278.html)

#### **2.最长上升子序列模型**

[**怪盗基德的滑翔翼**](https://www.dotcpp.com/oj/problem3053.html)
[登山](https://www.dotcpp.com/oj/problem3051.html)
[合唱队形](https://acwing.com/problem/content/484/)
[**友好城市**](https://www.dotcpp.com/oj/problem2126.html)  
[ 最大上升子序列和](https://www.dotcpp.com/oj/problem3052.html) 
[**拦截导弹**](https://www.dotcpp.com/oj/problem2123.html)   有点点难诶,贪心难难难
[导弹防御系统 ](https://www.acwing.com/problem/content/189/)  狠狠地dfs 
[最长公共上升子序列 ](https://www.acwing.com/problem/content/274/)  已经难死了，**下面写点儿东西**

状态表示：
fi ,j代表所有a[1 ~ i]和b[1 ~ j]中以b[j]结尾的公共上升子序列的集合；
f i,j的值等于该集合的子序列中长度的最大值；

状态计算（对应集合划分）：
首先依据公共子序列中是否包含a[i]，将f[i][j]所代表的集合划分成两个不重不漏的子集：
不包含a[i]的子集，最大值是f i-1,j；
 包含a[i]的子集，将这个子集继续划分，依据是子序列的倒数第二个元素在b[]中是哪个数：
子序列只包含b[j]一个数，长度是1；
子序列的倒数第二个数是b[1]的集合，最大长度是fi - 1,1+ 1； … 子序列的倒数第二个数是b[j - 1]的集合，最大长度是fi - 1,j - 1+ 1；

```c++
for (int i = 1; i <= n; i ++ )
{
    for (int j = 1; j <= n; j ++ )
    {
        f[i][j] = f[i - 1][j];
        if (a[i] == b[j])
        {
            int maxv = 1;
            for (int k = 1; k < j; k ++ )
                if (b[j] > b[k])//b[j]==a[i] ,等价于a[i] > b[k],
                    //即是求，1-j-1中f(i-1)()的最大值，在遍历j的时候就可求出，不需要再遍历一遍1-j
                    maxv = max(maxv, f[i - 1][k] + 1);
            f[i][j] = max(f[i][j], maxv);
        }
    }
}
```

优化如下

```c++
#include <cstdio>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 3010;

int n;
int a[N], b[N];
int f[N][N];

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &b[i]);

    for (int i = 1; i <= n; i ++ )
    {
        int maxv = 1;//a[i]==b[j]的时候，f(i)(j)的最小值是1
        for (int j = 1; j <= n; j ++ )
        {
            f[i][j] = f[i - 1][j];
            if (a[i] == b[j]) f[i][j] = max(f[i][j], maxv);
            if (a[i] > b[j]) maxv = max(maxv, f[i - 1][j] + 1);//更新maxv
        }
    }

    int res = 0;
    for (int i = 1; i <= n; i ++ ) res = max(res, f[n][i]);
    printf("%d\n", res);

    return 0;
}
```



#### **3.背包模型 **(continuing)

![](C:\Users\32765\AppData\Roaming\Typora\typora-user-images\image-20240830200426014.png)

[关于背包问题状态的定义](https://www.acwing.com/file_system/file/content/whole/index/content/1306630/)





[采药](https://www.acwing.com/problem/content/425/)
[**装箱问题**](https://www.dotcpp.com/oj/problem1283.html)
[二维费用的背包问题](https://www.acwing.com/problem/content/8/)
[**宠物小精灵之收服**](https://www.dotcpp.com/oj/problem3056.html)
[**潜水员**](https://www.dotcpp.com/oj/problem2135.html)  注意根据实际问题调整f数组的含义
[数字组合](https://www.acwing.com/problem/content/280/)
[**庆功会**](https://www.dotcpp.com/oj/problem2133.html)
[**买书**](https://www.dotcpp.com/oj/problem3057.html) 
[背包问题求具体方案](https://www.acwing.com/problem/content/12/)此题逆序主要是考虑字典序最小这一要求
[**机器分配**](https://www.dotcpp.com/oj/problem2130.html)
[金明的预算方案](https://www.acwing.com/problem/content/489/) 
[**货币系统**](https://www.dotcpp.com/oj/problem2137.html)
[货币系统 2](https://www.acwing.com/problem/content/534/) 
[混合背包问题](https://www.acwing.com/problem/content/7/)
[有依赖的背包问题](https://www.acwing.com/problem/content/10/)    确实不太懂
[背包问题求方案数](https://www.acwing.com/problem/content/11/) 
[能量石](https://www.acwing.com/problem/content/736/)

#### **4.状态机模型**

[**大盗阿福**](https://www.dotcpp.com/oj/problem3067.html)
[股票买卖 IV](https://www.acwing.com/problem/content/1059/)
[[买卖股票的最佳时机含冷冻期](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/)]

#### 5.状态压缩dp



#### 6.区间dp

[环形石子合并]







































## 6.贪心

- 大胆猜，严格证

> 好像反证法蛮好用

### 数学知识

1. 排序不等式
2. 绝对值不等式











## 7.再论搜索

### 1.BFS

- 每个点应在其入队前将其对应的st状态置为true，若在出队的时候设置会导致队内元素的st为false，从而将一个元素重复入队 

#### 1.flood fill

- 可以在线性时间复杂度内，找到某个点的连通块

[Lake Counting]([OpenJudge - 2386:Lake Counting](http://bailian.openjudge.cn/practice/2386))
[**山峰和山谷**](https://www.dotcpp.com/oj/problem2365.html)

#### 2.所有边权重相等时的最短路

- 在bfs中每个点最多被访问一次，所以时间复杂度就是O(点的数量)

[**迷宫问题**](https://www.dotcpp.com/oj/problem2178.html)
[武士风度的牛](https://www.acwing.com/problem/content/190/)
[**抓住那头牛**](https://www.dotcpp.com/oj/problem3048.html)

#### 3.多源BFS

- 求一个点到一堆点中最近的那个点的最短距离
- 把那一堆点看作起点，将其dist的值置为0并且放入队列就ok

[矩阵距离](https://www.acwing.com/problem/content/175/)

#### 4.最小步数模型

- 把一个问题的解决看作是一个状态到另一个状态的转变，将这些状态看作图中的一个点，一个状态若能转换到另一个状态我们就认为这两个点之间有边，一个状态到另一个状态看作一步，那么这个图就相当是所有边权重的为一的图，就可用bfs解决，
- 难点在于如何表示状态以及状态之间的转变，

[魔板](https://www.acwing.com/problem/content/1109/) ,借此题来说明这样题目的一般步骤

```c++
#include<iostream>
#include<unordered_map>
#include<queue>
#include<string>
#include<algorithm>
using namespace std;

unordered_map<string,int> dist; //dist存储每个状态对应的步数，根据对应的状态选取对应的map
unordered_map<string,pair<char,string>> pre; //pre存储每个状态的前一个状态，如果需要的话还要存储通过何种操作转移而来
//也要选取合适的map
queue<string> q; //用于bfs的队列
char g[2][4];


//依据题目要求的相应操作
void set(string state)//我们存储一个状态可能并不是按照这个状态原来的样子（如矩阵等等），而是将其变成了string，但对其进行操作时，若直接队string进行操作可能会很困难，我们可以先将其还原为原来的样子，在操作完成后再转变为string，这里的set和get函数做的就是这样的事情
{
    for (int i = 0; i < 4; i ++ ) g[0][i] = state[i]; // 将状态的前四个字符赋值给g的第一行
    for (int i = 7, j = 0; j < 4; i --, j ++ ) g[1][j] = state[i]; // 将状态的后四个字符赋值给g的第二行
}

string get()
{
    string res;
    for (int i = 0; i < 4; i ++ ) res += g[0][i]; // 将g的第一行字符拼接成字符串
    for (int i = 3; i >= 0; i -- ) res += g[1][i]; // 将g的第二行字符逆序拼接成字符串
    return res;
}

string move0(string state)
{
    set(state);
    for (int i = 0; i < 4; i ++ ) swap(g[0][i], g[1][i]); // 交换g的第一行和第二行的字符
    return get();
}

string move1(string state)
{
    set(state);
    int v0 = g[0][3], v1 = g[1][3]; // 保存g的第一行和第二行的最后一个字符
    for (int i = 3; i > 0; i -- )
    {
        g[0][i] = g[0][i - 1]; // 将g的第一行字符向右移动一位
        g[1][i] = g[1][i - 1]; // 将g的第二行字符向右移动一位
    }
    g[0][0] = v0, g[1][0] = v1; // 将保存的字符放到g的第一行和第二行的第一个位置
    return get();
}

string move2(string state)
{
    set(state);
    int v = g[0][1]; // 保存g的第一行的第二个字符
    g[0][1] = g[1][1]; // 将g的第二行的第二个字符赋值给g的第一行的第二个字符
    g[1][1] = g[1][2]; // 将g的第二行的第三个字符赋值给g的第二行的第二个字符
    g[1][2] = g[0][2]; // 将g的第一行的第三个字符赋值给g的第二行的第三个字符
    g[0][2] = v; // 将保存的字符放到g的第一行的第三个位置
    return get();
}


int bfs(string start, string end) {
    //基本的bfs流程
    if(start==end)return 0;
    dist[start] = 0;
    q.push(start);

    while(!q.empty()) {
        auto t = q.front();q.pop();
        string m[3];
        m[0] = move0(t);
        m[1] = move1(t);
        m[2] = move2(t);

        for(int i = 0; i < 3; i++) {
            if(dist.count(m[i])==0) {
                dist[m[i]] = dist[t] + 1;
                pre[m[i]] = {char('A'+i),t};
                q.push(m[i]);
                if(m[i] == end)return dist[m[i]];//为了保证可以倒推回去，应将pre处理好后再返回
            }
        }
    }
    return -1;
}



int main() {
    string start,end;
    int x;
    for(int i = 0; i < 8; i++) {
        cin>>x;
        end += char('0'+x);
    }
    for(int i = 0; i < 8; i++) start += char(i+'1');



    int ans = bfs(start,end);
    cout<<ans<<endl;
    string res;
    while(end!=start) {
        res += pre[end].first;
        end = pre[end] .second;
    }//常用的找处理方式的步骤
    reverse(res.begin(),res.end());
    
    if(!res.empty())cout<<res;

    return 0;

}
```





#### 4.双端队列广搜

- 双端队列主要解决图中边的权值只有0或者1的最短路问题，
- 1、若拓展某一元素的边权是0，则将该元素插入到队头
  2、若拓展某一元素的边权是1，则将该元素插入到队尾

与堆优化Dijkstra 一样，必须在出队时才知道每个点最终的最小值，而和一般的bfs不一样，故每个点的st值应在**出队时置为true**

dijksta的时间复杂度是**O(Elog⁡V**)，其中E是图的边数，V是图的顶点数，而双端队列广搜的时间复杂度是**O(E)**

[电路维修](https://www.acwing.com/problem/content/177/)

#### 5.双向广搜

直接BFS会=最差可能遍历所有的状态空间，状态空间太大的时候就需要相应的技巧来优化，**一般在最小步数模型里使用**

[字串变换](https://www.acwing.com/problem/content/description/192/);';'

```c++
    #include <iostream>
    #include <cstring>
    #include <algorithm>
    #include <queue>
    #include <unordered_map>

    using namespace std;

    const int N = 6;

    int n;
    string A, B;
    string a[N], b[N];
    //需要改变的东西要用引用的方式传参
    int extend(queue<string>& q,unordered_map<string,int>&da,unordered_map<string,int>&db,string a[N],string b[N]) {
        int s = q.size();
        while(q.size()&& s) {//s用来保证每次扩展一层
            auto t = q.front(); q.pop();
            s--;
            for(int i = 0; i < n; i++) {
                for(int j = 0; j < t.size(); j++) {
                    if(t.substr(j,a[i].size())==a[i]) {//substr的用法
                        string r = t.substr(0,j)+b[i]+t.substr(j+a[i].size());
                        if(db.count(r))return db[r]+da[t]+1;
                        if(da.count(r))continue;
                        da[r] = da[t]+1;
                        q.push(r);
                    }
                }
            }
        }
        return 11;
    }


    int bfs(){
        if(A==B)return 0;
        queue<string> qa,qb;
        unordered_map<string,int> da,db;
        qa.push(A);
        qb.push(B);
        da[A] = 0;
        db[B] = 0;
        int step = 0;
        while(qa.size() && qb.size()){
            int t = 0;
            if(qa.size()<qb.size())t = extend(qa,da,db,a,b);
            else  t = extend(qb,db,da,b,a);
            if(t<=10)return t;
            if(++step==10)return -1;
        }
        return -1;
    }


    int main(){
        cin>>A>>B;
        while(cin>>a[n]>>b[n])n++;
        int t = bfs();
        if(t==-1)cout<<"NO ANSWER!";
        else cout<<t;
        return 0;
    }
```







#### 6.A * 算法

- 启发式函数好像自己想不到诶，不过大多数时候可以用已经有的，好耶ヽ(✿ﾟ▽ﾟ)ノ

算法步骤：

```c++
A* 应用场景:
起点→终点的最短距离
状态空间 >> 1e10 
启发函数减小搜索空间

A*算法:
while(q.size())
    t ← 优先队列的队头  小根堆
        当终点第一次出队时 break;
        从起点到当前点的真实距离 d_real
        从当前点到终点的估计距离 d_estimate
        选择一个估计距离最小的点 min(d_real+d_estimate)
    for j in ne[t]:
        将邻边入队

A*算法条件:
估计距离<=真实距离
//证明如下
在有解的情况下，当前出队的dist[v]若不是起点u到v的最小值,那么此时一定存在一个从起点到最优路径上的某个点x在优先队列里(如果不存在这样的一个点的话说明目前走过的所有点都在最优路径上，那我们就不会得到错误的解，这与假设不服)
有：dist[v]>dist最优=dist[x]+f[x]>=dist[x]+g[x]
故v不能出队，寻找到矛盾
dist[v] <= dist最优，显然dist[v]>=dist最优
所以 dist[v]==dist最优
     
//需要注意的是除了终点外的其他点第一次出队是并不能得到起点倒他的最小值 因为我们不能保证f[x]<f[y]时，g[x]<g[y]，所以出队时不需要判重，出来一次更新一次，最终我们能保证这条最优路径上的点的dist值就是起点到该点的最小距离
    
**终点第k次出队时，得到的就是第k短路**
```



[八数码](https://www.acwing.com/problem/content/181/)

[第K短路](https://www.acwing.com/problem/content/180/) 

### 2.DFS



- **DFS时思考明白是非需要恢复现场**

#### 1.连通性模型

- 没啥好说的，学学代码的写法

[**走迷宫**](https://www.dotcpp.com/oj/problem2177.html)
[**红与黑**](https://www.dotcpp.com/oj/problem3036.html)

#### 2.搜索顺序

[**马走日**](https://www.dotcpp.com/oj/problem3038.html) 
[**单词接龙**](https://www.dotcpp.com/oj/problem1614.html)
[**分成互质组**](https://www.dotcpp.com/oj/problem3039.html)

#### 3.剪枝

1.优化搜索顺序，优先搜索分支较少的节点
2.排除等效冗余
3.可行性剪枝
4.最优性剪枝
5.记忆化搜索（dp），此处不再赘述



[小猫爬山](https://www.acwing.com/problem/content/167/)
[数独](https://www.acwing.com/problem/content/168/)

- 此题的二进制优化技巧使用得出神入化，在此详细说明

```c++
#include<iostream>
#include<algorithm>
using namespace std;
//用一个9位的二进制数表示某一行或某一列或某一个九宫格内数字的使用情情况，从第0为到第8位，为1则代表该数字（1-9）未被使用
const int N = 9, M = 1<<N;
int ones[M]；//可能出现的所有二进制数的1的个数
int map[M];//打表，相当于log2（）
int rows[N],cols[N],cell[3][3];//用来存储相应位置的数字使用情况
char s[100];//存储九宫格

int lowbit(int x){
	return x&(-x);
}//返回该数以二进制表示的时候的第一个1所在位置， 10010 返回 10

//改变九宫格的数字，并对其各个数字的使用状态做相应的更新
void draw(int x,int y,int t, bool isSet){
	if(isSet)s[x*N+y] = '1'+t;
	else s[x*N+y] = '.';
	
	int v = 1<<t;
	if(!isSet)v=-v;
	
	rows[x] -= v;
	cols[y] -= v;
	cell[x/3][y/3] -= v;
}

//初始化各个数字的使用状态，并计算有多少空位需要填
int init(){
    int cnt = 0;
    int state = (1<<N)-1;
    fill(rows,rows+N,state);
    fill(cols,cols+N,state);
    fill(cell[0],cell[0]+N,state);
    
    for(int i=0,k=0; i < N ;i++)
        for(int j = 0;j < N;j++,k++){
            if(s[k]!='.')draw(i,j,s[k]-'1',true);
            else cnt++;
        }
    
    return cnt;
}
//返回某个位置可用的数字
int get(int x,int y){
    return rows[x]&cols[y]&cell[x/3][y/3];
}
//dfs返回值为bool，这样只要找到一个答案就能迅速返回，达到剪枝的效果
bool dfs(int cnt){
    if(!cnt)return true;
    int minV=10;
    int x,y;
    //找到可以填的数字最少的位置，即搜索顺序剪枝
    for(int i = 0; i < N ;i++)
        for(int j = 0; j <N ;j++){
            if(s[i*N+j]=='.'){
                int state = get(i,j);
                if(ones[state]<minV){
                    x= i, y =j;
                    minV = ones[state];
                }
            }
        }
    //开始dfs
    for(int i = get(x,y); i; i -= lowbit(i)){
        int t = map[lowbit(i)];
        draw(x,y,t,true);
        if(dfs(cnt-1))return true;
        draw(x,y,t,false);
    }
    return false;
}



int main(){
    ios::sync_with_stdio(false); cin.tie(0);cout.tie(0);
    for(int i = 0; i < N ; i++)map[1<<i] = i;
    
    for(int i =0; i < M; i++){
        for(int j = i; j ; j-=lowbit(j)){
            ones[i] += 1;
        }
    }
    while(cin>>s,s[0]!='e'){
        flag = false;
        int k = init();
        dfs(k);
        puts(s);
    }
    
    
	return 0;
}
```

[木棒](https://www.acwing.com/problem/content/169/)

来看一看知道剪枝方法后怎么在代码上完成

1.搜索顺序剪枝     将l从大到小排序，这样就能1先用大的木棒，从而达到优先搜索分支结点较少的点的目的
2.可行性剪枝         partSum+l[i]>lenth时continue
3.排除等效冗余      对于此题，我们应当采用组合而非排列的方式搜索
4.可行性剪枝        如果当前木棍没有搜到方案，则跳过所有长度相等的木棍
5.可行性剪枝         如果是木棒的第一根木棍就搜索失败或者木棒的最后一根木棍（+ 上它木棒长度正好是 length）搜索失败，则此次搜索必定失败

```c++
#include<iostream>
#include<algorithm>
#include<cstring>
using namespace std;

const int N = 70;
int n;
bool st[N];//每一个木棒只能使用一次
int l[N];
int sum;
int lenth;

bool cmp(int a, int b) {
    return a > b;
}


bool dfs(int g, int partSum, int start) {
    if (g * lenth == sum)return true;
    if (partSum == lenth)return dfs(g + 1, 0, 1);
    //固定一个搜索顺序，即每次拼木棍的时候我们的小木条编号的顺序都是从大到小搜的（即按照组合的方式来搜索），为了完成这个剪枝我们可以向dfs中传入一个参数表示上次搜索结束的位置
    for (int i = start; i <= n; i++) {
        if (st[i])continue;
        if (partSum + l[i] > lenth)continue;//完成剪枝2
        st[i] = true;
        if (dfs(g, partSum + l[i], start + 1))return true;
        //运行到此处说明选上l[i]的方案失败了
        st[i] = false;//恢复现场
        if (!partSum || partSum + l[i] == lenth)return false;//剪枝5
        int j = i;
        while (j <= n && l[j] == l[i])j++;//剪枝4
        i = j - 1;
    }
    return false;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    while (cin >> n,n) {
        memset(st,false,sizeof st);
        sum = 0;
        for (int i = 1; i <= n; i++) {
            cin >> l[i];
            sum += l[i];
        }
        sort(l + 1, l + n + 1, cmp);//完成剪枝1
        lenth = 1;
        while (1) {
            if (sum % lenth == 0 && dfs(0, 0, 1)) {
                cout<< lenth<<endl;
                break;
            }
            lenth++;
        }
    }
    return 0;
}

```

[生日蛋糕](https://www.acwing.com/problem/content/170/)

剪枝很难想，且没有什么普适性，在此不再详细探讨

#### 4.迭代加深

- 某些分支特别深，但是正确答案在比较浅的地方的时候使用，可以证明这样的做法不会使时间复杂度在O的意义下变高。

[加成序列](https://www.acwing.com/problem/content/172/)

#### 5.双向DFS

[送礼物](https://www.acwing.com/problem/content/173/)

#### 6.IDA*

[排书](https://www.acwing.com/problem/content/182/)';;';
[回转游戏](https://www.acwing.com/problem/content/183/);';';

## 8.再探图论



### 1.单源最短路的建图方式

[**热浪**](https://www.dotcpp.com/oj/problem3108.html)
[**信使**](https://www.dotcpp.com/oj/problem3105.html)
[**香甜的黄油**]( https://www.dotcpp.com/oj/problem3104.html)
[**最小花费**](https://www.dotcpp.com/oj/problem3103.html)
[最优乘车](https://www.acwing.com/problem/content/922/)
[**昂贵的聘礼**](http://poj.org/problem?id=1062)

### 2.单源最短路的综合应用

[**新年好**](https://www.dotcpp.com/oj/problem2409.html)
[通信线路](https://www.acwing.com/problem/content/342/)
[道路与航线](https://www.acwing.com/problem/content/344/)  难难难难‘；’；‘；’；‘
[最优贸易](https://www.acwing.com/problem/content/343/)

### 3.单源最短路的扩展应用

一些技巧的使用

1.注意虚拟原点技巧的使用

AcWing 1137

2.



### 4. floyd的应用

#### 1.求传递闭包

- 可以在O(n^3)求出一个有向图的传递闭包

```c++
void floyd() //通过floyd来逐渐更新每两个点的连通情况
{
    for(int k = 0; k < n; k ++)
       for(int i = 0; i < n; i ++)
           for(int j = 0; j < n; j ++)
            d[i][j] |= d[i][k] & d[k][j];
}
```



#### 2.求最小环

```c++
for(int k = 1 ; k <= n ; k++ )
    {
        //至少包含三个点的环所经过的点的最大编号是k
        for(int i = 1 ; i < k ; i++ )  //至少包含三个点，i，j，k不重合
            for(int j = 1 ; j < k ; j ++ ){
            if(i==j)continue;
            if(res > (LL)d[i][j] + g[i][k] + g[k][j] )
            {
                res = d[i][j] + g[i][k] + g[k][j] ;
                get_path(i , j , k) ;
            }
        }

        for(int i = 1 ; i <= n ; i++ )
            for(int j = 1 ; j <= n ; j++ )
                if(d[i][j] > d[i][k] + d[k][j])
                {
                    d[i][j] = d[i][k] + d[k][j]
                    pos[i][j] = k ; 
                }
    }
```



#### 3.恰好经过k条边的最短路

O(logk * n^3)

```c++
void mul(int c[][N],int a[][N],int b[][N])
{
    int temp[N][N];
    memset(temp,0x3f,sizeof temp);
    for(int k=1;k<=n;k++)
        for(int i=1;i<=n;i++)
            for(int j=1;j<=n;j++)
                temp[i][j]=min(temp[i][j],a[i][k]+b[k][j]);
    memcpy(c,temp,sizeof temp);
}

void qmi()
{
    memset(res,0x3f,sizeof res);
    for(int i=1;i<=n;i++) res[i][i]=0;//经过0条边
    while(k)//更新的过程
    {
        if(k&1) mul(res,res,g);//res=res*g;根据k决定是否用当前g的结果去更新res
        mul(g,g,g);//g=g*g;g的更新
        k>>=1;
    }
}
```





### 5.最小生成树

- 求次小生成树

- 先求出最小生成数，依次枚举非树边，并将该边加入到树中，然后删去原来树的一边，并且得到的新树任然是最小生成树，这样得到的树的最小权值便是次小生成树

### 6.负环

1.统计每个点入队的次数，如果某个点入队n次，则说明存在负环
2.统计每个点的最短路所包含的边数，如果其>=n，则存在负环

















# 每日一题



## 2024.10

**10.12**

[借教室](https://www.acwing.com/problem/content/505/)

此题的暴力( O(n^3) )做法不难想出,但观察数据范围可以发现应该要想出 O(nlong)的算法，我们最终选择**差分+二分**的方法解决,问题的关键在于想到**随着订单数量的增加，教室的数量是否都>=0，存在二段性**，故对订单数进行二分，优化为 **O( (n+m)logn )**

关于差分： 何时使用差分？如何使用差分？
关于二分： 我们的二分模板当mid成为l或r的时候就没有check(因为默认存在两段的，所以到了l和r就直接认为其是我们要求的那段的端点)对于不确定是否一定存在两段的情况，我们只需要最后对lcheck一次就能判断了

**10.14**

[空调](https://www.acwing.com/problem/content/4265/)

从题目中的操作来看容易想到要使用差分，但是差分的具体使用需要一定的技巧

本题本质上就是将原序列的连续一段全部增加或者减少1，求出变成目标序列的最小操作数。
我们可以先用目标序列减去原序列就能得到每一个位置具体要增加或减少多少，
**我们最终的目的也就是把这个差序列全变为0**
**我们对这个差序列求其差分序列，就等价于把差分序列全变为0**   (在此处我们应当学习到原序列与其差分序列之间的一些联系，比如把原序列变为数字都一样的序列就等价于把差分序列的一项变成某个数字，其余的项都变为0)
我们对原序列的某一段进行加减就等于对差分序列的某两项执行加减操作，故**使数组全部变为0的最少操作方案就是 差分数组中负数和与正数和的绝对值的最大值**

**10.15**

[牛的学术圈 I](https://www.acwing.com/problem/content/3748/)

**贪心大法：**
我说**贪心**，你好难想
采用贪心策略，将数组按照降序进行排序，然后当a[i]>=i 且i最大的时候，i就是我们当前数组的指数

```c++
#include<bits/stdc++.h>
using namespace std;

const int N = 1e5+10;
int a[N];
int n,l;

bool cmp(int a,int b){
    return a>b;
}

int main(){
    scanf("%d%d",&n,&l);
    for(int i = 1; i <= n; i++)scanf("%d",&a[i]);
    // for(int i = 1; i <= n; i++)cout<<a[i]<<" ";
    // cout<<endl;
    sort(a+1,a+1+n,cmp);
    int t;
    for(int i = 1; i <= n; i++){
        if(a[i]<i){
            t = i-1;
            break;
        }
    }
    // for(int i = 1; i <= n; i++)cout<<a[i]<<" ";
    if(a[t+1]==t){
        // cout<<"!";
        int cnt = 0;
        for(int i = 1; i <= t; i++)
            if(a[i]==t)cnt++;
        if(cnt<l)t += 1;
    }
    cout<<t;
    return 0;
}
```

**10.16**

[壁画](https://www.acwing.com/problem/content/564/)

第一次靠自己AC的题目呜呜呜
很多题目不知道怎么算的可以自己做一些样例，看看自己作为人是如何完成这个题目的，并思考是否能将这种思路诉诸于算法实现
反过来，这个方法也可以验证自己算法的正确性.

**10.19**

我一辈子也自己做不出来这种题目
[火柴排队](https://www.acwing.com/problem/content/description/507/)
首先要想到两者排列的元素之间大小关系一样的时候取得最小值（这个可以靠感觉猜嗷，假设二者都有序，我们可以严格证明出当且仅当二者同为升序或者同为降序时取得最小值）进而推断出只要a中第i个数与b中第i个数相对于就可以了，我们用如下的思路完成：

a[i]的值为a[i]是a[i]是a数组中第几大的数，b[i]同理(用离散化可以完成),然后令**for (int i = 1; i <= n; i ++ ) c[a[i]] = i;**（相当于重新规定大小等级）再**for(int i = 1; i <= n; i++)b[i] = c[b[i]];**(这个时候再求b的逆序对数就可以完成)**具体的原理可在模拟数据中明白**

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 100010, MOD = 99999997;

int n;
int a[N], b[N], c[N], p[N];

int find(int x)
{
    int l = 1, r = n;
    while (l < r)
    {
        int mid = l + r >> 1;
        if (p[mid] >= x) r = mid;
        else l = mid + 1;
    }
    return r;
}

void work(int a[])
{
    for (int i = 1; i <= n; i ++ ) p[i] = a[i];
    sort(p + 1, p + n + 1);
    for(int i = 1; i <= n; i++)a[i] = find(a[i]);
}

int merge_sort(int l, int r)
{
    if (l >= r) return 0;
    int mid = l + r >> 1;
    int res = (merge_sort(l, mid) + merge_sort(mid + 1, r)) % MOD;
    int i = l, j = mid + 1, k = 0;
    while (i <= mid && j <= r)
        if (b[i] <= b[j]) p[k ++ ] = b[i ++ ];
        else p[k ++ ] = b[j ++ ], res = (res + mid - i + 1) % MOD;
    while (i <= mid) p[k ++ ] = b[i ++ ];
    while (j <= r) p[k ++ ] = b[j ++ ];
    for (i = l, j = 0; i <= r; i ++, j ++ ) b[i] = p[j];
    return res;
}

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &b[i]);

    work(a), work(b);
    for (int i = 1; i <= n; i ++ ) c[a[i]] = i;
    for(int i = 1; i <= n; i++)b[i] = c[b[i]];

    printf("%d\n", merge_sort(1, n));
    return 0;
}

```

**10.21**

多路归并算法：把多个有序数组合并为一个有序数组
[鱼塘钓鱼](https://www.acwing.com/problem/content/1264/)

[技能升级](https://www.acwing.com/problem/content/4659/) 
想出了一个能过70%数据点的算法，O(mlogn),可优化到nlogn(好难想。。。。),即二分最后一次升级技能所得到的攻击力

[冶炼金属](https://www.acwing.com/problem/content/4959/)过咯，需要尤其注意数据范围，判断会不会爆int



## 蓝桥杯刷题

### 1.递推与递归

[递归实现指数型枚举](https://www.acwing.com/problem/content/description/94/)

[递归实现组合型枚举](https://www.acwing.com/problem/content/95/)

[递归实现排列型枚举](https://www.acwing.com/problem/content/96/)

[带分数](https://www.acwing.com/problem/content/description/1211/)

[费解的开关](acwing.com/problem/content/description/97/)
曾经看来如此费解的一道题目现在也可以轻松解决了，还是有进步的嘛。

[飞行员兄弟](https://www.acwing.com/problem/content/118/)
考虑到状态有限就直接暴力枚举,首先想到在最优解中，一个灯泡一定只按一次且按的先后顺序无关，直接枚举所有方案，找出合法方案中按的灯泡最少的那个

### 2.二分

[四平方和](https://www.acwing.com/problem/content/description/1223/)

根据数据范围可以知道我们能接受的时间复杂度是在O(nlongn)以下的级别，暴力做法有三重循环，这是我们不能接受的，所以想到进行两重循环预处理出两个数的平方和，之后再用两重循环枚举另外两个数的平方和

### 3.前缀和与差分

[激光炸弹](https://www.acwing.com/problem/content/description/101/)

一道二维前缀和的题目，二维前缀和计算时画画图就能推得相关公式

[K倍区间](https://www.acwing.com/problem/content/description/1232/)

需要用数学知识进一步优化，简而言之 (a-b)%k==0  equals  a%k==b%k

### 4.数学知识与简单dp

[买不到的数目](https://www.acwing.com/problem/content/1207/)

考查数论知识，**如果a,b是正整数且互质，那么由ax+by不能表示的最大整数是(a-1)(b-1)-1**
这种题可以暴力做一下找规律或者骗分

[地宫取宝](https://www.acwing.com/problem/content/description/1214/)

这种线性dp终于做着算是有一点点感觉了，首先要想到可以用dp做，其次就是设计好状态表示，这个就是缺啥就加一维描述啥，转移方程一般也没有特别难想，初始状态很多时候不好设置，可以对物品的某些属性做个偏移(让他不为0，方便我们将0设置为初始状态)。可以考虑将第一次或者"第0次"设置为初始状态

### 5.枚举、模拟

[连号区间数](https://www.acwing.com/problem/content/1212/)

这种简单的模拟题就要多去寻找其中暗含的小规律

[递增三元组](https://www.acwing.com/problem/content/1238/)

很多时候都是先想到一个纯暴力的做法，然后再想想能不能优化，此题可用二分，双指针，哈希+前缀和

[特别数的和](https://www.acwing.com/problem/content/1247/)

比较简单，没啥好说的

[错误票据](https://www.acwing.com/problem/content/1206/)

刚好是一道考察我们不擅入的那种读入方式的题目，详情见应试技巧里的读入部分

[回文日期](https://www.acwing.com/problem/content/468/)

一道经典的日期题，注意闰年的定义，注意字符串与int之间转换时前导0的影响；
注意枚举两日期之间的日子的写法

[移动距离](https://www.acwing.com/problem/content/1221/)

比较简单，没啥好说的（蓝桥杯只给一个测试样例好可恶，下次记得自己写个小点儿的试一下，不然很容易出现错误）

[日期问题](https://www.acwing.com/problem/content/1231/)

超级大⑩山代码来啦！

[航班时间](https://www.acwing.com/problem/content/1233/)

输入输出流的使用方法？

[外卖店优先级](https://www.acwing.com/problem/content/1243/)

过了八个点，应该是存在细微的逻辑漏洞

### 6.双指针

[日志统计](https://www.acwing.com/problem/content/1240/)

双指针具体的内容参见基础算法里的双指针

### 7.BFS 、DFS

[大臣的旅费](https://www.acwing.com/problem/content/description/1209/)

本质上是求树的直径，该问题有O(n)的解法

**任取一点x**
**找到距离x最远的点y**
**从y开始遍历，找到离y最远的点，与y最远的点的距离是树的直径**

证明略



### 8.图论



### 9.贪心

[股票买卖 II](https://www.acwing.com/problem/content/1057/)

可用dp的状态机模型或者贪心来解决

[糖果传递](https://www.acwing.com/problem/content/124/)

使用绝对值不等式解决，我能想到就怪了

[雷达设备](https://www.acwing.com/problem/content/114/)

一道曾经无比困难的题现在自己就可以写出来啦

[乘积最大](https://www.acwing.com/problem/content/1241/)

需要考虑到一些比较特殊的情况 。。。
另外值得一提的是：

在c++ 中 负数的mod仍然是负数   -1%10 = -1， -5%3=-2， -5%-3=-1
另外：**(a*b)%mod = a % mod * b % mod**(取模的运算优先级和乘法一样，所以就是从左到右依次运算)

[后缀表达式](https://www.acwing.com/problem/content/1249/)

好难想的贪心，关键是要想清负号到底能有多少个
