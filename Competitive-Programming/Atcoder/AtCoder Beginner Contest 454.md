# キーサイト・テクノロジープログラミングコンテスト（AtCoder Beginner Contest 454）

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
