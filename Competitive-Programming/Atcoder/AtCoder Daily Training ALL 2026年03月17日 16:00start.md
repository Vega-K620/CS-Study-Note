# AtCoder Daily Training ALL 2026/03/17 16:00start
今日のトレーニングでA-F問題を解きました。E問題はまたDFSの問題でしたが、昨日よりもスムーズに解けるようになりました。一つずつ実力が上がっている感じがして、とても嬉しいです。  
## A - Count Down
### https://atcoder.jp/contests/adt_all_20260317_1/tasks/abc281_a
この問題はただNから0まで出力する問題です。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    for(int i=n;i>=0;i--)
    cout<<i<<"\n";
    return 0;
}
```
## B - Humidifier 1
### http://atcoder.jp/contests/adt_all_20260317_1/tasks/abc383_a
模擬問題のポイントは、状態の遷移を正確に再現することです。  
この問題では、水が減る処理（時間差の計算）と、水が負にならないようにする制約（0でクランプする）が重要でした。  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    int lastt=0,lastv=0;
    for(int i=0;i<n;i++)
    {
        int t,v;
        cin>>t>>v;
        lastv-=t-lastt;
        if(lastv<0)lastv=0;
        lastv+=v;
        lastt=t;
    }
    cout<<lastv<<"\n";
    return 0;
}
```
## C - AtCoder Amusement Park
### https://atcoder.jp/contests/adt_all_20260317_1/tasks/abc353_b
どちらの問題も「現在の状態（水量/空席数）」を管理し、「限界を超えたとき」（水が0になる、または席が足りなくなる）の処理を正しく書くのがコツです。  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,k;
    cin>>n>>k;
    queue<int> num;
    for(int i=0;i<n;i++)
    {
        int l;
        cin>>l;
        num.push(l);
    }
    int isusing=0;
    int ans=0;
    for(int i=0;i<n;i++)
    {
        if((k-isusing)>=num.front())
        {
            isusing+=num.front();
            num.pop();
        }
        else
        {
            ans++;
            isusing=num.front();
            num.pop();
        }
    }
    ans++;
    cout<<ans<<"\n";
    return 0;
}
```
## D - Split?
### https://atcoder.jp/contests/adt_all_20260317_1/tasks/abc267_b
図をコードに落とし込む: 複雑な配置（10本のピン）は、そのまま扱うのではなく、問題の条件である「列」にグループ化して整理するのが近道です。  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    string s;
    cin>>s;

    if (s[0]=='1')
    {
        cout<<"No"<<"\n";
        return 0;
    }

    vector<vector<int>> columns={
        {7},
        {4},
        {2, 8},
        {1, 5},
        {3, 9},
        {6},
        {10}
    };

    bool has_standing[7]={false};
    for(int i=0;i<7;i++)
    {
        for(int pin:columns[i])
        {
            if(s[pin-1]=='1')
            {
                has_standing[i]=true;
            }
        }
    }

    for(int i=0;i<7;i++)
    {
        for(int k=i+2;k<7;k++)
        {
            if(has_standing[i]&&has_standing[k])
            {
                for(int j=i+1;j<k;j++)
                {
                    if(!has_standing[j])
                    {
                        cout<<"Yes"<<"\n";
                        return 0;
                    }
                }
            }
        }
    }

    cout<<"No"<<"\n";
    return 0;
}
```
## E - Cycle Graph?
### https://atcoder.jp/contests/adt_all_20260317_1/tasks/abc404_c
DFS による連結性判定の教訓：  
visited 配列は必須: これがないと無限ループ（死の再帰）に陥る。  
カウント変数（dnf）の活用: visited にするタイミングでカウントすることで、最終的に「いくつの頂点に触れたか」が分かる。  
開始点の選定: グラフが 1-indexed なら、0 ではなく 1 からスタートさせる。  
「グラフ問題は、まず『次数』を疑い、次に『連結性』を疑うのが鉄則！」  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
int dnf=0;
vector<vector<int>> gf;
vector<bool> visited;

void checkdfs(int num)
{
    visited[num]=true;
    dnf++;
    for(auto i:gf[num])
    if(!visited[i])
    checkdfs(i);
    return;
}


int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    gf.resize(n+1);
    visited.assign(n+1,false);

    if(n!=m)
    {
        cout<<"No"<<"\n";
        return 0;
    }
    
    for(int i=0;i<m;i++)
    {
        int point,line;
        cin>>point>>line;
        gf[point].push_back(line);
        gf[line].push_back(point);
    }

    for(int i=1;i<=n;i++)
    {
        if(gf[i].size()!=2)
        {
            cout<<"No"<<"\n";
            return 0;
        }
    }
    checkdfs(1);
    if(dnf==n)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
    return 0;
}
```
## F - ABC conjecture
### https://atcoder.jp/contests/adt_all_20260317_1/tasks/abc227_c
計算量の工夫:  
単純な全探索はNG: $N=10^{11}$ なので、3重ループだと一生終わりません。  
制約を活用する: $A \le B \le C$ という条件を使うと、Aは $\sqrt[3]{N}$ まで、Bは $\sqrt{N/A}$ まで探索すれば十分だと分かります。  
最後は数学で: Cをループで回さず、計算式 (n / (a * b)) - b + 1 で一気に出すのがポイントです。  
「全ての変数をループさせる必要はない。最後の1つは算数で出せる！」  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    ll n;
    cin >> n;
    ll ans=0;

    for(ll a=1;a*a*a<=n;a++) 
    {
        for(ll b=a;a*b*b<=n;b++) 
        {
            ll max_c=n/(a*b);
            if(max_c>=b) 
            {
                ans+=(max_c-b+1);
            }
        }
    }
    cout<<ans<<"\n";
    return 0;
}
```
