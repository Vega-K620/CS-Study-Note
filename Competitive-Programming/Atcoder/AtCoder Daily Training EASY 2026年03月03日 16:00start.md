# AtCoder Daily Training EASY 2026/03/03 16:00start
<details>
  <summary>(今回の感想)</summary>
  今日のDaily Training EASYもAKしました。
  でも時間がたくさん掛かりました。
  第A問題は13分掛かって、大体いい方法が見つかりませんでした。
</details>

## A - Jiro
### https://atcoder.jp/contests/adt_easy_20260303_1/tasks/abc371_a
この問題、僕は脳死で解きました。
次回からは、各キャラクターの勝利数をカウントしてランキングを作るような、
より拡張性のあるコードを書きたいです。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    ios::sync_with_stdio(false);cin.tie(NULL);
    char char1,char2,char3;

    cin>>char1>>char2>>char3;
    if((char1=='<'&&char2=='<'&&char3=='<')||char1=='>'&&char2=='>'&&char3=='>')
    {
        cout<<'B'<<"\n";
    }
    else if((char1=='<'&&char2=='<'&&char3=='>')||char1=='>'&&char2=='>'&&char3=='<')
    {
        cout<<'C'<<"\n";
    }
    else
    {
        cout<<'A'<<"\n";
    }
    return 0;
}
```
改善後のコード
```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
    ios::sync_with_stdio(false);cin.tie(NULL);
    char s_ab,s_ac,s_bc;
    cin>>s_ab>>s_ac>>s_bc;

    map<char, int> wins;
    
    if (s_ab=='>')
    wins['A']++;
    else wins['B']++;
    if (s_ac=='>')
    wins['A']++;
    else wins['C']++;
    if (s_bc=='>')
    wins['B']++;
    else wins['C']++;

    for(char name : {'A', 'B', 'C'})
    {
        if(wins[name]==1)
        {
            cout << name << endl;
            break;
        }
    }
    return 0;
}
```
## B - New Scheme
### https://atcoder.jp/contests/adt_easy_20260303_1/tasks/abc308_a
この問題は簡単です。
一つ一つでチェックします。もし問題あれば「No」を出力します。
最後までないと「Yes」を出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    ios::sync_with_stdio(false);cin.tie(NULL);
    int num[8];
    for(int i=0;i<8;i++)
    cin>>num[i];
    int flag=true;
    for(int i=0;i<8;i++)
    {
        if(i!=7)
        {
            if(num[i]>num[i+1])
            {
                cout<<"No"<<"\n";
                flag=false;
                break;
            }
        }
        if(num[i]<100||num[i]>675)
        {
            cout<<"No"<<"\n";
            flag=false;
            break;
        }
        if(num[i]%25!=0)
        {
            cout<<"No"<<"\n";
            flag=false;
            break;
        }
    }
    if(flag)
    {
        cout<<"Yes"<<"\n";
    }
    return 0;
}
```
## C - Sum of Digits Sequence
### https://atcoder.jp/contests/adt_easy_20260303_1/tasks/abc427_b
特殊なアルゴリズムは不要。問題文の数式をそのまま適用するだけで解ける問題です。
```cpp
#include<bits/stdc++.h>
using namespace std;

int f(int n)
{
    int ans=0;
    while(n>0)
    {
        ans+=n%10;
        n/=10;
    }
    return ans;
}

int main()
{
    ios::sync_with_stdio(false);cin.tie(NULL);
    vector<int> num(101,0);
    num[0]=1;num[1]=1;
    for(int i=2;i<101;i++)
    {
        num[i]=num[i-1]+f(num[i-1]);
    }
    int n;
    cin>>n;
    cout<<num[n]<<"\n";
    return 0;
}
```
## D - Subscribers
### https://atcoder.jp/contests/adt_easy_20260303_1/tasks/abc304_b
この問題も特殊なアルゴリズムは不要。問題文の数式をそのまま適用するだけで解ける問題です。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    ios::sync_with_stdio(false);cin.tie(NULL);
    long long n;
    cin>>n;
    if(n<1e3)
    cout<<n<<"\n";
    else if(n<1e4)
    cout<<n/10*10<<"\n";
    else if(n<1e5)
    cout<<n/100*100<<"\n";
    else if(n<1e6)
    cout<<n/1000*1000<<"\n";
    else if(n<1e7)
    cout<<n/10000*10000<<"\n";
    else if(n<1e8)
    cout<<n/100000*100000<<"\n";
    else if(n<1e9)
    cout<<n/1000000*1000000<<"\n";
    return 0;
}
```
## E - Snake Queue
### https://atcoder.jp/contests/adt_easy_20260303_1/tasks/abc389_c
一見すると「Queue」を用いる問題のように見えますが、
実際にシミュレーションを行うと計算量やメモリ消費が膨大になってしまいます。
しかし、この問題は累積和の考え方に変換することで、効率的に解くことが可能です。
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int main()
{
    ios::sync_with_stdio(false);cin.tie(NULL);
    int q,front_ptr=0;
    cin >> q;
    vector<ll> abs_pos;
    ll total_len=0;
    while(q--)
    {
        int type;
        cin>>type;
        if(type==1)
        {
            ll l;
            cin >> l;
            abs_pos.push_back(total_len);
            total_len+=l;
        }
        else if(type==2)
        {
            front_ptr++;
        }
        else
        {
            int k;
            cin>>k;
            cout<<abs_pos[front_ptr+k-1]-abs_pos[front_ptr]<<"\n";
        }
    }
    return 0;
}
```
