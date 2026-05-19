# AtCoder Daily Training ALL 2026/05/19 16:00start
## A - Online Shopping
### https://atcoder.jp/contests/adt_all_20260519_1/tasks/abc332_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,s,k;
    cin>>n>>s>>k;
    vector<pair<int,int>> p(n);
    for(int i=0;i<n;i++)
    {
        cin>>p[i].first>>p[i].second;
    }
    ll sum=0;
    for(int i=0;i<n;i++)
    {
        sum+=p[i].first*p[i].second;
    }
    if(sum<s)
    {
        sum+=k;
    }
    cout<<sum<<'\n';
    return 0;
}
```
## B - Odd Position Sum
### https://atcoder.jp/contests/adt_all_20260519_1/tasks/abc403_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    int sum=0;
    for(int i=1;i<=n;i++)
    {
        int temp;
        cin>>temp;
        if(i%2==1)
        sum+=temp;
    }
    cout<<sum<<"\n";
    return 0;
}
```
