# AtCoder Daily Training ALL 2026/04/14 16:00start
コンテスト中に解ききれなかった問題（F, H）を復習し、特に「木構造におけるパスの状態管理」について重要な知見を得たので、ここにまとめます。  
A-E問題については基本的な内容だったため、解説は省略させていただきます。  
## A - Five Integers
### https://atcoder.jp/contests/adt_all_20260414_1/tasks/abc268_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    unordered_set<int> num;
    int ans=0;
    for(int i=0;i<5;i++)
    {
        int temp;
        cin>>temp;
        if(num.count(temp)!=0)
        continue;
        else
        {
            ans++;
            num.insert(temp);
        }
    }
    cout<<ans<<"\n";
    return 0;
}
```
## B - Is it rated?
### https://atcoder.jp/contests/adt_all_20260414_1/tasks/abc405_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int r,x;
    cin>>r>>x;
    if(1600<=r&&r<=2999&&x==1)
    {
        cout<<"Yes"<<"\n";
        return 0;
    }
    else if(1200<=r&&r<=2399&&x==2)
    {
        cout<<"Yes"<<"\n";
        return 0;
    }
    else
    cout<<"No"<<"\n";
    return 0;
}
```
## C - Locked Rooms
### https://atcoder.jp/contests/adt_all_20260414_1/tasks/abc423_b
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<int> door(n);
    vector<bool> visited(n+1,false);
    
    for(int i=0;i<n;i++)
    {
        cin>>door[i];
    }
    for(int i=0;i<n;i++)
    {
        if(door[i]==1)break;
        else
        visited[i+1]=true;
    }
    for(int i=n-1;i>=0;i--)
    {
        if(door[i]==1)break;
        else
        visited[i]=true;
    }
    visited[0]=visited[n]=true;
    int ans=0;
    for(int i=0;i<n+1;i++)
    {
        if(!visited[i])ans++;
    }
    cout<<ans<<"\n";
    return 0;
}
```
## D - AtCoder Quiz
### https://atcoder.jp/contests/adt_all_20260414_1/tasks/abc217_b
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    bool check[4]={false};
    for(int i=0;i<3;i++)
    {
        string str;
        cin>>str;
        if(str=="ABC")check[0]=true;
        if(str=="ARC")check[1]=true;
        if(str=="AGC")check[2]=true;
        if(str=="AHC")check[3]=true;
    }
    if(!check[0])cout<<"ABC"<<"\n";
    else if(!check[1])cout<<"ARC"<<"\n";
    else if(!check[2])cout<<"AGC"<<"\n";
    else if(!check[3])cout<<"AHC"<<"\n";
    return 0;
}
```
## E - Σ
### https://atcoder.jp/contests/adt_all_20260414_1/tasks/abc346_c
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    ll n,k;
    cin>>n>>k;
    ll ans=(1+k)*k/2;
    ll sum=0;
    unordered_set<ll> num;
    for(int i=0;i<n;i++)
    {
        ll temp;
        cin>>temp;
        if(1<=temp&&temp<=k)
        {
            num.insert(temp);
        }
    }
    for(ll i:num)
    sum+=i;
    cout<<ans-sum<<"\n";
    return 0;
}
```
## F - 1122 Substring 2
### https://atcoder.jp/contests/adt_all_20260414_1/tasks/abc433_c
文字列 $S$ をそのまま扱うのではなく、**「どの数字が何回連続しているか」**という情報のペア（groups）に変換します。  
ステップ1：  
圧縮処理例えば $S = 111222233$ の場合：  
groups = {(1, 3), (2, 4), (3, 2)}  
（数字 1 が 3 回、数字 2 が 4 回...）という形式にまとめます。  
ステップ2：  
隣接するブロックの判定圧縮された groups を走査し、隣り合う 2 つのブロック  
$(val_1, len_1)$ と $(val_2, len_2)$   
が以下の条件を満たすか確認します。$val_2 = val_1 + 1$この条件を満たすとき、作れる 1122 文字列の数は $\min(len_1, len_2)$ 個となります。  
例：111 与 2222 の場合、12, 1122, 111222 の 3 つが条件を満たします。  
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string s;
    cin>>s;
    int n=s.size();
    vector<pair<int,int>> groups;
    if(n>0)
    {
        int cnt=1;
        for(int i=1;i<=n;i++)
        {
            if(i<n&&s[i]==s[i-1])
            {
                cnt++;
            }
            else
            {
                groups.push_back({s[i-1]-'0',cnt});
                cnt = 1;
            }
        }
    }
    ll ans=0;
    for (int i=0;i<(int)groups.size()-1;i++)
    {
        int val1=groups[i].first;
        int len1=groups[i].second;
        int val2=groups[i+1].first;
        int len2=groups[i+1].second;
        if(val2==val1+1)
        {
            ans+=min(len1,len2);
        }
    }
    cout<<ans<<"\n";
    return 0;
}
```
## G - Integer-duplicated Path
### https://atcoder.jp/contests/adt_all_20260414_1/tasks/abc448_d
本問の核心は、DFS（深さ優先探索）を用いて木を走査しながら、根から現在地までのパス上の状態を動的に管理する点にあります。  
状態の更新（行きがけ）：  
DFSで頂点 $u$ に入る際、その頂点にある整数 point[u] の出現回数を cnt (Map) に加算します。出現回数が 2 になった瞬間、パス内で重複が発生したとみなし、グローバルな重複カウンター has_dup をインクリメントします。  
回答の記録：  
has_dup > 0 であれば、その頂点までのパスに重複が含まれているため Yes、そうでなければ No と記録します。木構造の特性上、DFSの再帰スタックがそのまま根からの唯一のパスを表すため、この判定が可能です。  
バックトラッキング（帰りがけ）：  
探索を終えて親ノードに戻る際、**現在の頂点がパスに与えた影響を取り消す（バックトラック）**必要があります。cnt[point[u]] をデクリメントし、もし値が从 2 に戻るタイミングであれば、has_dup もデクリメントして状態を復元します。  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

vector<vector<int>> adj;
vector<int> point;
vector<string> ans;
map<int,int> cnt;
int has_dup=0;

void dfs(int u,int p)
{
    if(++cnt[point[u]]==2)
    has_dup++;
    
    if(has_dup>0)
    ans[u]="Yes";
    else
    ans[u]="No";

    for(int v:adj[u])
    {
        if(v==p)
        continue;
        dfs(v,u);
    }
    if(cnt[point[u]]--==2)
    has_dup--;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    
    point.resize(n+1);
    for(int i=1;i<=n;i++)
    cin>>point[i];

    adj.assign(n+1,vector<int>());
    ans.resize(n+1);
    for(int i=0;i<n-1;i++)
    {
        int u,v;
        cin>>u>>v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    dfs(1,0);
    for(int i=1;i<=n;i++)
    {
        cout<<ans[i]<<"\n";
    }
    return 0;
}
```
## H - Sightseeing Tour
### https://atcoder.jp/contests/adt_all_20260414_1/tasks/abc369_e
この問題は、**全源最短路（Floyd-Warshall）と再帰による全探索（DFS）**を組み合わせることで解決できます。$K$ が最大 $5$ と非常に小さいことがポイントです。  
最短路の事前計算：  
まず、すべての島ペア間の最短距離を Floyd-Warshall アルゴリズム で求めます。制約が $N \le 400$ なので、$O(N^3)$ で十分に間に合います。  
DFSによる順列と向きの全探索：  
クエリで与えられた $K$ 本の橋を「どの順番で渡るか」と「どちらの向き（$U \to V$ か $V \to U$ か）で渡るか」をDFSで探索します。  
順列：$K! = 5! = 120$ 通り  
向き：$2^K = 2^5 = 32$ 通り  
$120 \times 32 = 3840$ 通りとなり、クエリ数 $Q=3000$ を掛けても制限時間内に収まります。  
遷移の計算：  
現在地を u とし、次に渡るべき橋を $i$（端点は $b\_u, b\_v$、重さは $b\_t$）とすると、以下の2パターンを再帰的に試します：  
dist[u][b_u] + b_t + dfs(b_v, ...) （$b\_u$ から入って $b\_v$ から出る）  
dist[u][b_v] + b_t + dfs(b_u, ...) （$b\_v$ から入って $b\_u$ から出る）  
すべての指定された橋を渡り終えたら、最後に現在地から島 $N$ までの最短距離を足して終了します。  
```cpp
#include <bits/stdc++.h>
using namespace std;

using ll=long long;
const ll INF=1e18;
int N,M,Q,K;
ll dist[405][405];
struct Bridge {
    int u,v;
    ll t;
};
vector<Bridge> all_bridges;
vector<int> target_indices;
bool used[10];
ll dfs(int u,int count)
{
    if(count==K)
    {
        return dist[u][N];
    }
    ll res=INF;
    for(int i=0;i<K;i++)
    {
        if(!used[i])
        {
            used[i]=true;
            int b_idx=target_indices[i];
            int b_u=all_bridges[b_idx].u;
            int b_v=all_bridges[b_idx].v;
            ll b_t=all_bridges[b_idx].t;

            ll res1=dist[u][b_u]+b_t+dfs(b_v,count+1);
            if(res1<res)
            res=res1;

            ll res2=dist[u][b_v]+b_t+dfs(b_u,count+1);
            if(res2<res)
            res=res2;

            used[i]=false;
        }
    }
    return res;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    cin>>N>>M;

    for(int i=1;i<=N;i++)
    {
        for(int j=1;j<=N;j++)
        {
            if(i==j)
            dist[i][j]=0;
            else
            dist[i][j]=INF;
        }
    }

    for(int i=0;i<M;i++)
    {
        int u,v;
        ll t;
        cin>>u>>v>>t;
        all_bridges.push_back({u,v,t});
        if(t<dist[u][v])
        {
            dist[u][v]=dist[v][u]=t;
        }
    }

    for(int k=1;k<=N;k++)
    {
        for(int i=1;i<=N;i++)
        {
            if(dist[i][k]==INF)
            continue;
            for(int j=1;j<=N;j++)
            {
                if(dist[k][j]<INF)
                {
                    dist[i][j]=min(dist[i][j],dist[i][k]+dist[k][j]);
                }
            }
        }
    }

    cin>>Q;
    while(Q--)
    {
        cin>>K;
        target_indices.resize(K);
        for(int i=0;i<K;i++)
        {
            cin>>target_indices[i];
            target_indices[i]--;
        }

        memset(used,false,sizeof(used));
        cout<<dfs(1,0)<<"\n";
    }

    return 0;
}
```
