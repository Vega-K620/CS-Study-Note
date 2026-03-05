# AtCoder Daily Training EASY 2026/03/05 18:00start
<details>
  <summary>(今回の感想)</summary>
  今日の分もACしました！スキルが向上しました！まだまだ伸びしろはあるので、これからも頑張ります。
</details>
## A - Adjacent Product
### https://atcoder.jp/contests/adt_easy_20260305_2/tasks/abc346_a
A問題はループしながら、同時に結果を出して出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<int> num(n);
    for(int i=0;i<n;i++)
    {
        cin>>num[i];
    }
    for(int i=0;i<n-1;i++)
    {
        num[i]=num[i]*num[i+1];
        cout<<num[i]<<" ";
    }
    return 0;
}
```
## B - Water Station
### https://atcoder.jp/contests/adt_easy_20260305_2/tasks/abc305_a
この問題、僕は一の位の数字を見て、一番近い 5 の倍数を判断しました。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    if(n%10<=2)
    {
        cout<<n/10*10<<"\n";
    }
    else if(n%10>=3&&n%10<=7)
    {
        cout<<n/10*10+5<<"\n";
    }
    else
    {
        cout<<n/10*10+10<<"\n";
    }

    return 0;
}
```
後で、もっと簡単な方法に気づきました。
(n+2)/5*5は答えです。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    cout<<(n+2)/5*5<<"\n";
    return 0;
}
```
## C - Go Straight and Turn Right
### https://atcoder.jp/contests/adt_easy_20260305_2/tasks/abc244_b
この問題は2つの配列で方向を保持します。そして次の座標が計算できます。
```cpp
#include<bits/stdc++.h>
using namespace std;

int runx[4]={1,0,-1,0};
int runy[4]={0,-1,0,1};
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    int x=0,y=0;
    int num=0;
    while(n--)
    {
        char t;
        cin>>t;
        if(t=='S')
        {
            x+=runx[num];
            y+=runy[num];
        }
        else
        {
            num++;
            num%=4;
        }
    }
    cout<<x<<" "<<y<<"\n";
    return 0;
}
```
## D - Discord
### https://atcoder.jp/contests/adt_easy_20260305_2/tasks/abc303_b
2次元配列に「仲間」の状態を保存し、それを更新してから、最後に状態を確認して答えを出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n, m;
    cin>>n>>m;
    
    bool nakama[55][55]={false}; 
    vector<int> hito(n);

    while(m--)
    {
        for(int i=0;i<n;i++)
        {
            cin>>hito[i];
        }
        for(int i=0;i<n-1;i++)
        {
            int u=hito[i],v=hito[i+1];
            nakama[u][v]=true;
            nakama[v][u]=true;
        }
    }

    int ans=0;
    for(int i=1;i<=n;i++)
    {
        for(int j=i+1;j<=n;j++)
        {
            if(!nakama[i][j])
            {
                ans++;
            }
        }
    }

    cout<<ans<<"\n";
    return 0;
}
```
## E - Changing Jewels
### https://atcoder.jp/contests/adt_easy_20260305_2/tasks/abc260_c
問題の条件に従って、高い方から低い方へ走査しながら計算し、結果を出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,x,y;
    cin>>n>>x>>y;
    long long redrock[15]={0};
    long long bluerock[15]={0};
    redrock[n]=1;
    for(int i=n;i>1;i--)
    {
        if(redrock[i]!=0)
        {
            redrock[i-1]+=redrock[i];
            bluerock[i]+=redrock[i]*x;
            redrock[i]=0;
        }
        if(bluerock[i]!=0)
        {
            redrock[i-1]+=bluerock[i];
            bluerock[i-1]+=bluerock[i]*y;
            bluerock[i]=0;
        }
    }
    cout<<bluerock[1]<<"\n";
    return 0;
}
```
