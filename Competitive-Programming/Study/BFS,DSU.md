# BFS & DSU 訓練ノート
## BFS（幅優先探索）: 最短経路の基本
グリッド（格子状のマップ）での探索において、BFS は最強の武器です。  
状態管理 (キュー): std::queue を使用。  
【鉄則】訪問済みフラグ (vis) のタイミング:  
キューに push した直後に vis[nx][ny] = true にすること。  
pop した時にフラグを立てると、同じ場所を何度もキューに入れてしまい、計算量が爆発します。  
### C - 幅優先探索
#### https://atcoder.jp/contests/abc007/tasks/abc007_3
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
int r,c;
int sy,sx,gy,gx;
int dist[55][55];
char excel[55][55];
struct temp{
    int y,x,step;
};
void bfs()
{
    queue<temp> R;
    R.push({sy,sx,0});
    while(!R.empty())
    {
        int y=R.front().y,x=R.front().x;
        int step=R.front().step;
        R.pop();
        if(dist[gy][gx]!=-1)
        {
            return;
        }
        if(excel[y+1][x]=='.'&&dist[y+1][x]==-1&&y+1<=r)
        {
            dist[y+1][x]=step+1;
            R.push({y+1,x,step+1});
        }
        if(excel[y][x+1]=='.'&&dist[y][x+1]==-1&&x+1<=c)
        {
            dist[y][x+1]=step+1;
            R.push({y,x+1,step+1});
        }
        if(excel[y][x-1]=='.'&&dist[y][x-1]==-1&&x-1>=1)
        {
            dist[y][x-1]=step+1;
            R.push({y,x-1,step+1});
        }
        if(excel[y-1][x]=='.'&&dist[y-1][x]==-1&&y-1>=1)
        {
            dist[y-1][x]=step+1;
            R.push({y-1,x,step+1});
        }
    }
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    cin>>r>>c;
    cin>>sy>>sx>>gy>>gx;
    for(int i=1;i<=r;i++)
    {
        for(int j=1;j<=c;j++)
        {
            cin>>excel[i][j];
            dist[i][j]=-1;
        }
    }
    dist[sy][sx]=0;
    bfs();
    cout<<dist[gy][gx]<<"\n";
    return 0;
}
```
##  DSU（素集合データ構造 / 並查集）: 連結性の判定
「C - Bridge」のような、辺を一本ずつ取り除いて連結性を確認する問題で活躍します。  
経路圧縮: find 関数で parent[x] = find(parent[x]) と書くことで、木の高さを抑え、処理を高速化（ほぼ $O(1)$）します。  
全探索と組み合わせ: $N, M \le 50$ 程度の規模なら、すべての辺に対して「その辺を除いて DSU を作り直す」という $O(M^2)$ の力技が最も確実です。  
### C - Bridge
#### https://atcoder.jp/contests/abc075/tasks/abc075_c
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
vector<int> parents;
vector<pair<int,int>> brige;
int block;

void reset()
{
    for(int i=0;i<parents.size();i++)
    parents[i]=i;
    return;
}
int find(int son)
{
    if(son==parents[son])return son;
    return parents[son]=find(parents[son]);
}
void bin(int a,int b)
{
    int roota=find(a);
    int rootb=find(b);
    if(roota!=rootb)
    {
        parents[roota]=parents[rootb];
        block--;
    }
}
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;

    parents.resize(n+1);
    brige.resize(m+1);
    for(int i=0;i<m;i++)
    {
        int a,b;
        cin>>a>>b;
        brige[i]={a,b};
    }
    int ans=0;
    for(int i=0;i<m;i++)
    {
        block=n;
        reset();
        for(int j=0;j<m;j++)
        {
            if(i==j)continue;
            bin(brige[j].first,brige[j].second);
        }
        if(block!=1)
        ans++;
    }
    cout<<ans<<"\n";
    return 0;
}
```
