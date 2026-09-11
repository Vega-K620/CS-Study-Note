# Codeforces Round 1103 (Div. 3)
## A. Games on the Train
### https://codeforces.com/contest/2236/problem/A
この問題の解法は全ての数字を「一番大きいの数字+1」になります。
ですので、 $ num_max-num_min+1 $ は答えです。
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

void solve()
{
    int n,maxnum=-1,minnum=1e5;
    cin>>n;
    vector<int> num(n);
    for(int i=0;i<n;i++)
    {
        cin>>num[i];
        maxnum=max(maxnum,num[i]);
        minnum=min(minnum,num[i]);
    }
    cout<<maxnum-minnum+1<<"\n";
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
}
```
## B. Tatar TV Show
### https://codeforces.com/contest/2236/problem/B
この問題 $ n<=2e5 $ ですから直接配列を走査して、  
もし $ s_i $ は $ 1 $ なら、 $ s_i $ と $ s_i+k $ を操作します。  
最後配列を走査して答えがでてきます。
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

void solve()
{
    int n,k;
    cin>>n>>k;
    string s;
    cin>>s;
    bool flag=true;
    for(int i=0;i<=n-k-1;i++)
    {
        if(s[i]=='1')
        {
            if(i+k<n)
            {
                s[i]='0';
                if(s[i+k]=='1')
                s[i+k]='0';
                else
                s[i+k]='1';
            }
            else
            {
                flag=false;
            }
        }
    }
    if(flag)
    {
        for(int i=0;i<n;i++)
        {
            if(s[i]=='1')
            {
                flag=false;
            }
        }
    }
    // cout<<s<<" ";
    if(flag)
    {
        cout<<"Yes"<<"\n";
    }
    else
    {
        cout<<"No"<<"\n";
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
}
```
## C. Omsk Programmers
### https://codeforces.com/contest/2236/problem/C
この問題はまず大きい $ num/=x $ 操作回数をメモして。
答えは $ a==b $ までに $ cnt=min(cnt,abs(a-b)+i) $ 「iは $ num/=x $ の操作回数」をして。
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

void solve()
{
    int a,b,x;
    cin>>a>>b>>x;
    int cnt=1e18;
    int i=0;
    while(a!=b)
    {
        if(a<b)swap(a,b);
        cnt=min(cnt,abs(a-b)+i);
        i++;
        a/=x;
    }
    cnt=min(cnt,i);
    cout<<cnt<<"\n";
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
}
```
