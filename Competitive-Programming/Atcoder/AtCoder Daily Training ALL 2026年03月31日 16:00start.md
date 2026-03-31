# AtCoder Daily Training ALL 2026/03/31 16:00start
僕は「coding」速力がまだまだですから、今日のトレーニングはA-Dがしませんで、E-Gを解きました。
## E - Ladder Takahashi
https://atcoder.jp/contests/adt_all_20260331_1/tasks/abc277_c
この問題は僕「dfs」を使って、最も高い階段を簡単に見つけました。一般的な「dfs」は引き返するでしょ、しかしこの問題の「dfs」は引き返しません。最初はきずいませんでした。  
追記：
さっきこの問題は「bfs」でもっといいとかんがえました。DFSでは再帰（さいき）が深くなりすぎるとスタックオーバーフローの恐れがありますが、BFSならキュー（Queue）を使うため、より安全に探索できるからです。
```cpp
#include<bits/stdc++.h>
using namespace std;
long long highest=1;
unordered_map<long long,vector<long long>> tizi;
unordered_map<long long,bool> serched;
void dfs(long long start)
{
    serched[start]=true;
    highest=max(highest,start);
    if(tizi.count(start)==0)
    {
        
        return;
    }
    for(auto i:tizi[start])
    {
        if(serched.count(i)!=0)
        if(serched[i]==true)
        continue;
        dfs(i);
    }
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    long long n;
    cin>>n;
    for(int i=0;i<n;i++)
    {
        long long num,num2;
        cin>>num>>num2;
        
        tizi[num].push_back(num2);
        tizi[num2].push_back(num);
    }
    if(tizi.count(1)==0)
    {
        cout<<1<<"\n";
        return 0;
    }
    else
    {
        dfs(1);
        cout<<highest<<"\n";
    }
    return 0;
}
```
## F - Reindeer and Sleigh 2
### https://atcoder.jp/contests/adt_all_20260331_1/tasks/abc437_c
この問題は「貪欲法」の問題です。荷物の重さとトナカイの力の和でソートする貪欲法が有効です。
```cpp
#include<bits/stdc++.h>
using namespace std;

bool cmp(pair<long long,long long> a,pair<long long,long long> b)
{
    return a.first+a.second>b.first+b.second;
}
void solve()
{
    int n;
    cin>>n;
    long long sumw=0;
    long long sum=0;
    vector<pair<long long,long long>> lu(n);
    for(int i=0;i<n;i++)
    {
        long long a,b;
        cin>>a>>b;
        sumw+=a;
        lu[i].first=a;
        lu[i].second=b;
    }
    sort(lu.begin(),lu.end(),cmp);
    int i=0;
    while(sum<sumw)
    {
        sum+=lu[i].second+lu[i].first;
        n--;
        i++;
    }
    cout<<n<<"\n";
    return;
}
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        solve();
    }
    return 0;
}
```
## G - Domino Covering XOR
### https://atcoder.jp/contests/adt_all_20260331_1/tasks/abc407_d
この問題の制約が小さいので、「dfs」で解けます。ドミノの置きかどうかを全探索して、一番大きいの答えが見つけます。
```cpp
#include<bits/stdc++.h>
using namespace std;
long long excel[20][20]={-1};
bool checked[20][20]={false};
long long ans=0;
long long maxans=0;
long long h,w;

void dfs(int x,int y)
{
    if(y==w)
    {
        dfs(x+1,0);
        return;
    }
    if(x==h)
    {
        maxans=max(ans,maxans);
        return;
    }
    if(checked[x][y]==true)
    {
        dfs(x,y+1);
        return;
    }
    dfs(x,y+1);
    if(y+1<w&&!checked[x][y+1])
    {
        checked[x][y]=checked[x][y+1]=true;
        ans^=excel[x][y]^excel[x][y+1];
        dfs(x,y+1);
        ans^=excel[x][y]^excel[x][y+1];
        checked[x][y]=checked[x][y+1]=false;
    }
    if(x+1<h&&!checked[x+1][y])
    {
        checked[x][y]=checked[x+1][y]=true;
        ans^=excel[x][y]^excel[x+1][y];
        dfs(x,y+1);
        ans^=excel[x][y]^excel[x+1][y];
        checked[x][y]=checked[x+1][y]=false;
    }
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    
    cin>>h>>w;
    
    for(int i=0;i<h;i++)
    {
        for(int j=0;j<w;j++)
        {
            cin>>excel[i][j];
            ans^=excel[i][j];
        }
    }
    dfs(0,0);
    cout<<maxans<<"\n";
    return 0;
}
```
