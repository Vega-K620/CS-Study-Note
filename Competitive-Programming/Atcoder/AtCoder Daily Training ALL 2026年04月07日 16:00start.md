# AtCoder Daily Training ALL 2026/04/07 16:00start
今日はトレーニングの途中で始めて、EFの正解もたくさんの時間掛かって、今日ただEF問が解きました。少し残念だと思います。これからもちゃんと精進しなきゃ。
## E - Tile Distance 2
https://atcoder.jp/contests/adt_all_20260407_1/tasks/abc359_c
この問題僕は最初簡単に「y」の変化と「x」は $y<x$ の状況で「x/2」は答えだと思いますが実は間違いです。
正解は座標変換の考え方です。
まずは座標を補正をします。その後上下移動 ($dy$): 1歩進むごとに必ず別のタイルに入るため、コストは $1$。左右移動 ($dx$): タイルが $2 \times 1$ なので、2マス進んでコストが $1$ 。  
起点 $(sX, sY)$ と終点 $(tX, tY)$ の差を $dx = |sX - tX|, dy = |sY - tY|$ とすると  
$$\text{Ans} = \begin{cases} dy & (dx \le dy \text{ のとき}) \\ dy + \frac{dx - dy}{2} & (dx > dy \text{ のとき}) \end{cases}$$
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    ll sx,sy,tx,ty;
    cin>>sx>>sy>>tx>>ty;
    ll sX, sY, tX, tY;
    sY=sy;
    if((sx+sy)%2!=0)
    sX=sx-1;
    else
    sX =sx;
    tY=ty;
    if((tx+ty)%2!=0)
    tX=tx-1;
    else
    tX=tx;
    ll dy=abs(sY-tY);
    ll dx=abs(sX-tX);
    if(dx<=dy)
    cout<<dy<<"\n";
    else
    cout<<dy+(dx-dy)/2<<"\n";
    return 0;
}
```
## F - Buy Balls
### https://atcoder.jp/contests/adt_all_20260407_1/tasks/abc396_c
この問題は「貪欲法」の問題です。まず二つの配列を降順にします。そして正の黒玉を優先的に選び、その後に「白玉単体」または「黒玉＋白玉のペア」で正の寄与（正の合計値）**があるものを選んでいきます。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
bool cmp(ll a,ll b)
{
    return a>b;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    vector<ll> b(n),w(m);
    for(int i=0;i<n;i++)
    cin>>b[i];
    for(int i=0;i<m;i++)
    cin>>w[i];
    sort(b.begin(),b.end(),cmp);
    sort(w.begin(),w.end(),cmp);

    ll ans=0;
    int last=min(n,m);
    for(int i=0;i<min(n,m);i++)
    {
        if(b[i]+w[i]>0&&w[i]>=0)
        {
            ans+=b[i]+w[i];
        }
        else
        {
            last=i;
            break;
        }
    }
    for(int i=last;i<n;i++)
    {
        if(b[i]>0)
        ans+=b[i];
        else
        break;
    }
    cout<<ans<<"\n";
    return 0;
}
```
