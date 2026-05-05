# AtCoder Daily Training ALL 2026/05/05 16:00start
GWが終わって、今日からまた練習を再開しました。今日は肩慣らしにE題とF題を解きました。
## E - X drawing
### https://atcoder.jp/contests/adt_all_20260505_1/tasks/abc230_c
この問題は、一見 $10^{18}$ という制約を見て圧倒されましたが、よく考えると条件式を整理するだけで解ける問題でした。判定式を次のように変形すれば、単純な if-else 文で各マスを黒く塗るかどうか判定できます：  
$i - j = A - B$  
$i + j = A + B$  
巨大なグリッドをそのまま扱うのではなく、出力範囲内だけをループで回して、数式で判定するのがポイントでした。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    ll N,A,B;
    ll P,Q,R,S;

    cin>>N>>A>>B>>P>>Q>>R>>S;
    for(ll i=P;i<=Q;i++)
    {
        string ans="";
        for(ll j=R;j<=S;j++)
        {
            if((i-j==A-B)||(i+j==A+B))
            {
                ans+='#';
            }
            else
            {
                ans+='.';
            }
        }
        cout<<ans<<"\n";
    }
    return 0;
}
```
## F - Cards Query Problem
### https://atcoder.jp/contests/adt_all_20260505_1/tasks/abc298_c
この問題のポイントは、箱とカードの「双方向のインデックス」を管理することでした。  
クエリ2（箱の中身）は重複を許して昇順に出力するため、multiset を使用。  
クエリ3（カードが含まれる箱）は重複を除いて昇順に出力するため、set を使用。  
最初は全走査を考えてしまいましたが、空間計算量を少し使って二つのコンテナで管理することで、効率よくACできました。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<multiset<int>> box(n+1);
    vector<set<int>> card(200000+1);
    int q;
    cin>>q;
    while(q--)
    {
        int did;
        cin>>did;
        if(did==1)
        {
            int i,j;
            cin>>i>>j;
            box[j].insert(i);
            card[i].insert(j);
        }
        else if(did==2)
        {
            int i;
            cin>>i;
            bool start=true;
            for(auto j:box[i])
            {
                if(!start)
                cout<<" ";
                start=false;
                cout<<j;
            }
            cout<<"\n";
        }
        else
        {
            int i;
            cin>>i;
            bool start=true;
            for(auto j:card[i])
            {
                if(!start)
                cout<<" ";
                start=false;
                cout<<j;
            }
            cout<<"\n";
        }
    }
    return 0;
}
```
