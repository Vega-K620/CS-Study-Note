# AtCoder Daily Training ALL 2026/05/07 18:00start
今日のトレーニングでは A 側から F 側までの 6 問を完答しました。  
A 題から D 題までは基本的な問題だったので説明は省略し、E 題と F 題のポイントをまとめます。
## A - Leftrightarrow
### https://atcoder.jp/contests/adt_all_20260507_2/tasks/abc345_a

```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string s;
    cin>>s;
    if(s[0]=='<'&&s[s.size()-1]=='>')
    {
        bool flag=true;
        for(int i=1;i<s.size()-1;i++)
        {
            if(s[i]!='=')
            {
                flag=false;
                break;
            }
        }
        if(flag)
        cout<<"Yes"<<"\n";
        else
        cout<<"No"<<"\n";
    }
    else
    {
        cout<<"No"<<"\n";
    }
    return 0;
}
```
## B - Apple
### https://atcoder.jp/contests/adt_all_20260507_2/tasks/abc265_a

```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int x,y,n;
    cin>>x>>y>>n;
    if(x*3<=y)
    {
        cout<<x*n<<"\n";
    }
    else
    {
        cout<<n/3*y+n%3*x<<"\n";
    }
    return 0;
}
```
## C - Piano 3
### https://atcoder.jp/contests/adt_all_20260507_2/tasks/abc369_b

```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    int left=-1,right=-1;
    int ans=0;
    while(n--)
    {
        int num;
        char c;
        cin>>num>>c;
        if(c=='L')
        {
            if(left==-1)
            {
                left=num;
            }
            else
            {
                ans+=abs(left-num);
                left=num;
            }
        }
        else
        {
            if(right==-1)
            {
                right=num;
            }
            else
            {
                ans+=abs(right-num);
                right=num;
            }
        }
    }
    cout<<ans<<"\n";
    return 0;
}
```
## D - Get Min
### https://atcoder.jp/contests/adt_all_20260507_2/tasks/abc419_b

```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int q;
    cin>>q;
    multiset<int> bag;
    int ans=0;
    while(q--)
    {
        int num;
        cin>>num;
        if(num==1)
        {
            int temp;
            cin>>temp;
            bag.insert(temp);
        }
        else
        {
            for(auto i:bag)
            {
                ans=i;
                bag.erase(bag.begin());
                cout<<ans<<"\n";
                break;
            }
        }
    }
    return 0;
}
```
## E - PC on the Table
### https://atcoder.jp/contests/adt_all_20260507_2/tasks/abc297_c
左から右へ 1 文字ずつスキャンし、s[i] と s[i+1] が共に 'T' であるかを確認します。条件を満たした瞬間に 'P' と 'C' に書き換えることで、同じ文字を二度操作対象にするミスを防げます。非常に直感的なシミュレーション問題でした。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int h,w;
    cin>>h>>w;
    while(h--)
    {
        string s;
        cin>>s;
        for(int i=0;i<w-1;i++)
        {
            if(s[i]==s[i+1]&&s[i]=='T')
            {
                s[i]='P';
                s[i+1]='C';
            }
        }
        cout<<s<<"\n";
    }
    return 0;
}
```
## F - Mixture
### https://atcoder.jp/contests/adt_all_20260507_2/tasks/abc415_c
この問題の本質は状態圧縮DPです。  
なぜ DP なのか？  
薬を混ぜる順序は $N!$ 通りあり、$N=18$ のような制約では全探索（爆発的増加）は不可能です。しかし、「どの薬が瓶に入っているか」という「状態」に着目すると、$2^N$ 通りに抑えることができます。  
dp[mask] を「その薬の組み合わせが安全に到達可能か」というフラグとして定義します。  
空の状態 dp[0] を true とします。  
各状態 mask から、まだ入っていない薬 i を一つずつ試します (mask | (1 << i))。  
新しい状態が安全（s[next_mask-1] == '0'）であれば、遷移先を true に更新します。  
最終的に dp[(1 << n) - 1] が true であれば、全種類の薬を無事に混ぜられたことになります。順序ではなく「集合」で管理することで、計算量を $O(N \cdot 2^N)$ まで落とすことができました。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

void solve()
{
    int n;
    cin>>n;
    string s;
    cin>>s;

    int num_states=(1<<n);

    vector<bool> dp(num_states,false);
    dp[0]=true;

    for(int mask=0;mask<num_states;mask++)
    {
        if(!dp[mask]) continue;

        for(int i=0;i<n;i++)
        {    
            if(!((mask>>i)&1))
            {
                int next_mask=mask|(1<<i);
                if (s[next_mask-1]=='0')
                {
                    dp[next_mask]=true;
                }
            }
        }
    }

    if(dp[num_states-1])
    {
        cout<<"Yes"<<endl;
    }
    else
    {
        cout<<"No"<<endl;
    }
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
