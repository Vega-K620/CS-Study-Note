# AtCoder Beginner Contest 449
今日のABCは問題A-Dを解きました。
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
## D - Make Target 2
### https://atcoder.jp/contests/abc449/tasks/abc449_d
各 $x$ について、$|y|$ との大小関係（だいしょうかんけい）で $max$ の値が変わるため、$y$ の範囲を 3 つのセクション（$y < -|x|$, $-|x| \le y \le |x|$, $y > |x|$）に分割（ぶんかつ）して計算する。
```cpp
#include<bits/stdc++.h>
using namespace std;

using ll = long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    ll L,R,D,U;
    if (!(cin>>L>>R>>D>>U))return 0;

    ll total=0;
    for(ll x=L;x<=R;++x)
    {
        ll ax=abs(x);
        
        ll mid_l=max(D,-ax);
        ll mid_r=min(U,ax);
        if(mid_l<=mid_r)
        {
            if(ax%2==0)
            {
                total+=(mid_r-mid_l+1);
            }
        }

        ll up_l=max(D,ax+1),up_r=U;
        if (up_l<=up_r)
        {
            ll first=(up_l%2==0) ? up_l : up_l+1;
            ll last=(up_r%2==0) ? up_r : up_r-1;
            if(first<=last)
            total+=(last-first)/2+1;
        }

        ll down_l=D,down_r=min(U,-ax-1);
        if(down_l<=down_r)
        {
            ll first=(down_l%2==0) ? down_l : down_l+1;
            ll last=(down_r%2==0) ? down_r : down_r-1;
            if(first<=last)
            total+=(last-first)/2+1;
        }
    }

    cout<<total<<"\n";
    return 0;
}
```
