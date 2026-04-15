# AtCoder Daily Training ALL 2026/04/15 18:00start
今日のトレーニングでは、無事にAからGまでを完答することができました。  
A-E題はサインイン問題ので今回はコードだけメモします。  
## A - 369
### https://atcoder.jp/contests/adt_all_20260415_2/tasks/abc369_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int a,b;
    cin>>a>>b;
    if(a==b)
    {
        cout<<1<<"\n";
        return 0;
    }
    else if((a+b)%2==0)
    {
        cout<<3<<"\n";
        return 0;
    }
    else
    {
        cout<<2<<"\n";
        return 0;
    }
    return 0;
}
```
## B - Misdelivery
### https://atcoder.jp/contests/adt_all_20260415_2/tasks/abc421_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<string> room(n+1);
    for(int i=1;i<=n;i++)
    {
        cin>>room[i];
    }
    int a;
    string b;
    cin>>a>>b;
    if(room[a]==b)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
    return 0;
}
```
## C - Adjacency Matrix
### https://atcoder.jp/contests/adt_all_20260415_2/tasks/abc343_b
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    for(int i=0;i<n;i++)
    {
        bool outed=false;
        for(int j=0;j<n;j++)
        {
            int num;
            cin>>num;
            if(num==1)
            {
                outed=true;
                cout<<j+1<<" ";
            }
        }
        if(outed)cout<<"\n";
    }
    return 0;
}
```
## D - Bombs
### https://atcoder.jp/contests/adt_all_20260415_2/tasks/abc295_b
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

struct k{
    int r,c,power;
};

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int r,c;
    cin>>r>>c;
    vector<k> bomb;
    vector<vector<char>> excel(r,vector<char>(c));
    for(int i=0;i<r;i++)
    {
        for(int j=0;j<c;j++)
        {
            cin>>excel[i][j];
            if(excel[i][j]>='0'&&excel[i][j]<='9')
            {
                bomb.push_back({i,j,(int)(excel[i][j]-'0')});
            }
        }
    }
    vector<vector<bool>> is_destroyed(r, vector<bool>(c, false));
    for(auto &b:bomb)
    {
        for(int i=0;i<r;i++)
        {
            for(int j=0;j<c;j++)
            {
                if (abs(b.r-i)+abs(b.c-j)<=b.power)
                {
                    is_destroyed[i][j]=true;
                }
            }
        }
    }

    for(int i=0;i<r;i++)
    {
        for(int j=0;j<c;j++)
        {
            if(is_destroyed[i][j])
            cout<<'.';
            else
            cout<<excel[i][j];
        }
        cout<<'\n';
    }
    return 0;
}
```
## E - Route Map
### https://atcoder.jp/contests/adt_all_20260415_2/tasks/abc236_c
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    vector<string> s(n);
    unordered_map<string,bool> t(m);
    for(int i=0;i<n;i++)cin>>s[i];
    for(int i=0;i<m;i++)
    {
        string temp;
        cin>>temp;
        t[temp]=true;
    }
    for(int i=0;i<n;i++)
    {
        if(t.find(s[i])!=t.end())
        {
            cout<<"Yes\n";
        }
        else
        {
            cout<<"No\n";
        }
    }
    return 0;
}
```
## F - Index × A(Continuous ver.) 
### https://atcoder.jp/contests/adt_all_20260415_2/tasks/abc267_c
さ $N$ の数列 $A$ から、長さ $M$ の連続部分列 $B$ を選び、$\sum_{i=1}^{M} i \times B_i$ を最大化する問題。  
$N$ が最大 $2 \times 10^5$ であるため、愚直に計算すると $O(NM)$ となり間に合わない。**スライディングウィンドウ（差分更新）**を用いることで $O(N)$ で解くことができる。
現在のウィンドウの値を $S_k$、要素の総和を $Sum_k$ とすると、次のように更新できる。  
初期状態 (最初の $M$ 個):  
$cur\_sum = \sum_{i=0}^{M-1} A_i$  
$cur\_weight = \sum_{i=0}^{M-1} (i+1) \times A_i$  
ウィンドウを 1 つ右にずらす際の差分:  
新しいウィンドウは、前のウィンドウから $A_i$ を除き、$A_{i+M}$ を加えたものである。重み付き和の変動を考えると：  
$cur\_weight_{new} = cur\_weight_{old} - cur\_sum + M \times A_{i+M}$  
$cur\_sum_{new} = cur\_sum_{old} - A_i + A_{i+M}$  
この更新式により、各ステップ $O(1)$ で計算が可能となる。  

```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;

    vector<ll> a(n);
    for(int i=0;i<n;i++)
    {
        cin>>a[i];
    }

    ll cur_sum=0;
    ll cur_weight=0;
    
    for(int i=0;i<m;i++)
    {
        cur_sum+=a[i];
        cur_weight+=(ll)(i+1)*a[i];
    }
    ll ans = cur_weight;

    for(int i=0;i<n-m;i++)
    {
        cur_weight=cur_weight-cur_sum+(ll)m*a[i+m];
        cur_sum=cur_sum-a[i]+a[i+m];
        ans=max(ans,cur_weight);
    }
    cout<<ans<<"\n";
    return 0;
}
```
## G - Odd or Even
### https://atcoder.jp/contests/adt_all_20260415_2/tasks/abc313_d
$N$ 個の未知の 0/1 からなる数列 $A$ がある。一度の質問で $K$ 個の要素の和の偶奇（XOR 和）を知ることができる。質問回数 $N$ 回以内で全要素を特定するインタラクティブ問題。
※ $K$ は奇数であることが重要なヒント。  
まず、最初の $K+1$ 個の要素（$A_1, \dots, A_{K+1}$）を特定することを考える。
クエリの構築:  
$K+1$ 個の中から「1つだけ除いた $K$ 個の和」を $K+1$ 回質問する。  
$Q_1 = A_2 \oplus A_3 \oplus \dots \oplus A_{K+1}$  
$Q_2 = A_1 \oplus A_3 \oplus \dots \oplus A_{K+1}$  
...  
$Q_{i} = (\bigoplus_{j=1}^{K+1} A_j) \oplus A_i$  
全要素の和を求める:  
全ての $Q_i$ の合計（XOR）を $S$ とすると、各 $A_i$ は合計 $K$ 回ずつ加算される。$K$ が奇数であるため、$S = \bigoplus Q_i = \bigoplus_{i=1}^{K+1} A_i$ となる。  
個別の値を特定:  
$A_i = S \oplus Q_i$ によって、$A_1$ から $A_{K+1}$ までが確定する。  
残りの要素（$A_{K+2} \dots A_N$）:  
既に判明している $A_1, \dots, A_{K-1}$ （計 $K-1$ 個、これは偶数個）を固定のベースとして使い、残りの $A_i$ と組み合わせて質問すれば、1回につき1つずつ特定できる。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,k;
    cin>>n>>k;
    vector<bool> ans(n+1);
    vector<bool> temp(k+2);
    bool sum=0;
    int no=0;
    for(int i=0;i<k+1;i++)
    {
        cout<<'?'<<' ';
        
        for(int j=0;j<k+1;j++)
        {
            if(j==no)continue;
            cout<<j+1<<' ';
        }
        cout<<endl;
        no++;
        int in;
        cin>>in;
        if(in==-1)return 0;
        temp[i+1]=in;
        sum^=in;
    }
    for(int i=0;i<k+1;i++)
    {
        ans[i+1]=temp[i+1]^sum;
    }
    bool base_xor=0;
    for(int i=1;i<=k-1;i++)
    {
        base_xor^=ans[i];
    }
    for(int i=k+2;i<=n;i++)
    {
        cout<<'?'<<' ';
        for(int j=1;j<=k-1;j++)
        {
            cout<<j<<' ';
        }
        cout<<i<<endl;
        int in;
        cin>>in;
        if(in==-1) return 0;
        ans[i]=(bool)in^base_xor;
    }
    cout<<'!'<<' ';
    for(int i=1;i<=n;i++)
    cout<<ans[i]<<' ';
    cout<<endl;
    return 0;
}
```
