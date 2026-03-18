# AtCoder Daily Training ALL 2026/03/18 18:00start
今日A-FがACしました。A-Dは暴力の方法で解きました。
## A - First Grid
### https://atcoder.jp/contests/adt_all_20260318_2/tasks/abc229_a
4つの隣接パターンを網羅して、AC（正解）できました。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    char c[4];
    cin>>c[0]>>c[1]>>c[2]>>c[3];
    if(c[0]==c[1]||c[2]==c[3]||c[0]==c[2]||c[1]==c[3])
        cout<<"Yes\n";
    else
        cout<<"No\n";
    return 0;
}
```
## B - Wrong Answer
### https://atcoder.jp/contests/adt_all_20260318_2/tasks/abc343_a
0から9までを全探索し、条件を満たす値を最初に見つけた時点で出力して終了しました。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int a,b;
    cin>>a>>b;
    for(int i=0;i<10;i++)
    {
        if(i!=a+b)
        {
            cout<<i<<"\n";
            break;
        }
    }
    return 0;
}
```
## C - Qualification Contest
### https://atcoder.jp/contests/adt_all_20260318_2/tasks/abc288_b
問題文の辞書順の定義を忠実に cmp 関数で実装し、上位K人をソートしてACしました。  
string のデフォルトの比較演算と同じ処理を自作して、正しく動作させることができました。  
```cpp
#include<bits/stdc++.h>
using namespace std;

bool cmp(string a,string b)
{
    int l=min(a.size(),b.size());
    for(int i=0;i<l;i++)
    {
        if(a[i]!=b[i])
        {
            return a[i]<b[i];
        }
    }
    return a.size()<b.size();
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,k;
    cin>>n>>k;
    vector<string> s(k);
    for(int i=0;i<k;i++)
    cin>>s[i];
    sort(s.begin(),s.end(),cmp);
    for(int i=0;i<k;i++)
    {
        cout<<s[i]<<"\n";
    }
    return 0;
}
```
## D - A^A
### https://atcoder.jp/contests/adt_all_20260318_2/tasks/abc327_b
$A^A$ は $A$ の増加に対して非常に急激に大きくなるため、全探索の範囲がごくわずかで済むことに着目しました。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    long long b;
    cin>>b;
    vector<long long> box;
    for(int i=1;i<=15;i++)
    {
        long long ans=1;
        for(int j=0;j<i;j++)
        {
            ans*=i;
        }
        box.push_back(ans);
    }
    for(int i=0;i<box.size();i++)
    {
        if(box[i]==b)
        {
            cout<<i+1<<"\n";
            return 0;
        }
    }
    cout<<-1<<"\n";
    return 0;
}
```
## E - Dislike Foods
### https://atcoder.jp/contests/adt_all_20260318_2/tasks/abc402_c
考え方: 各料理 $i$ について、使われている食材の数 $K_i$ を保持する配列 rem を用意。  
工夫: 食材 $B_j$ を克服した際、その食材を含む料理すべての rem を $-1$ する。rem が $0$ になった料理は「食べられる料理」としてカウントに加える。  
ポイント: 「どの食材がどの料理に使われているか」を事前にリスト化しておくことで、無駄なループを回さずに済んだ。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;

    vector<int> rem(m);
    vector<vector<int>> adj(n+1);

    for (int i=0;i<m;i++)
    {
        int k;
        cin>>k;
        rem[i]=k;
        for(int j=0;j<k;j++)
        {
            int a;
            cin>>a;
            adj[a].push_back(i);
        }
    }

    int edible_count=0;
    for(int i=0;i<n;i++)
    {
        int b;
        cin>>b;
        
        for(int dish_idx : adj[b])
        {
            rem[dish_idx]--;
            if(rem[dish_idx]==0)
            {
                edible_count++;
            }
        }
        cout<<edible_count<<"\n";
    }
    return 0;
}
```
## F - One Time Swap
### https://atcoder.jp/contests/adt_all_20260318_2/tasks/abc345_c
全通りのペアから、同じ文字同士を入れ替える『無意味な操作』を数学的に差し引くことで、重複を回避しました。  
余事象（入れ替えても変わらないパターン）を利用して、$O(N)$ で計算量を抑えるのがポイントでした。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string s;
    cin>>s;
    long long n=s.size();

    vector<long long> cnt(26,0);
    for(char c : s)
    {
        cnt[c-'a']++;
    }

    long long ans=n*(n-1)/2;

    bool has_duplicate = false;
    for(int i=0;i<26;i++)
    {
        if(cnt[i]>=2)
        {
            has_duplicate=true;
            ans-=cnt[i]*(cnt[i]-1)/2;
        }
    }
    if(has_duplicate)
    {
        ans+=1;
    }

    cout<<ans<<"\n";
    return 0;
}
```
