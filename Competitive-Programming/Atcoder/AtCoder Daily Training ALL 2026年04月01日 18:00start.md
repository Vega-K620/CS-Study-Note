# AtCoder Daily Training ALL 2026/04/01 18:00start
今日の ADT では、あえて A-D までの基本問題を飛ばし、E-F-G という少し高難易度問題に挑戦しました。結果としてコンテスト時間内に AC することはできませんでしたが、その後の復習で非常に多くの「気づき」があったので、備忘録としてまとめます。
## E - K Swap
### https://atcoder.jp/contests/adt_all_20260401_2/tasks/abc254_c
最初は「隣接要素の入れ替え」による最小手数を求める問題だと条件反射的に思い込んでしまい、転倒数（Inversion Number）やバブルソートの応用で解こうとしてしまいました。しかし、この問題の本質は「手数」ではなく、**「最終的にソート可能か（到達可能性）」**を問うものでした。  
「無限に交換可能」かつ「歩長 $K$ が固定」：この条件を見た瞬間、インデックス $i$ と $j$ の間に $i \equiv j \pmod K$ という関係があることに気づくべきでした。この構造は、配列を $K$ 個の独立した「連通成分」に分割します。  
同じ剰余を持つ要素同士を抽出し、グループごとにソートする。
ソート後の要素を元の位置に「インターリーブ（交互に配置）」して戻す。
出来上がった配列が全体で昇顺になっているかチェックする。  
間違った実装：(全ヘビの合計長) - (特定のヘビまでの長さ) を計算してしまい、座標ではなく「そのヘビから後ろにどれだけ長さが残っているか」を求めてしまった。
絶対座標の保持: 累積和 S[i] を用いて、各ヘビが追加された瞬間の「不変の初期座標」を記録する。  
相対座標への変換: 現在の座標 = S[target] - S[head]。  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,k;
    cin>>n>>k;
    vector<ll> num(n);
    for(int i=0;i<n;i++)
    {
        cin>>num[i];
    }
    vector<vector<ll>> group(k);
    for(int i=0;i<n;i++)
    {
        group[i%k].push_back(num[i]);
    }
    for(int i=0;i<k;i++)
    {
        sort(group[i].begin(),group[i].end());
    }
    vector<ll> ans(n);
    vector<int> pr(k,0);
    for(int i=0;i<n;i++)
    {
        int need=i%k;
        ans[i]=group[need][pr[need]];
        pr[need]++;
    }
    for(int i=0;i<n-1;i++)
    {
        if(ans[i]>ans[i+1])
        {
            cout<<"No"<<"\n";
            return 0;
        }
    }
    cout<<"Yes"<<"\n";
    return 0; 
}
```
## F - Snake Queue
### https://atcoder.jp/contests/adt_all_20260401_2/tasks/abc389_c
この問題の考え方は完璧でした。「ヘビを動かさずに原点を動かす」というオフセットの概念も理解していました。しかし、いざコードを書く段になって、「累積和を使って座標を求める」 ロジックを、なぜか 「全体の長さから引いて残りの長さを求める」 という逆の処理にしてしまいました。  
ダブリングの構造:文字列が $[S, \text{flip}(S)]$ と倍々に増えていく構造は、本質的に**バイナリツリー（二分木）**と同じです。Thue-Morse 序列:インデックス $K$ が最終的に反転しているかどうかは、そのブロック番号 $G = (K-1)/|S|$ を 2 進数で表したときの 「1 の個数（popcount）」 の奇偶に依存します。    
スケールダウン: $K$ を $K-1$ にし、元々の文字列の長さ $|S|$ で割ることで、どの「ブロック」に属するかを特定する。  
ビットカウント: そのブロック番号に __builtin_popcountll を適用し、ビットの立っている数（右側の反転領域に落ちた回数）を数える。  
判定: ビット数が奇数なら大文字小文字を反転、偶数ならそのまま出力。  

```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    vector<ll> snake;
    snake.push_back(0);
    ll del=0;
    int q;
    cin>>q;
    while(q--)
    {
        ll d;
        cin>>d;
        if(d==1)
        {
            ll len;
            cin>>len;
            if(!snake.empty())
            {
                ll last=len+snake[snake.size()-1];
                snake.push_back(last);
            }
            else
            snake.push_back(len);
        }
        else if(d==2)
        {
            del++;
        }
        else if(d==3)
        {
            ll k;
            cin>>k;
            ll sum=snake[del+k-1]-snake[del];
            cout<<sum<<"\n";
        }
    }
    return 0; 
}
```
## G - Strange Mirroring
### https://atcoder.jp/contests/adt_all_20260401_2/tasks/abc380_d
問題文にある「$10^{100}$ 回の操作」という数字を見た瞬間、思考が停止してしまいました。しかし、競技プログラミングにおいて、「ありえないほど巨大な制約」は、実は「シミュレーションを捨てて、対数時間（$O(\log N)$）か定数時間（$O(1)$）で解け」という強力なヒントであることに気づくべきでした。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
char flip(char c)
{
    if(islower(c)) return toupper(c);
    return tolower(c);
}
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string s;
    cin>>s;
    ll l=s.length();
    int q;
    cin>>q;
    while(q--)
    {
        ll k;
        cin>>k;
        k--;
        ll group=k/l;
        ll res_idx=k%l;

        int count=__builtin_popcountll(group);

        char ans = s[res_idx];
        if (count%2==1)
        {
            ans=flip(ans);
        }
        cout<<ans<<(q==0?"":" ");
    }
    cout<<"\n";
    return 0; 
}
```
