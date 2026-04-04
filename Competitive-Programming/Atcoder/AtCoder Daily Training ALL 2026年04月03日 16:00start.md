# AtCoder Daily Training ALL 2026/04/03 16:00start
今日のトレーニングはE-Gを解きました。
## E - Minimum Glutton
### https://atcoder.jp/contests/adt_all_20260403_1/tasks/abc364_c
料理の個数を最小にするためには、甘さまたは塩辛さがより早く合計値を上回るように、それぞれを大きい順に並べるのが最適です。甘さの合計が $X$ を超える最短の手順と、塩辛さの合計が $Y$ を超える最短の手順を独立に考え、そのうち小さい方の値が答えになります。
```cpp
#include<bits/stdc++.h>
using namespace std;

bool cmpa(pair<int,int> a,pair<int,int> b)
{
    return a.first>b.first;
}
bool cmpb(pair<int,int> a,pair<int,int> b)
{
    return a.second>b.second;
}
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    long long n,x,y;
    cin>>n>>x>>y;
    vector<pair<long long,long long>> deal(n);
    vector<pair<long long,long long>> copya;
    vector<pair<long long,long long>> copyb;
    for(int i=0;i<n;i++)
    {
        cin>>deal[i].first;
    }
    for(int i=0;i<n;i++)
    {
        cin>>deal[i].second;
    }
    copya=copyb=deal;
    long long suma=0,sumb=0;
    long long ansa=0;
    sort(copya.begin(),copya.end(),cmpa);
    sort(copyb.begin(),copyb.end(),cmpb);
    long long i=0;

    while(suma<=x&&sumb<=y&&i<n)
    {
        suma+=copya[i].first;
        sumb+=copyb[i].second;

        ansa++;
        i++;
    }
    cout<<ansa<<"\n";
    return 0;
}
```
## F - Yamanote Line Game
### https://atcoder.jp/contests/adt_all_20260403_1/tasks/abc244_c
高橋君は先手ですから、必ず勝ちます。既に出た数字を「unordered_set」で管理し、自分の手番ではまだ使われていない最小の数字を宣言する、という操作を繰り返すことで、確実に勝利できます。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    int n;
    cin>>n;
    unordered_set<int> num;
    int aoki;
    cout<<1<<"\n";
    num.insert(1);
    while(cin>>aoki)
    {
        if(aoki==0)
        break;
        num.insert(aoki);
        for(int i=1;i<=2*n+1;i++)
        {
            if(num.count(i)==0)
            {
                cout<<i<<"\n";
                num.insert(i);
                break;
            }
        }
    }   
    return 0;
}
```
## G - Coprime 2
### https://atcoder.jp/contests/adt_all_20260403_1/tasks/abc215_d
$A_i$ を素因数分解して、出てきた素因数の倍数をエラトステネスの篩のように消していくことで、$O(N\sqrt{A_{max}} + M \log \log M)$ 程度の計算量で解くことができました。GCDを愚直に計算せず、素数に着目するのがポイントですね。
```cpp
#include <bits/stdc++.h>
using namespace std;
void get_prime_factors(int n, set<int>& factors)
{
    for(int i=2;i*i<=n;i++)
    {
        if(n%i==0)
        {
            factors.insert(i);
            while(n%i==0)n/=i;
        }
    }
    if(n>1)factors.insert(n);
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    vector<int> a(n);
    set<int> all_prime_factors;
    for (int i=0;i<n;i++)
    {
        cin>>a[i];
        get_prime_factors(a[i],all_prime_factors);
    }
    vector<bool> is_bad(m+1,false);

    for(int p:all_prime_factors)
    {
        for(int j=p;j<=m;j+=p)
        {
            is_bad[j]=true;
        }
    }
    vector<int> ans;
    for(int i=1;i<=m;i++)
    {
        if(!is_bad[i])
        {
            ans.push_back(i);
        }
    }
    cout<<ans.size()<<"\n";
    for(int x:ans)
    {
        cout<<x<<"\n";
    }
    return 0;
}

```
