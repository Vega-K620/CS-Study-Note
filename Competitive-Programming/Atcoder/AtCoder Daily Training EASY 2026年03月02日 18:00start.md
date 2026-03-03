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
## D - Strictly Superior
```
実行時間制限: 2 sec / メモリ制限: 1024 MiB

配点 : 200 点

問題文
AtCoder 商店には 
N 個の商品があります。 
i (1≤i≤N) 番目の商品の価格は Pi です。 
i (1≤i≤N) 番目の商品は Ci 個の機能をもち、
i (1≤i≤N) 番目の商品の j (1≤j≤Ci) 番目の機能は 1 以上 M 以下の整数 
Fi,jとして表されます。
高橋くんは、AtCoder 商店の商品で一方が一方の上位互換であるものがないか気になりました。 
i 番目の商品と j 番目の商品 
(1≤i,j≤N) であって、次の条件をすべて満たすものがあるとき Yes と、ないとき No と出力してください。
Pi≥Pjである。
j 番目の製品は i 番目の製品がもつ機能をすべてもつ。
Pi>Pjであるか、
j 番目の製品は i 番目の製品にない機能を 1 つ以上もつ。

制約
2≤N≤100
1≤M≤100
1≤Pi≤10^5 (1≤i≤N)
1≤Ci≤M (1≤i≤N)
1≤Fi,1<Fi,2<⋯<Fi,Ci≤M (1≤i≤N)
入力はすべて整数

入力
入力は以下の形式で標準入力から与えられる。

N M
P1 C1 F1,1 F1,2 … F1,C1
​P2 C2 F2,1 F2,2 … F2,C2
​⋮
PN CN FN,1 FN,2 … FN,CN
​
出力
答えを 
1 行で出力せよ。

入力例 1
5 6
10000 2 1 3
15000 3 1 2 4
30000 3 1 3 5
35000 2 1 5
100000 6 1 2 3 4 5 6
出力例 1
Yes
(i,j)=(4,3) とすると、条件を全て満たします。
他の組は条件を満たしません。例えば 
(i,j)=(4,5) とすると 
j 番目の商品は i 番目の商品の機能をすべてもっていますが、
Pi<Pjなので上位互換ではありません。

入力例 2
4 4
3 1 1
3 1 2
3 1 2
4 2 2 3
出力例 2
No
まったく同じ価格と機能をもった商品がある場合もあります。

入力例 3
20 10
72036 3 3 4 9
7716 4 1 2 3 6
54093 5 1 6 7 8 10
25517 7 3 4 5 6 7 9 10
96930 8 2 3 4 6 7 8 9 10
47774 6 2 4 5 6 7 9
36959 5 1 3 4 5 8
46622 7 1 2 3 5 6 8 10
34315 9 1 3 4 5 6 7 8 9 10
54129 7 1 3 4 6 7 8 9
4274 5 2 4 7 9 10
16578 5 2 3 6 7 9
61809 4 1 2 4 5
1659 5 3 5 6 9 10
59183 5 1 2 3 4 9
22186 4 3 5 6 8
98282 4 1 4 7 10
72865 8 1 2 3 4 6 8 9 10
33796 6 1 3 5 7 9 10
74670 4 1 2 6 8
出力例 3
Yes
```
この問題は全探索で答えることができます。基準を満たすものを見つけるだけです。
まずはPi>=Pjのものを探します。
そして条件を満たすの中でiのすべての機能を走査し、それらがすべてF[j]にあるかどうかを確認します。
最後に値段と機能をチェックします、もし中の一つが条件を満たすたら「Yes」を出力します。
もし最後まで条件が満たすものがないなら「No」を出力します。
```cpp
#include <bits/stdc++.h>
using namespace std;
int main()
{
    int N,M;
    cin>>N>>M;
    vector<int> P(N);
    vector<int> C(N);
    vector<set<int>> F(N);
    
    for(int i=0;i<N;i++)
	{
        cin>>P[i]>>C[i];
        for (int j=0;j<C[i];j++)
		{
            int f;
            cin >> f;
            F[i].insert(f);
        }
    }
    
    for(int i=0;i<N;i++)
	{
        for(int j=0;j<N;j++)
		{
            if(i==j)
            continue;
            if(P[i]<P[j])
            continue;
            bool hasAllFunctions=true;
            for(int func:F[i])
			{
                if(F[j].find(func)==F[j].end())
				{
                    hasAllFunctions=false;
                    break;
                }
            }
            if(!hasAllFunctions)
            continue;
            if(P[i]>P[j])
			{
                cout<<"Yes"<<endl;
                return 0;
            }
            for(int func:F[j])
			{
                if(F[i].find(func)==F[i].end())
				{
                    cout<<"Yes"<<endl;
                    return 0;
                }
            }
        }
    }
    cout<<"No"<<endl;
    return 0;
}
```
