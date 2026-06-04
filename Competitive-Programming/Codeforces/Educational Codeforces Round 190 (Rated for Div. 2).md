# Educational Codeforces Round 190 (Rated for Div. 2)
今回のコンテストはABC問題を解きました。ほとんど貪欲法で解けます。
## A. Optimal Purchase
### https://codeforces.com/contest/2230/problem/A
この問題は3つのケースに分けて考えます。  
1.全部個人利用のチケットを買います。  
2.全部団体利用のチケットを買います。  
3.まず団体利用のチケットを買って、残りは個人利用のチケットを買います。  
その中に一番安いのを選択します。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

void solve()
{
    ll n,a,b;
    cin>>n>>a>>b;
    ll temp1=n*a;
    ll temp2=(n+2)/3*b;
    ll temp3=n/3*b+n%3*a;
    cout<<min({temp1,temp2,temp3})<<"\n";
}

int main()
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
## B. Digit String
### https://codeforces.com/contest/2230/problem/B
この問題はまず4の倍数を書いて、1から4までの数字ですから、4の倍数はただ 4 12 24 32 を考えます。「beautiful」ですから、その4の倍数が並べる数字は削除しなければならない。でも 21 42 23 を削除しなくても大丈夫です。  
動の計画を使って簡単にできます。dp1の中には後ろからここまで削除しなければならない「2」を削除するの操作回数。dp0の中にはここまで削除しなければならない「1」と「3」を削除するの操作回数。そしてここまで最低の操作回数を更新します。
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

void solve()
{
    string s;
    cin>>s;
    int cnt4=0;
    int dp0=0;
    int dp1=0;

    for(char c:s)
    {
        if(c=='4')
        {
            cnt4++;
        }
        else if (c=='2')
        {
            dp1=dp1+1;
        }
        else
        {
            dp1=min(dp0, dp1);
            dp0=dp0+1;
        }
    }
    cout<<cnt4+min(dp0,dp1)<<"\n";
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
//4 12 32
```
## C. Arrange the Numbers in a Circle
### https://codeforces.com/contest/2230/problem/C
「連続する3枚の中に、少なくとも2枚の同じカードが必要」という条件を、カードの枚数ごとの役割に落とし込みます。  
枚数が2枚以上あるカード（temp >= 2）
これらはお互いに隣り合わせることで、簡単に条件を満たす塊を作れます（例：A, A, B, B, B など）。そのため、2枚以上あるカードはすべて（残さず）円に並べることができます。  
枚数が1枚しかないカード（temp == 1、単独カード）
単独カード X を置く場合、両隣が異なるカード（例：A, X, B）だと、トリプル (A, X, B) の中に同じカードが1枚もなくなってしまいアウトになります。
つまり、単独カードを救うためには、同じ種類のカードのペアで両側から挟み込む（例：A, A, X, A, A） 必要があります。
もし2枚以上あるカードは1種だけあれば、もう一つ枚数が1枚しかないカードが入れます。
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

void solve()
{
    int n;
    cin>>n;
    vector<int> num(n);
    int ans=0;
    int canuse=0,have1=0;
    for(int i=0;i<n;i++)
    {
        int temp;
        cin>>temp;
        num[i]=temp;
        if(temp>=3)
        canuse+=temp/2-1;
        if(temp==1)
        have1++;
        else ans+=temp;
    }
    if(have1==n-1&&n>1)
    {
        canuse++;
    }
    ans+=min(canuse,have1);
    if(ans<=2)
    cout<<0<<"\n";
    else
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
}
```
