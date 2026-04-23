# Educational Codeforces Round 189 (Rated for Div. 2)
今日codeforcesのdiv.2を参加しました。A-D問題が解きました。
## A. A Number Between Two Others
### https://codeforces.com/contest/2225/problem/A
この問題簡単です。もしxはyの三倍以上、答えの数字が見つける、その時「Yes」を出力します。ないと「No」を出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

void solve()
{
    ll a,b;
    cin>>a>>b;
    if(b/a>2)
    {
        cout<<"Yes"<<"\n";
    }
    else
    cout<<"No"<<"\n";
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
## B. Alternating String
### https://codeforces.com/contest/2225/problem/B
この問題は貪欲法です。例を見て発見した規律、もし文字列の中に同じ対(例えば「aa」「bb」)が三個以上なら変化出来ないです。だから文字列を走査して同じ対の数量をメモして、最後に判断します。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

void solve()
{
    string str;
    cin>>str;
    int temp=0;
    for(int i=0;i<str.size()-1;i++)
    {
        if(str[i]==str[i+1])
        temp++;
    }
    if(temp<=2)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
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
## C. Red-Black Pairs
### https://codeforces.com/contest/2225/problem/C
この問題は動的計画法です。二つの文字列を同時に走査して、「DP」を更新します。状态转移方程は $ dp[i]=min(dp[i],dp[i-1]+costV); $ と $ dp[i]=min(dp[i],dp[i-2]+costH); $
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

void solve()
{
    ll n;
    cin>>n;
    string str1,str2;
    cin>>str1>>str2;
    vector<int> dp(n+1,1e9);
    dp[0]=0;
    for(int i=1;i<=n;i++)
    {
        int costV=(str1[i-1]==str2[i-1]?0:1);
        dp[i]=min(dp[i],dp[i-1]+costV);
        if(i>=2)
        {
            int costH=(str1[i-2]==str1[i-1]?0:1)+(str2[i-2]==str2[i-1]?0:1);
            dp[i]=min(dp[i],dp[i-2]+costH);
        }
    }
    cout<<dp[n]<<"\n";
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
## D. Exceptional Segments
### https://codeforces.com/contest/2225/problem/D
この問題の鍵は、連続する整数のXOR和の性質にあります。まず前にの数をそれぞれ計算して、規律が簡単に見つける。以下の周期性を持ちます。  
$m \equiv 0 \pmod 4 \implies f(m) = m$  
$m \equiv 1 \pmod 4 \implies f(m) = 1$  
$m \equiv 2 \pmod 4 \implies f(m) = m+1$  
$m \equiv 3 \pmod 4 \implies f(m) = 0$  
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll=long long;

const ll MOD=998244353;
ll mod(ll R,ll rem)
{
    if(R<rem) return 0;
    return(R-rem)/4+1;
}

void solve()
{
    ll n,x;
    cin>>n>>x;
    ll a0=(1+mod(x-1,3)%MOD)%MOD;
    ll r0=(mod(n,3)%MOD-mod(x-1,3)%MOD+MOD)%MOD;
    ll a1=mod(x-1,1)%MOD;
    ll r1=(mod(n,1)%MOD-mod(x-1,1)%MOD+MOD)%MOD;

    ll ans=(a0*r0)%MOD+(a1*r1)%MOD;
    ans%=MOD;
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
