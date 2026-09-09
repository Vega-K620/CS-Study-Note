# AtCoder Beginner Contest 474
長い間にメモを書きませんでした。
夏休みにたくさんのものを勉強しました。
codeforcesも1600+のratingに着きました。
久しぶりのAtcoderのABCコンテストは前より簡単になりました。
それは練習の成果だと思います。
今日から前に参加したコンテストのメモを一つ一つで書きます。
## A - Not X
### https://atcoder.jp/contests/abc474/tasks/abc474_a
これは簡単のチェックイン問題です。
```cpp
#include<bits/stdc++.h>
using namespace std;

#define int long long
#define pb push_back
#define all(x) x.begin(), x.end()
#define sz(x) ((int)(x).size())

using ll=long long;
using pii=pair<int, int>;
using vi=vector<int>;

// g++ -O2 test.cpp -o ~/program/bin/index -DLOCAL && ~/program/bin/index
#ifdef LOCAL
    void _print(long long t) { cerr << t; }
    void _print(string t) { cerr << '"' << t << '"'; }
    void _print(char t) { cerr << "'" << t << "'"; }
    template <typename T, typename V>
    void _print(pair<T, V> p) { cerr << "(" << p.first << ", " << p.second << ")"; }
    template <typename T>
    void _print(vector<T> v) {
        cerr << "{ ";
        for (auto a : v) {
            _print(a);
            cerr << " ";
        }
        cerr << "}";
    }
    #define debug(x) cerr << #x <<" = "; _print(x); cerr << endl;
    #define debug2(x, y) cerr << #x <<"=" << x <<" | "#y <<"=" << y << endl;
#else
    #define debug(x)
    #define debug2(x, y)
#endif

void solve()
{
    int n;
    cin>>n;
    for(int i=1;i<=3;i++)
    {
        if(i!=n)
        {
            cout<<i<<"\n";
            return;
        }
    }
}

signed main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int t=1;
    cin>>t;
    while(t--)
    {
        solve();
    }
}
/*
 1. 基础数组与序列 (Arrays & Sequences)
    int n, m, k;                // 数据规模：长度 / 询问数 / 限制条件
    int a[N], b[N];             // 原始序列 / 辅助数组
    int raw[N];                 // 去重/离散化后的原值 (raw value)
    int val[N];                 // 映射/离散化后的编号 (value index)
    vector<int> vec;            // 动态数组

 2. 区间、前缀和与差分 (Ranges, Prefix Sums & Difference)
    int l, r;                   // 局部查询区间左右端点 (left, right)
    int L[B], R[B];             // 静态/分块区间左右边界
    int pref[N], suf[N];        // 前缀和 (prefix sum) / 后缀和 (suffix sum)
    int diff[N];                // 差分数组 (difference)
    int mid;                    // 二分/线段树中点 (middle)

 3. 统计、映射与标记 (Statistics, Mapping & Tags)
    int cnt[N], freq[N];        // 计数/频次数组 (count / frequency)
    vector<int> pos[N];         // 某数值出现的下标列表 (positions)
    int vis[N];                 // 访问标记数组 (visited)
    int tag[N], lazy[N];        // 线段树延迟标记 (lazy tag)

 4. 算法专题命名 (Specialized Algorithms)
    [分块 / 莫队 (Block Decomposition)]
    int B;                      // 块大小 (Block size, 如 B = sqrt(n))
    int num_blocks;             // 总块数
    int belong[N];              // 元素所属块编号 (belong[i])
    int last / last_ans;        // 强制在线加密的上一轮答案

    [图论 / 树 (Graph & Tree)]
    vector<int> adj[N];         // 邻接表 (adjacency list)
    int u, v, w;                // 边的起点 / 终点 / 边权
    int dist[N];                // 最短距离 (distance)
    int dep[N];                 // 节点深度 (depth)
    int fa[N], p[N];            // 父节点 / 并查集祖先 (father / parent)
    int deg[N], in_deg[N];      // 节点度数 / 入度 (degree / in-degree)

    [动态规划 (Dynamic Programming)]
    int dp[N][N], f[N], g[N];   // DP 状态定义数组
    int memo[N];                // 记忆化搜索缓存 (memorization)
    int opt[N];                 // 最优决策点 (optimal decision)

 5. 局部与临时变量 (Locals & Temps)
    int cand;                   // 候选值 (candidate，如挑选众数/极值时)
    int tmp / temp;             // 临时过渡变量
    int cur, nxt;               // 当前状态 / 下一状态 (current / next)
    int ans, res;               // 最终答案 / 结果 (answer / result)

 6. 全局变量避坑禁忌 (与 std 库冲突易导致 CE)
    禁用变量名: y1, j1, next, rank, index, left, right, hash, time, free
*/
```
## B - Exit Order
### https://atcoder.jp/contests/abc474/tasks/abc474_b
毎10個の数字は同じグループに入って、数字をチェックして、 $ ((i-1)/10)*10>=p[i]||(((i-1)/10)+1)*10<p[i] $ を使って速い判断できます。
```cpp
#include<bits/stdc++.h>
using namespace std;

#define int long long
#define pb push_back
#define all(x) x.begin(), x.end()
#define sz(x) ((int)(x).size())

using ll=long long;
using pii=pair<int, int>;
using vi=vector<int>;

// g++ -O2 test.cpp -o ~/program/bin/index -DLOCAL && ~/program/bin/index
#ifdef LOCAL
    void _print(long long t) { cerr << t; }
    void _print(string t) { cerr << '"' << t << '"'; }
    void _print(char t) { cerr << "'" << t << "'"; }
    template <typename T, typename V>
    void _print(pair<T, V> p) { cerr << "(" << p.first << ", " << p.second << ")"; }
    template <typename T>
    void _print(vector<T> v) {
        cerr << "{ ";
        for (auto a : v) {
            _print(a);
            cerr << " ";
        }
        cerr << "}";
    }
    #define debug(x) cerr << #x <<" = "; _print(x); cerr << endl;
    #define debug2(x, y) cerr << #x <<"=" << x <<" | "#y <<"=" << y << endl;
#else
    #define debug(x)
    #define debug2(x, y)
#endif

void solve()
{
    int n;
    cin>>n;
    vector<int> p(n+1);
    bool flag=true;
    for(int i=1;i<=n;i++)
    {
        cin>>p[i];
        if(((i-1)/10)*10>=p[i]||(((i-1)/10)+1)*10<p[i])
        {
            flag=false;
        }
    }
    cout<<(flag?"Yes":"No")<<"\n";
}

signed main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int t=1;
    cin>>t;
    while(t--)
    {
        solve();
    }
}
/*
 1. 基础数组与序列 (Arrays & Sequences)
    int n, m, k;                // 数据规模：长度 / 询问数 / 限制条件
    int a[N], b[N];             // 原始序列 / 辅助数组
    int raw[N];                 // 去重/离散化后的原值 (raw value)
    int val[N];                 // 映射/离散化后的编号 (value index)
    vector<int> vec;            // 动态数组

 2. 区间、前缀和与差分 (Ranges, Prefix Sums & Difference)
    int l, r;                   // 局部查询区间左右端点 (left, right)
    int L[B], R[B];             // 静态/分块区间左右边界
    int pref[N], suf[N];        // 前缀和 (prefix sum) / 后缀和 (suffix sum)
    int diff[N];                // 差分数组 (difference)
    int mid;                    // 二分/线段树中点 (middle)

 3. 统计、映射与标记 (Statistics, Mapping & Tags)
    int cnt[N], freq[N];        // 计数/频次数组 (count / frequency)
    vector<int> pos[N];         // 某数值出现的下标列表 (positions)
    int vis[N];                 // 访问标记数组 (visited)
    int tag[N], lazy[N];        // 线段树延迟标记 (lazy tag)

 4. 算法专题命名 (Specialized Algorithms)
    [分块 / 莫队 (Block Decomposition)]
    int B;                      // 块大小 (Block size, 如 B = sqrt(n))
    int num_blocks;             // 总块数
    int belong[N];              // 元素所属块编号 (belong[i])
    int last / last_ans;        // 强制在线加密的上一轮答案

    [图论 / 树 (Graph & Tree)]
    vector<int> adj[N];         // 邻接表 (adjacency list)
    int u, v, w;                // 边的起点 / 终点 / 边权
    int dist[N];                // 最短距离 (distance)
    int dep[N];                 // 节点深度 (depth)
    int fa[N], p[N];            // 父节点 / 并查集祖先 (father / parent)
    int deg[N], in_deg[N];      // 节点度数 / 入度 (degree / in-degree)

    [动态规划 (Dynamic Programming)]
    int dp[N][N], f[N], g[N];   // DP 状态定义数组
    int memo[N];                // 记忆化搜索缓存 (memorization)
    int opt[N];                 // 最优决策点 (optimal decision)

 5. 局部与临时变量 (Locals & Temps)
    int cand;                   // 候选值 (candidate，如挑选众数/极值时)
    int tmp / temp;             // 临时过渡变量
    int cur, nxt;               // 当前状态 / 下一状态 (current / next)
    int ans, res;               // 最终答案 / 结果 (answer / result)

 6. 全局变量避坑禁忌 (与 std 库冲突易导致 CE)
    禁用变量名: y1, j1, next, rank, index, left, right, hash, time, free
*/
```
## C - Remove and Append
### https://atcoder.jp/contests/abc474/tasks/abc474_c
最初僕は数字を毎個消して最後に入って、それはもちろん「TLE」しました。
その後ちゃんと考えて、ただ数字を最後に入って、そして後ろから走査して、初めて出て来た数字をメモして、それは答えです。
```cpp
#include<bits/stdc++.h>
using namespace std;

#define int long long
#define pb push_back
#define all(x) x.begin(), x.end()
#define sz(x) ((int)(x).size())

using ll=long long;
using pii=pair<int, int>;
using vi=vector<int>;

// g++ -O2 test.cpp -o ~/program/bin/index -DLOCAL && ~/program/bin/index
#ifdef LOCAL
    void _print(long long t) { cerr << t; }
    void _print(string t) { cerr << '"' << t << '"'; }
    void _print(char t) { cerr << "'" << t << "'"; }
    template <typename T, typename V>
    void _print(pair<T, V> p) { cerr << "(" << p.first << ", " << p.second << ")"; }
    template <typename T>
    void _print(vector<T> v) {
        cerr << "{ ";
        for (auto a : v) {
            _print(a);
            cerr << " ";
        }
        cerr << "}";
    }
    #define debug(x) cerr << #x <<" = "; _print(x); cerr << endl;
    #define debug2(x, y) cerr << #x <<"=" << x <<" | "#y <<"=" << y << endl;
#else
    #define debug(x)
    #define debug2(x, y)
#endif

void solve()
{
    int n,q;
    cin>>n>>q;
    vector<int> p(n+1);
    for(int i=1;i<=n;i++)
    {
        cin>>p[i];
    }
    for(int i=0;i<q;i++)
    {
        int temp;
        cin>>temp;
        p.push_back(temp);
    }
    unordered_map<int,int> cnt;
    vector<int> ans;
    for(int i=p.size()-1;i>0;i--)
    {
        if(cnt[p[i]]==0)
        {
            ans.push_back(p[i]);
            cnt[p[i]]++;
        }
    }
    for(int i=ans.size()-1;i>=0;i--)
    {
        cout<<ans[i]<<" ";
    }
    cout<<"\n";
}

signed main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int t=1;
    cin>>t;
    while(t--)
    {
        solve();
    }
}
/*
 1. 基础数组与序列 (Arrays & Sequences)
    int n, m, k;                // 数据规模：长度 / 询问数 / 限制条件
    int a[N], b[N];             // 原始序列 / 辅助数组
    int raw[N];                 // 去重/离散化后的原值 (raw value)
    int val[N];                 // 映射/离散化后的编号 (value index)
    vector<int> vec;            // 动态数组

 2. 区间、前缀和与差分 (Ranges, Prefix Sums & Difference)
    int l, r;                   // 局部查询区间左右端点 (left, right)
    int L[B], R[B];             // 静态/分块区间左右边界
    int pref[N], suf[N];        // 前缀和 (prefix sum) / 后缀和 (suffix sum)
    int diff[N];                // 差分数组 (difference)
    int mid;                    // 二分/线段树中点 (middle)

 3. 统计、映射与标记 (Statistics, Mapping & Tags)
    int cnt[N], freq[N];        // 计数/频次数组 (count / frequency)
    vector<int> pos[N];         // 某数值出现的下标列表 (positions)
    int vis[N];                 // 访问标记数组 (visited)
    int tag[N], lazy[N];        // 线段树延迟标记 (lazy tag)

 4. 算法专题命名 (Specialized Algorithms)
    [分块 / 莫队 (Block Decomposition)]
    int B;                      // 块大小 (Block size, 如 B = sqrt(n))
    int num_blocks;             // 总块数
    int belong[N];              // 元素所属块编号 (belong[i])
    int last / last_ans;        // 强制在线加密的上一轮答案

    [图论 / 树 (Graph & Tree)]
    vector<int> adj[N];         // 邻接表 (adjacency list)
    int u, v, w;                // 边的起点 / 终点 / 边权
    int dist[N];                // 最短距离 (distance)
    int dep[N];                 // 节点深度 (depth)
    int fa[N], p[N];            // 父节点 / 并查集祖先 (father / parent)
    int deg[N], in_deg[N];      // 节点度数 / 入度 (degree / in-degree)

    [动态规划 (Dynamic Programming)]
    int dp[N][N], f[N], g[N];   // DP 状态定义数组
    int memo[N];                // 记忆化搜索缓存 (memorization)
    int opt[N];                 // 最优决策点 (optimal decision)

 5. 局部与临时变量 (Locals & Temps)
    int cand;                   // 候选值 (candidate，如挑选众数/极值时)
    int tmp / temp;             // 临时过渡变量
    int cur, nxt;               // 当前状态 / 下一状态 (current / next)
    int ans, res;               // 最终答案 / 结果 (answer / result)

 6. 全局变量避坑禁忌 (与 std 库冲突易导致 CE)
    禁用变量名: y1, j1, next, rank, index, left, right, hash, time, free
*/
```
## D - Outweigh
### https://atcoder.jp/contests/abc474/tasks/abc474_d
これは構成する問題です。
この問題の要点は高橋君は勝ちのとき直接1e18を使って、じゃないと1を使って。
それ高橋君は最後に一番勝ちやすいです。
```cpp
#include<bits/stdc++.h>
using namespace std;

#define int long long
#define pb push_back
#define all(x) x.begin(), x.end()
#define sz(x) ((int)(x).size())

using ll=long long;
using pii=pair<int, int>;
using vi=vector<int>;

// g++ -O2 test.cpp -o ~/program/bin/index -DLOCAL && ~/program/bin/index
#ifdef LOCAL
    void _print(long long t) { cerr << t; }
    void _print(string t) { cerr << '"' << t << '"'; }
    void _print(char t) { cerr << "'" << t << "'"; }
    template <typename T, typename V>
    void _print(pair<T, V> p) { cerr << "(" << p.first << ", " << p.second << ")"; }
    template <typename T>
    void _print(vector<T> v) {
        cerr << "{ ";
        for (auto a : v) {
            _print(a);
            cerr << " ";
        }
        cerr << "}";
    }
    #define debug(x) cerr << #x <<" = "; _print(x); cerr << endl;
    #define debug2(x, y) cerr << #x <<"=" << x <<" | "#y <<"=" << y << endl;
#else
    #define debug(x)
    #define debug2(x, y)
#endif

void solve()
{
    int n;
    cin>>n;
    vector<int> a(n+1),b(n+1);
    for(int i=1;i<=n;i++)
    {
        cin>>a[i];
    }
    for(int i=1;i<=n;i++)
    {
        cin>>b[i];
    }
    int add=0,out=0;
    for(int i=1;i<=n;i++)
    {
        if(a[i]>b[i])
        {
            add+=a[i]-b[i];
        }
        else
        {
            out+=b[i]-a[i];
        }
    }
    if(add*1e18>out)
    {
        cout<<"Yes"<<"\n";
        for(int i=1;i<=n;i++)
        {
            if(a[i]>b[i])
            {
                cout<<1000000000000000000<<" ";
            }
            else
            {
                cout<<1<<" ";
            }
        }
    }
    else
    {
        cout<<"No"<<"\n";
    }
}

signed main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int t=1;
    cin>>t;
    while(t--)
    {
        solve();
    }
}
/*
 1. 基础数组与序列 (Arrays & Sequences)
    int n, m, k;                // 数据规模：长度 / 询问数 / 限制条件
    int a[N], b[N];             // 原始序列 / 辅助数组
    int raw[N];                 // 去重/离散化后的原值 (raw value)
    int val[N];                 // 映射/离散化后的编号 (value index)
    vector<int> vec;            // 动态数组

 2. 区间、前缀和与差分 (Ranges, Prefix Sums & Difference)
    int l, r;                   // 局部查询区间左右端点 (left, right)
    int L[B], R[B];             // 静态/分块区间左右边界
    int pref[N], suf[N];        // 前缀和 (prefix sum) / 后缀和 (suffix sum)
    int diff[N];                // 差分数组 (difference)
    int mid;                    // 二分/线段树中点 (middle)

 3. 统计、映射与标记 (Statistics, Mapping & Tags)
    int cnt[N], freq[N];        // 计数/频次数组 (count / frequency)
    vector<int> pos[N];         // 某数值出现的下标列表 (positions)
    int vis[N];                 // 访问标记数组 (visited)
    int tag[N], lazy[N];        // 线段树延迟标记 (lazy tag)

 4. 算法专题命名 (Specialized Algorithms)
    [分块 / 莫队 (Block Decomposition)]
    int B;                      // 块大小 (Block size, 如 B = sqrt(n))
    int num_blocks;             // 总块数
    int belong[N];              // 元素所属块编号 (belong[i])
    int last / last_ans;        // 强制在线加密的上一轮答案

    [图论 / 树 (Graph & Tree)]
    vector<int> adj[N];         // 邻接表 (adjacency list)
    int u, v, w;                // 边的起点 / 终点 / 边权
    int dist[N];                // 最短距离 (distance)
    int dep[N];                 // 节点深度 (depth)
    int fa[N], p[N];            // 父节点 / 并查集祖先 (father / parent)
    int deg[N], in_deg[N];      // 节点度数 / 入度 (degree / in-degree)

    [动态规划 (Dynamic Programming)]
    int dp[N][N], f[N], g[N];   // DP 状态定义数组
    int memo[N];                // 记忆化搜索缓存 (memorization)
    int opt[N];                 // 最优决策点 (optimal decision)

 5. 局部与临时变量 (Locals & Temps)
    int cand;                   // 候选值 (candidate，如挑选众数/极值时)
    int tmp / temp;             // 临时过渡变量
    int cur, nxt;               // 当前状态 / 下一状态 (current / next)
    int ans, res;               // 最终答案 / 结果 (answer / result)

 6. 全局变量避坑禁忌 (与 std 库冲突易导致 CE)
    禁用变量名: y1, j1, next, rank, index, left, right, hash, time, free
*/
```
