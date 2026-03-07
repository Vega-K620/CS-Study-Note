# AtCoder Beginner Contest 448
今回のABC（AtCoder Beginner Contest）は、自分にとって単なるスコア以上の収穫があった。特にD問題。最初、木構造のパス上の重複判定という壁にぶつかった時、正直「これは無理だ」と一瞬頭をよぎった。
しかし、そこからの50分間はまさに **「戦いながら学ぶ（習うより慣れろ）」**の連続だった。
**DFS（深さ優先探索）**の再帰構造を、ただの「呪文」ではなく「スタックと状態の管理」として脳内にビジュアライズできたこと。
vector の resize 忘れによる RE の恐怖から、メモリ管理の重要性を身をもって学んだこと。
そして、値の範囲が $10^9$ という絶望的な数字を、 **「座標圧縮（離散化）」** という魔法で $2 \times 10^5$ の手のひらサイズに収める発想に辿り着いたこと。
数論（E問題）の逆元と床関数の組み合わせにはまだ「数学的な壁」を感じるが、今日のD問題を自力で（しかも本番中に！）理解し、コードに落とし込めた経験は、今後の精進における大きな自信になるはずだ。
「死記硬背（暗記）」ではなく「直感的な理解」へ。今日のノートには、その激闘の軌跡を残しておきたい。
## A - chmin
### https://atcoder.jp/contests/abc448/tasks/abc448_a
この問題は、ただの比較ではなく、**「条件付きの状態更新」**を正確に行えるかを問うている。状態 (State): 変数 X。更新のルール:  $A_i < X$ の時のみ、新記録として X を書き換える。
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,x;
    cin>>n>>x;
    vector<int> num(n);
    for(int i=0;i<n;i++)
    {
        cin>>num[i];
        if(num[i]<x)
        {
            x=num[i];
            cout<<"1"<<"\n";
        }
        else 
        cout<<"0"<<"\n";
    }
    

    return 0;
}
```
## B - Pepper Addiction
### https://atcoder.jp/contests/abc448/tasks/abc448_b
問題の本質：リソース制限下の最大化
この問題の核心は、**「各料理が必要とする胡椒の総量」と「レストランにある胡椒の在庫」**をどう戦わせるかにある。
料理側の視点: 料理 $i$ は種類 $A_i$ の胡椒を最大 $B_i$ 必要とする。
在庫側の視点: 種類 $j$ の胡椒は $C_j$ しか存在しない。
アルゴリズムの着眼点：バケット集計個々の料理について考えるのではなく、**「胡椒の種類ごと」**にグループ化（バケツ分け）して考えるのが最善。
ステップ1 : 種類 $j$ の胡椒を欲しがっている全ての料理の需要合計 $D_j$ を計算する。
ステップ2 : 実際に出せる量は、需要 $D_j$ と在庫 $C_j$ のうち、小さい方（min）である。
ステップ3 : 全ての胡椒の種類について、その min を合計すれば答えになる。
料理一つ一つにどれだけかけるかをシミュレーションする必要はなく、**「種類ごとにまとめて考える」**という抽象化を行うだけで、問題がぐっとシンプルになった。
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    vector<int> type(m);
    for(int i=0;i<m;i++)
    cin>>type[i];
    vector<pair<int,int>> ryori(n);
    for(int i=0;i<n;i++)
    cin>>ryori[i].first>>ryori[i].second;

    vector<int> ireru(m+1,0);
    for(int i=0;i<n;i++)
    ireru[ryori[i].first]+=ryori[i].second;
    long long ans=0;
    for(int i=0;i<m;i++)
    {
        ans+=min(ireru[i+1],type[i]);
    }
    cout<<ans<<"\n";
    return 0;
}
```
今日はもう遅い時間だから残りCとDは明日で追加する。
