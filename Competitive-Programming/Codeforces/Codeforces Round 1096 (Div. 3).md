# Codeforces Round 1096 (Div. 3)
昨日のdiv3はA-Eを解きました。A-Cはとても簡単です。DとEは少しチャレンジがあるので、時間が掛かりました。
## A. Koshary
### https://codeforces.com/contest/2227/problem/A
この問題のポイントは、座標の偶奇性（Parity）に注目することです。  
初期位置が $(0, 0)$（どちらも偶数）であるため、長い一歩だけでは常に $x, y$ ともに偶数の座標にしか到達できません。短い一歩を1回使うことで、どちらか一方の座標を奇数に変えることができます。  
つまり、到達可能な座標 $(x, y)$ の条件は、「奇数である座標が最大で1つであること」になります。  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

void solve()
{
    int a,b;
    cin>>a>>b;
    if(a%2==1&&b%2==1)
    cout<<"No"<<"\n";
    else
    cout<<"Yes"<<"\n";
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
## B. Party Monster
### https://codeforces.com/contest/2227/problem/B
( と ) の総数が一致していること。  
どのプレフィックス（前方からの累積）をとっても、( の数が ) の数以上であること。  
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

    if(n%2!=0)
    {
        cout<<"No"<<"\n";
        return;
    }

    int zuo=0,you=0;
    for(char c:s)
    {
        if(c=='(')
        zuo++;
        else
        you++;
    }
    if(zuo!=you)
    {
        cout<<"No"<<"\n";
        return;
    }

    cout<<"Yes"<<"\n";
    
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
## C. Snowfall
### https://codeforces.com/contest/2227/problem/C
「積が 6 で割り切れる区間」を少なくするためには、2 の要素と 3 の要素をできるだけ遠ざけ、かつ 6 の倍数を端に寄せるのが定石です。  
2 と 3 の隔離: Group A (2の倍数) と Group C (3の倍数) の間に Group B (どちらの倍数でもない数) を挟むことで、2 と 3 が出会って 6 を作る区間の発生を最小限に抑えます。  
6 の倍数の集約: Group D (6の倍数) は単体で条件を満たしてしまうため、これらを配列の端（最後尾など）にまとめて配置します。これにより、他の要素と絡んで余計な区間を作るリスクを最小化できます。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

void solve()
{
    int n;
    cin>>n;
    vector<int> san;
    vector<int> ou;
    vector<int> num6;
    vector<int> qita;
    int num2=0,num3=0;
    for(int i=0;i<n;i++)
    {
        int temp;
        cin>>temp;
        if(temp%6==0)
        num6.push_back(temp);
        else
        {
            if(temp%2==0)
            ou.push_back(temp);
            else
            {
                if(temp%3==0)
                san.push_back(temp);
                else
                qita.push_back(temp);
            }
        }
    }
    vector<int> ans;

    for(int i:ou)
    ans.push_back(i);
    for(int i:qita)
    ans.push_back(i);
    for(int i:san)
    ans.push_back(i);
    for(int i:num6)
    ans.push_back(i);
    for(int i=0;i<ans.size();i++)
    {
        cout<<ans[i]<<(i==ans.size()-1?"\n":" ");
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
## D. Palindromex
### https://codeforces.com/contest/2227/problem/D
MEX を $1$ 以上にするためには、その子配列に必ず $0$ が含まれていなければなりません。  
回文の中心構造を考えると、以下の 2 つのケースしかありません。  
$0$ は 2 つあるので、それぞれの $0$ を中心として左右に同じ数字が続く限り拡張します。  
2 つの $0$ の間にある部分がすでに回文になっている場合、その 2 つの $0$ を端点とする回文が作れます。さらにその外側を拡張可能です。
もし 2 つの $0$ の間の区間が回文でない場合、その区間を含む回文を作ることは不可能です。
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll=long long;

int get_mex(int l,int r,const vector<int>& a)
{
    static bool has[200005];
    vector<int> active;
    for(int i=l;i<=r;i++)
    {
        if(a[i]<200005)
        {
            has[a[i]]=true;
            active.push_back(a[i]);
        }
    }
    int res=0;
    while(has[res])
    res++;
    for(int x:active) has[x]=false;
    return res;
}

bool is_palin(int l,int r,const vector<int>& a)
{
    while(l<r)
    {
        if(a[l]!=a[r]) return false;
        l++; r--;
    }
    return true;
}

void solve()
{
    int n;
    cin>>n;
    int temp=2*n;
    vector<int> a(temp);
    vector<int> p; 
    for (int i = 0; i < temp; i++)
    {
        cin>>a[i];
        if(a[i]==0) p.push_back(i);
    }

    int ans=1;
    {
        int l=p[0],r=p[0];
        while(l>0&& r<temp-1&&a[l-1]==a[r+1])
        {
            l--; r++;
        }
        ans=max(ans,get_mex(l,r,a));
    }

    {
        int l=p[1],r=p[1];
        while(l>0&&r<temp-1&&a[l-1]==a[r+1])
        {
            l--; r++;
        }
        ans=max(ans,get_mex(l,r,a));
    }
    if(is_palin(p[0],p[1],a))
    {
        int l=p[0],r=p[1];
        while(l>0&&r<temp-1&&a[l-1]==a[r+1])
        {
            l--; r++;
        }
        ans=max(ans,get_mex(l,r,a));
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
## E. It All Went Sideways
### https://codeforces.com/contest/2227/problem/E
「移動するブロック」を直接数えるよりも、「移動しないブロック」を最小化する方が考えやすいです。  
右側に重力がかかった後、高さ $h$ にあるブロックは、「もともと高さ $h$ 以上だったブロックの総数」だけ右側に並びます。  
具体的には、高さ $h$ のブロックが $C_h$ 個あるとき、それらは最終的にインデックス $n, n-1, \dots, n-C_h+1$ の列に配置されます。  
あるブロックが移動しない条件は：  
そのブロックがもともと列 $i$ にあり、右に移動した後の位置も列 $i$ であること。  
これは、「列 $i$ よりも右側に、自分と同じ高さ以上のブロックがいくつあるか」に依存します。  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

void solve()
{
    int n;
    cin>>n;
    vector<int> a(n);
    for(int i=0;i<n;i++) cin>>a[i];

    vector<int> s(n);
    s[n-1]=a[n-1];
    for(int i=n-2;i>=0;i--)
    {
        s[i]=min(a[i],s[i+1]);
    }

    ll ans_init=0;
    map<int,int> temp;
    for(int i=0;i<n;i++)
    {
        ans_init+=(a[i]-s[i]);
        temp[s[i]]++;
    }

    int max_temp=0;
    for(auto const& [val,count]:temp)
    {
        max_temp=max(max_temp,count);
    }

    cout<<ans_init+max(0,max_temp-1)<<"\n";
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
