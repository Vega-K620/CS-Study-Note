# AtCoder Beginner Contest 456

## A - Dice
### https://atcoder.jp/contests/abc456/tasks/abc456_a

```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int num;
    cin>>num;
    if(num>=3&&num<=18)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
    return 0;
}
```
## B - 456
### https://atcoder.jp/contests/abc456/tasks/abc456_b

```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    int count[3][7]={0};
    for(int i=0;i<3;i++)
    {
        for(int j=0;j<6;j++)
        {
            int val;
            cin>>val;
            count[i][val]++;
        }
    }

    vector<int> target={4,5,6};
    long long total=0;

    do
    {
        total+=(long long)count[0][target[0]]*count[1][target[1]]*count[2][target[2]];
    }while(next_permutation(target.begin(),target.end()));

    cout<<fixed<<setprecision(10)<<total/216.0<<"\n";

    return 0;
}
```
## C - Not Adjacent
### https://atcoder.jp/contests/abc456/tasks/abc456_c

```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    string S;
    cin>>S;

    long long total_count=0;
    long long current_len=0;
    char last_char='\0';

    for(char c:S)
    {
        if(c!=last_char)
        {
            current_len++;
        }
        else
        {
            current_len=1;
        }
        total_count+=current_len;
        last_char=c;
    }
    cout<<total_count%998244353<<"\n";

    return 0;
}
```
## D - Not Adjacent 2
### https://atcoder.jp/contests/abc456/tasks/abc456_d

```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    string s;
    cin>>s;
    
    long long dp[3]={0,0,0};
    long long mod=998244353;
    
    for(char c:s)
    {
        int idx=c-'a';
        int other1=(idx+1)%3;
        int other2=(idx+2)%3;
        
        long long current=(dp[other1]+dp[other2]+1)%mod;
        
        dp[idx]=(dp[idx]+current)%mod;
    }
    
    long long ans=(dp[0]+dp[1]+dp[2])%mod;
    cout<<ans<<"\n";
    
    return 0;
}
```
## E - Endless Holidays
### https://atcoder.jp/contests/abc456/tasks/abc456_e

```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

/**
 1. 状态拆分：每个城市在每一天都是一个独立状态 id。
 2. 初始过滤：只有目的地是假日的移动（或停留）才算作合法的“边”。
 3. 拓扑清洗：利用出度为 0 的点作为源头，像剥洋葱一样删掉所有无法到达环的状态。
 */
void solve()
{
    int N, M;
    cin>>N>>M;

    vector<pair<int,int>> edges(M);
    for(int i=0;i<M;i++)
    {
        cin>>edges[i].first>>edges[i].second;
        edges[i].first--;
        edges[i].second--;
    }

    int W;
    cin>>W;
    vector<string> S(N);
    for(int i=0;i<N;i++) cin>>S[i];

    int total_states=N*W;
    vector<vector<int>> rev_adj(total_states);
    vector<int> out_degree(total_states, 0);

    for(int d=0;d<W;d++)
    {
        int next_d=(d+1)%W;
        
        auto add_valid_edge=[&](int u,int v)
        {
            if(S[v][next_d]=='o')
            {
                int from=u*W+d;
                int to=v*W+next_d;
                rev_adj[to].push_back(from);
                out_degree[from]++;
            }
        };

        for(int u=0;u<N;u++)
        {
            add_valid_edge(u, u);
        }
        for(auto& e:edges)
        {
            add_valid_edge(e.first, e.second);
            add_valid_edge(e.second, e.first);
        }
    }

    queue<int> q;
    vector<bool> is_dead(total_states,false);

    for(int i=0;i<total_states;i++)
    {
        if(out_degree[i]==0)
        {
            q.push(i);
        }
    }

    while(!q.empty())
    {
        int v=q.front();
        q.pop();
        if(is_dead[v]) continue;
        is_dead[v]=true;

        for(int u:rev_adj[v])
        {
            if(is_dead[u]) continue;
            out_degree[u]--;
            if(out_degree[u]==0)
            {
                q.push(u);
            }
        }
    }

    bool can_survive=false;
    for(int i=0;i<N;i++)
    {
        if(S[i][0]=='o'&&!is_dead[i*W+0])
        {
            can_survive=true;
            break;
        }
    }

    cout<<(can_survive?"Yes":"No")<<"\n";
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
