# AtCoder Daily Training ALL 2026/03/13 16:00start
今日はただA-Dの問題を解きました。まだまだ未熟ですが、頑張ります。
## A - Divisible
### https://atcoder.jp/contests/adt_all_20260313_1/tasks/abc347_a
解法  
数列の要素を一つずつ受け取ります。  
その要素が $K$ で割り切れるか（num % k == 0）を判定します。  
割り切れる場合は、その値を $K$ で割った結果を出力します。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,k;
    cin>>n>>k;
    for(int i=0;i<n;i++)
    {
        int num;
        cin>>num;
        if(num%k==0)
        cout<<num/k<<" ";
    }
    return 0;
}
```
## B - To Be Saikyo
### https://atcoder.jp/contests/adt_all_20260313_1/tasks/abc313_a
解法  
まず、人1のスコア $P_1$ を変数に保持します。  
残りの $N-1$ 人のスコアを確認し、その中での最大値 $M$ を見つけます。  
条件を判定します。  
もし $P_1 > M$ なら、すでに最強なので $x = 0$。  
そうでないなら、人1を $M+1$ にする必要があるため、$x = (M + 1) - P_1$。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int p;
    cin>>p;
    int num1;
    cin>>num1;
    int maxnum=num1;
    bool flag=false;
    for(int i=0;i<p-1;i++)
    {
        int num;
        cin>>num;
        if(maxnum<=num)
        flag=true;
        maxnum=max(maxnum,num);
    }
    if(flag)
    cout<<maxnum-num1+1<<"\n";
    else
    cout<<0<<"\n";
    return 0;
}
```
## C - Humidifier 2
### https://atcoder.jp/contests/adt_all_20260313_1/tasks/abc383_b
解法：全探索  
一見すると組み合わせが多く見えますが、制約を計算してみると全探索が可能であることがわかります。  
床のマスの座標をリスト化する  
まず、グリッドをスキャンして床（.）の座標を vector<Point> などに格納します。床の最大個数は $H \times W = 100$ 個です。  
加湿器を置く2マスの組み合わせを全列挙する  
床の個数を $N$ とすると、組み合わせは $_NC_2$ 通りです。 $N \le 100$ なので、最大でも $\frac{100 \times 99}{2} = 4950$ 通り程度です。  
加湿されるマスをカウントする  
選んだ2つの加湿器それぞれについて、全マス（または全床マス）をチェックし、どちらかの加湿器からの距離が $D$ 以下かどうかを判定します。  
```cpp
#include<bits/stdc++.h>
using namespace std;
struct Point {
    int r, c;
};
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);

    int H,W,D;
    cin>>H>>W>>D;

    vector<string> S(H);
    vector<Point> floors;
    for(int i=0;i<H;++i)
    {
        cin>>S[i];
        for(int j=0;j<W;++j)
        {
            if(S[i][j]=='.')
            {
                floors.push_back({i,j});
            }
        }
    }

    int max_humidified=0;
    int n=floors.size();

    for (int i=0;i<n;++i)
    {
        for (int j=i+1;j<n;++j)
        {
            int current_count=0;
            Point p1=floors[i];
            Point p2=floors[j];

            for(int r=0;r<H;++r)
            {
                for(int c=0;c<W;++c)
                {
                    if(S[r][c]=='.')
                    {
                        int dist1=abs(r-p1.r)+abs(c-p1.c);
                        int dist2=abs(r-p2.r)+abs(c-p2.c);
                        if (dist1<=D||dist2<=D)
                        {
                            current_count++;
                        }
                    }
                }
            }
            max_humidified=max(max_humidified,current_count);
        }
    }
    cout<<max_humidified<<"\n";
    return 0;
}
```
## D - Adjacency Matrix
### https://atcoder.jp/contests/adt_all_20260313_1/tasks/abc343_b
解法  
隣接行列の性質をそのまま利用します。  
行列の $i$ 行目を走査します。  
$A_{i,j} = 1$ であれば、頂点 $i$ と頂点 $j$ は繋がっています。  
頂点番号は $1$-indexed（1から始まる）で出力する必要があるため、列番号 $j$ に $1$ を足した値を出力します。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int N;
    cin>>N;
    vector<vector<int>> A(N, vector<int>(N));

    for(int i=0;i<N;++i)
    {
        for(int j=0;j<N;++j)
        {
            cin>>A[i][j];
        }
    }

    for (int i=0;i<N;++i)
    {
        bool first=true;
        for(int j=0;j<N;++j)
        {
            if (A[i][j]==1)
            {
                if(!first)cout<<" ";
                cout<<j+1;
                first=false;
            }
        }
        cout<<"\n";
    }
    return 0;
}
```
