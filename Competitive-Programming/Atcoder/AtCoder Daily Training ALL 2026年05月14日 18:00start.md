# AtCoder Daily Training ALL 2026/05/14 18:00start

## E - Remembering the Days
### https://atcoder.jp/contests/adt_all_20260514_2/tasks/abc317_c
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

vector<vector<pair<ll,ll>>> road;
vector<int> visited;
ll sum=0;

void dfs(int deep,ll len,int now)
{
    visited[now]++;
    sum=max(sum,len);
    for(auto i:road[now])
    {
        if(visited[i.first]!=0)
        {
            continue;
        }
        else
        {
            dfs(deep+1,len+i.second,i.first);
        }
    }
    visited[now]--;
    return;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    road.resize(n+1);
    visited.resize(n+1,0);
    for(int i=0;i<m;i++)
    {
        ll a,b,c;
        cin>>a>>b>>c;
        pair<ll,ll> temp;
        temp={b,c};
        road[a].push_back(temp);
        temp={a,c};
        road[b].push_back(temp);
    }
    for(int i=0;i<n;i++)
    {
        dfs(0,0,i);
    }
    cout<<sum<<"\n";
    return 0;
}
```
## F - Triangle?
### https://atcoder.jp/contests/adt_all_20260514_2/tasks/abc224_c
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<ll> x(n),y(n);
    for(int i=0;i<n;i++)
    {
        cin>>x[i]>>y[i];
    }

    int count=0;
    for(int i=0;i<n;i++)
    {
        for(int j=i+1;j<n;j++)
        {
            for(int k=j+1;k<n;k++)
            {
                ll val=(x[j]-x[i])*(y[k]-y[i])-(x[k]-x[i])*(y[j]-y[i]);
                if(val!=0)
                {
                    count++;
                }
            }
        }
    }
    cout<<count<<"\n";
    return 0;
}
```
## G - Polyomino
### https://atcoder.jp/contests/adt_all_20260514_2/tasks/abc322_d
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

struct Poly {
    vector<pair<int, int>> cells;
};

Poly rotate(const Poly& p)
{
    Poly res;
    for(auto& cell:p.cells)
    {
        res.cells.push_back({cell.second,3-cell.first});
    }
    return res;
}

vector<Poly> all_polys[3];
int grid[4][4];

bool dfs(int id)
{
    if(id==3)
    {
        for(int i=0;i<4;i++)
        for(int j=0;j<4;j++)
        if(grid[i][j]==0)
        return false;
        
        return true;
    }

    for(const auto& shape:all_polys[id])
    {
        for(int dr=-3;dr<=3;dr++)
        {
            for(int dc=-3;dc<=3;dc++)
            {
                bool can_place=true;
                vector<pair<int,int>> current_pos;

                for(auto& cell:shape.cells)
                {
                    int nr=cell.first+dr;
                    int nc=cell.second+dc;
                    if(nr>=0&&nr<4&&nc>=0&&nc<4&&grid[nr][nc]==0)
                    {
                        current_pos.push_back({nr, nc});
                    }
                    else
                    {
                        can_place=false;
                        break;
                    }
                }

                if(can_place)
                {
                    for(auto& p:current_pos)
                    grid[p.first][p.second]=1;
                    if(dfs(id+1))
                    return true;
                    for(auto& p:current_pos)
                    grid[p.first][p.second]=0;
                }
            }
        }
    }
    return false;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int total_cells=0;
    for(int i=0;i<3;i++)
    {
        Poly p;
        for(int r=0;r<4;r++)
        {
            string row;
            cin>>row;
            for(int c=0;c<4;c++)
            {
                if(row[c]=='#')
                {
                    p.cells.push_back({r,c});
                    total_cells++;
                }
            }
        }
        Poly curr=p;
        for(int rot=0;rot<4;rot++)
        {
            all_polys[i].push_back(curr);
            curr = rotate(curr);
        }
    }

    if(total_cells!=16)
    {
        cout<<"No"<<"\n";
        return 0;
    }
    if(dfs(0))
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";

    return 0;
}
```
