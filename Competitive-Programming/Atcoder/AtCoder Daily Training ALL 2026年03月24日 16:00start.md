# AtCoder Daily Training ALL 2026/03/24 16:00start
今日のADTは40分ぐらい掛かってA-Fを解きました。 第BとE問題は少しmissしましたが、最後にACしました。良かったです。
## A - Too Many Requests
### https://atcoder.jp/contests/adt_all_20260324_1/tasks/abc429_a
Nまで走査して、答えを出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    for(int i=1;i<=n;i++)
    {
        if(i<=m)
        cout<<"OK"<<"\n";
        else
        cout<<"Too Many Requests"<<"\n";
    }
    return 0;
}
```
## B - 3.14
### https://atcoder.jp/contests/adt_all_20260324_1/tasks/abc314_a
この問題は少しずるいです。ながすぎので最初「double」と「acos(-1.0)」を使ってWAしました。結果問題文に100までの「PI」がありまして直接出力してACを手に入れる。
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    string pi = "3.1415926535897932384626433832795028841971693993751058209749445923078164062862089986280348253421170679";
    cout<<pi.substr(0,n+2)<<"\n";
    return 0;
}
```
## C - Extended ABC
### https://atcoder.jp/contests/adt_all_20260324_1/tasks/abc337_b
文字 $ 'A'<'B'<'C' $ ですから、「if」判断して直答えが出てきます。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string str;
    cin>>str;
    for(int i=0;i<(int)str.size()-1;i++)
    {
        if(str[i]>str[i+1])
        {
            cout<<"No"<<"\n";
            return 0;
        }
    }
    cout<<"Yes"<<"\n";
    return 0;
}
```
## D - Inverse Prefix Sum
### https://atcoder.jp/contests/adt_all_20260324_1/tasks/abc280_b
問題文で $ S_N-S_{(N-1)}=A_N $ が知っているからSを走査して答えを出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<long long> s(n);
    for(int i=0;i<n;i++)
    cin>>s[i];
    cout<<s[0]<<" ";
    for(int i=1;i<n;i++)
    cout<<s[i]-s[i-1]<<(i==n-1?'\n':' ');
    return 0;
}
``` 
## E - 343
### https://atcoder.jp/contests/adt_all_20260324_1/tasks/abc343_c
Nは $10^{18}$ 以下の正整数そして $K^3$ は $K=2097151$ のときlong longより多いですから、Kを走査して $K^3$ に回文を判断して。最後一番大きいの答えを出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    unordered_set<long long> num;
    for(long long i=1;i<=2097151;i++)
    {
        string str=to_string(i*i*i);
        string strc=str;
        reverse(str.begin(),str.end());
        if(str==strc)
        {
            num.insert(i*i*i);
        }
    }
    long long n;
    cin>>n;
    long long ans=-1;
    for(auto i:num)
    if(i<=n)
    ans=max(ans,i);
    cout<<ans<<"\n";
    return 0;
}
```
## F - Collision 2
### https://atcoder.jp/contests/adt_all_20260324_1/tasks/abc243_c
map<int, vector<Person>> を使って、同じ $y$ 座標（同じ行）の人たちだけを比較対象にしている点。各行で $x$ 座標が小さい順に並べることで、左から右への位置関係を明確にしている点。foundR を使って、「左側に R がいる状態で、右側に L が現れたか」を $O(N)$ で判定している点。
```cpp
#include<bits/stdc++.h>
using namespace std;
struct Person{
    int x;
    char way;
};
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<int> x(n),y(n);
    for (int i=0;i<n;i++) cin>>x[i]>>y[i];
    string s;
    cin >> s;

    map<int,vector<Person>> groups;
    for (int i=0;i<n;i++)
    {
        groups[y[i]].push_back({x[i],s[i]});
    }
    for (auto& [y_val, people] : groups)
    {
        sort(people.begin(),people.end(),[](const Person&a, const Person&b)
        {
            return a.x < b.x;
        });
        bool foundR=false;
        for(const auto& p:people)
        {
            if(p.way=='R')
            {
                foundR=true;
            }
            else if(p.way=='L'&&foundR)
            {
                cout<<"Yes"<<"\n";
                return 0;
            }
        }
    }
    cout<<"No"<<"\n";
    return 0;
}
```
