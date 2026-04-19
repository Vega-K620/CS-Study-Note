# キーサイト・テクノロジープログラミングコンテスト（AtCoder Beginner Contest 454）
今日のコンテストはA~Dの4題を解くことができました。ABはかなり易しい内容だったので、ここでは詳しい解説は省きます。
## A - Closed interval
### https://atcoder.jp/contests/abc454/tasks/abc454_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int a,b;
    cin>>a>>b;
    cout<<b-a+1<<"\n";
    return 0;
}
```
## B - Mapping
### https://atcoder.jp/contests/abc454/tasks/abc454_b
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    int num[105]={0};
    bool used[105]={false};
    for(int i=0;i<n;i++)
    {
        int temp;
        cin>>temp;
        num[temp-1]++;
        used[temp-1]=true;
    }
    bool q1=true,q2=true;
    for(int i=0;i<m;i++)
    {
        if(num[i]>1)
        q1=false;
        if(used[i]==false)
        q2=false;
    }
    if(q1)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
    if(q2)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
    return 0;
}
```
## C - Straw Millionaire
### https://atcoder.jp/contests/abc454/tasks/abc454_c
この問題は、アイテム間の交換関係を有向グラフとして捉えることで解決できます。  
高橋君は最初アイテム1を持っており、交換を通じて辿り着けるノードの総数を求める問題です。  
各友達の交換条件を隣接リスト adj に格納します。アイテム1を初期ノードとしてキューに入れ、visited 配列で訪問済みかどうかを管理します。while ループを用いて、辿り着ける全てのアイテムをスキャンし、最終的なカウントを出力します。  
友達の数 $M$ とアイテムの数 $N$ に対して、各エッジとノードを一度ずつ走査するため、計算量は $O(N + M)$ となり、十分に間に合います。
```cpp
#include<bits/stdc++.h>
using namespace std;

vector<int> adj[300005];
bool visited[300005];

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    for(int i=0;i<m;i++)
    {
        int a,b;
        cin>>a>>b;
        adj[a].push_back(b);
    }

    queue<int> q;
    q.push(1);
    visited[1]=true;
    int count=0;
    while(!q.empty())
    {
        int curr=q.front();
        q.pop();
        count++;

        for(int next_item:adj[curr])
        {

            if(!visited[next_item])
            {
                visited[next_item]=true;
                q.push(next_item);
            }
        }
    }
    cout<<count<<"\n";

    return 0;
}
```
## D - (xx)
### https://atcoder.jp/contests/abc454/tasks/abc454_d
この問題のポイントは、操作が可逆的であることに気付く点です。(xx) ↔ xx という変換が自由にできるため、文字列 $A$ と $B$ が同じ状態に到達できるかどうかを判定するには、両者を「これ以上操作できない状態」まで**正規化**して比較すれば良いです。  
文字列を1文字ずつ走査し、新しい文字列（スタックの役割）に追加していきます。末尾の4文字が (xx) になった瞬間、それを xx に置換します。この操作を繰り返すと、最短形式の文字列が得られます。$A$ と $B$ をそれぞれこの方法で簡略化し、最終的に一致するかを判定します。  
計算量： 各テストケースに対して文字列の長さ $N$ の線形時間 $O(N)$ で処理できるため、総和が $2 \times 10^6$ という制約下でも非常に高速に動作します。
```cpp
#include <bits/stdc++.h>
using namespace std;

string simplify(const string& s)
{
    string res;
    res.reserve(s.length());
    
    for(char c:s)
    {
        res+=c;
        int n=res.length();
        if(n>=4&&res[n-4]=='('&&res[n-3]=='x'&&res[n-2]=='x'&&res[n-1]==')')
        {
            res.resize(n-4);
            res+="xx";
        }
    }
    return res;
}

void solve()
{
    string A,B;
    cin>>A>>B;
    if(simplify(A)==simplify(B))
    {
        cout<<"Yes"<<"\n";
    }
    else
    {
        cout<<"No"<<"\n";
    }
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int T;
    cin>>T;
    while(T--)
    {
        solve();
    }
    return 0;
}
```
