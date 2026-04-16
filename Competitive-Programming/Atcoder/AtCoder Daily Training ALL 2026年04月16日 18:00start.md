# AtCoder Daily Training ALL 2026/04/16 18:00start
今日の練習では A 题から G 题までを完答できました。全体的にスムーズに進められましたが、特に B 题については、ビット演算（位运算）の性質を利用する問題であることに気づくのに少し時間がかかりました。  
A-D 题：基本的には実装力と単純な論理を問う問題で、素早く解くことができました。  
B 题：配点が $1, 2, 4$ という $2^n$ の形式であったため、本質的には「ビット和（OR）」を求める問題でした。この視点を持つことでコードを大幅に簡略化できることを再認識しました。  
E-G 题：F 题のインデックス・オフセットや、G 题の前後綴分解など、競技プログラミングで頻出のテクニックを復習する良い機会になりました。
## A - World Cup
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int year;
    cin>>year;
    if(year%4==2)
    cout<<year<<"\n";
    else if(year%4==0)
    cout<<year+2<<"\n";
    else
    cout<<year+year%4<<"\n";
    return 0;
}
```
## B - 1-2-4 Test
### https://atcoder.jp/contests/adt_all_20260416_2/tasks/abc270_a
実はビット演算の典型問題でした。配点が $1, 2, 4$ という $2^n$ の形式であったため、本質的には a | b だけで完結します。数字に対する敏感さが足りず、冗長な if-else 文を書いてしまい、時間をロスしました。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int a,b;
    cin>>a>>b;
    bool ab[3]={false},bb[3]={false};
    if(a==1)
    ab[0]=1;
    else if(a==2)
    ab[1]=1;
    else if(a==4)
    ab[2]=1;
    else if(a==3)
    ab[0]=ab[1]=1;
    else if(a==5)
    ab[0]=ab[2]=1;
    else if(a==6)
    ab[1]=ab[2]=1;
    else if(a==7)
    ab[0]=ab[1]=ab[2]=1;
    if(b==1)
    bb[0]=1;
    else if(b==2)
    bb[1]=1;
    else if(b==4)
    bb[2]=1;
    else if(b==3)
    bb[0]=bb[1]=1;
    else if(b==5)
    bb[0]=bb[2]=1;
    else if(b==6)
    bb[1]=bb[2]=1;
    else if(b==7)
    bb[0]=bb[1]=bb[2]=1;
    bool ans[3]={false};
    for(int i=0;i<3;i++)
    {
        if(ab[i]||bb[i])
        ans[i]=1;
    }
    int out=0;
    if(ans[0])out+=1;
    if(ans[1])out+=2;
    if(ans[2])out+=4;
    cout<<out<<"\n";
    return 0;
}
```
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int a,b;
    cin>>a>>b;
    cout<<(a|b)<<endl; 
    return 0;
}
```
## C - chess960
### https://atcoder.jp/contests/adt_all_20260416_2/tasks/abc297_b
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string s;
    cin>>s;
    int b1=-1,b2=-1;
    int r1=-1,r2=-1,k=-1;
    for(int i=0;i<s.size();i++)
    {
        if(s[i]=='B')
        {
            if(b1==-1)b1=i;
            else
            b2=i;
        }
        if(s[i]=='R')
        {
            if(r1==-1)r1=i;
            else
            r2=i;
        }
        if(s[i]=='K')
        k=i;
    }
    if(b1%2!=b2%2&&r1<k&&k<r2)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
    return 0;
}
```
## D - Digit Sum
### https://atcoder.jp/contests/adt_all_20260416_2/tasks/abc444_b
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int check(int i)
{
    int sum=0;
    while(i>0)
    {
        sum+=i%10;
        i/=10;
    }
    return sum;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,k;
    cin>>n>>k;
    int ans=0;
    for(int i=1;i<=n;i++)
    {
        if(check(i)==k)
        ans++;
    }
    cout<<ans<<"\n";
    return 0;
}
```
## E - ~
### http://atcoder.jp/contests/adt_all_20260416_2/tasks/abc406_c
E 题では、連続する「増加・減少・増加」のパターンをカウントしました。
最初は**単調スタックの利用も検討しましたが、本題は「連続性」が重要であるため、単純な線形走査**によるセグメント分割の方が効率的だと判断しました。  
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main() {
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<int> num(n);
    for(int i=0;i<n;i++)cin>>num[i];

    if(n<4)
    {
        cout<<0<<endl;
        return 0;
    }
    vector<ll> lengths;
    int cur_len=1;
    for(int i=1;i<n;i++)
    {
        bool trend_changed=false;
        if(i>1)
        {
            if((num[i]>num[i-1]&&num[i-1]<num[i-2])||(num[i]<num[i-1]&&num[i-1]>num[i-2]))
            {
                trend_changed=true;
            }
        }
        
        if(trend_changed)
        {
            lengths.push_back(cur_len);
            cur_len=2;
        }
        else
        {
            cur_len++;
        }
    }
    lengths.push_back(cur_len);
    ll ans=0;
    bool is_up=(num[1]>num[0]);

    for(int i=0;i+2<lengths.size();i++)
    {
        if(is_up)
        {
            ans+=(lengths[i]-1)*(lengths[i+2]-1);
        }
        is_up=!is_up;
    }
    cout<<ans<<endl;
    return 0;
}
```
## F - Rotatable Array
### https://atcoder.jp/contests/adt_all_20260416_2/tasks/abc410_c
配列の先頭を末尾に移動させる操作（タイプ 3）は、実際に配列を組み替えると $O(N)$ かかり、クエリ数 $Q$ に対して $O(NQ)$ で TLE になります。  
これを解決するために、**「配列のどこが現在の先頭か」を示す offset（偏移量）**のみを管理する手法を用いました。  
タイプ 3：offset = (offset + k) % n とすることで、計算量を $O(1)$ に抑制。  
タイプ 1 & 2：現在の $p$ 番目は、元の配列では (p - 1 + offset) % n 番目に対応します。
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,q;
    cin>>n>>q;
    vector<int> a(n);
    for (int i=0;i<n;i++)
    {
        a[i]=i+1;
    }
    ll offset=0;
    while(q--)
    {
        int type;
        cin>>type;
        if(type==1)
        {
            int p,x;
            cin>>p>>x;
            int real_idx=(p-1+offset)%n;
            a[real_idx]=x;
        }
        else if(type==2)
        {
            int p;
            cin>>p;
            int real_idx=(p-1+offset)%n;
            cout<<a[real_idx]<<"\n";
        }
        else if(type==3)
        {
            ll k;
            cin>>k;
            offset=(offset+k)%n;
        }
    }

    return 0;
}
```
## G - Left Right Operation
### https://atcoder.jp/contests/adt_all_20260416_2/tasks/abc263_d
この問題の難しさは、「左側を $L$ で置き換える範囲 $x$」と「右側を $R$ で置き換える範囲 $y$」が互いに干渉し合うように見える点にあります。しかし、ある境界 $i$ で数列を分割して考えると、非常にシンプルになります。  
左側からの最適化 ($f[i]$)：$1$ から $i$ 番目までの要素について、どこまでを $L$ に置き換えるのが得かを DP で求めます。$f[i] = \min(i \times L, f[i-1] + a[i])$（「全部 $L$ にしちゃう」か、「今までの最適解に現在の値をそのまま足す」かの選択）  
右側からの最適化 ($g[i]$)：同様に、$n$ から $i$ 番目に向かって、どこまでを $R$ に置き換えるのが得かを求めます。$g[i] = \min((n-i+1) \times R, g[i+1] + a[i])$  
マージ（第三のループ）：最後に、すべての可能な分割点 $i$ について、$f[i] + g[i+1]$ を計算し、その最小値を探します。これは「左側のベスト」と「右側のベスト」をどこで繋ぐのが最強かを全探索していることになります。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    ll n,l,r;
    cin>>n>>l>>r;
    vector<ll> a(n+1);
    for(int i=1; i<=n;i++)
    cin>>a[i];

    vector<ll> f(n+2,0),g(n+2,0);

    for (int i=1;i<=n;i++)
    {
        f[i]=min(i*l,f[i-1]+a[i]);
    }

    for(int i=n;i>=1;i--)
    {
        g[i]=min((n-i+1)*r,g[i+1]+a[i]);
    }

    ll ans=g[1];
    for(int i=0;i<=n;i++)
    {
        ans=min(ans,f[i]+g[i+1]);
    }

    cout<<ans<<endl;
    return 0;
}
```
