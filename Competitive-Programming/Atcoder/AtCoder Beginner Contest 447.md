# AtCoder Beginner Contest 447
<details>
<summary>(今日の感想)</summary>
  今日のコンテストは少し残念と思っています、別の用事があるのでA問題だけいりました。
  で初めてのコンテストで３分間掛かってA問題を解決したはいい始まると思います。
</details>

## A - Seats 2
```
実行時間制限: 2 sec / メモリ制限: 1024 MiB

配点 : 100 点

問題文
N 個の座席が横一列に並んでいます。各座席には最大で 1 人まで座ることができます。
以下の条件を満たすように M 人の人を座席に座らせることができるかどうか判定してください。
隣り合う 2 つの席の両方に人が座ってはいけない。

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
  6 3
出力例 1
  Yes
例えば、左から 
1,3,6 番目に座らせれば可能です。したがって Yes を出力してください。

入力例 2
  4 3
出力例 2
  No
条件を満たすような座らせ方はありません。したがって No を出力してください。

入力例 3
  5 3
出力例 3
  Yes

入力例 4
  44 7
出力例 4
  Yes
```
A問題は簡単だと思います。
2つの席の両方に人がすわってはいけないから、席かM個あるなら、座らせるの最大数は(M+1)/2
```cpp
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

配点 : 200 点

問題文
英小文字からなる文字列 S が与えられます。 
S の中で出現回数が最も多い文字をすべて取り除き、
残った文字を元の順序を保ったまま連結して出力してください。
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
  mississippi
出力例 1
  mpp
mississippi に最も多く出現する文字は s と i でともに 
4 回出現します。s と i をすべて取り除いた文字列は mpp となります。

入力例 2
  atcoder
出力例 2
  
入力例 3
  beginner
出力例 3
  bgir
```
この問題はまず文字の出現した回数を記録する。そして最も多く出現する文字は出力する時出力しません、他の文字は普通で出力します。
```cpp
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

配点 : 300 点

問題文
英大文字からなる文字列 S,T が与えられます。
あなたは以下の 2 種類の操作を好きな順序で好きな回数（0 回でも良い）行うことができます。
  S の好きな位置（先頭および末尾を含む）に文字 A を 1 つ挿入する。
  S に含まれる文字 A を 1 つ選んで削除する。なお、残った文字は元の順序を保ったまま連結される。
S を T に一致させることが可能かどうか判定し、可能な場合は必要な操作回数の合計の最小値を求めてください。

制約
S,T は英大文字からなる長さ 
1 以上 3×10^5 以下の文字列
入力
入力は以下の形式で標準入力から与えられる。
S
T
出力
S を T に一致させることが可能ならば必要な操作回数の合計の最小値を、不可能ならば -1 を出力せよ。

入力例 1
  ABC
  BACA
出力例 1
  3
以下のように、合計 3 回の操作で S を T に一致させることが可能です。

S の 2 文字目と 3 文字目の間に A を 1 つ挿入する。
S= ABAC となる。
S の 1 文字目にある A を削除する。
S= BAC となる。
S の末尾に A を 1 つ挿入する。
S= BACA となる。
合計 
2 回以下の操作で S を T に一致させることはできないため、答えは 3 です。

入力例 2
  ABC
  ARC
出力例 2
  -1
どのように操作を行っても、S を T に一致させることはできません。

入力例 3
  ABC
  ABC
出力例 3
  0
1 回も操作を行う必要がありません。

入力例 4
  AAAWAZAAAABAAAU
  AWAAZABAAAAAUA
出力例 4
  9
```
C問題はまずはSとTは同じ時、この状況は直接「0」を出力する。
他の状況は文字を一つ一つ対比する。
そして
「S の好きな位置（先頭および末尾を含む）に文字 A を 1 つ挿入する。
  S に含まれる文字 A を 1 つ選んで削除する。なお、残った文字は元の順序を保ったまま連結される。」
  上記の内容をシミュレートする。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    ios::sync_with_stdio(false);cin.tie(NULL);
    string str_S,str_T;
    cin>>str_S>>str_T;
    if(str_S==str_T)
    cout<<0<<"\n";
    else
    {
        int ans=0;
        int len_S=str_S.size();
        int len_T=str_T.size();
        bool flag=true;
        for(int l=0,r=0;r<=len_T&&l<=len_S;)//「for」の内容はこの問題の最重要のこと
        {
            if(str_S[l]==str_T[r])
            {
                l++;
                r++;
                continue;
            }
            else
            {
                if(str_T[r]=='A')
                {
                    r++;
                    ans++;
                    continue;
                }
                else
                {
                    if(str_S[l]!='A')
                    {
                        flag=false;
                        break;
                    }
                    else
                    {
                        l++;
                        ans++;
                        continue;
                    }
                }
            }
        }
        if(flag)
        cout<<ans<<"\n";
        else
        cout<<-1<<"\n";
    }
    return 0;
}
```
## D - Take ABC 2
```
実行時間制限: 2 sec / メモリ制限: 1024 MiB

配点 : 400 点

問題文
A , B , C の 3 種類の文字のみからなる文字列 S が与えられます。

操作を以下のように定義します。

1≤i<j<k≤∣S∣ かつ Si = A , Sj = B , Sk = C を満たす (i,j,k) の組を選び、
S の i,j,k 文字目を取り除く。残った文字を元の順序を保ったまま左に詰める。

文字列 S に対して最大で何回操作を行うことができるかを求めてください。

制約
1≤∣S∣≤10^6
S は A , B , C のみからなる文字列である
入力
入力は以下の形式で標準入力から与えられる。
S
出力
答えを出力せよ。

入力例 1
ABACBCC
出力例 1
2
以下のように操作をすることで 
2 回操作できます。
ABACBCC に対して 
(i,j,k)=(1,2,7) として操作する。残った文字列は ACBC となる。
ACBC に対して 
(i,j,k)=(1,3,4) として操作する。残った文字列は C となる。
3 回以上操作することはできないため、答えは 
2 となります。よって 
2 と出力してください。

入力例 2
CBACBB
出力例 2
0

入力例 3
BBBAAABCBCBAACBBCAAC
出力例 3
5
```
この問題は「前から貪欲」の問題です。まずはABCの添え字を保存する。もしABCの添え字は単調増加ならansは1増やして、最後に答えがみつけます。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    ios::sync_with_stdio(false);cin.tie(NULL);
    string s;
    cin>>s;
    int len;
    len=s.size();
    vector<int> num_a;
    vector<int> num_b;
    vector<int> num_c;
    for(int i=0;i<len;i++)
    {
        if(s[i]=='A')
        num_a.push_back(i);
        if(s[i]=='B')
        num_b.push_back(i);
        if(s[i]=='C')
        num_c.push_back(i);
    }
    int ans=0;
    for(int a=0,b=0,c=0;a<num_a.size()&&b<num_b.size()&&c<num_c.size();)
    {
        if(num_a[a]<num_b[b]&&num_b[b]<num_c[c])
        {
            ans++;
            a++;
            b++;
            c++;
            continue;
        }
        else
        {
            if(num_b[b]<num_a[a])
            {
                b++;
                continue;
            }
            if(num_c[c]<num_b[b])
            {
                c++;
                continue;
            }
        }
    }
    cout<<ans<<"\n";
    return 0;
}
```
EとFは僕にとってまだ難しいですから、努力を続けなければなりません。
