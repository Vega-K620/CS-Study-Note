# AtCoder Daily Training ALL 2026/04/27 16:00start
今日はE-Gを解きました。G問題に新しいアルゴリズムを勉強しました。
## E - Even Digits
### https://atcoder.jp/contests/adt_all_20260427_1/tasks/abc336_c
この問題は主に進数変換の考え方を問うものです。  
使用される数字が $(0, 2, 4, 6, 8)$ の 5 種類しかないことに注目すると、 $N-1$ を 5 進法に変換するだけで解くことができます。  
具体的には、5 進法で表した各桁の数字を、順番に $(0, 2, 4, 6, 8)$ へとマッピング（対応付け）すれば答えが得られます。  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    ll n;
    cin>>n;
    if(n-1==0)
    {
        cout<<0<<"\n";
        return 0;
    }
    vector<ll> temp;
    temp.push_back(1);
    for(int i=0;temp[i]<=n;i++)
    {
        temp.push_back(temp[i]*5);
    }

    string ans="";
    n-=1;
    for(int i=temp.size()-1;i>=0;i--)
    {
        ans+=(char)((n/temp[i])+'0');
        n%=temp[i];
    }

    bool start=false;
    for(int i=0;i<ans.size();i++)
    {
        if(ans[i]!='0')
        start=true;
        if(start)
        {
            if(ans[i]=='0')
            cout<<0;
            else if(ans[i]=='1')
            cout<<2;
            else if(ans[i]=='2')
            cout<<4;
            else if(ans[i]=='3')
            cout<<6;
            else
            cout<<8;
        }
    }
    cout<<"\n";
    return 0;
}
```
## F - Various Kagamimochi
### https://atcoder.jp/contests/adt_all_20260427_1/tasks/abc388_c
この問題は、条件を満たす餅の組み合わせを**二分探索（Binary Search）**を利用して効率的に数え上げることで解決できます。  
具体的には、各餅 $A_i$ を「下の餅」としたとき、その上に乗せることができる「上の餅」（大きさが $A_i/2$ 以下のもの）がいくつあるかを求めます。  
配列が昇順にソートされているため、upper_bound を使って境界となる位置を素早く特定し、そのインデックスから条件を満たす餅の総数を加算していくことで、 $O(N \log N)$ で正解を導き出すことができました。  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<int> A(n);
    for(int i=0;i<n;i++)
    {
        cin>>A[i];
    }

    ll ans=0;
    for(int i=0;i<n;i++)
    {
        int limit=A[i]/2;
        auto it=upper_bound(A.begin(),A.end(),limit);
        ans+=(it-A.begin());
    }
    cout<<ans<<"\n";
    return 0;
}
```
## G - Restricted Permutation
### https://atcoder.jp/contests/adt_all_20260427_1/tasks/abc223_d
この問題では、今日新しく学んだ**トポロジカルソート（Topological Sort）**というアルゴリズムを使用しました。これは、依存関係や順序の制約がある要素を並べる際に非常に有効です。  
具体的な手順としては、まずグラフを用いて要素間の制約を記録し、各ノードの**入次数（indegree）を配列で管理します。入次数が 0 になった（＝制約がなくなった）ノードから順に優先度付きキュー（priority_queue）**に追加していきます。  
今回は「辞書順で最小」という条件があるため、優先度付きキューを使って常に最小の値を取り出すように工夫しました。キューから要素を取り出す際に、その要素に依存していた先のノードの入次数を更新し、新たに入次数が 0 になったものをキューに追加していくことで、すべての順序を決定できます。もし最終的な結果が $N$ 個に満たない場合は、サイクルが存在するため「-1」を出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    vector<vector<int>> xianzhi(n+5);
    vector<int> front(n+5,0);
    priority_queue<int,vector<int>,greater<int>> num;

    for(int i=0;i<m;i++)
    {
        int a,b;
        cin>>a>>b;
        front[b]++;
        xianzhi[a].push_back(b);
    }

    for(int i=1;i<=n;i++)
    {
        if(front[i]==0)
        num.push(i);
    }
    vector<int> ans;
    while(!num.empty())
    {
        int i=num.top();
        ans.push_back(i);
        num.pop();
        for(auto j:xianzhi[i])
        {
            front[j]--;
            if(front[j]==0)
            num.push(j);
        }
    }
    
    if(ans.size()!=n)
    cout<<-1<<"\n";
    else
    {
        for(int i=0;i<ans.size();i++)
        cout<<ans[i]<<(i==ans.size()-1?"\n":" ");
    }
    return 0;
}
```
