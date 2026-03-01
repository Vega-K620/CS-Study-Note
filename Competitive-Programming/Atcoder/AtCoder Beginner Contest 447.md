# AtCoder Beginner Contest 447
<details>
<summary>(今日の感想)</summary>
  今日のコンテストは少し残念と思っています、別の用事があるのでA問題だけいりました。
  で初めてのコンテストで３分間掛かってA問題を解決したはいい始まると思います。
</details>

## A - Seats 2
```
実行時間制限: 2 sec / メモリ制限: 1024 MiB

配点 : 
100 点

問題文
N 個の座席が横一列に並んでいます。各座席には最大で 
1 人まで座ることができます。以下の条件を満たすように 
M 人の人を座席に座らせることができるかどうか判定してください。

隣り合う 
2 つの席の両方に人が座ってはいけない。

制約
1≤N,M≤100
入力される値は全て整数
入力
入力は以下の形式で標準入力から与えられる。

N 
M
出力
条件を満たすように座席に座らせることが可能ならば Yes を、不可能ならば No を出力せよ。

入力例 1
Copy
6 3
出力例 1
Copy
Yes
例えば、左から 
1,3,6 番目に座らせれば可能です。したがって Yes を出力してください。

入力例 2
Copy
4 3
出力例 2
Copy
No
条件を満たすような座らせ方はありません。したがって No を出力してください。

入力例 3
Copy
5 3
出力例 3
Copy
Yes
入力例 4
Copy
44 7
出力例 4
Copy
Yes
```
A問題は簡単だと思います。2つの席の両方に人がすわってはいけないから、席かM個あるなら、座らせるの最大数は(M+1)/2
```
#include<bits/stdc++.h>
using namespace std;
int main()
{
  ios::sync_with_stdio(false);cin.tie(NULL);
  int N,M;
  cin>>N>>M;
  if((N+1)/2>=M)
    cout<<"Yes"<<"\n";
  else
    cout<<"No"<<"\n";
  return 0;
}
```
## B - mpp
```
実行時間制限: 2 sec / メモリ制限: 1024 MiB

配点 : 
200 点

問題文
英小文字からなる文字列 
S が与えられます。 
S の中で出現回数が最も多い文字をすべて取り除き、残った文字を元の順序を保ったまま連結して出力してください。

なお、出現回数が最大の文字が複数種類ある場合は、そのすべてを取り除いてください。

制約
1≤∣S∣≤100
S は英小文字からなる文字列である
入力
入力は以下の形式で標準入力から与えられる。

S
出力
答えを出力せよ。

入力例 1
Copy
mississippi
出力例 1
Copy
mpp
mississippi に最も多く出現する文字は s と i でともに 
4 回出現します。s と i をすべて取り除いた文字列は mpp となります。

入力例 2
Copy
atcoder
出力例 2
Copy
入力例 3
Copy
beginner
出力例 3
Copy
bgir
```
この問題はまず文字の出現した回数を記録する。そして最も多く出現する文字は出力する時出力しません、他の文字は普通で出力します。
```
#include<bits/stdc++.h>
using namespace std;
int main()
{
  ios::sync_with_stdio(false);cin.tie(NULL);
  string s;
  cin>>s;
  int len;
  len=s.size();
  int words[26]={0};
  for(int i=0;i<len;i++)
  {
      words[s[i]-'a']++;
  }
  int maxwords=0;
  for(int i=0;i<26;i++)
    maxwords=max(maxwords,words[i]);
  vector<int> outwords(len);
  for(int i=0;i<len;i++)
  {
      outwords[i]=words[s[i]-'a'];
  }
  for(int i=0;i<len;i++)
  {
      if(outwords[i]!=maxwords)
      cout<<s[i];
  }
  cout<<"\n";
  return 0;
}
```
## C - Insert and Erase A
```
実行時間制限: 2 sec / メモリ制限: 1024 MiB

配点 : 
300 点

問題文
英大文字からなる文字列 
S,T が与えられます。

あなたは以下の 
2 種類の操作を好きな順序で好きな回数（
0 回でも良い）行うことができます。

S の好きな位置（先頭および末尾を含む）に文字 A を 
1 つ挿入する。
S に含まれる文字 A を 
1 つ選んで削除する。なお、残った文字は元の順序を保ったまま連結される。
S を 
T に一致させることが可能かどうか判定し、可能な場合は必要な操作回数の合計の最小値を求めてください。

制約
S,T は英大文字からなる長さ 
1 以上 
3×10 
5
  以下の文字列
入力
入力は以下の形式で標準入力から与えられる。

S
T
出力
S を 
T に一致させることが可能ならば必要な操作回数の合計の最小値を、不可能ならば -1 を出力せよ。

入力例 1
Copy
ABC
BACA
出力例 1
Copy
3
以下のように、合計 
3 回の操作で 
S を 
T に一致させることが可能です。

S の 
2 文字目と 
3 文字目の間に A を 
1 つ挿入する。
S= ABAC となる。
S の 
1 文字目にある A を削除する。
S= BAC となる。
S の末尾に A を 
1 つ挿入する。
S= BACA となる。
合計 
2 回以下の操作で 
S を 
T に一致させることはできないため、答えは 
3 です。

入力例 2
Copy
ABC
ARC
出力例 2
Copy
-1
どのように操作を行っても、
S を 
T に一致させることはできません。

入力例 3
Copy
ABC
ABC
出力例 3
Copy
0
1 回も操作を行う必要がありません。

入力例 4
Copy
AAAWAZAAAABAAAU
AWAAZABAAAAAUA
出力例 4
Copy
9
```
C問題はまだしませんから、後で追加する。
