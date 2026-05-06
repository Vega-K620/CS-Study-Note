# AtCoder Daily Training ALL 2026/05/06 18:00start
EF問題を解きました。
## E - Security 2
### https://atcoder.jp/contests/adt_all_20260506_2/tasks/abc407_c
この問題は、文字列 $S$ を後ろ（末尾）から逆算して考えると非常にシンプルになります。  
ターゲット $S$ から空文字列に戻す操作を考えます。逆操作A: 末尾の 0 を取り除く。（$S$ の末尾が 0 の時のみ可能）逆操作B: 全ての数字を $x \to (x-1) \mod 10$ に置き換える。  
隣り合う文字の差分を $d$ とすると、 $S[i] \ge S[i-1]$ のとき： $d = S[i] - S[i-1]$ $S[i] < S[i-1]$ のとき： $d = 10 + S[i] - S[i-1]$（これは (S[i] - S[i-1] + 10) % 10 で計算できます）これにボタンAの「追加」操作 (+1回) を加えると、各文字ごとのコストは：1 + (S[i] - S[i-1] + 10) % 10となります。  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string s;
    cin>>s;
    ll max_num=s.size();
    max_num+=s[s.size()-1]-'0';
    for(int i=s.size()-2;i>=0;i--)
    {
        max_num+=((s[i]-'0')-(s[i+1]-'0')+10)%10;
    }

    cout<<max_num<<"\n";
    return 0;
}
```
## F - T-shirts
### https://atcoder.jp/contests/adt_all_20260506_2/tasks/abc332_c
この問題の鍵は、「洗濯（'0'）から次の洗濯までの区間」を独立させて考えることです。  
条件1（競プロの日）: 競技プログラミングのイベント（'2'）の回数分は、必ずロゴ入りTシャツが必要です。  
条件2（Tシャツの総数）: 食事（'1'）と競プロ（'2'）の合計日数が、手持ちの無地Tシャツ $M$ 枚を超えた場合、その超えた分もロゴ入りTシャツで補う必要があります。  
ある区間の食事の日数を $x$ 、競プロの日数を $y$ とすると、その区間で必要なロゴ入りTシャツの枚数は： $$\max(y, (x + y) - M)$$ となります。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m,futu=0,rogu=0;
    cin>>n>>m;
    string s;
    cin>>s;
    s+='0';

    int ans=0;
    int x=0;
    int y=0;

    for(int i=0;i<s.size();i++)
    {
        if(s[i]=='1')
        {
            x++;
        }
        else if(s[i]=='2')
        {
            y++;
        }
        else
        {
            int need=max(y,(x+y)-m);
            ans=max(ans,need);
            x=0;
            y=0;
        }
    }
    cout<<ans<<"\n";
    return 0;
}
```
