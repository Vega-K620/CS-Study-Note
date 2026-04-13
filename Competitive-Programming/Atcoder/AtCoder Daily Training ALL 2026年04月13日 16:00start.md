# AtCoder Daily Training ALL 2026/04/13 16:00start
A〜E問題：
これらは「サインイン問題（セットアップ問題）」のような難易度で、特に詰まることなくスムーズに解き終えることができた。
F〜H問題：
E問題以降の4点は難易度がぐっと上がり、手応えがあった。残念ながらコンテスト（練習）時間内に全問解き切ることはできなかったが、粘り強く取り組んだ結果、バチャ（バーチャルコンテスト）後に無事自力で全てACまで持っていくことができた。
## A - 2UP3DOWN
### https://atcoder.jp/contests/adt_all_20260413_1/tasks/abc326_a
簡単の「if」判断です。 $ a-b>=-2&&a-b<=3 $　は判断式です。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int a,b;
    cin>>a>>b;
    if(a-b>=-2&&a-b<=3)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
    return 0;
}
```
## B - Weak Beats
### https://atcoder.jp/contests/adt_all_20260413_1/tasks/abc323_a
文字列を走査して、偶数の場所をチェックして、1があれば直ぐに「No」を出力します。ないと最後に「Yes」を出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string s;
    cin>>s;
    for(int i=1;i<s.size();i+=2)
    if(s[i]!='0')
    {
        cout<<"No"<<"\n";
        return 0;
    }
    cout<<"Yes"<<"\n";
    return 0;
}
```
## C - Postal Card
### https://atcoder.jp/contests/adt_all_20260413_1/tasks/abc287_b
「substr」を活用して文字列の末尾3文字を手に入れて判断して答えを出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int a,b;
    cin>>a>>b;
    vector<string> str(a);
    vector<string> strcheck(b);
    for(int i=0;i<a;i++)
    cin>>str[i];
    for(int i=0;i<b;i++)
    cin>>strcheck[i];
    int ans=0;
    for(int i=0;i<a;i++)
    {
        string temp=str[i].substr(3,3);
        if(find(strcheck.begin(),strcheck.end(),temp)!=strcheck.end())
        ans++;
    }
    cout<<ans<<"\n";
    return 0;
}
```
## D - Trimmed Mean
### https://atcoder.jp/contests/adt_all_20260413_1/tasks/abc291_b
評点をソートし、端の $N$ 個ずつを切り捨ててから残りの $3N$ 個の合計を求め、その平均を出力します。平均値を求める際、精度を保つために double 型を使用し、fixed と setprecision で誤差を許容範囲内に収めます。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<int> num(5*n);
    for(int i=0;i<5*n;i++)
    cin>>num[i];
    sort(num.begin(),num.end());

    double ans=0;
    for(int i=n;i<4*n;i++)
    ans+=num[i];

    ans/=(double)(3*n);
    cout<<fixed<<setprecision(10)<<ans<<"\n";
    return 0;
}
```
## E - Distance Indicators
### https://atcoder.jp/contests/adt_all_20260413_1/tasks/abc417_c
与えられた式 $j-i=A_i+A_j$ を変形すると、以下のような形になります。$$j-A_j=i+A_i$$ この変形により、左辺は $j$ のみに、右辺は $i$ のみに依存するようになります。つまり、すべての要素について「インデックス $k$ と値 $A_k$」から得られる $k-A_k$ と $k+A_k$ を計算し、一致するペアを数える問題に置き換えられます。  
$i < j$ という制約があるため、配列を走査しながら「それまでに現れた $i + A_i$ の値」を map や unordered_map で記録（カウント）していきます。現在の $j$ において、$j - A_j$ と等しい $i + A_i$ が過去にいくつあったかを答えに加算します。データの範囲が $2 \times 10^5$ なので、$O(N^2)$ の全探索では TLE しますが、ハッシュマップを使うことで $O(N)$ で解くことができます。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<int> num(n);
    vector<pair<int,int>> num2(n);
    for(int i=0;i<n;i++)
    cin>>num[i];
    for(int i=0;i<n;i++)
    {
        num2[i].first=num[i]+i;
        num2[i].second=i-num[i];
    }
    long long ans=0;
    unordered_map<int,int> have;
    for(int i=0;i<n;i++)
    {
        ans+=have[num2[i].second]; 
        have[num2[i].first]++;
    }
    cout<<ans<<"\n";
    return 0;
}
```
## F - 403 Forbidden
### https://atcoder.jp/contests/adt_all_20260413_1/tasks/abc403_c
各ユーザが閲覧できるページを unordered_set で管理します。  
閲覧権限の付与 (Type 1): person[X].insert(Y) で特定のページを追加します。  
全ページの権限付与 (Type 2): すべてのページをセットに入れると時間がかかるため、代わりに特殊なフラグとして -1 をセットに挿入します。  
判定 (Type 3): ユーザ X のセットに -1（全権限）または Y（個別権限）が含まれているかを count 関数で確認します。  
Type 2 の「全権限付与」をそのまま実装（全ページをループで追加）すると、最悪 $O(MQ)$ となり TLE してしまいます。-1 などの特殊な値で「全開放状態」をフラグ管理するのが効率化の鍵です。unordered_set を使うことで、クエリごとの平均計算量を $O(1)$ に抑えることができます。  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m,q;
    cin>>n>>m>>q;
    vector<unordered_set<int>> person(n+1);
    for(int i=0;i<q;i++)
    {
        int did;
        cin>>did;
        if(did==1)
        {
            int x,y;
            cin>>x>>y;
            person[x].insert(y);
        }
        else if(did==2)
        {
            int x;
            cin>>x;
            person[x].insert(-1);
        }
        else
        {
            int x,y;
            cin>>x>>y;
            if(person[x].count(-1)!=0)
            {
                cout<<"Yes"<<"\n";
                continue;
            }
            else
            {
                if(person[x].count(y)!=0)
                {
                    cout<<"Yes"<<"\n";
                    continue;
                }
                else
                cout<<"No"<<"\n";
            }
        }
    }
    return 0;
}
```
## G - General Weighted Max Matching
### https://atcoder.jp/contests/adt_all_20260413_1/tasks/abc318_d
$N \le 16$ と非常に小さいため、バックトラッキングを用いた探索（DFS）、もしくは**ビット DP（状態圧縮 DP）**で解くことができます。  
DFS の戦略:まだ選ばれていない頂点のうち、最も番号が小さいものを $u$ とします。  
$u$ を「どの辺の端点にもしない」場合と、「他の未割り当ての頂点 $v$ とペアにする」場合のすべてのパターンを再帰的に探索します。  
$N=16$ の場合、全てのペアを作る組み合わせ（完全マッチング）は $15 \times 13 \times 11 \dots \times 1 = 2,027,025$ 通り程度なので、制限時間内に十分間に合います。  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll = long long;

int n;
ll ans = 0;
vector<vector<ll>> weight;
vector<bool> used;

void dfs(ll sum)
{
    int u=-1;
    for(int i=0;i<n;i++)
    {
        if(!used[i])
        {
            u=i;
            break;
        }
    }
    if(u==-1)
    {
        ans=max(ans,sum);
        return;
    }
    for(int v=u+1;v<n;v++)
    {
        if(!used[v])
        {
            used[u]=used[v]=true;
            dfs(sum+weight[u][v]);
            used[u]=used[v]=false;
        }
    }
    used[u] = true;
    dfs(sum);
    used[u] = false;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    cin>>n;
    weight.assign(n,vector<ll>(n,0));
    used.assign(n,false);
    
    for(int i=0;i<n;i++)
    {
        for(int j=i+1;j<n;j++)
        {
            cin>>weight[i][j];
            weight[j][i]=weight[i][j];
        }
    }
    dfs(0);
    cout<<ans<<"\n";
    
    return 0;
}
```
## H - Water Tank
### https://atcoder.jp/contests/adt_all_20260413_1/tasks/abc359_e
この問題は、**単調スタック**を用いて、各仕切り板が「どこまでの範囲をカバーするか」を動的に管理することで、$O(N)$ で解くことができます。  
スタックの管理: スタックには {板の高さ, インデックス} を格納し、常に高さが減少するように維持します。  
貢献度の更新:  
新しい板 board[i] がスタックのトップより高い場合、スタックのトップ（低い板）をポップします。  
その際、その低い板がそれまで占めていた面積（貢献度）を合計値 pure_water から差し引きます（「貢献度の回戻し」）。  
板 board[i] をスタックに積む際、それが支配する範囲（前の高い板までの距離）に基づいて、新しい面積を pure_water に加算します。  
最終的な答えは pure_water + 1 となります。 
暴力的な解法（$O(N^2)$）ではスタックのコピーなどが発生し TLE しますが、**「古い貢献を引いて新しい貢献を足す」**という差分更新の考え方により、各要素を 1 回ずつ処理するだけで済みます。  
左側の境界を -1 と見なすことで、スタックが空になった場合の処理を簡潔に記述できます。  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<ll> board(n);
    stack<pair<ll,int>> pool;
    for(int i=0;i<n;i++)
    cin>>board[i];
    vector<ll> ans(n+1,0);
    ll pure_water=0; 
    for(int i=0;i<n;i++)
    {
        while(!pool.empty()&&pool.top().first<=board[i])
        {
            pair<ll,int> pooltemp = pool.top();
            pool.pop();
            int left_idx=pool.empty()?-1:pool.top().second;
            ll width=pooltemp.second-left_idx;
            pure_water-=pooltemp.first*width;
        }

        int left_idx=pool.empty()?-1:pool.top().second;
        ll current_width=i-left_idx;
        pure_water+=board[i]*current_width;
        pool.push({board[i],i});
        ans[i+1]=pure_water+1;
    }
    for(int i=1;i<=n;i++)
    cout<<ans[i]<<(i==n?"\n":" ");
    return 0;
}
```
