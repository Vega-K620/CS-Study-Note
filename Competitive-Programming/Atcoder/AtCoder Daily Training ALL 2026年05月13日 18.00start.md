# AtCoder Daily Training ALL 2026/05/13 18:00start
## E - One Time Swap
### https://atcoder.jp/contests/adt_all_20260513_2/tasks/abc345_c
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string str;
    cin>>str;
    map<ll,ll> cnt;
    for(int i=0;i<str.size();i++)
    {
        cnt[str[i]-'a']++;
    }
    ll ans=str.size()*(str.size()-1)/2;
    ll same=0;
    bool flag=false;
    for(auto i:cnt)
    {
        if(i.second>=2)
        {
            flag=true;
            same+=i.second*(i.second-1)/2;
        }
    }
    ans-=same;
    if(flag)
    ans+=1;

    cout<<ans<<"\n";
    return 0;
}
```
## F - Reindeer and Sleigh 2
### https://atcoder.jp/contests/adt_all_20260513_2/tasks/abc437_c
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

bool cmp(pair<ll,ll> a,pair<ll,ll> b)
{
    return a.first+a.second>b.first+b.second;
}

void solve()
{
    int n;
    cin>>n;
    vector<pair<int,int>> num(n);
    ll sumw=0;
    ll sump=0;
    for(int i=0;i<n;i++)
    {
        cin>>num[i].first>>num[i].second;
        sumw+=num[i].first;
    }
    sort(num.begin(),num.end(),cmp);
    for(int i=0;i<n;i++)
    {
        sump+=num[i].second+num[i].first;
        if(sump>=sumw)
        {
            cout<<n-i-1<<"\n";
            break;
        }
    }
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        solve();
    }
    return 0;
}
```
