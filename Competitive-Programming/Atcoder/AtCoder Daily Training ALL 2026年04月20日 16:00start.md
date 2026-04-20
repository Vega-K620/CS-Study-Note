# AtCoder Daily Training ALL 2026/04/20 16:00start
今日のトレーニングはA-Fを解きました。A-Eは簡単ですから、ここでは詳しい解説は省きます。
## A - ASCII code
### https://atcoder.jp/contests/adt_all_20260420_1/tasks/abc252_a
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int num;
    cin>>num;
    cout<<(char)num<<"\n";
    return 0;
}
```
## B - First ABC
### https://atcoder.jp/contests/adt_all_20260420_1/tasks/abc311_a
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    string str;
    cin>>str;
    bool check[3]={false};
    for(int i=0;i<n;i++)
    {
        
        if(check[str[i]-'A']==false)
        {
            check[str[i]-'A']=true;
        }
        if(check[0]==true&&check[1]==true&&check[2]==true)
        {
            cout<<i+1<<"\n";
            return 0;
        }
    }
    return 0;
}
```
## C - Weak Password
### https://atcoder.jp/contests/adt_all_20260420_1/tasks/abc212_b
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string num;
    cin>>num;
    if(num[0]==num[1]&&num[1]==num[2]&&num[2]==num[3])
    {
        cout<<"Weak"<<"\n";
        return 0;
    }
    bool is_sequential=true;
    for(int i=0;i<3;i++)
    {
        int current=num[i]-'0';
        int next=num[i+1]-'0';
        if((current+1)%10!=next)
        {
            is_sequential=false;
            break;
        }
    }
    
    if(is_sequential)
        cout<<"Weak"<<"\n";
    else
        cout<<"Strong"<<"\n";
    return 0;
}
```
## D - Citation
### https://atcoder.jp/contests/adt_all_20260420_1/tasks/abc409_b
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<int> a(n);
    for(int i=0;i<n;i++)
    {
        cin>>a[i];
    }
    int ans=0;
    for(int x=0;x<=n;x++)
    {
        int cnt=0;
        for(int i=0;i<n;i++)
        {
            if(a[i]>=x)cnt++;
        }
        if(cnt>=x)ans=x;
    }
    cout<<ans<<"\n";
    return 0;
}
```
## E - Distribution
### https://atcoder.jp/contests/adt_all_20260420_1/tasks/abc214_c
高橋君から直接もらう: 時刻 $T_i$  
前のすぬけ君から渡される: 時刻 $ans[i-1] + S_{i-1}$  
これを式にすると、以下のようになります。  
$$ans[i] = \min(T[i], ans[i-1] + S_{i-1})$$
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<long long> s(n);
    vector<long long> t(n);
    for(int i=0;i<n;i++)
    cin>>s[i];
    for(int i=0;i<n;i++)
    cin>>t[i];
    vector<long long> ans=t;
    for(int i=0;i<n-1;i++)
    {
        ans[i+1]=min(ans[i+1],ans[i]+s[i]);
    }
    
    ans[0]=min(ans[0],ans[n-1]+s[n-1]);
    for(int i=0;i<n-1;i++)
    {
        ans[i+1]=min(ans[i+1],ans[i]+s[i]);
    }

    for(int i=0;i<n;i++)
    {
        cout<<ans[i]<<"\n";
    }
    return 0;
}
```
## F - Bipartize
### https://atcoder.jp/contests/adt_all_20260420_1/tasks/abc427_c
各頂点について「白」か「黒」の 2 通りがあります。$N=10$ なので、塗り方は $2^{10} = 1024$ 通りしかありません。これは DFS やビット全探索で十分に間に合います。  
ある塗り方を決めたとき、辺 $(u, v)$ について：$u$ と $v$ が 異なる色 $\rightarrow$ そのまま残せる。$u$ と $v$ が 同じ色 $\rightarrow$ 二部グラフの条件を満たさないため、削除必須。  
全ての塗り方の中で、削除が必要な辺の数の最小値を保持します。
```cpp
#include<bits/stdc++.h>
using namespace std;
int n,m;
vector<vector<int>> line;
vector<bool> check;
int ans;
void dfs(int did)
{
    if(did==n)
    {
        int temp=0;
        for(int i=0;i<n;i++)
        {
            for(int j:line[i])
            {
                if(i<j&&check[i]==check[j])
                {
                    temp++;
                }
            }
        }
        ans=min(ans,temp);
        return;
    }
    check[did]=true;
    dfs(did+1);
    check[did]=false;
    dfs(did+1);
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    
    cin>>n>>m;
    line.resize(n+1);
    check.resize(n+1);
    ans=m;
    for(int i=0;i<m;i++)
    {
        int a,b;
        cin>>a>>b;
        line[a].push_back(b);
        line[b].push_back(a);
    }
    dfs(0);
    cout<<ans<<"\n";
    return 0;
}
```
