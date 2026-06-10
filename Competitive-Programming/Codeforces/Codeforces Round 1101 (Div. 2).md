# Codeforces Round 1101 (Div. 2)
## A. Convergence
### https://codeforces.com/contest/2232/problem/A
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

void solve()
{
    int n;
    cin>>n;
    vector<int> num(n);
    int cnt=0;
    for(int i=0;i<n;i++)
    {
        cin>>num[i];
    }
    sort(num.begin(),num.end());
    for(int i=0;i<n;i++)
    {
        if(i>=n-1-i)
        {
            break;
        }
        else
        {
            if(num[i]!=num[n-1-i])
            {
                cnt++;
            }
            else
            {
                break;
            }
        }
    }
    cout<<cnt<<"\n";
}

signed main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int t;
    //t=1;
    cin>>t;
    while(t--)
    {
        solve();
    }
}
```
## B. Cake Leveling
### https://codeforces.com/contest/2232/problem/B
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

void solve()
{
    int n;
    cin>>n;
    vector<int> num(n);
    for(int i=0;i<n;i++)
    {
        cin>>num[i];
    }
    int now=num[0];
    for(int i=0;i<n;i++)
    {
        if(now==num[i])
        {
            cout<<now<<" ";
        }
        else
        {
            if(now<num[i])
            {
                if(i!=n-1)
                num[i+1]+=num[i]-now;
                cout<<now<<" ";
            }
            else if(now>num[i])
            {
                int temp=(now-(now*i+num[i])/(i+1));
                now-=temp;
                if(i!=n-1)
                num[i+1]+=temp*i-(now-num[i]);
                cout<<now<<" ";
            }
        }
    }
    cout<<"\n";
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
## C1. Seating Arrangement (Easy Version)
### https://codeforces.com/contest/2232/problem/C1
```cpp
#include <bits/stdc++.h>
using namespace std;

void solve()
{
    int n,x,s;
    cin>>n>>x>>s;
    string str;
    cin>>str;

    // dp[j][k] 表示用了 j 张桌子，还剩 k 个空座位时的最大就坐人数
    // 初始化为 -1
    vector<vector<int>> dp(x+1, vector<int>(x*s+1,-1));
    dp[0][0]=0;

    for(char c:str)
    {
        // 创建一个临时阵列用于当前这一步的转移，防止重复转移
        vector<vector<int>> next_dp=dp;

        for(int j=0;j<=x;j++)
        {
            for(int k=0;k<=x*s;k++)
            {
                if(dp[j][k]==-1) continue;

                // 情况 1: 内向者 (I) 或是中向者 (A) 选择开一张新桌子
                if((c=='I'||c=='A')&&j+1<=x)
                {
                    next_dp[j+1][k+s-1]=max(next_dp[j+1][k+s-1],dp[j][k]+1);
                }

                // 情况 2: 外向者 (E) 或是中向者 (A) 选择坐已有人的桌子
                if((c=='E'||c=='A')&&k>0)
                {
                    next_dp[j][k-1]=max(next_dp[j][k-1],dp[j][k]+1);
                }
            }
        }
        dp=move(next_dp);
    }

    // 统计所有可行状态中的最大人数
    int ans=0;
    for(int j=0;j<=x;j++)
    {
        for(int k=0;k<=x*s;k++)
        {
            ans=max(ans,dp[j][k]);
        }
    }
    cout<<ans<<"\n";
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
