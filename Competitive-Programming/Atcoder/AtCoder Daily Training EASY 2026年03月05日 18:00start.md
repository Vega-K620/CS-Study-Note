# AtCoder Daily Training EASY 2026/03/06 16:00start

今日のDailyコンテストのA-DはACでした。Eはコンテストが終わってバグを取って ACしました。(計算量がボトルネックになっているのせいで)  

## A - A Unique Letter
### https://atcoder.jp/contests/adt_easy_20260306_1/tasks/abc260_a
3文字という非常に短い文字列から「1度だけ出現する文字」を見つける問題でした。  

最初は if (s[0] != s[1] && s[0] != s[2])... のように書こうか迷いましたが、今後の応用も考えて 出現頻度を記録する配列（バケット） を作成して解きました。  

str[i] - 'a' という書き方で文字を数値（0～25）に変換して配列にカウントする方法は、競技プログラミングの基本テクニックなので、しっかり身につけておきたいです。  

無事に AC できて良かったです！  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    string str;
    cin>>str;
    int out[26]={0};
    for(int i=0;i<3;i++)
    {
        out[str[i]-'a']++;
    }
    int flag=false;
    for(int i=0;i<26;i++)
    {
        if(out[i]==1)
        {
            flag=true;
            cout<<(char)(i+'a')<<"\n";
            break;
        }
    }
    if(!flag)
    cout<<-1<<"\n";
    return 0;
}
```
## B - Misdelivery
### https://atcoder.jp/contests/adt_easy_20260306_1/tasks/abc421_a
配列の要素とインデックスの対応関係を確認する問題でした。  

失敗した点：  
最初に実装したときは、名前の一致だけを判定してしまい、部屋番号の条件を忘れていました。入力例 4（名前は存在するが部屋が違うケース）を見て、自分のミスに気づくことができました。  

学んだこと：  
競技プログラミングでは、問題文の条件をすべて満たしているか細かくチェックする必要があります。今回は for 文で全探索しましたが、name[x-1] のように直接アクセスすればもっとシンプルに書けることも分かりました。  

初歩的なミスでしたが、次からは問題文の条件をもっと丁寧に読み込みたいと思います！  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<string> name(n);
    for(int i=0;i<n;i++)
    cin>>name[i];
    int x;
    string y;
    cin>>x>>y;
    if(x>n)
    cout<<"No"<<"\n";
    else
    {
        int flag=false;
        for(int i=0;i<n;i++)
        {
            if(y==name[i]&&i+1==x)
            {
                flag=true;
                cout<<"Yes"<<"\n";
                break;
            }
        }
        if(!flag)
        cout<<"No"<<"\n";
    } 
    return 0;
}
```
## C - Tombola
### https://atcoder.jp/contests/adt_easy_20260306_1/tasks/abc437_b
2次元グリッドのデータをどう扱うかが鍵となる問題でした。  

工夫した点：2次元配列をそのまま使うのではなく、1次元配列にフラット化して i * w + j のような形でアクセスしました。この手法はメモリ管理や特定のアルゴリズムで役立つので、良い復習になりました。  

反省点：最大値の更新 ans = max(temp, ans) を最内ループで行っていますが、行ごとの処理が終わってから更新するほうがコードの意図が明確だったかもしれません。  

計算量：$ O(H \times W \times N) $ ですが、最大でも $ 3 \times 5 \times 90 = 1350 $ 回程度の計算なので、全く問題ありませんでした！  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int h,w,n;
    cin>>h>>w>>n;
    vector<int> grid(h*w);
    for(int i=0;i<h*w;i++)
    {
        cin>>grid[i];
    }
    int ans=0;
    vector<int> num(n);
    for(int i=0;i<n;i++)
    cin>>num[i];
    for(int i=0;i<h;i++)
    {
        int temp=0;
        for(int j=i*w;j<i*w+w;j++)
        {
            for(int z=0;z<n;z++)
            {
                if(num[z]==grid[j])
                temp++;
            }
        }
        ans=max(temp,ans);
    }
    cout<<ans<<"\n";
    return 0;
}
```
## D - Longest Segment
### https://atcoder.jp/contests/adt_easy_20260306_1/tasks/abc234_b
2点間の距離の最大値を求める問題を通じて、幾何問題の基礎を学びました。  

学んだテクニック：  

2乗のまま比較する： 距離そのもの（$ \sqrt{(x_1-x_2)^2 + (y_1-y_2)^2} $）ではなく、2乗の和（$ (x_1-x_2)^2 + (y_1-y_2)^2 $）で最大値を管理しました。これにより精度と速度の両面でメリットがあります。  

出力精度の指定： setprecision を使って、問題文の要求（$ 10^{-6} $ 以下の誤差）を満たすようにしました。  

計算量：2点の組み合わせをすべて調べるため $ O(N^2) $ となりますが、$ N \le 100 $ なので $ 10,000 $ 回以下の計算で済み、十分に高速です。  

幾何問題は誤差との戦いになることが多いので、今回の「最後に平方根をとる」というアプローチを今後の問題でも活かしたいです！  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<pair<int,int>> xy(n);
    for(int i=0;i<n;i++)
    cin>>xy[i].first>>xy[i].second;
    int longest=0;
    for(int i=0;i<n;i++)
    {
        for(int j=i+1;j<n;j++)
        {
            longest=max(longest,(xy[i].first-xy[j].first)*(xy[i].first-xy[j].first)+(xy[i].second-xy[j].second)*(xy[i].second-xy[j].second));
        }
    }
    cout<<fixed<<setprecision(10)<<sqrt(longest)<<"\n";
    return 0;
}
```
## E - Peer Review
### https://atcoder.jp/contests/adt_easy_20260306_1/tasks/abc442_c
数え上げとグラフ理論の基礎を組み合わせた、非常に学びの多い問題でした。  

学んだこと：  

ボトルネックの解消： $ 2 \times 10^5 $ という制約に対して $ O(N^2) $ や $ O(NM) $ は通用しません。各要素の状態（今回は次数）を前処理で計算しておくことで、劇的に高速化できることを学びました。  

型の選択： int 型の最大値は約 $ 2 \times 10^9 $ ですが、今回の計算では $ 10^{15} $ を超える可能性があります。大きな数を扱うときは迷わず long long を使う癖をつけたいです。  

最後に：試行錯誤の末に AC をもらえた時の達成感は最高でした！この $ O(N+M) $ の考え方は、他のグラフ問題でも応用していきたいです。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    vector<pair<int,int>> kannkei(m);
    for(int i=0;i<m;i++)
    cin>>kannkei[i].first>>kannkei[i].second;
    vector<int> num(n+1);
    for(int i=0;i<m;i++)
    {
        num[kannkei[i].first]++;
        num[kannkei[i].second]++;
    }
    for(int i=0;i<n;i++)
    {
        long long cando=n-1;
        cando=cando-num[i+1];
        if(cando<3)
        {
            cout<<0<<" ";
        }
        else
        {
            cout<<(cando*(cando-1)*(cando-2))/6<<" ";
        }
    }
    return 0;
}
```
