# AtCoder Daily Training ALL 2026/03/25 18:00start
今日のADTはA-E解きました。たくさんのミスがあるので、ちゃんと復習しないと。
## A - Thermometer
### https://atcoder.jp/contests/adt_all_20260325_2/tasks/abc397_a
この問題は「以上」「未満」がありまして、注意しませんでしたから、たくさんの「WA」が手に入れてしました。大体この問題で10分ぐらい掛かりました。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie()->sync_with_stdio(false);
    double n;
    cin>>n;
    if(n<37.5)
    cout<<3<<"\n";
    else if(n>=38.0)
    cout<<1<<"\n";
    else
    cout<<2<<"\n";
    return 0;
}
```
## B - Stage Clear
### https://atcoder.jp/contests/adt_all_20260325_2/tasks/abc422_a
この問題は単純なシミュレーションです。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie()->sync_with_stdio(false);
    int i,j;
    char c;
    cin>>i>>c>>j;
    if(j<8)
    {
        cout<<i<<c<<j+1<<"\n";
        return 0;
    }
    else if(i<8&&j==8)
    {
        cout<<i+1<<c<<1<<"\n";
        return 0;
    }
    else 
    {
        cout<<8<<c<<8<<"\n";
        return 0;
    }
    return 0;
}
```
## C - Permute to Minimize
### https://atcoder.jp/contests/adt_all_20260325_2/tasks/abc432_b
この問題は昇順にソートしたあと、先頭の 0 を処理する。これは一番簡単の方法だと思います。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie()->sync_with_stdio(false);
    string s;
    cin>>s;
    sort(s.begin(),s.end());
    if(s[0]=='0')
    {
        for(int i=0;i<(int)s.size();i++)
        {
            if(s[i]!='0')
            {
                swap(s[0],s[i]);
                break;
            }
        }
    }
    cout<<s<<"\n";
    return 0;
}
```
## D - Four Hidden
### https://atcoder.jp/contests/adt_all_20260325_2/tasks/abc403_b
$T$ の中から $U$ と一致する可能性のある連続部分文字列を全探索します。'?' はどの文字にもなれるため、T[i+j] == '?' または T[i+j] == U[j] であれば一致判定とします。
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie()->sync_with_stdio(false);
    string T,U;
    cin>>T>>U;

    int n=(int)T.size();
    int m=(int)U.size();
    bool possible=false;

    for(int i=0;i<=n-m;i++)
    {
        bool ok=true;
        for(int j=0;j<m;j++)
        {
            if(T[i+j]!='?'&&T[i+j]!=U[j])
            {
                ok=false;
                break;
            }
        }
        if(ok)
        {
            possible=true;
            break;
        }
    }
    if(possible) 
    cout<<"Yes"<<"\n";
    else 
    cout<<"No"<<"\n";
    return 0;
}
```
## E - Various Kagamimochi
### https://atcoder.jp/contests/adt_all_20260325_2/tasks/abc388_c
二分探索を使って、条件を満たす境界線を素早く見つけます。餅の大きさを比較する際、どちらが上でどちらが下かを間違えないように注意します。
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie()->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<int> a(n);
    for(int i=0;i<n;i++) cin>>a[i];
    long long ans=0;
    for(int i=0;i<n;i++)
    {
        int target=a[i]/2;
        auto it=upper_bound(a.begin(),a.begin()+i,target);
        ans+=distance(a.begin(), it);
    }
    
    cout<<ans<<"\n";
    return 0;
}
```
