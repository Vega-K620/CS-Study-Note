# AtCoder Daily Training ALL 2026/04/17 18:00start

## E - 1D puyopuyo
### https://atcoder.jp/contests/adt_all_20260417_2/tasks/abc438_c

```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    stack<pair<int,int>> num;
    for(int i=0;i<n;i++)
    {
        int temp;
        cin>>temp;
        if(num.empty())
        {
            num.push({temp,1});
        }
        else
        {
            if(num.top().first!=temp)
            num.push({temp,1});
            else
            {
                if(num.top().second==3)
                {
                    int k=3;
                    while(k--)
                    {
                        num.pop();
                    }
                }
                else
                num.push({temp,num.top().second+1});
            }
        }
    }
    cout<<num.size()<<"\n";
    return 0;
}
```
## F - XX to XXX
### https://atcoder.jp/contests/adt_all_20260417_2/tasks/abc259_c

```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string s,t;
    cin>>s>>t;
    vector<pair<char,int>> cnt_s,cnt_t;
    int temp=1;
    for(int i=1;i<s.size();i++)
    {
        if(s[i-1]!=s[i])
        {
            cnt_s.push_back({s[i-1],temp});
            temp=1;
        }
        else
        temp++;
    }
    cnt_s.push_back({s[s.size()-1],temp});
    temp=1;
    for(int i=1;i<t.size();i++)
    {
        if(t[i-1]!=t[i])
        {
            cnt_t.push_back({t[i-1],temp});
            temp=1;
        }
        else
        temp++;
    }
    cnt_t.push_back({t[t.size()-1],temp});
    if(cnt_s.size()!=cnt_t.size())
    {
        cout<<"No"<<"\n";
        return 0;
    }
    for(int i=0;i<cnt_s.size();i++)
    {
        if(cnt_s[i].first!=cnt_t[i].first)
        {
            cout<<"No"<<"\n";
            return 0;
        }
        if(cnt_t[i].second<cnt_s[i].second)
        {
            cout<<"No"<<"\n";
            return 0;
        }
        if(cnt_t[i].second!=1&&cnt_s[i].second==1)
        {
            cout<<"No"<<"\n";
            return 0;
        }
    }
    cout<<"Yes"<<"\n";
    return 0;
}
```
## G - Paid Walk
### https://atcoder.jp/contests/adt_all_20260417_2/tasks/abc441_d

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

struct Edge {
    int to;
    int cost;
};

int n, m, l;
ll s, t;
vector<Edge> adj[200005];
bool vis[200005];

void dfs(int u,int step,ll sum)
{
    if(step==l)
    {
        if(sum>=s&&sum<=t)
        vis[u]=true;
        return;
    }
    for(auto &e:adj[u])
    {
        dfs(e.to,step+1,sum+e.cost);
    }
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    cin>>n>>m>>l>>s>>t;

    for(int i=0;i<m;i++)
    {
        int u,v,c;
        cin>>u>>v>>c;
        adj[u].push_back({v,c});
    }

    dfs(1,0,0);

    vector<int> ans;
    for(int i=1;i<=n;i++)
    {
        if(vis[i])
        ans.push_back(i);
    }

    for(int i=0;i<ans.size();i++)
    {
        cout<<ans[i]<<(i==(int)ans.size()-1?"":" ");
    }
    cout<<"\n";
    return 0;
}
```
