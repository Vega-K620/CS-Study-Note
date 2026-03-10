# AtCoder Daily Training EASY 2026/03/10 16:00start
ああ、今日はあんまり満足のいく結果じゃなかったな。何問か一発でAC（正解）できなくて、何度も修正してようやく通った感じ。時間をかなり無駄にしちゃったし、やっぱり演習不足で問題のパターンにまだ敏感になれていない気がする。
## A - Five Integers
### https://atcoder.jp/contests/adt_easy_20260310_1/tasks/abc268_a
失敗と反省  
最初は二重ループで要素を比較してカウントしようとしましたが、ループの境界条件（j < i-1）を書き間違えて、一部の重複を見逃してしまいました。  

解決策：set の活用  
「重複を除きたい」「ユニークな数を知りたい」ときは、自作ロジックよりも標準ライブラリの set を使うのが最も確実で速いです。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    set<int> num;
    int ans;
    for(int i=0;i<5;i++)
    {
        int num_1;
        cin>>num_1;
        num.insert(num_1);
    }
    ans=num.size();
    cout<<ans<<"\n";
    return 0;
}
```
## B - Majority
### https://atcoder.jp/contests/adt_easy_20260310_1/tasks/abc287_a
解法  
この問題は一発でACできました。解法としては、入力を受け取ると同時に『For』の出現回数をカウントし、最後に『For』の数と『Against』の数（ $N - \text{For}$ ）を比較するだけです。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    int For=0;
    for(int i=0;i<n;i++)
    {
        string str;
        cin>>str;
        if(str=="For")
        For++;
    }
    if(For<n-For)
    cout<<"No"<<"\n";
    else
    cout<<"Yes"<<"\n";
    return 0;
}
```
## C - Pentagon
### https://atcoder.jp/contests/adt_easy_20260310_1/tasks/abc333_b
失敗と反省  
この問題では、最初に『2点間の差をとる』という間違った方法を使ってしまいました。単純に文字コードの差が等しければ長さも等しいと考えたのですが、実際には五角形が環状であるため、差の結果は4パターン出てしまいます。そのため、少し工夫して距離を正規化することで正解にたどり着きました。  

考察と解法  
五角形の辺の長さは、実は以下の2種類しかありません：  
隣り合う辺（距離 1）: 例 AB, BC, CD, DE, EA (差は 1 または 4)  
対角線（距离 2）: 例 AC, BD, CE, AD, BE (差は 2 または 3)  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    char point[4];

    for(int i=0;i<4;i++)
    cin>>point[i];

    int d1=abs(point[0]-point[1]);
    int d2=abs(point[2]-point[3]);
    if (d1==4)d1=1;
    if (d2==4)d2=1;
    if (d1==3)d1=2;
    if (d2==3)d2=2;
    
    if(d1==d2)cout<<"Yes\n";
    else cout<<"No\n";
    return 0;
}
```
## D - Christmas Trees
### https://atcoder.jp/contests/adt_easy_20260310_1/tasks/abc334_b
失敗と反省  
この問題は、基準点 $A$ と区間 $[L, R]$ の位置関係によって3つのパターン（場合分け）が考えられますが、最初は $A$ が区間内にある場合しか考慮できていませんでした。また、端点（境界値）の扱いといったコーナーケースにも気づけておらず、苦戦しました。  

考察と解法  
数直線上で木が立つ座標は $A + kM$ です。これは $A$ を原点と見なせば「$M$ の倍数」を探す問題に帰着します。  
1. $A$ が区間内の場合 ($L \le A \le R$):  
$A$ から左側の本数：abs(A - L) / M  
$A$ から右側の本数：abs(R - A) / M  
合計：左 + 右 + 1 (基点の $A$ 自身)  
2. $A$ が区間の外側にある場合:  
例えば $A < L$ の場合、区間 $[L, R]$ 内の本数は「$A$ から $R$ までの本数」から「$A$ から $L-1$ までの本数」を引くことで求められます。  
ここで $L$ ではなく $L-1$ を使うのは、$L$ 地点に木がある場合にそれをカウントから除外しないためです。  
振り返り  
$N \ge 10^{18}$ の問題では、実際にシミュレーションすることが不可能なため、「どの範囲の値を引くか」という累積和的な考え方や、図を描いて境界値（ $L, R$ ）を含めるかどうかを慎重に確認する癖をつける必要があると感じました。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    long long a,m,l,r;
    cin>>a>>m>>l>>r;
    
    if(a>=l&&a<=r)
    {
        long long len1=abs(a-l)/m;
        long long len2=abs(a-r)/m;
        cout<<len1+len2+1<<"\n";
    }
    else if(a<l)
    {
        long long len1=abs(a-(l-1))/m;
        long long len2=abs(a-r)/m;
        cout<<len2-len1<<"\n";
    }
    else if(a>r)
    {
        long long len1=abs(a-l)/m;
        long long len2=abs(a-r-1)/m;
        cout<<len1-len2<<"\n";
    }
    return 0;
}
```
## E - digitnum
### https://atcoder.jp/contests/adt_easy_20260310_1/tasks/abc238_c
失敗と反省  
最後は E 問題です。最初は逆元を使う必要があると思い込んで、解法を複雑に考えてしまいました。また、最初に書いたコードは $1$ から $N$ まで一つずつ累積していく $O(N)$ のアルゴリズムだったため、実行時間制限を突破できませんでした。しかし、桁数ごとに区切って考えることで、等差数列の和として効率的に計算できることに気付きました。
考察と解法  
$f(x)$ の定義を整理すると、同じ桁数の範囲内では $1, 2, 3, \dots$ と増えていく等差数列になっていることがわかります。  
1桁 (1-9): $1, 2, \dots, 9$ の和 $\rightarrow$ $\frac{9 \times 10}{2}$  
2桁 (10-99): $1, 2, \dots, 90$ の和 $\rightarrow$ $\frac{90 \times 91}{2}$  
k桁: その桁の範囲にある個数を $L_k$ とすると、$1$ から $L_k$ までの和。  
これを $N$ の桁数まで繰り返すことで、計算量は $O(\log_{10} N)$ になり、一瞬で答えが出ます。
