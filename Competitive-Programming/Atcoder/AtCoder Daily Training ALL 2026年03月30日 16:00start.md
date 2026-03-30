# AtCoder Daily Training ALL 2026/03/30 16:00start
今日のトレーニングはA-F解きました。解題をメモします。
## A - 321-like Checker
### https://atcoder.jp/contests/adt_all_20260330_1/tasks/abc321_a
文字列で入力して、それは狭義単調減少かどうか簡単に判断できる。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int num;
    cin>>num;
    int len=0;
    int copy=num;
    while(copy>0)
    {
        copy/=10;
        len++;
    }
    for(int i=0;i<len-1;i++)
    {
        if(num[i]<=num[i+1])
        {
            cout<<"No"<<"\n";
            return 0;
        }
    }
    cout<<"Yes"<<"\n";
    return 0;
}

```
## B - Jogging
### https://atcoder.jp/contests/adt_all_20260330_1/tasks/abc249_a
これは単純なシミュレーションですが、最初問題の意味を正解的に理解していませんから、「WA」しました。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int A,B,C,D,E,F,X;
    cin>>A>>B>>C>>D>>E>>F>>X;

    int takahashi=0;
    int cycle_t=A+C;
    int full_cycles=X/cycle_t;
    int remainder=X%cycle_t;
    takahashi=full_cycles*A*B;
    if(remainder>A)
    takahashi+=A*B;
    else
    takahashi+=remainder*B;
    int aoki=0;
    int cycle_a=D+F;
    full_cycles=X/cycle_a;
    remainder=X%cycle_a;
    
    aoki=full_cycles*D*E;
    if(remainder>D)
    aoki+=D*E;
    else
    aoki+=remainder*E;
    if(aoki==takahashi)
    cout<<"Draw"<<"\n";
    else if(takahashi>aoki)
    cout<<"Takahashi"<<"\n";
    else
    cout<<"Aoki"<<"\n";
        
    return 0;
}
```
## C - Piano 3
### https://atcoder.jp/contests/adt_all_20260330_1/tasks/abc369_b
これは簡単に毎回の「疲労度」を累加して、答えが出てきます。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<int> l;
    vector<int> r;
    for(int i=0;i<n;i++)
    {
        int num;
        char way;
        cin>>num>>way;
        if(way=='L')
        l.push_back(num);
        if(way=='R')
        r.push_back(num);
    }
    int suml=0;
    int sumr=0;
    for(int i=0;i+1<l.size();i++)
    {
        suml+=abs(l[i]-l[i+1]);
    }
    for(int i=0;i+1<r.size();i++)
    {
        sumr+=abs(r[i]-r[i+1]);
    }
    cout<<suml+sumr<<"\n";
    return 0;
}
```
## D - Second Best
### https://atcoder.jp/contests/adt_all_20260330_1/tasks/abc365_b
「struct」を利用して、「ID」と「数字」をメモして、「sort」の後第二番の「ID」出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;
struct p{
    int id;
    int num;
};
bool cmp(p a,p b)
{
    return a.num>b.num;
}
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<p> num(n);
    for(int i=0;i<n;i++)
    {
        cin>>num[i].num;
        num[i].id=i+1;
    }
    sort(num.begin(),num.end(),cmp);
    cout<<num[1].id<<"\n";
    return 0;
}
```
## E - Equilateral Triangle
### https://atcoder.jp/contests/adt_all_20260330_1/tasks/abc409_c
各座標の出現回数をカウントし、正三角形の条件を確認する。各地点 $d_i$ を累積（足し合わせる）して、各座標にいくつ点が存在するかを配列 cnt に格納します。正三角形が成立するためには、円周 $L$ が 3 で割り切れる必要があり、かつ 3 点の距離がすべて $L/3$ である必要があります。
```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int N,L;
    cin>>N>>L;
    vector<int> d(N-1);
    for(int i=0;i<N-1;++i)
    cin>>d[i];
    vector<int> cnt(L,0);
    int pos=0;
    cnt[pos]++;
    for(int i=0;i<N-1;++i)
    {
        pos=(pos+d[i])%L;
        cnt[pos]++;
    }
    if(L%3!=0)
    {
        cout<<0<<'\n';
        return 0;
    }
    int step=L/3;
    long long ans=0;
    for(int p=0;p<step;++p)
    {
        int a=cnt[p];
        int b=cnt[(p+step)%L];
        int c=cnt[(p+2*step)%L];
        ans+=1LL*a*b*c;
    }
    cout<<ans<<'\n';
    return 0;
}
```
## F - Sugoroku Destination
### https://atcoder.jp/contests/adt_all_20260330_1/tasks/abc445_c
この問題は前にABCコンテストに見たことがあいまして、方法も覚えていますので、簡単に解きました。
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int N;
    cin >> N;
    vector<int> A(N+1);
    for(int i=1;i<=N;i++)
    {
        cin>>A[i];
    }
    
    vector<int> ans(N+1,-1);
    function<int(int)> dfs=[&](int x)
    {
        if(ans[x]!=-1)
        return ans[x];
        if(A[x]==x)
        {
            return ans[x]=x;
        }
        return ans[x]=dfs(A[x]);
    };
    for(int i=1;i<=N;i++)
    {
        cout<<dfs(i)<<(i==N?"\n":" ");
    }
    
    return 0;
}
```
