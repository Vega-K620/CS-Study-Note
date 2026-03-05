# AtCoder Beginner Contest 447
<details>
<summary>(今日の感想)</summary>
  今日のコンテストは少し残念と思っています、別の用事があるのでA問題だけいりました。
  で初めてのコンテストで３分間掛かってA問題を解決したはいい始まると思います。
</details>

## A - Seats 2
### https://atcoder.jp/contests/abc447/tasks/abc447_a
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
### https://atcoder.jp/contests/abc447/tasks/abc447_b
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
### https://atcoder.jp/contests/abc447/tasks/abc447_c
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
### https://atcoder.jp/contests/abc447/tasks/abc447_d
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
##解き方を追加
以前の解法（インデックス保持）'A', 'B', 'C' それぞれの出現位置（index）を vector に記録し、3つのポインタで走査する。
欠点： メモリ使用量が $O(N)$ となり、実装も複雑になる。
効率的な解法（カウンターによる貪欲法）
「今、何がいくつ準備できているか」という**状態（state）**だけを管理する。
変数：
a: 未ペアの 'A' の数
ab: 'A' とペアになったが、まだ 'C' を待っている 'B'（つまり "AB"）の数
abc: 完成した "ABC" の総数
アルゴリズム：
文字列を1文字ずつ走査する。
'A' なら： a を増やす。
'B' なら： 手元に a があれば、それを一つ減らして ab を増やす。
'C' なら： 手元に ab があれば、それを一つ減らして abc を増やす。
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    string S;
    cin >> S;
    
    int a = 0, ab = 0, abc = 0;
    for (char c : S)
    {
        if (c == 'A')
        {
            a++;
        }
        else if (c == 'B')
        {
            if (a > 0) { a--; ab++; }
        }
        else if (c == 'C')
        {
            if (ab > 0) { ab--; abc++; }
        }
    }
    cout << abc << endl;
    return 0;
}
```
EとFは僕にとってまだ難しいですから、努力を続けなければなりません。
