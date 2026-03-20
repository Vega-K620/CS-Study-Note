# AtCoder Daily Training ALL 2026/03/20 16:00start
A-Fまではスムーズでしたが、G問題で時間切れ。まだまだ精進が足りないと感じました。次はもっと早く解けるように頑張ります！
## A - Good morning
### https://atcoder.jp/contests/adt_all_20260320_1/tasks/abc245_a
この問題「if」判断です。時と分別々で判断します。答えがでてきます。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int a,b,c,d;
    cin>>a>>b>>c>>d;
    if(a==c)
    {
        if(b<=d)
        {
            cout<<"Takahashi"<<"\n";
        }
        else
        {
            cout<<"Aoki"<<"\n";
        }
    }
    else
    {
        if(a<c)
        {
            cout<<"Takahashi"<<"\n";
        }
        else
        {
            cout<<"Aoki"<<"\n";
        }
    }
    return 0;
}
```
## B - Spoiler
### https://atcoder.jp/contests/adt_all_20260320_1/tasks/abc344_a
この問題は「string」「erase」を活用する問題です。  
l , r は「|」の場所を記録して、「erase」を使って消去します。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string str,out;
    cin>>str;
    int l,r;
    for(int i=0;i<str.size();i++)
    {
        if(str[i]=='|')
        {
            l=i;
            break;
        }
    }
    for(int i=l+1;i<str.size();i++)
    {
        if(str[i]=='|')
        {
            r=i;
            break;
        }
    }
    str.erase(l,r-l+1);
    cout<<str<<"\n";
    return 0;
}
```
## C - 1122 String
### https://atcoder.jp/contests/adt_all_20260320_1/tasks/abc381_b
まず長さを判断すろ。もし2の倍数ではないなら直接Noを出力します。  
そして文字列を走査して、字の回数を記録する同時に、隣接も判断する。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string s;
    cin>>s;
    int map[26]={0};
    if(s.size()%2!=0)
    {
        cout<<"No"<<"\n";
        return 0;
    }
    else
    {
        for(int i=0;i<s.size();i+=2)
        {
            if(s[i]!=s[i+1]||map[s[i]-'a']!=0)
            {
                cout<<"No"<<"\n";
                return 0;
            }
            else
            {
                map[s[i]-'a']+=2;
            }
        }
        cout<<"Yes"<<"\n";
    }
    return 0;
}
```
## D - Cut .0
### https://atcoder.jp/contests/adt_all_20260320_1/tasks/abc367_b
この問題は直接に出力していいです。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    double num;
    cin>>num;
    cout<<num<<"\n";
    return 0;
}
```
## E - King's Summit
### https://atcoder.jp/contests/adt_all_20260320_1/tasks/abc419_c
最小時間は最大の距離の2点が掛かる時間です。  
ですから最もそとの点を記録して、答えが簡単に出てきます。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<pair<long long,long long>> xy(n);
    for(int i=0;i<n;i++)
    cin>>xy[i].first>>xy[i].second;
    long long leftest=LONG_LONG_MAX,highest=LONG_LONG_MAX,rightest=0,lowest=0;
    for(int i=0;i<n;i++)
    {
        lowest=max(lowest,xy[i].first);
        highest=min(highest,xy[i].first);
        leftest=min(leftest,xy[i].second);
        rightest=max(rightest,xy[i].second);
    }
    int ans=(max(lowest-highest,rightest-leftest)+1)/2;
    cout<<ans<<"\n";
    return 0;
}
```
## F - The Kth Time Query
### https://atcoder.jp/contests/adt_all_20260320_1/tasks/abc235_c
この問題は「map」と「vector」を組み合わせて活用します。  
数字 $x$ を key とし、その数字が出現したすべての位置を vector に記録します。クエリごとに vector のサイズを確認して、直接答えを出します。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    long long n,q;
    cin>>n>>q;
    map<long long,vector<int>> num;
    for(long long i=0;i<n;i++)
    {
        long long temp;
        cin>>temp;
        num[temp].push_back(i+1);
    }
    for(long long i=0;i<q;i++)
    {
        long long x,k;
        cin>>x>>k;
        if(num.count(x)&&num[x].size()>=k)
        {
            cout<<num[x][k-1]<<"\n";
        }
        else
        {
            cout<<-1<<"\n";
        }
    }
    return 0;
}
```
