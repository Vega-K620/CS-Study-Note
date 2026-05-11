# AtCoder Beginner Contest 457
先週土曜日授業があるので、ABCを参加しませんでした。今日A-Dを解きました。ABはかなり易しい内容だったので、ここでは詳しい解説は省きます。
## A - Array
### https://atcoder.jp/contests/abc457/tasks/abc457_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<int> num(n+1);
    for(int i=1;i<=n;i++)
    {
        cin>>num[i];
    }
    int x;
    cin>>x;
    cout<<num[x]<<"\n";
    return 0;
}
```
## B - Arrays
### https://atcoder.jp/contests/abc457/tasks/abc457_b
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<vector<int>> num(n+1);
    for(int i=1;i<=n;i++)
    {
        int l;
        cin>>l;
        num[i].push_back(0);
        for(int j=1;j<=l;j++)
        {
            int temp;
            cin>>temp;
            num[i].push_back(temp);
        }
    }
    int x,y;
    cin>>x>>y;
    cout<<num[x][y]<<"\n";
    return 0;
}
```
## C - Long Sequence
### https://atcoder.jp/contests/abc457/tasks/abc457_c
制約は少し大きいですので、O(nlogn)以下の方法を使いなければならない。  
毎回　$ c[i]*(ll)(num[i].size()) $ と $ K $ を比較して、もし $ K $ はもっと大きいなら、直接 $ k $ 引く $ c[i]*(num[i].size()) $ して、もっと快速です。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    ll n,k;
    cin>>n>>k;
    vector<vector<int>> num(n);
    for(int i=0;i<n;i++)
    {
        int temp;
        cin>>temp;
        for(int j=0;j<temp;j++)
        {
            int l;
            cin>>l;
            num[i].push_back(l);
        }
    }
    vector<ll> c(n+1);
    for(int i=0;i<n;i++)
    {
        cin>>c[i];
    }
    for(int i=0;i<n;i++)
    {
        if(c[i]*(ll)(num[i].size())<k)
        {
            k-=c[i]*(num[i].size());
        }
        else
        {
            k--;
            k%=num[i].size();
            cout<<num[i][k]<<"\n";
            break;
        }
    }
    return 0;
}
```
## D - Raise Minimum
### https://atcoder.jp/contests/abc457/tasks/abc457_d
この問題、最初僕は「priority_queue」を使うと思います。しかし $ K $ は本当に大きすぎてこの方法はダメです。ですから、正解は答の二分探索です。  
チェックの方法も簡単です。答え $ X $ を二分探索して、もし全ての要素を $ X $ 以上にできるなら、checkは真（True）となる。
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll=long long;

ll n,k;
vector<ll> a;
bool check(ll x)
{
        ll need=0;
        for(int i=0;i<n;i++)
        {
            if(a[i]>=x)
            continue;
            ll diff=x-a[i];
            ll add=(diff+i)/(i+1);
            need+=add;
            if(need>k)
            return false;
        }
        return need<=k;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    cin>>n>>k;
    a.resize(n);
    for(int i=0;i<n;i++)
    cin>>a[i];

    ll lo=0,hi=2e18;
    while(lo<hi)
    {
        ll mid=(lo+hi+1)/2;
        if(check(mid))lo=mid;
        else hi=mid-1;
    }
    cout<<lo<<'\n';
    return 0;
}
```
