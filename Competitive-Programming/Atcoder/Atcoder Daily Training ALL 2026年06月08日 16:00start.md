# AtCoder Daily Training ALL 2026/06/08 16:00start

## A - 123233
### https://atcoder.jp/contests/adt_all_20260608_1/tasks/abc380_a
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

signed main()
{
    string s;
    cin>>s;
    int cnt[10]={0};
    for(int i=0;i<6;i++)
    {
        cnt[s[i]-'0']++;
    }
    if(cnt[1]==1&&cnt[2]==2&&cnt[3]==3)
    {
        cout<<"Yes"<<"\n";
    }
    else
    {
        cout<<"No"<<"\n";
    }
}
```
## B - ^{-1}
### https://atcoder.jp/contests/adt_all_20260608_1/tasks/abc277_a
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

signed main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,x;
    cin>>n>>x;
    vector<int> num(n);
    for(int i=0;i<n;i++)
    {
        cin>>num[i];
    }
    for(int i=0;i<n;i++)
    {
        if(num[i]==x)
        {
            cout<<i+1<<"\n";
            break;
        }
    }
}
```
## C - Extended ABC
### https://atcoder.jp/contests/adt_all_20260608_1/tasks/abc337_b
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

signed main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string s;
    cin>>s;
    bool havea=false,haveb=false,havec=false;
    bool flag=true;
    for(int i=0;i<(int)s.size();i++)
    {
       if(s[i]=='A')
        {
            havea=true;
            if(!(!haveb&&!havec))
            {
                flag=false;
                break;
            }
        }
        if(s[i]=='B')
        {
            haveb=true;
            if(!(!havec))
            {
                flag=false;
                break;
            }
        }
       
        if(s[i]=='C')
        {
            havec=true;
        }
    }
    if(flag)
    {
        cout<<"Yes"<<"\n";
    }
    else
    {
        cout<<"No"<<"\n";
    }
}
```
## D - Hands on Ring (Easy)
### https://atcoder.jp/contests/adt_all_20260608_1/tasks/abc376_b
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

void solve()
{
    int n,q;
    cin>>n>>q;
    int l=1,r=2;
    int ans=0;
    while(q--)
    {
        char c;
        int ip;
        cin>>c>>ip;

        int cur,other;
        if(c=='L')
        {
            cur=l;
            other=r;
        }
        else
        {
            cur=r;
            other=l;
        }

        if(cur==ip)continue;
        int d_cw=(ip-cur+n) % n;
        int d_ccw = n - d_cw;
        int dist_o=(other-cur+n)%n;

        if(dist_o>0&&dist_o<d_cw)
        {
            ans+=d_ccw;
        }
        else
        {
            ans+=d_cw;
        }

        if(c=='L')
        l=ip;
        else
        r=ip;
    }
    cout<<ans<<"\n";
}

signed main()
{
    cin.tie(NULL)->sync_with_stdio(false);

    int t;
    t=1;
    //cin>>t;
    while(t--)
    {
        solve();
    }
}
```
## E - Mixture
### https://atcoder.jp/contests/adt_all_20260608_1/tasks/abc415_c
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

void solve()
{
    int n;
    cin>>n;
    string s;
    cin>>s;

    int num_states=(1<<n);

    vector<bool> dp(num_states,false);
    dp[0]=true;

    for(int mask=0;mask<num_states;mask++)
    {
        if(!dp[mask]) continue;

        for(int i=0;i<n;i++)
        {    
            if(!((mask>>i)&1))
            {
                int next_mask=mask|(1<<i);
                if (s[next_mask-1]=='0')
                {
                    dp[next_mask]=true;
                }
            }
        }
    }

    if(dp[num_states-1])
    {
        cout<<"Yes"<<endl;
    }
    else
    {
        cout<<"No"<<endl;
    }
}

signed main()
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