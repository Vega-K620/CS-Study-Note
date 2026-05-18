# AtCoder Daily Training ALL 2026/05/18 16:00start
## A - flip
### https://atcoder.jp/contests/adt_all_20260518_1/tasks/abc289_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string s;
    cin>>s;
    for(int i=0;i<s.size();i++)
    {
        if(s[i]=='0')
        cout<<'1';
        else
        cout<<'0';
    }
    cout<<"\n";
    return 0;
}
```
## B - Distinct Strings
### https://atcoder.jp/contests/adt_all_20260518_1/tasks/abc225_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;


int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string s;
    cin>>s;
    set<string> ans;
    sort(s.begin(),s.end());
    do{
        ans.insert(s);
    }while(next_permutation(s.begin(),s.end()));
    cout<<(int)ans.size()<<"\n";
    return 0;
}
```
## C - Mapping
### https://atcoder.jp/contests/adt_all_20260518_1/tasks/abc454_b
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    set<int> human;
    vector<int> cloths(m,0);
    for(int i=0;i<n;i++)
    {
        int temp;
        cin>>temp;
        human.insert(temp);
        cloths[temp-1]++;
    }
    if(human.size()==n)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
    int flag=true;
    for(int i=0;i<m;i++)
    {
        if(cloths[i]==0)
        {
            flag=false;
            break;
        }
    }
    if(flag)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
    return 0;
}
```
## D - Ticket Counter
### https://atcoder.jp/contests/adt_all_20260518_1/tasks/abc358_b
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,a;
    cin>>n>>a;
    queue<int> que;
    for(int i=0;i<n;i++)
    {
        int temp;
        cin>>temp;
        que.push(temp);
    }
    ll time=0;
    for(int i=0;i<n;i++)
    {
        if(time>que.front())
        {
            cout<<time+a<<"\n";
            time+=a;
            if(!que.empty())
            que.pop();
        }
        else
        {
            cout<<que.front()+a<<"\n";
            time=que.front()+a;
            if(!que.empty())
            que.pop();
        }
    }
    return 0;
}
```
## E - Transportation Expenses
### https://atcoder.jp/contests/adt_all_20260518_1/tasks/abc365_c
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

bool check(ll mid,const vector<ll>& human,ll m)
{
    ll sum=0;
    for(ll a:human)
    {
        sum+=min((ll)a,mid);
    }
    return sum<=m;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    ll n,m;
    cin>>n>>m;
    vector<ll> human(n);
    ll sum=0;
    ll maxhuman=0;
    for(int i=0;i<n;i++)
    {
        cin>>human[i];
        sum+=human[i];
        maxhuman=max(maxhuman,human[i]);
    }
    if(sum<=m)
    {
        cout<<"infinite"<<"\n";
    }
    else
    {
        ll l=0,r=maxhuman;
        ll ans=0;
        while(l<=r)
        {
            ll mid=l+(r-l)/2;
            if(check(mid,human,m))
            {
                ans=mid;
                l=mid+1;
            }
            else
            {
                r=mid-1;
            }
        }
        cout<<ans<<"\n";
    }
    return 0;
}
```
