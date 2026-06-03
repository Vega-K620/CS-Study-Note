# SCCPC 2026 SiChuan
这次四川省赛稍微有点可惜，差一点就能拿牌子，但是由于多种失误导致与牌子失之交臂，从最后结果来看能ac的题目至少应该是4道题以上，但是实际上只做出来2道题，接下来对失误的题目进行分析。
## D 那一天的回文字符串
这道题是这次的签到题，但是竟然吃了两次wa，第一次wa吃得特别不应该，我们队伍在题目都没有看完的情况下就自认为这道题是考的简单的字符串回文判断，于是写了个reverse加判断直接就交上去了。这反映出我们在比赛的时候稍微有点心急了，导致出现这种低级错误。因此我们交流后优化了三个人的分工，以避免再次出现这种情况。
## H 最大权独立集问题
这道题并非很难，想到解法之后就很简单，但是这道题出现的最大问题是忘记开long long了，看到数据只有1e5就以为不用开long long，结果交上去wa了算大小才发现需要开long long，为了避免再次出现栽在long long的情况，在对内存不敏感的题目中之后都使用#define int long long。
## G 禁忌教典的消失咒文
这道题陷入了一些问题，我们发现要去枚举删除范围复杂度会来到 $ O(n^2) $ ，这肯定是跑不出来的，所以我们一直在思考优化方法，但是思考方向出现了一些问题，我们一直觉得这道题是什么数据结构可以处理这个问题，尝试了st表树状数组线段树这些解决区间查询的数据结构，但是都没能解决复杂度太高的问题，最后比赛结束看题解才反应过来这道题只需要前后缀和加上哈希就能 $ O(n) $ 解决。
### 补题代码
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

void solve()
{
    int n,k;
    cin>>n>>k;
    vector<int> num(n+2,0);
    vector<int> s(n+2,0),t(n+2,0);
    for(int i=1;i<=n;i++)
    cin>>num[i];

    for(int i=1;i<=n;i++)
    {
        if(i%2!=0)
        s[i]=s[i-1]+num[i];
        else
        s[i]=s[i-1]-num[i];
    }

    for(int i=n;i>=1;i--)
    {
        t[i]=num[i]-t[i+1];
    }

    map<int,int> cnt;
    for(int i=1;i<=n+1;i++)
    {
        cnt[t[i]]++;
    }

    int ans=0;
    for(int l=1;l<=n;l++)
    {
        cnt[t[l]]--; 
        if((l-1)%2==0)
        {
            ans+=cnt[k-s[l-1]];
        }
        else
        {
            ans+=cnt[s[l-1]-k];
        }
    }
    cout<<ans<<"\n";
}

signed main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        solve();
    }
}
```
## C 精灵对战
在G题上消耗了太多时间，等我们看这道题的时候已经没什么精力和耐心处理C题了，读题的时候关键信息竟然没有看到(小z可以多次派出同一种精灵)，导致以为是一道背包dp类型的题目。队友之间的交流到后面也几乎没有所以没有纠正这个问题，导致在错误的路上越走越远。
### 补题代码
```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
bool check(int X,int Y,const vector<vector<int>>& kezhi)
{
    return binary_search(kezhi[Y].begin(),kezhi[Y].end(),X);
}

int main()
{
    cin.tie(nullptr)->sync_with_stdio(false);
    int n,m,k;
    cin>>n>>m>>k;
    vector<vector<int>> kezhi(n+1);
    for(int i=1;i<=n;i++)
    {
        int len;
        cin>>len;
        kezhi[i].resize(len);
        for(int j=0;j<len;j++)
        {
            cin>>kezhi[i][j];
        }
        sort(kezhi[i].begin(),kezhi[i].end());
    }
    vector<int> a(m+1);
    for(int i=1;i<=m;i++)
    {
        cin>>a[i];
    }
    int ans=0;
    int i=1;
    while(i<=m)
    {
        int max_reach=i;
        for(int A:kezhi[a[i]])
        {
            int curr=i;
            while(curr<=m)
            {
                if(check(A,a[curr],kezhi))
                {
                    curr++;
                }
                else if(check(a[curr],A,kezhi))
                {
                    break;
                }
                else
                {
                    curr++;
                    break;
                }
            }
            max_reach=max(max_reach,curr-1);
        }
        ans++;
        i=max_reach+1;
    }
    cout<<ans<<"\n";
    return 0;
}
// #include <bits/stdc++.h>
// using namespace std;
// #define int long long

// const int INF=1e18;

// void solve()
// {
//     int n,m,k;
//     cin>>n>>m>>k;

//     vector<vector<int>> kezhi(n+1);
//     for(int i=1;i<=n;i++)
//     {
//         int len;
//         cin>>len;
//         kezhi[i].resize(len);
//         for(int j=0;j<len;j++)
//         {
//             cin>>kezhi[i][j];
//         }
//         sort(kezhi[i].begin(), kezhi[i].end());
//     }
//     vector<int> shunxu(m+1);
//     for(int i=1;i<=m;i++)
//     {
//         cin>>shunxu[i];
//     }
//     vector<int> dp(m+1,INF);
//     dp[0]=0;
//     vector<int> min_cost(n+1,INF);
//     vector<int> spr;
//     for(int i=1; i<=m;i++)
//     {
//         int ai=shunxu[i];
//         dp[i]=dp[i-1]+1;

//         for(int X:spr)
//         {
//             bool X_kezhi_ai=binary_search(kezhi[ai].begin(),kezhi[ai].end(),X);
//             bool ai_kezhi_X=binary_search(kezhi[X].begin(),kezhi[X].end(),ai);
//             if(X_kezhi_ai||(!X_kezhi_ai&&!ai_kezhi_X))
//             {
//                 dp[i]=min(dp[i],min_cost[X]+1);
//             }
//         }
//         vector<pair<int,int>> old_active;
//         for(int X:spr)
//         {
//             old_active.push_back({X, min_cost[X]});
//             min_cost[X]=INF;
//         }
//         vector<int> next_active;
//         for(int X:kezhi[ai])
//         {
//             int prev_cost=INF;
//             for(auto& p:old_active)
//             {
//                 if(p.first==X)
//                 {
//                     prev_cost=p.second;
//                     break;
//                 }
//             }
//             int best_prev=min(prev_cost,dp[i-1]);
//             if(best_prev<INF)
//             {
//                 min_cost[X] = best_prev;
//                 next_active.push_back(X);
//             }
//         }
//         spr=next_active;
//     }
//     cout<<dp[m]<<"\n";
// }
// signed main()
// {
//     cin.tie(NULL)->sync_with_stdio(false);
//     int t=1;
//     while(t--)
//     {
//         solve();
//     }
//     return 0;
// }
```
## 总结反思
3人的配合还不是很好，需要明确分工，每个人的想法都应该说出来综合判断。一道题卡住太久就应该及时换题说不定换题之后再回来看这道题就豁然开朗了。应该求稳而不是只求快，读题得仔细看清楚每个限制条件。
