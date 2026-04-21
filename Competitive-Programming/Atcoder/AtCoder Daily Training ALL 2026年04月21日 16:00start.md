# AtCoder Daily Training ALL 2026/04/21 16:00start
今夜は Codeforces があるので、AtCoder の E-G 3問のみ解きました。
## E - Omelette Restaurant
### https://atcoder.jp/contests/adt_all_20260421_1/tasks/abc446_c
卵の在庫管理をキューで管理し、「先入れ先出し」で消費・期限切れを処理。  
have 変数を用意して在庫の増減をリアルタイムに記録することで、最後にキューを走査して再計算する手間を省き、計算量を $O(1)$ に短縮した。  
eggs.front().firstを直接操作することで、コードを簡潔に保った。
```cpp
#include<bits/stdc++.h>
using namespace std;
void solve()
{
    int n,d;
    cin>>n>>d;
    queue<pair<long long,long long>> eggs;
    vector<pair<long long,long long>> did(n);
    for(int i=0;i<n;i++)
    {
        cin>>did[i].first;
    }
    for(int i=0;i<n;i++)
    {
        cin>>did[i].second;
    }
    long long have=0;
    for(int i=0;i<n;i++)
    {
        eggs.push({did[i].first,i});
        have+=did[i].first;
        while(did[i].second>0&&!eggs.empty())
        {
            if (eggs.front().first<=did[i].second)
            {
                did[i].second-=eggs.front().first;
                have-=eggs.front().first;
                eggs.pop();
            }
            else
            {
                eggs.front().first-=did[i].second;
                have-=did[i].second;
                did[i].second=0;
            }
        }
        while(!eggs.empty()&&i-eggs.front().second>=d)
        {
            have-=eggs.front().first;
            eggs.pop();
        }
    }
    cout<<have<<"\n";
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
## F - NewFolder(1)
### https://atcoder.jp/contests/adt_all_20260421_1/tasks/abc261_c
各文字列が何回出現したかを map で記録し、2回目以降は str + (count-1) の形式で出力。  
今回は vector<string> ans に一度保存してから最後に出力する形をとったが、これにより出力処理を一括で行える。  
制限時間内（2.0秒）で $N=2 \times 10^5$ を処理するのに、map の $O(N \log N)$ は十分に高速。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    map<string,int> fold;
    vector<string> ans;
    for(int i=0;i<n;i++)
    {
        string str;
        cin>>str;
        if(fold.count(str)==0)
        {
            fold[str]=1;
            ans.push_back(str);
        }
        else
        {
            fold[str]++;
            string temp=str;
            temp+='(';
            temp+=to_string(fold[str]-1);
            temp+=')';
            ans.push_back(temp);
        }
    }
    for(int i=0;i<ans.size();i++)
    {
        cout<<ans[i]<<"\n";
    }
    return 0;
}
```
## G - Happy New Year 2023
### https://atcoder.jp/contests/adt_all_20260421_1/tasks/abc284_d
$N$ が非常に大きいため（$9 \times 10^{18}$）、通常の素因数分解では間に合わないが、$p$ と $q$ のどちらかは必ず $\sqrt[3]{N}$ 以下になることに注目。  
$\sqrt[3]{N} \approx 2 \times 10^6$ までの試し割りで最初の因数 $i$ を見つければ、それが $p$ か $q$ かを $N/i$ が $i$ で割り切れるかどうかで判定可能。  
$p = \sqrt{N/q}$ を計算する際は、精度を保つために long double と round を使用。
```cpp
#include<bits/stdc++.h>
using namespace std;

void solve()
{
    long long n;
    cin>>n;

    for(long long i=2;i*i*i<=n;++i)
    {
        if(n%i==0)
        {
            if((n/i)%i==0)
            {
                long long p=i;
                long long q=n/(p*p);
                cout<<p<<" "<<q<<endl;
            }
            else
            {
                long long q=i;
                long long p=round(sqrt((long double)n/q));
                cout<<p<<" "<<q<<endl;
            }
            return;
        }
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
