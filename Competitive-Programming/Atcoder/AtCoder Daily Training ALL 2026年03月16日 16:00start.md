# AtCoder Daily Training ALL 2026/03/16 16:00start
今日の結果:  
コンテスト内 AC: A, B, C, D / コンテスト後 AC: E, F  
A-Dは20分掛かりましたが、E問題でE題で詰まってしまいました。
## A - Last Letter
### https://atcoder.jp/contests/adt_all_20260316_1/tasks/abc244_a
Aは単純な出力問題です。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    string s;
    cin>>n>>s;
    cout<<s[n-1]<<"\n";
    return 0;
}
```
## B - Many A+B Problems
### https://atcoder.jp/contests/adt_all_20260316_1/tasks/abc288_a
Bは単純な出力問題です。入力の同時にA＋Bを出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    while(n--)
    {
        long long a,b;
        cin>>a>>b;
        cout<<a+b<<"\n";
    }
    return 0;
}
```
## C - String Too Long
### https://atcoder.jp/contests/adt_all_20260316_1/tasks/abc414_b
入力を受け取りながら、現在の文字列の総長を逐次チェックしました。長さが 100 を超えた時点で、即座に Too Long を出力して早期終了させることで、余計なメモリ消費や処理を避けています。  
$l_i$ の値が非常に大きいため（最大 $10^{18}$）、先に長さを判定せずに append するとメモリ制限に引っかかります。そのため、加算のタイミングで判定することが重要です。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    string ans;
    long long len=0;
    while(n--)
    {
        char c;
        long long l;
        cin>>c>>l;
        len+=l;
        if(len>100)
        {
            cout<<"Too Long"<<"\n";
            return 0;
        }
        else
        {
            ans.append(l,c);
        }
    }
    cout<<ans<<"\n";
    return 0;
}
```
## D - Calculator
### https://atcoder.jp/contests/adt_all_20260316_1/tasks/abc386_b
後ろから順に走査する強欲法を採用しました。0 が現れた際、その前も 0 であれば 00 ボタンとしてカウントし、インデックスを2つ進めます。そうでなければ通常のボタンとして1つ進めます。」  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string num;
    cin>>num;
    int ans;
    while(num.size()>0)
    {
        if(num[num.size()-1]=='0')
        {
            if(num[num.size()-2]=='0')
            {
                ans++;
                num.erase(num.end()-1);
                num.erase(num.end()-1);
            }
            else
            {
                ans++;
                num.erase(num.end()-1);
            }
        }
        else
        {
            ans++;
            num.erase(num.end()-1);   
        }
    }
    cout<<ans<<"\n";
    return 0;
}
```
## E - Almost Equal
### https://atcoder.jp/contests/adt_all_20260316_1/tasks/abc302_c
$N$ が最大 8 と小さいため、DFS（深さ優先探索） による全探索で解きました。各文字列を頂点、1文字違いの文字列間に辺を張ったグラフにおいて、ハミルトン路を見つける問題と言い換えられます。  
コンテスト後の解き直しでは、バックトラッキング時の状態管理に注意しました。ある探索ルートが失敗した際、used フラグを正しくリセットすることで、他のルートを正確に探索できるよう改善しました。  
実はこの問題は$N$ が小さい順列の問題で、STL の std::next_permutation を用いた順列全探索が非常に便利です。再帰関数によるバックトラッキングを記述する必要がなく、実装ミスを減らすことができます。  
```cpp
#include<bits/stdc++.h>
using namespace std;

vector<bool> used(10,false);
vector<string> str;

bool check(string a,string b)
{
    int len=a.size();
    int diff=0;
    for(int i=0;i<len;i++)
    {
        if(a[i]!=b[i])
        diff++;
    }
    if(diff==1)
    return true;
    return false;
}

bool checkdfs(int i,int count)
{
    if(count==str.size())
    return true;

    used[i]=true;
    for(int j=0;j<str.size();j++)
    {
        if(i==j)continue;
        if(used[j])continue;
        if(check(str[i],str[j]))
        {
            if(checkdfs(j,count+1))
            return true;
        }
    }
    used[i]=false;
    return false;
    
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,len;
    cin>>n>>len;
    for(int i=0;i<n;i++)
    {
        string tp;
        cin>>tp;
        str.push_back(tp);
    }
    
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<used.size();j++)
        used[j]=false;
        if(checkdfs(i,1))
        {
            cout<<"Yes"<<"\n";
            return 0;
        }
    }
    cout<<"No"<<"\n";
    return 0;
}
```
## F - Triangle?
### https://atcoder.jp/contests/adt_all_20260316_1/tasks/abc224_c
2次元ベクトルの外積（クロス積）を用いて、3点が一直線上にあるかどうかを判定しました。傾き（スロープ）を計算するとゼロ除算のリスクがありますが、この外積の式なら整数のみで安全に判定できます。  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<pair<ll,ll>> xy(n);
    for(int i=0;i<n;i++)
    cin>>xy[i].first>>xy[i].second;

    int ans=0;
    for(int i=0;i<n;i++)
    {
        for(int j=i+1;j<n;j++)
        {
            for(int k=j+1;k<n;k++)
            {
                if((xy[j].first-xy[i].first)*(xy[k].second-xy[i].second)-(xy[k].first-xy[i].first)*(xy[j].second-xy[i].second)!=0)ans++;
            }
        }
    }
    cout<<ans<<"\n";
    return 0;
}
```
