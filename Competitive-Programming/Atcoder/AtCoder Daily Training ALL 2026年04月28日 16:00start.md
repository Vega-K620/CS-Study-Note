# AtCoder Daily Training ALL 2026/04/28 16:00start
今日のコンテストでは E と F の 2 問を解きました。F 題でかなり手こずってしまったのが反省点です。  
最初は数学的な組み合わせ（順列・組合せ）の公式で解けると思い込んでいましたが、結局うまくいきませんでした。  
その後、実は全探索による DFS（深さ優先探索） のブルートフォース（暴力法）で解くべき問題だと気づきました。  
## E - Many Balls
### https://atcoder.jp/contests/adt_all_20260428_1/tasks/abc216_c
この問題は、目標の数 $N$ から $0$ へ逆算するのが最も効率的。  
$N$ が奇数なら、直前の操作は「魔法A (+1)」のはず $\rightarrow$ $N = N - 1$  
$N$ が偶数なら、直前の操作は「魔法B (×2)」のはず $\rightarrow$ $N = N / 2$  
この手順を $N=0$ になるまで繰り返し、得られた操作列を反転（reverse）させれば答えになる。  
最大でも $\log_2(10^{18}) \approx 60$ 回の B 操作と、高々 60 回の A 操作で済むため、合計 120 回以内に収まる。  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    ll n;
    cin>>n;
    string ans="";
    while(n>0)
    {
        if(n%2==0)
        {
            ans+='B';
            n/=2;
        }
        else
        {
            ans+='A';
            n--;
        }
    }
    reverse(ans.begin(),ans.end());

    cout<<ans<<"\n";
    return 0;
}
```
## F - False Hope
### https://atcoder.jp/contests/adt_all_20260428_1/tasks/abc319_c
最初は「順列・組合せ」の公式を当てはめようとしたが、計算が合わず断念。途中で「全探索（DFS）」による全パターンのシミュレーションが必要だと気づき、方針を転換した。$9! = 362,880$ なので、全探索でも十分に間に合う。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
int excel[9];
int order[9];
ll ans=0;
bool used[9]={false};
int lines[8][3]={
    {0, 1, 2}, {3, 4, 5}, {6, 7, 8}, // 行
    {0, 3, 6}, {1, 4, 7}, {2, 5, 8}, // 列
    {0, 4, 8}, {2, 4, 6}             // 对角线
};

bool is_safe()
{
    for(int i=0;i<8;i++)
    {
        int a=lines[i][0],b=lines[i][1],c=lines[i][2];
        
        int t[9];
        for(int step=0;step<9;step++)
        t[order[step]]=step;

        vector<pair<int,int>> v;
        v.push_back({t[a],excel[a]});
        v.push_back({t[b],excel[b]});
        v.push_back({t[c],excel[c]});
        sort(v.begin(),v.end());

        if(v[0].second==v[1].second)
        return false; 
    }
    return true;
}

void dfs(int num)
{
    if(num==9)
    {
        if(is_safe())
        {
            ans++;
            // cout<<+1<<"\n";
        }
        return;
    }
    for(int i=0;i<9;i++)
    {
        if(!used[i])
        {
            used[i]=true;
            order[num]=i;
            dfs(num+1);
            used[i]=false;
        }
    }
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    
    for(int i=0;i<9;i++)
    cin>>excel[i];

    dfs(0);
    cout<<fixed<<setprecision(15)<<(double)ans/362880.0<<"\n";
    return 0;
}
```
