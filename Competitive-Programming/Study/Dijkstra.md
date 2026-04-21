# Dijkstra
## D - Transit Tree Path
### https://atcoder.jp/contests/abc070/tasks/abc070_d
クエリごとに $X \to K \to Y$ の最短距離を求める問題。  
本来、木構造なので DFS や LCA（最近共通祖先）を用いて解くのが一般的だが、今回は Dijkstra法 の練習として実装した。
事前に $K$ から全頂点への最短距離 dist を Dijkstra で計算しておけば、各クエリには $dist[X] + dist[Y]$ で $O(1)$ で回答可能。  
実装のポイント  
優先度付きキュー (priority_queue):  
距離が小さい順に取り出すため、カスタム比較クラス cmp を定義して小根堆（Min-Heap）を構築。
緩和のロジック:  
temp:キューから取り出した「確定間近」の頂点と距離。  
edge: その頂点に隣接する辺の情報（行き先と重み）。  
新路 vs 老路: dist[u] + cost が dist[v] より小さい場合のみ距離を更新し、キューにプッシュする。  
枝刈り:
if(temp.second > dist[temp.first]) continue; により、古い（最短ではない）情報の処理をスキップして高速化。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
ll k;
vector<vector<pair<ll,ll>>> tree;
vector<ll> dist;
struct cmp{
    bool operator()(const pair<ll,ll>& a,const pair<ll,ll>& b)
    {
        return a.second>b.second;
    }
};
void solve()//Dijkstra算k为起点各点的最短距离直接相加得到答案
{
    int x,y;
    cin>>x>>y;
    ll ans=dist[x]+dist[y];
    cout<<ans<<"\n";
    return ;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    tree.resize(n+1);
    dist.resize(n+1,-1);
    for(int i=0;i<n-1;i++)
    {
        int a,b,c;
        cin>>a>>b>>c;
        tree[a].push_back({b,c});
        tree[b].push_back({a,c});
    }
    int q;
    cin>>q>>k;
    dist[k]=0;
    priority_queue<pair<ll,ll>,vector<pair<ll,ll>>,cmp> Dijkstra;
    Dijkstra.push({k,0});
    while(!Dijkstra.empty())
    {
        pair<ll,ll> temp=Dijkstra.top();
        Dijkstra.pop();
        if(temp.second>dist[temp.first]&&dist[temp.first]!=-1)continue;
        for(auto edge:tree[temp.first])
        {
            //temp存的是当前pop出来的点和他到起点最近的距离
            //edge里面存的是temp这条边接下来的边和权重
            //需要判断是老路更近还是新路更近
            //老路：原先dist存的距离
            //新路：当前点到起点距离加上要去的点的权重
            if(dist[edge.first]==-1||dist[temp.first]+edge.second<dist[edge.first])
            {
                dist[edge.first]=dist[temp.first]+edge.second;
                Dijkstra.push({edge.first,dist[edge.first]});
            }
        }
    }
    while(q--)
    {
        solve();
    }
    return 0;
}
```
