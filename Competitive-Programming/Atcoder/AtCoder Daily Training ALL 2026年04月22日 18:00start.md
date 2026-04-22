# AtCoder Daily Training ALL 2026/04/22 18:00start
今日のトレーニングはE-Gを解きましたが、今日の問題は僕にとって少し大変でした。ほとんどトレーニングの時間内「AC」が取りませんでした。でもトレーニング後ちゃんと復習しました。
## E - Upgrade Required
### https://atcoder.jp/contests/adt_all_20260422_2/tasks/abc426_c
この問題僕は最初全探索して「TLE」しました。それに全然最適化の方法が見つかりません。以下は最初「TLE」のコードです。時間複雑度はO(NxQ)ですので、「TLE」は当然の問題です。
```cpp
//僕が最初「TLE」の方法。
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,q;
    cin>>n>>q;

    vector<int> os(n+1,1);
    vector<int> ans;
    int lowest=1;
    while(q--)
    {
        int x,y;
        cin>>x>>y;
        int sum=0;
        for(int i=lowest;i<=x;i++)
        {
            sum+=os[i];
            os[i]=0;
        }
        os[y]+=sum;
        cout<<ans<<"\n";
        lowest=x;
    }

    return 0;

}
```
本当は最適化の方法が本当に簡単です。
$ lowest=x; $ -> $ lowest=max(lowest,x); $ これを変更して「AC」ができました。  
でも、なぜですか。実は $ lowest=x; $ は $ x<lowest $ の時まずいことが出て来た！lowestの効果がなくなちゃた。ですから $ lowest=max(lowest,x); $ に変更して正解になりました。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,q;
    cin>>n>>q;

    vector<int> os(n+1,1);
    int lowest=1;
    while(q--)
    {
        int x,y;
        cin>>x>>y;
        int sum=0;
        for(int i=lowest;i<=x;i++)
        {
            sum+=os[i];
            os[i]=0;
        }
        os[y]+=sum;
        cout<<sum<<"\n";
        lowest=max(x,lowest);
    }

    return 0;
}
```
## F - Not All Covered
### https://atcoder.jp/contests/adt_all_20260422_2/tasks/abc408_c
この問題は差分の問題です。これは簡単です。まず差分を使って「砲台」の範囲を記録して、累積和を計算する同時に最低の数を更新して、答えがでてきます。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    vector<int> diff(n+2,0);
    for (int i=0;i<m;i++)
    {
        int l,r;
        cin>>l>>r;
        diff[l]++;
        diff[r+1]--;
    }

    int min_destroyed=m+1;
    int current_coverage=0;

    for(int i=1;i<=n;i++)
    {
        current_coverage+=diff[i];
        if(current_coverage<min_destroyed)
        {
            min_destroyed=current_coverage;
        }
    }

    cout<<min_destroyed<<"\n";
    return 0;
}
```
## G - Tiling
### https://atcoder.jp/contests/adt_all_20260422_2/tasks/abc345_d
この問題の制約は大きくないですから「DFS」を使って答えが分かります。少し複雑のは「DFS」じゃなくて「タイルをマスに入れる」の操作です。  
盤面上のまだタイルが置かれていないマスのうち、最も左上にあるマス $(r, c)$ を選ぶ。もし全てのマスが埋まっていれば成功である。そうでなければ、未使用のタイルを一つ選び、そのタイルの左上角が $(r, c)$ に重なるように配置を試みる。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int N,H,W;
int A[10],B[10];
bool used_tile[10];
bool board[10][10];

bool dfs()
{
    int r=-1,c=-1;
    for(int i=0;i<H;i++)
    {
        for(int j=0;j<W;j++)
        {
            if(!board[i][j])
            {
                r=i;
                c=j;
                goto found;
            }
        }
    }
found:
    if(r==-1)
    {
        return true;
    }
    for(int i=0;i<N;i++)
    {
        if(used_tile[i])
        {
            continue;
        }
        int rot[2][2]={{A[i],B[i]},{B[i],A[i]}};
        for(int s=0;s<2;s++)
        {
            int h=rot[s][0],w=rot[s][1];
            if(r+h>H||c+w>W)
            {
                continue;
            }
            bool ok = true;
            for(int i2=r;i2<r+h;i2++)
            {
                for(int j2=c;j2<c+w;j2++)
                {
                    if(board[i2][j2])
                    {
                        ok=false;
                    }
                }
            }
            if(ok)
            {
                for (int i2=r;i2<r+h;i2++)
                {
                    for(int j2=c;j2<c+w;j2++)
                    {
                        board[i2][j2]=true;
                    }
                }
                used_tile[i]=true;
                if(dfs())
                {
                    return true;
                }
                used_tile[i]=false;
                for (int i2=r;i2<r+h;i2++)
                {
                    for(int j2=c;j2<c+w;j2++)
                    {
                        board[i2][j2] = false;
                    }
                }
            }
            if(h==w)
            {
                break;
            }
        }
    }
    return false;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    cin>>N>>H>>W;
    for(int i=0;i<N;i++)
    {
        cin>>A[i]>>B[i];
    }
    if(dfs())
    {
        cout<<"Yes"<<"\n";
    }
    else
    {
        cout<<"No"<<"\n";
    }
    return 0;
}
```
