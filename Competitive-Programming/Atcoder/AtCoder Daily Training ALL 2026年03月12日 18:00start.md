# AtCoder Daily Training ALL 2026/03/12 18:00start
今日もALLのトレーニングに参加しました。A-Eの問題が解いました。  
残念ながら今日はDとE問題にたくさんの時間が掛かりました。  
## A - Move Right
### https://atcoder.jp/contests/adt_all_20260312_2/tasks/abc247_a
解い方法:  
この問題はまずSを入力します。そして「0」を出力して、「Sの長さ-1」を出力します。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string str;
    cin>>str;
    int len=(int)str.size();
    cout<<0;
    for(int i=0;i<len-1;i++)
    {
        cout<<str[i];
    }
    cout<<"\n";
    return 0;
}
```
## B - Streamer Takahashi
### https://atcoder.jp/contests/adt_all_20260312_2/tasks/abc414_a
解い方法:  
x1<=lとx2>=rなら「ans」を++します。最後に答えを出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,l,r;
    cin>>n>>l>>r;
    int ans=0;
    while(n--)
    {
        int x,y;
        cin>>x>>y;
        if(x<=l&&y>=r)
        {
            ans++;
        }
    }
    cout<<ans<<"\n";
    return 0;
}
```
## C - Most Minority
### https://atcoder.jp/contests/adt_all_20260312_2/tasks/abc420_b
 $N$ 人（奇数）で$M$回の投票を行い、各回で少数派になった人が1点を得るというルールです。  
ただし、全員が同じ選択（全0または全1）をした場合は、例外として全員が1点を得ます。  
最終的に合計得点が最も高かった人の番号をすべて出力する問題です。  
この問題のデータは「1人ずつの投票内容（行）」として与えられますが、得点の判定は「1回ごとの投票（列）」で行う必要があります。  
入力の持ち方: vector<string> s(n) で受け取り、s[i][j] で「$i$番目の人の$j$回目の投票」にアクセスします。  
列ごとの集計: 外側のループで j (0から$M-1$) を回し、内側のループで i (0から$N-1$) を回して、その回の 0 の個数を数えます。 
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    vector<string> s(n);
    for(int i=0;i<n;i++)
    cin>>s[i];
    vector<int> ans(n,0);
    for(int i=0;i<m;i++)
    {
        int have0=0;
        for(int j=0;j<n;j++)
        {
            if(s[j][i]=='0')
            have0++;
        }
        if(have0==0||have0==n)
        {
            for(int j=0;j<n;j++)
            ans[j]++;
        }
        else if(have0>n-have0)
        {
            for(int j=0;j<n;j++)
            {
                if(s[j][i]=='1')
                {
                    ans[j]++;
                }
            }
        }
        else
        {
            for(int j=0;j<n;j++)
            {
                if(s[j][i]=='0')
                {
                    ans[j]++;
                }
            }
        }
    }
    int maxnum=0;
    for(int i=0;i<n;i++)
    {
        maxnum=max(maxnum,ans[i]);
    }
    for(int i=0;i<n;i++)
    {
        if(maxnum==ans[i])
        cout<<i+1<<" ";
    }
    return 0;
}
```
##
### 
$N$ 個の頂点と $M$ 本の辺からなる無向グラフが与えられます。  
$1 \le a < b < c \le N$ を満たす頂点の組のうち、3つの頂点が互いに辺で結ばれている（三角形を形成している）ものの個数を求める問題です。  
解法のポイント  
この問題の最大のポイントは、制約の小ささにあります。$N \le 100$頂点を3つ選ぶ組み合わせは、最大でも ${}_{100}C_3 = \frac{100 \times 99 \times 98}{6} = 161,700$ 通りです。これはコンピュータにとっては非常に小さな数字なので、**3重ループによる全探索（$O(N^3)$）**で十分に間に合います。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    bool mapnum[101][101]={false};
    for(int i=0;i<m;i++)
    {
        int a,b;
        cin>>a>>b;
        mapnum[a][b]=mapnum[b][a]=true;
    }
    int ans=0;
    for(int a=1;a<=n;a++)
    {
        for(int b=a+1;b<=n;b++)
        {
            if(mapnum[a][b]==false)
            continue;
            for(int c=b+1;c<=n;c++)
            {
                if(mapnum[b][c]&&mapnum[c][a])
                ans++;
            }
        }
    }
    cout<<ans<<"\n";
    return 0;
}
```
## E - Σ
### https://atcoder.jp/contests/adt_all_20260312_2/tasks/abc346_c
1から $K$ までの整数のうち、与えられた数列 $A$ に一度も現れないものの総和を求める問題です。$K$ が最大 $2 \times 10^9$ と非常に大きいため、1から順にループを回して判定することはできません。  
解法のポイント：余事象を考える  
「現れないものの和」を直接求めるのが難しい場合、「全体の和」から「現れたものの和」を引くという考え方（余事象）が有効です。  
$$\text{求める和} = (\sum_{i=1}^{K} i) - (\text{数列 } A \text{ に現れる } K \text{ 以下のユニークな数の総和})$$  
全体の和を求める  
1から $K$ までの総和は、等差数列の和の公式を使って $O(1)$ で計算できます。$$\text{Sum} = \frac{K(K+1)}{2}$$ $K = 2 \times 10^9$ のとき、この値は約 $2 \times 10^{18}$ になるため、必ず long long 型を使用する必要があります。  
現れた数の処理数列  
$A$ の中には、$K$ より大きい数や重複した数が含まれている可能性があります。これらは無視しなければなりません。  
重複排除: std::set を使って、一度現れた数を管理します。  
範囲外の排除: $A_i \le K$ のものだけを set に入れます。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    long long n,k;
    cin>>n>>k;
    set<long long> num;
    for(long long i=0;i<n;i++)
    {
        long long temp;
        cin>>temp;
        if(temp<=k)
        num.insert(temp);
    }
    long long sum=0;
    long long maxnum=0;
    for(auto l: num)
    {
        sum+=l;
        maxnum=max(maxnum,l);
    }
    
    long long all=0;
    all=k*(k+1)/2;
    cout<<all-sum<<"\n";
    return 0;
}
```
