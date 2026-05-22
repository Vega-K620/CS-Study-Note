# Codeforces Round 1097 (Div. 2, Based on Zhili Cup 2026)
今回のコンテストはA-Cを解きました。「codeforces」らしいの問題です。全部は「greedy」です。
## A. Zhily and Array Operating
### https://codeforces.com/contest/2224/problem/A
$ a_i $ は $ a_i+a(i+1) $ に変えるから、 $ a_i $ と $ a_i+a(i+1) $ を対比して、大きいのを選択します。それは最も大きいの数量です。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

void solve()
{
    ll n;
    cin>>n;
    vector<ll> num(n);
    for(int i=0;i<n;i++)
    {
        cin>>num[i];
    }
    for(ll i=n-2;i>=0;i--)
    {
        if(num[i]+num[i+1]>num[i])
        {
            num[i]=num[i]+num[i+1];
        }
    }
    ll ans=0;
    for(ll i=0;i<n;i++)
    {
        if(num[i]>0)
        ans++;
    }
    cout<<ans<<"\n";
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
## B. Zhily and Mex and Max
### https://codeforces.com/contest/2224/problem/B
この問題は $\sum\limits_{i=1}^n (\operatorname{mex}(a_1,a_2,\cdots,a_i)+\max(a_1,a_2,\cdots,a_i))$ の最大数を求めます。
ポイント：  
まず最大数を第一に置いて、他の数字は昇順で並びます。もし重複した数字があれば最後に置きます。
こうすれば、最大数が見つけます。
「MEX」を見つける方法：  
1: $ 1-n $ を全部「set」に入って、 $ a_1 $ - $ a_n $ を「set」に消します。残るの最小数字は「MEX」です。
2: $ a_1 $ - $ a_n $ を「vis」にメモして、メモしない数字は「MEX」です。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

void solve()
{
    ll n;
    cin>>n;
    vector<ll> num(n);
    for(int i=0;i<n;i++)
    {
        cin>>num[i];
    }
    sort(num.begin(),num.end());
    swap(num[0],num[n-1]);
    sort(num.begin()+1,num.end());
    vector<ll> temp1,temp2;
    for(int i=1;i<n;i++)
    {
        if(temp1.size()!=0)
        {
            if(temp1.back()==num[i])
            temp2.push_back(num[i]);
            else
            temp1.push_back(num[i]);
        }
        else
        {
            temp1.push_back(num[i]);
        }
        
    }
    ll bigest=num[0];
    num.clear();
    num.push_back(bigest);
    for(ll i:temp1)
    num.push_back(i);
    for(ll i:temp2)
    num.push_back(i);

    ll ans=0;
    ans=num[0]*n;
    ll maxmex=0;
    vector<bool> vis(n+5,false);
    for(ll i=0;i<n;i++)
    {
        if(num[i]>=0&&num[i]<n)
        vis[num[i]]=true;
        while(vis[maxmex])
        {
            maxmex++;
        }
        ans+=maxmex;
    }
    cout<<ans<<"\n";
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
## C. Zhily and Bracket Swapping
### https://codeforces.com/contest/2224/problem/C
「(」と「)」囲まれた2つの文字列。文字列の $ s_i $ と $ s2_i $ が交換できます。もし正しい括弧が生成できるなら「YES」を出力します。
ポイント
「s」と「s2」の同じ場所の文字は同じなら変化できません、違い文字は互い違いに配置する。
こうして、最後チェックして答えを出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

bool check(string temp)
{
    int l=0,r=0;
    for(int i=0;i<temp.size();i++)
    {
        if(temp[i]=='(')
        l++;
        if(temp[i]==')')
        {
            r++;
            if(r>l)return false;
        }
    }
    if(l==r)
    return true;
    else
    return false;
}

void solve()
{
    int n;
    cin>>n;
    string a,b,temp1="",temp2="";
    cin>>a>>b;
    bool flag=false;
    for(int i=0;i<n;i++)
    {
        if(a[i]==b[i])
        {
            temp1+=a[i];
            temp2+=b[i];
        }
        else
        {
            if(flag)
            {
                temp1+='(';
                temp2+=')';
                flag=!flag;
            }
            else
            {
                temp1+=')';
                temp2+='(';
                flag=!flag;
            }
        }
    }
    if(check(temp1)&&check(temp2))
    {
        cout<<"Yes"<<"\n";
        return;
    }
    temp1="";
    temp2="";
    flag=true;
    for(int i=0;i<n;i++)
    {
        if(a[i]==b[i])
        {
            temp1+=a[i];
            temp2+=b[i];
        }
        else
        {
            if(flag)
            {
                temp1+='(';
                temp2+=')';
                flag=!flag;
            }
            else
            {
                temp1+=')';
                temp2+='(';
                flag=!flag;
            }
        }
    }
    if(check(temp1)&&check(temp2))
    {
        cout<<"Yes"<<"\n";
    }
    else
    {
        cout<<"No"<<"\n";
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
