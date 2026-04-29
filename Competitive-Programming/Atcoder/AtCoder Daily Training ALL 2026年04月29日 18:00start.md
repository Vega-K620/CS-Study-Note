# AtCoder Daily Training ALL 2026/04/29 18:00start
今日のトレーニングはE-Gを解きました。E問題で少し時間が掛かりましたが、F問題とG問題はそんな難しいじゃなくて簡単に解きました。
## E - Candy Tribulation
### https://atcoder.jp/contests/adt_all_20260429_2/tasks/abc432_c
この問題は僕にとって少し難しいです。式の誘導に、随分時間を費やしました
子供 $1,k$ について飴の総重量が等しいという条件は $$ (A_1 - x_1) X + x_1 Y = (A_k - x_k) X + x_k Y $$ と書けて，これを変形すると $$ x_k = x_1 - \frac{(A_k - A_1)X}{Y-X} $$ となります．
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);

    ll N,X,Y;
    cin>>N>>X>>Y;

    vector<ll> A(N);
    ll w_min=-1;
    ll w_max=-1;

    for(int i=0;i<N;i++)
    {
        cin>>A[i];
        ll cur_x=X*A[i];
        ll cur_y=Y*A[i];
        if(i==0)
        {
            w_min=cur_x;
            w_max=cur_y;
        }
        else
        {
            w_min=max(w_min,cur_x);
            w_max=min(w_max,cur_y);
        }
    }

    if(w_min>w_max)
    {
        cout<<-1<<"\n";
        return 0;
    }

    ll mod=Y-X;
    ll target_rem=(X*A[0])%mod;
    for(int i=1;i<N;i++)
    {
        if((X*A[i])%mod!=target_rem)
        {
            cout<<-1<<"\n";
            return 0;
        }
    }

    ll current_rem=w_max%mod;
    ll diff=(current_rem-target_rem+mod)%mod;
    ll w_best=w_max-diff;

    if(w_best<w_min)
    {
        cout<<-1<<"\n";
    }
    else
    {
        ll total_big_candies=0;
        for(int i=0;i<N;i++)
        {
            total_big_candies+=(w_best-X*A[i])/mod;
        }
        cout<<total_big_candies<<"\n";
    }

    return 0;
}
```
## F - Sum of Product
### https://atcoder.jp/contests/adt_all_20260429_2/tasks/abc405_c
この問題は簡単だと思います。全探索はかならず「TLE」なります。ですから、式の変形を使って、一回のループで計算できるようにしました。求める値は $\sum_{i<j} A_i A_j$ です。これは、$(\sum A_i)^2 = \sum A_i^2 + 2\sum_{i<j} A_i A_j$ という性質を利用すると、 $$\sum_{i<j} A_i A_j = \frac{(\sum A_i)^2 - \sum A_i^2}{2}$$ と変形できます。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    ll sum=0;
    int n;
    cin>>n;
    vector<ll> num(n);
    for(int i=0;i<n;i++)
    {
        cin>>num[i];
        sum+=num[i];
    }
    ll ans=0;
    for(int i=0;i<n;i++)
    {
        ans+=(sum-num[i])*num[i];
    }
    cout<<ans/2<<"\n";
    return 0;
}
```
## G - Sum of Differences
### https://atcoder.jp/contests/adt_all_20260429_2/tasks/abc437_d
ソートと累積和（るいせきわ）を組み合わせて、計算量を $O(NM)$ から $O((N+M)\log M)$ に改善しました。  
絶対値（ぜったいち）を外すために、配列 B をソートして二分探索（にぶんたんさく）を行いました。  
各 $A_i$ に対して、それより大きい要素と小さい要素を分けて計算することで、高速化（こうそくか）を図りました。
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int MOD=998244353;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);

    int n,m;
    cin>>n>>m;

    vector<ll> a(n),b(m);
    for(int i=0;i<n;i++) cin>>a[i];
    for(int i=0;i<m;i++) cin>>b[i];

    sort(b.begin(),b.end());

    vector<ll> prefB(m+1,0);
    for(int i=0;i<m;i++)
    {
        prefB[i+1]=prefB[i]+b[i];
    }

    ll total_ans=0;

    for (int i=0;i<n;i++)
    {
        int idx=upper_bound(b.begin(),b.end(),a[i])-b.begin();

        ll count_small=idx;
        ll sum_small=prefB[idx];
        ll res_small=(a[i]*count_small%MOD-sum_small%MOD+MOD)%MOD;

        ll count_large=m-idx;
        ll sum_large=prefB[m]-prefB[idx];
        ll res_large=(sum_large%MOD-a[i]*count_large%MOD+MOD)%MOD;

        total_ans=(total_ans+res_small+res_large)%MOD;
    }

    cout<<total_ans<<"\n";

    return 0;
}
```
