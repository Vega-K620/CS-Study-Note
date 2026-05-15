# AtCoder Daily Training ALL 2026/05/15 18:00start
## A - Power
### https://atcoder.jp/contests/adt_all_20260515_2/tasks/abc283_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

ll getpow(ll a,ll b)
{
    ll ans=1;
    while(b--)
    {
        ans*=a;
    }
    return ans;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    ll a,b;
    cin>>a>>b;
    ll ans=getpow(a,b);
    cout<<ans<<"\n";
    return 0;
}
```
## B - CBC
### https://atcoder.jp/contests/adt_all_20260515_2/tasks/abc402_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string str;
    cin>>str;
    string ans="";
    for(int i=0;i<(int)str.size();i++)
    {
        if(str[i]>='A'&&str[i]<='Z')
        {
            ans+=str[i];
        }
    }
    cout<<ans<<"\n";
    return 0;
}
```
## C - Caesar Cipher
### https://atcoder.jp/contests/adt_all_20260515_2/tasks/abc232_b
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string a,b;
    cin>>a>>b;
    
    int len=a.size();
    if(len==0||len==1)
    {
        cout<<"Yes"<<"\n";
        return 0;
    }
    else
    {
        for(int i=0;i<len-1;i++)
        {
            if((a[i]-b[i]+26)%26!=(a[i+1]-b[i+1]+26)%26)
            {
                cout<<"No"<<"\n";
                return 0;
            }
        }
        cout<<"Yes"<<"\n";
    }
    return 0;
}
```
