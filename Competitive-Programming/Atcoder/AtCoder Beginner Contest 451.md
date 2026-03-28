# AtCoder Beginner Contest 451
C問題でのタイムロスが響き、最終はA-Eを解きました。
## A - illegal
### https://atcoder.jp/contests/abc451/tasks/abc451_a
今回のコンテストでは、A問題で「5の倍数の長さなら法律違反（Yes）」という条件に対し、無意識に「5の倍数＝ダメ＝No」と直感で逆の論理を実装してしまい、ケアレスミスでWAを出してしまいました。 
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string s;
    cin>>s;
    if(s.size()%5==0)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
    return 0;
}
``` 
## B - Personnel Change
### https://atcoder.jp/contests/abc451/tasks/abc451_b
B問題では各部門の人数変化を転出（-1）と転入（+1）で管理することで手際よく処理できました
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    vector<pair<int,int>> fount(n);
    for(int i=0;i<n;i++) 
    cin>>fount[i].first>>fount[i].second;
    vector<int> bumen(m,0);
    for(int i=0;i<n;i++)
    {
        bumen[fount[i].first-1]--;
        bumen[fount[i].second-1]++;
    }
    for(auto i:bumen)
    cout<<i<<"\n";
    return 0;
}
```
## C - Understory
### https://atcoder.jp/contests/abc451/tasks/abc451_c
C問題では multiset の upper_bound を用いて、特定の値以下の要素を一括削除する $O(Q \log Q)$ の解法を実装しましたが、境界値の判定など細部で時間をロスしてしまいました。
```cpp
#include<bits/stdc++.h>
using namespace std;
multiset<long long> trees;
void solve()
{
    int type;
        long long h;
        cin>>type>>h;
        if(type==1)
        {
            trees.insert(h);
        }
        else
        {
            auto it=trees.upper_bound(h);
            if(it != trees.begin())
            {
                trees.erase(trees.begin(),it);
            }
        }
        cout<<trees.size()<<"\n";
}
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    long long n;
    cin>>n;
    while(n--)
    {
        solve();
    }
    return 0;
}
```
## D - Concat Power of 2
### https://atcoder.jp/contests/abc451/tasks/abc451_d
D問題では、数値の連結によって「良い整数」を生成するため、オーバーフローを厳密に考慮したBFSによる状態走査を行い、10万件程度の全候補を効率的に列挙することができました。
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;cin>>n;
    vector<pair<long long, long long>> parts;
    for (long long i=1;i<=1e9;i*=2)
    {
        long long multiplier=1;
        long long temp=i;
        while(temp>0)
        {
            multiplier*=10;
            temp/=10;
        }
        parts.push_back({i,multiplier});
    }
    vector<long long> result;
    for(auto p:parts)
    {
        if(p.first<=1e9) result.push_back(p.first);
    }
    for(int j=0;j<result.size();j++)
    {
        long long num=result[j];
        for(auto p:parts)
        {
            long long val=p.first;
            long long mult=p.second;
            if(num<=(1000000000LL-val)/mult)
            {
                long long next_num=num*mult+val;
                if(next_num<=1e9)
                {
                    result.push_back(next_num);
                }
            }
        }
        if(result.size()>2000000) break;
    }
    sort(result.begin(),result.end());
    result.erase(unique(result.begin(),result.end()), result.end());

    cout<<result[n-1]<<"\n";
    return 0;
}
```
## E - Tree Distance
### https://atcoder.jp/contests/abc451/tasks/abc451_e
E問題は、与えられた距離行列から木を構成できるか判定する難問でしたが、距離の昇順に辺を走査するクラスカル法の発想で木を構築し、最後にBFSで全点間距離の正当性を検証する $O(N^2)$ の解法で完答できました。
```cpp
#include<bits/stdc++.h>
using namespace std;

int parent[3005];
int find(int i) {
    if (parent[i] == i) return i;
    return parent[i] = find(parent[i]);
}

struct Edge {
    int u, v, w;
};

int dist_matrix[3005][3005];
vector<pair<int, int>> adj[3005];
long long d[3005];

int main() {
    cin.tie(NULL)->sync_with_stdio(false);
    int n; 
    if (!(cin >> n)) return 0;

    vector<Edge> all_edges;
    for (int i = 1; i <= n; i++) {
        for (int j = i + 1; j <= n; j++) {
            int w; cin >> w;
            dist_matrix[i][j] = dist_matrix[j][i] = w;
            all_edges.push_back({i, j, w});
        }
    }

    sort(all_edges.begin(), all_edges.end(), [](const Edge& a, const Edge& b) {
        return a.w < b.w;
    });

    for (int i = 1; i <= n; i++) parent[i] = i;
    int edges_count = 0;
    for (auto& e : all_edges) {
        int rootU = find(e.u);
        int rootV = find(e.v);
        if (rootU != rootV) {
            parent[rootU] = rootV;
            adj[e.u].push_back({e.v, e.w});
            adj[e.v].push_back({e.u, e.w});
            edges_count++;
        }
    }

    if (edges_count != n - 1) {
        cout << "No" << endl;
        return 0;
    }

    for (int i = 1; i <= n; i++) {
        for (int k = 1; k <= n; k++) d[k] = -1;
        queue<int> q;
        d[i] = 0;
        q.push(i);

        while (!q.empty()) {
            int u = q.front(); q.pop();
            for (auto& edge : adj[u]) {
                int v = edge.first;
                int w = edge.second;
                if (d[v] == -1) {
                    d[v] = d[u] + w;
                    if (d[v] != dist_matrix[i][v]) {
                        cout << "No" << endl;
                        return 0;
                    }
                    q.push(v);
                }
            }
        }
        
        for (int j = 1; j <= n; j++) {
            if (d[j] == -1 || d[j] != dist_matrix[i][j]) {
                cout << "No" << endl;
                return 0;
            }
        }
    }

    cout << "Yes" << endl;
    return 0;
}
```
