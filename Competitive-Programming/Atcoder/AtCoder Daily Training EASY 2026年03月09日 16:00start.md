# AtCoder Daily Training EASY 2026/03/09 16:00start
今日は ABC に参加し、A 題から E 題まで挑戦しました。コンテスト本番では A、B の 2 完に留まりましたが、終了後に C、D、E 題を自力で解き直し、無事に全て AC することができました。非常に学びの多い（というか罠の多い）一日でした。特に C 題は、実装中にもっとも試行錯誤し、結果として今日一番の収穫を得られた一問でした。 座標系の管理や $10^{18}$ 規模の数論的アプローチ、そして実装上の細かいミスについて、忘れないうちに復習してまとめておきます。
## A - 2UP3DOWN
### https://atcoder.jp/contests/adt_easy_20260309_1/tasks/abc326_a
この問題はとても残念です。僕は「2階分までの上り」「3階分までの下り」を、最初は「ちょうど2階」や「ちょうど3階」と誤解してしまった。　　
でも最後は正解を見つけました。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int x,y;
    cin>>x>>y;
    int d=y-x;
    if(d>=-3&&d<=2)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
    return 0;
}

```
## B - ASCII code
### https://atcoder.jp/contests/adt_easy_20260309_1/tasks/abc252_a
この問題は簡単だと思います。 ASCIIと数値の変換の問題です。  
中には　char 型から int 型へのキャストを使います。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    cout<<(char)(n-97+'a')<<"\n";
    return 0;
}
```
## C - Langton's Takahashi
### https://atcoder.jp/contests/adt_easy_20260309_1/tasks/abc339_b
トーラス状（上下左右が繋がっている）のグリッド上で、現在地のマスの色に応じて「色を反転 → 回転 → 前進」という操作を $N$ 回繰り返す。  
【主なハマりポイントと解決策】  
1. 座標系と方向配列の不一致グリッド問題では、通常 i（行）が縦方向、j（列）が横方向を指します。  
上 : (-1, 0)  
右 : (0, 1)  
下 : (1, 0)  
左 : (0, -1)  
この順序で配列を定義すれば、時計回りは (way + 1) % 4、反時計回りは (way + 3) % 4 でスマートに処理できます。  
2. トーラス状（ループ）の処理  
グリッドの端を超えた際に反対側へ戻る処理は、「(移動後の座標 + 周期) % 周期」 という形にするのが最も安全です。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int h,w,n;
    cin>>h>>w>>n;
    bool excel[105][105]={false};
    pair<int,int> now;
    now.first=0;now.second=0;
    int wayx[4]={-1,0,1,0};
    int wayy[4]={0,1,0,-1};
    int way=0;
    for(int i=0;i<n;i++)
    {
        if(excel[now.first][now.second]==false)
        {
            excel[now.first][now.second]=true;
            way++;
            way%=4;
            now.first+=wayx[way];
            now.second+=wayy[way];
            
        }
        else
        {
            excel[now.first][now.second]=false;
            way--;
            way+=4;
            way%=4;
            now.first+=wayx[way];
            now.second+=wayy[way];
        }
        now.first=(now.first+h)%h;
        now.second=(now.second+w)%w;
    }
    for(int i=0;i<h;i++)
    {
        for(int j=0;j<w;j++)
        {
            if(excel[i][j]==false)
            cout<<'.';
            else
            cout<<'#';
        }
        cout<<"\n";
    }
    return 0;
}
```
## D - Garbage Collection
### https://atcoder.jp/contests/adt_easy_20260309_1/tasks/abc378_b
周期 $q$、余り $r$ で定義されるゴミ収集日に対し、ある日 $d$ 以降で最も近い収集日を求める。  
【失敗点：相対値と絶対値】  
ロジック自体は正解に辿り着いていたが、出力すべき「日付（絶対値）」ではなく「あと何日か（相対値）」を出力してしまった。  
誤: ans = (target_r - cur_r)  
正: ans = d + (target_r - cur_r)  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<pair<int,int>> rule(n);
    for(int i=0;i<n;i++)
    cin>>rule[i].first>>rule[i].second;
    int q;
    cin>>q;
    while(q--)
    {
        int t,d;
        cin>>t>>d;
        int cur=d%rule[t-1].first;

        if(cur<=rule[t-1].second)
        {
            cout<<d+rule[t-1].second-cur<<"\n";
            continue;
        }
        else
        {
            cout<<d+rule[t-1].second-cur+rule[t-1].first<<"\n";
            continue;
        }
    }
    return 0;
}
```
## E - 2^a b^2
### https://atcoder.jp/contests/adt_easy_20260309_1/tasks/abc400_c
$X = 2^a \times b^2$ （$a, b \ge 1$）と表せる $1 \le X \le N$ の個数を求める。  
制約：$N \le 10^{18}$  
【解法ポイント】  
1. 探索範囲の圧縮  
$2^{60} > 10^{18}$ であるため、$a$ の範囲は $[1, 59]$ に限定されます。このため、外側のループはわずか 60 回で済みます。  
2. 一意性の構築（重複排除の工夫）  
普通に数えると、$X=16$（$2^4 \cdot 1^2$ と $2^2 \cdot 2^2$）のように重複が発生します。これを std::set で処理しようとすると、$10^{18}$ の範囲ではメモリと時間が足りません。  
解決策: $b$ を 奇数（Odd number） に限定します。  
理由: すべての良い整数 $X$ は $2^a \times b^2$ （$b$ は奇数）の形で**一意に（Uniquely）**表現できるからです。  
3. 精度への配慮  
$10^{18}$ 規模の計算では、pow 関数などの浮動小数点演算を避け、ビット演算や整数演算を使用します。また、sqrt の誤差を考慮し、微調整を行うのが安全です。
