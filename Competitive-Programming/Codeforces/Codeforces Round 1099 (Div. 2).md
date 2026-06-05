# Codeforces Round 1099 (Div. 2)

## 
### 
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

vector<bool> flag;

void solve()
{
    int n;
    cin>>n;
    int out=1;
    int ans=1;
    int last=1;
    flag.assign(2005,false);
    cout<<1<<" ";
    flag[1]=true;
    while(out!=n)
    {
        ans++;
        if(!flag[ans])
        {
            cout<<ans<<" ";
            flag[ans]=true;
            flag[ans+last]=true;
            last=ans;
            out++;
        }
        else
        {
            continue;
        }
    }
    cout<<"\n";
}

signed main()
{
    cin.tie(nullptr)->sync_with_stdio(false);
    int t;
    //t=1;
    cin>>t;
    while(t--)
    {
        solve();
    }
}
```
## 
### 
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

const int INF=1e18;

void solve()
{
    int n;
    cin>>n;
    vector<int> a(n);
    for(int i=0;i<n;i++)
    {
        cin>>a[i];
    }

    int k=0;
    for(int i=0;i<n-1;i++)
    {
        if(a[i]>a[i+1])
        {
            k=max(k,a[i]-a[i+1]);
        }
    }

    for(int i=1;i<n;i++)
    {
        if(a[i]<a[i-1])
        {
            a[i]+=k;
        }
    }

    if(is_sorted(a.begin(),a.end()))
    {
        cout<<"YES"<<"\n";
    }
    else
    {
        cout<<"NO"<<"\n";
    }
}

signed main()
{
    cin.tie(nullptr)->sync_with_stdio(false);
    int t;
    //t=1;
    cin>>t;
    while(t--)
    {
        solve();
    }
}
```
## 
### 
```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

const int INF=1e18;

void solve()
{
    int n;
    cin>>n;
    vector<int> a(n);
    for(int i=0;i<n;i++)
    {
        cin>>a[i];
    }

    map<int,int> cnt1;
    map<int,int> cnt2;

    int x = a[0];
    set<int> s;
    int c=0;
    while(s.find(x)==s.end())
    {
        cnt1[x]=1;
        cnt2[x]=c;
        s.insert(x);
        
        if(x%2==1) x+=1;
        else x/=2;
        c++;
    }

    for(int i=1;i<n;i++)
    {
        x=a[i];
        s.clear();
        c=0;
        while(s.find(x)==s.end())
        {
            if(cnt1.count(x))
            {
                cnt1[x]+=1;
                cnt2[x]+=c;
            }
            s.insert(x);

            if(x%2==1) x+=1;
            else x/=2;
            c++;
        }
    }

    int ans=INF;
    for(auto const& [val, count]:cnt1)
    {
        if(count==n)
        {
            ans=min(ans, cnt2[val]);
        }
    }

    cout<<ans<<"\n";
}

signed main()
{
    cin.tie(nullptr)->sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        solve();
    }
    return 0;
}
```
