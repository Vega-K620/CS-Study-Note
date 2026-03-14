# AtCoder Beginner Contest 449
今日のABCは問題A-Dを解きました。今夜は遅いだからまずA-Cを書きます。
## A - π
### https://atcoder.jp/contests/abc449/tasks/abc449_a
この問題は逆余弦関数を用いて円周率を求める。
```cpp
#include<bits/stdc++.h>
using namespace std;

const double PI = acos(-1.0);

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);

    int d;
    cin>>d;
    double r = d/2.0;
    double ans = PI*r*r;
    cout<<fixed<<setprecision(15)<<ans<<"\n";
    return 0;
}
```
## B - Deconstruct Chocolate
### https://atcoder.jp/contests/abc449/tasks/abc449_b
現在のチョコレートの高さ（ $H$ ）と幅（ $W$ ）を保持し、クエリごとに更新する。  
下から $R$ 行食べる場合： 食べる数は $R \times W$ 。その後、 $H = H - R$ 。  
右から $C$ 列食べる場合： 食べる数は $C \times H$ 。その後、 $W = W - C$ 。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int h,w,q;
    cin>>h>>w>>q;
    for(int i=0;i<q;i++)
    {
        int way;
        cin>>way;
        if(way==1)
        {
            int r;
            cin>>r;
            cout<<r*w<<"\n";
            h-=r;
        }
        else
        {
            int c;
            cin>>c;
            cout<<c*h<<"\n";
            w-=c;
        }
    }
    return 0;
}
```
## C - Comfortable Distance
### https://atcoder.jp/contests/abc449/tasks/abc449_c
二重ループでは $O(N^2)$ かかるため、スライディングウィンドウを用いて $O(N)$ で解く。  
ウィンドウの更新： インデックス $j$ を右に動かしながら、新しく範囲内に入った文字 $S_{j-L}$ をカウントし、範囲外になった文字 $S_{j-R-1}$ を除外する。  
カウントの合算： 各ステップで、現在のウィンドウ内にある文字 $S_j$ と同じ文字の数を答えに加算する。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,l,r;
    cin>>n>>l>>r;
    string s;
    cin>>s;
    long long ans=0;
    vector<int> cnt(26,0);
    for(int j=0;j<n;j++)
    {
        int new_i=j-l;
        if(new_i>=0)
        cnt[s[new_i]-'a']++;

        int old_i=j-r-1;
        if(old_i>=0)
        cnt[s[old_i]-'a']--;

        if(j>=l)
        ans+=cnt[s[j]-'a'];
    }

    cout<<ans<<"\n";
    
    return 0;
}
```
