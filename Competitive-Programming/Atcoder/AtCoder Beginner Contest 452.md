# AtCoder Beginner Contest 452
今日のコンテストはA-Dを解きました。
## A - Gothec
### https://atcoder.jp/contests/abc452/tasks/abc452_a
これは「if」を使って「1月7日 3月3日 5月5日 7月7日 9月9日」は「Yes」を出力して、ほかのは「No」を出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int m,d;
    cin>>m>>d;
    if(m==1&&d==7||m==3&&d==3||m==5&&d==5||m==7&&d==7||m==9&&d==9)
    {
        cout<<"Yes"<<"\n";
    }
    else
    {
        cout<<"No"<<"\n";
    }
    return 0;
}
```
## B - Draw Frame
### https://atcoder.jp/contests/abc452/tasks/abc452_b
最初と最後の行は「#」で他の行内は最初「#」で、最後「#」で、別のは「.」で。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int h,w;
    cin>>h>>w;
    for(int i=0;i<w;i++)
    {
        cout<<'#';
    }
    cout<<"\n";
    for(int j=0;j<h-2;j++)
    {
    for(int i=0;i<w;i++)
        {
            cout<<(i==0|i==w-1?'#':'.');
        }
        cout<<"\n";
    }
    for(int i=0;i<w;i++)
    {
        cout<<'#';
    }
    cout<<"\n";
    return 0;
}
```
## C - Fishbones
### https://atcoder.jp/contests/abc452/tasks/abc452_c
exists[長さ][位置][文字]という3次元配列を用意する。与えられた $M$ 個の文字列すべてを走査し、各文字列について「どの位置にどの文字があるか」を true にする。各 $S_j$ を「脊椎」と仮定して判定を行う。
```cpp
#include<bits/stdc++.h>
using namespace std;
bool exists[11][11][26];

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin >> n;
    vector<pair<int, int>> ribs(n);
    for(int i=0;i<n;i++)
    {
        cin>>ribs[i].first>>ribs[i].second;
    }

    int m;
    cin >> m;
    vector<string> s(m);
    for(int i=0;i<m;i++)
    {
        cin>>s[i];
        int len=s[i].size();
        for(int pos=1;pos<=len;pos++)
        {
            exists[len][pos][s[i][pos-1]-'a']=true;
        }
    }
    for(int i=0;i<m;i++)
    {
        if(s[i].size()!=n)
        {
            cout << "No\n";
            continue;
        }
        bool possible = true;
        for(int j=0;j<n;j++) {
            int target_len=ribs[j].first;
            int target_pos=ribs[j].second;
            char target_char=s[i][j];
            if (!exists[target_len][target_pos][target_char-'a'])
            {
                possible=false;
                break;
            }
        }
        if(possible)cout<<"Yes\n";
        else cout<<"No\n";
    }
    return 0;
}
```
## D - No-Subsequence Substring
### https://atcoder.jp/contests/abc452/tasks/abc452_d
子系列オートマトン、ある位置以降で、特定の文字が次に現れるインデックスを $O(1)$ で取得する手法。nxt[i][char]：$i$ 文字目以降で最初に char が出現する位置を前計算しておく。　　
部分文字列の左端 $L$ を $0$ から $N-1$ まで固定する。各 $L$ について、貪欲法を用いて $T$ を部分列として完成させるために必要な最小の右端 $R$ を求める。この $R$ は子系列オートマトンを使って $O(|T|)$ で計算可能。もし $T$ が完成できるなら、右端が $R$ 以降のすべての部分文字列は条件を満たさない。
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string S,T;
    cin>>S>>T;
    int n=S.size();
    int m=T.size();
    vector<vector<int>> nxt(n+1,vector<int>(26,-1));
    for(int i=n-1;i>=0;--i)
    {
        for(int c=0;c<26;++c)
        {
            nxt[i][c]=nxt[i+1][c];
        }
        nxt[i][S[i]-'a']=i;
    }

    long long total_subsegments=(long long)n*(n+1)/2;
    long long contains_T=0;
    for(int L=0;L<n;++L)
    {
        int curr=L;
        bool possible=true;
        for(int k=0;k<m;++k)
        {
            if(curr>=n||nxt[curr][T[k]-'a']==-1)
            {
                possible=false;
                break;
            }
            curr=nxt[curr][T[k]-'a']+1;
        }
        if(possible)
        {
            contains_T+=(n-(curr-1));
        }
    }
    cout<<total_subsegments-contains_T<<"\n";
    return 0;
}
```
