# AtCoder Daily Training EASY 2026/03/02 18:00start
<details>
  <summary>(今回の感想)</summary>
  今日Daily Training EASYをしました。成功的にAKをもらいました。これが私の問題解決アプローチです。
</details>

## A - Attack
```
実行時間制限: 2 sec / メモリ制限: 1024 MiB

配点 : 100 点

問題文
体力が A の敵がいます。あなたは、1 回攻撃をすることで敵の体力を B 減らすことが出来ます。
敵の体力を 0 以下にするためには、最小で何回攻撃をする必要があるでしょうか？

制約
1≤A,B≤10^18
A,B は整数である。
入力
入力は以下の形式で標準入力から与えられる。
A 
B
出力
答えを出力せよ。

入力例 1
7 3
出力例 1
3
3 回攻撃すると敵の体力が 
−2 となります。
2 回攻撃しただけでは敵の体力は 
1 であるため、
3 回攻撃する必要があります。

入力例 2
123456789123456789 987654321
出力例 2
124999999

入力例 3
999999999999999998 2
出力例 3
499999999999999999
```
問題Aは簡単だと思います。A/Bは答えです。しかしA％B!=0の時は答えを1つ増やす必要があります。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    long long A,B;
    cin>>A>>B;
    if(A%B==0)
    {
        cout<<A/B<<"\n";
    }
    else
    {
        cout<<A/B+1<<"\n";
    }
    return 0;
}
```
## B - Status Code
```
実行時間制限: 2 sec / メモリ制限: 1024 MiB

配点 : 100 点

問題文
100 以上 999 以下の整数 S が与えられます。
S が 200 以上 299 以下のとき Success 、そうでないとき Failure と出力してください。

制約
100≤S≤999
S は整数
入力
入力は以下の形式で標準入力から与えられる。
S
出力
答えを出力せよ。

入力例 1
200
出力例 1
Success
200 は 200 以上 299 以下なので、Success と出力してください。

入力例 2
401
出力例 2
Failure
401 は 200 以上 299 以下ではないので、Failure と出力してください。

入力例 3
999
出力例 3
Failure
```
問題Bも簡単です。「if」を使って解決できます。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    int s;
    cin>>s;
    if(s>=200&&s<=299)
    {
        cout<<"Success"<<"\n";
    }
    else
    {
        cout<<"Failure"<<"\n";
    }
    return 0;
}
```
## C - cat 2
```
実行時間制限: 2 sec / メモリ制限: 1024 MiB

配点 : 200 点

問題文
N 種類の文字列 S1 ,S2 ,…,SNが与えられます。
あなたは、次の操作を 1 度だけ行います。
相異なる整数 i,j (1≤i≤N,1≤j≤N) を選び、Si と Sjをこの順で連結する。
この操作で連結した結果の文字列としてありえるものは何通りあるか求めてください。
選んだ (i,j) が異なっても、連結した文字列が同じ場合は 
1 通りと数えることに注意してください。

制約
2≤N≤100
N は整数
Siは英小文字からなる長さ 1 以上 10 以下の文字列
Si!=Sj(1≤i<j≤N)
入力
入力は以下の形式で標準入力から与えられる。
N
S1
S2
⋮
SN
出力
操作の結果できる文字列が何通りあるかを出力せよ。

入力例 1
4
at
atco
coder
der
出力例 1
11
できる文字列は、atatco, atcoat, atcoder, atcocoder, atder,
coderat, coderatco, coderder, derat, deratco, dercoder の 
11 通りです。
よって、11 を出力してください。

入力例 2
5
a
aa
aaa
aaaa
aaaaa
出力例 2
7
できる文字列は、aaa, aaaa, aaaaa, aaaaaa, aaaaaaa,
aaaaaaaa, aaaaaaaaa の 7 通りです。
よって、7 を出力してください。

入力例 3
10
armiearggc
ukupaunpiy
cogzmjmiob
rtwbvmtruq
qapfzsitbl
vhkihnipny
ybonzypnsn
esxvgoudra
usngxmaqpt
yfseonwhgp
出力例 3
90
```
この問題は「set」を使って全探索で答えを得る。主に「set」の自動重複排除機能を活用します。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    set<string> ans;
    int n;
    cin>>n;
    vector<string> s(n);
    for(int i=0;i<n;i++)
    cin>>s[i];
    for(int l=0;l<n;l++)
    {
        for(int r=0;r<n;r++)
        {
            if(l==r)
            continue;
            else
            {
                ans.insert(s[l]+s[r]);
            }
        }
    }
    cout<<ans.size()<<"\n";
    return 0;
}
```
DとEは後で追加する。
