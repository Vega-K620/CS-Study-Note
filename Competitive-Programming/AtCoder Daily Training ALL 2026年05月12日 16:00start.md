# AtCoder Daily Training ALL 2026/05/12 16:00start

## E - 1122 Substring 2
### https://atcoder.jp/contests/adt_all_20260512_1/tasks/abc433_c

```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string str;
    cin>>str;
    vector<pair<int,int>> cnt;
    int temp=1;
    for(int i=0;i<str.size();i++)
    {
        if(i==str.size()-1||str[i]!=str[i+1])
        {
            cnt.push_back({str[i]-'0',temp});
            temp=1;
        }
        else
        {
            temp++;
        }
    }
    // for(int i=0;i<cnt.size();i++)
    // {
    //     cout<<cnt[i].first<<" "<<cnt[i].second<<"\n";
    // }
    int ans=0;
    if(cnt.size()>1)
    {
        for(int i=0;i<cnt.size()-1;i++)
        {
            if(cnt[i].first-cnt[i+1].first==-1)
            {
                ans+=min(cnt[i].second,cnt[i+1].second);
            }
        }
    }
    cout<<ans<<"\n";
    return 0;
}
```
## F - RANDOM
### https://atcoder.jp/contests/adt_all_20260512_1/tasks/abc279_c

```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int h,w;
    cin>>h>>w;
    vector<string> s(w,string(h,' ')),t(w,string(h,' '));
    for(int i=0;i<h;i++)
    {
        string str;
        cin>>str;
        for(int j=0;j<w;j++)
        {
            s[j][i]=str[j];
        }
    }
    for(int i=0;i<h;i++)
    {
        string str;
        cin>>str;
        for(int j=0;j<w;j++)
        {
            t[j][i]=str[j];
        }
    }
    sort(s.begin(),s.end());
    sort(t.begin(),t.end());
    // bool flag=true;
    // for(int i=0;i<h;i++)
    // {
    //     if(s[i]!=t[i])flag=false;
    //     cout<<s[i]<<" "<<t[i]<<"\n";
    // }
    if(s==t)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
    return 0;
}
```
## G - Prime Sum Game
### https://atcoder.jp/contests/adt_all_20260512_1/tasks/abc239_d

```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

bool check(int num)
{
    if(num < 2) return false;
    for(int i=2;i*i<=num;i++)
    {
        if(num%i==0) return false;
    }
    return true;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int a,b,c,d;
    cin>>a>>b>>c>>d;
    for(int i=a;i<=b;i++)
    {
        bool aoki=false;
        for(int j=c;j<=d;j++)
        {
            if(check(i+j))
            aoki=true;
        }
        if(!aoki)
        {
            cout<<"Takahashi"<<"\n";
            return 0;
        }
    }
    cout<<"Aoki"<<"\n";
    return 0;
}
```
