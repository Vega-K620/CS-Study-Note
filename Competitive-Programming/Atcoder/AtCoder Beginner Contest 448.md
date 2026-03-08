# AtCoder Beginner Contest 448
今回のABCでは、A问题からD问题までの4问を无事に解き切ることができました。  
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
## C - Except and Min
### https://atcoder.jp/contests/abc448/tasks/abc448_c
この問題の最大の鍵は、**「取り出すボールの個数 $K$ が非常に小さい ( $K \le 5$ )」**という点にある。  
直感的な考察:袋の中の最小値を出したい。もし一番小さいボールが取り出されたら、二番目に小さいボールが最小値になる。もし二番目も取り出されたら、三番目……。  
結論:最大でも $K=5$ 個しか取り出さないので、**「元々の数列の中で小さい方から 6 個」**さえキープしておけば、その中のどれかが必ず袋の中に残っており、それが答えになる。  
アルゴリズムの進化： $O(NQ)$ から $O(Q \cdot K)$ へ  
失敗した解法 (暴力):クエリごとに $N$ 個の要素をスキャンすると $O(NQ)$ になり、今回の制約 ($N, Q \approx 10^5$) では TLE。  
成功した解法 (候補選出):最初に $N$ 個のボールを値でソートする。上位 6 個（ $K+1$ 個）のボールだけを「候補 (candidates)」として保持する。クエリごとに、候補の中で「取り出されていないもの」を小さい順にチェックし、最初に見つかったものが答え。  
1. 暴力のコード：  
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,q;
    cin>>n>>q;
    vector<int> num(n);
    for(int i=0;i<n;i++)
    cin>>num[i];
    while(q--)
    {
        int k;
        cin>>k;
        vector<bool> check(n,true);
        vector<int> num_k(k);
        for(int i=0;i<k;i++)
        {
            cin>>num_k[i];
            check[num_k[i]-1]=false;
        }

        int ans=INT_MAX;
        for(int i=0;i<n;i++)
        {
            if(check[i])
            {
                ans=min(ans,num[i]);
            }
        }
        cout<<ans<<"\n";
    }
    return 0;
}
```
2. 候補選出のコード：  
```cpp
#include<bits/stdc++.h>
using namespace std;

struct Ball{
    int val;
    int id;
};

bool cmp(const Ball& a, const Ball& b)
{
    return a.val < b.val;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,q;
    cin>>n>>q;
    vector<Ball> all_balls(n);
    for (int i=0;i<n;i++)
    {
        cin>>all_balls[i].val;
        all_balls[i].id=i+1;
    }

    sort(all_balls.begin(),all_balls.end(),cmp);

    int limit=min(n,6); 
    vector<Ball> candidates;
    for(int i=0;i<limit;i++)
    {
        candidates.push_back(all_balls[i]);
    }

    while(q--)
    {
        int k;
        cin>>k;
        vector<int> removed(k);
        for(int i=0;i<k;i++)
        cin>>removed[i];

        for(auto& candy:candidates)
        {
            bool is_removed=false;
            for(int r:removed)
            {
                if(candy.id==r)
                {
                    is_removed=true;
                    break;
                }
            }
            if(!is_removed)
            {
                cout<<candy.val<<"\n";
                break;
            }
        }
    }

    return 0;
}
```
## D - Integer-duplicated Path
### https://atcoder.jp/contests/abc448/tasks/abc448_d
問題の本質：パス上の状態管理  
この問題の難しい点は、「全ての頂点 $k$ について、根（頂点1）からのパス上に重複があるか」を判定すること。  
直感的なアプローチ: 各頂点ごとに毎回パスを辿ると $O(N^2)$ になり、TLE は避けられない。  
効率的な解法: DFSを使い、一本のパスを掘り進めながら状態を更新していく。  
アルゴリズムの鍵：バックトラッキング
DFS の再帰を利用して、「今通っているパス」に含まれる数字の出現回数を管理する。  
進む時 (Pre-order):現在の頂点の数字 A[u] を cnt（出現回数表）に加える。もし cnt[A[u]] が 2 になったら、重複カウンター dup_cnt を増やす。  
判定:dup_cnt > 0 ならば、その頂点までのパスには重複が存在する（Yes）。  
戻る時 (Post-order):ここが最重要！ 親ノードに戻る前に、現在の頂点の数字の影響を「消去」する。cnt[A[u]] を減らし、必要なら dup_cnt も戻す。
```cpp
#include<bits/stdc++.h>
using namespace std;

void dfs(int u, int p,vector<int>& A,vector<vector<int>>& adj,vector<string>& ans,map<int, int>& cnt,int& dup_cnt) {

    cnt[A[u]]++;
    if (cnt[A[u]] == 2) dup_cnt++;

    if(dup_cnt>0)
    ans[u]="Yes";
    else
    ans[u]="No";

    for(int v:adj[u])
    {
        if(v==p)
        continue;
        dfs(v,u,A,adj,ans,cnt,dup_cnt); 
    }

    if (cnt[A[u]]==2)
    dup_cnt--;
    
    cnt[A[u]]--; 
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int N;
    cin>>N;
    vector<int> A(N+1);
    vector<vector<int>> adj(N+1);
    vector<string> ans(N+1);
    for(int i=1;i<=N;i++)
    cin>>A[i];

    for(int i=0;i<N-1;i++)
    {
        int u,v;
        cin>>u>>v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    int dup_cnt = 0;
    map<int,int> cnt;
    dfs(1,-1,A,adj,ans,cnt,dup_cnt);

    for(int i=1;i<=N;i++)
    cout<<ans[i]<<"\n";

    return 0;
}
```
