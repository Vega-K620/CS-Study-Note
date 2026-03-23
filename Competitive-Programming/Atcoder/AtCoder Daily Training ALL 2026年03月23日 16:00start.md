# AtCoder Daily Training ALL 2026/03/23 16:00start
今日のADTはA-E問題を解きました。Eは正解が見つけました、残念です。
## A - Shampoo
### https://atcoder.jp/contests/adt_all_20260323_1/tasks/abc243_a
解法1(僕が使う方法)：シミュレーションによる解法（愚直な減算）まずは、問題文の通りに順番に引き算を行っていく方法です。$V$ が 0 未満になるまで while ループを回します。  
解法2：数学的アプローチ（剰余算の活用）もし $V$ が非常に大きい場合（例：$10^{18}$ など）、シミュレーションでは TLE（実行時間制限超過）になってしまいます。そこで、1サイクルの合計消費量で割った「余り」に注目します。1サイクルの合計は $A + B + C$ です。V %= (A + B + C) とすることで、何周したかを一気にスキップし、「最後の1サイクル」の状態からスタートできます。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int v,a,b,c;
    cin>>v>>a>>b>>c;
    while(1)
    {
	    if(v<a)
	    {
	        cout<<"F"<<"\n";
	        return 0;
	    }
	    else
	    {
	        v-=a;
	        if(v<b)
	        {
	            cout<<"M"<<"\n";
	            return 0;
	        }
	        else
	        {
	            v-=b;
	            if(v<c)
	            {
	                cout<<"T"<<"\n";
	                return 0;
	            }
	            v-=c;
	    	}
		}
	}
    return 0;
}
```
```cpp
#include <iostream>
using namespace std;

int main()
{
    int v,a,b,c;
    cin>>v>>a>>b>>c;
    v%=(a+b+c);
    if(v<a)
    {
        cout<<"F"<<endl;
    }
    else if(v<a+b)
    {
        cout<<"M"<<endl;
    }
    else
    {
        cout<<"T"<<endl;
    }
    return 0;
}
```
## B - First Player
### https://atcoder.jp/contests/adt_all_20260323_1/tasks/abc304_a
最小値のインデックス no を見つけた後、no から末尾までと、先頭から no-1 までを順番に表示します。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<pair<string,long long>> per(n);
    int minnum=INT_MAX;
    int no=0;
    for(int i=0;i<n;i++)
    {
        cin>>per[i].first>>per[i].second;
        if(minnum>per[i].second)
        {
        	no=i;
        	minnum=per[i].second;
		}
    }
    for(int i=no;i<per.size();i++)
    cout<<per[i].first<<"\n";
    for(int i=0;i<no;i++)
    cout<<per[i].first<<"\n";
    return 0;
}
```
## C - Election
### https://atcoder.jp/contests/adt_all_20260323_1/tasks/abc231_b
最も効率的で直感的な方法は、**連想配列（ハッシュマップなど）**を使って、各候補者の名前を「キー（Key）」、得票数を「値（Value）」として管理することです。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    map<string,int> per;
    for(int i=0;i<n;i++)
    {
        string name;
        cin>>name;
        per[name]++;
    }

    string winner;
    int maxVotes=0;
    for(auto &i:per)
    {
        if(i.second>maxVotes)
        {
            maxVotes=i.second;
            winner=i.first;
        }
    }
    
    cout<<winner<<"\n";
    return 0;
}
```
## D - Better Students Are Needed!
### https://atcoder.jp/contests/adt_all_20260323_1/tasks/abc260_b
構造体 (struct) で「ID・数学・英語・合格フラグ」をまとめて管理。  
ラムダ式 を使った std::sort で、各段階の優先順位（点数降順、ID昇順）を定義。  
各段階のループ内で、if (!passed) をチェックして合格者を確定させる。  
```cpp
#include<bits/stdc++.h>
using namespace std;

struct Student {
    int id;
    int math;
    int english;
    bool passed;
};

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    
    int N,X,Y,Z;
    cin>>N>>X>>Y>>Z;
    
    vector<Student> students(N);
    for(int i=0;i<N;i++)
    {
        students[i].id =i+1;
        cin >> students[i].math;
        students[i].passed=false;
    }
    for(int i=0;i<N;i++)
    {
        cin>>students[i].english;
    }
    
    sort(students.begin(),students.end(),[](const Student& a, const Student& b)
    {
        if(a.math!=b.math) return a.math>b.math;
        return a.id<b.id;
    });
    
    int passedCount=0;
    for(int i=0;i<N&&passedCount<X;i++)
    {
        if(!students[i].passed)
        {
            students[i].passed=true;
            passedCount++;
        }
    }
    
    sort(students.begin(), students.end(), [](const Student& a, const Student& b) {
        if(a.english!=b.english) return a.english>b.english;
        return a.id<b.id;
    });
    
    passedCount=0;
    for(int i=0;i<N&&passedCount<Y;i++)
    {
        if(!students[i].passed)
        {
            students[i].passed=true;
            passedCount++;
        }
    }
    
    sort(students.begin(), students.end(), [](const Student& a, const Student& b) {
        int sumA=a.math+a.english;
        int sumB=b.math+b.english;
        if(sumA!=sumB) return sumA>sumB;
        return a.id<b.id;
    });
    
    passedCount=0;
    for(int i=0;i<N&&passedCount<Z;i++)
    {
        if(!students[i].passed)
        {
            students[i].passed=true;
            passedCount++;
        }
    }
    vector<int> result;
    for(const auto& s:students)
    {
        if(s.passed)
        {
            result.push_back(s.id);
        }
    }
    
    sort(result.begin(),result.end());
    for(int id:result)
    {
        cout<<id<<"\n";
    }
    
    return 0;
}
```
## E - Reversible
### https://atcoder.jp/contests/adt_all_20260323_1/tasks/abc310_c
「反転しても同じ」というルールがある場合、そのまま比較すると $S_i$ と $\text{reverse}(S_i)$ を別々にカウントしてしまいます。  
そこで、各文字列を以下のルールで**正規化（標準化）**します。  
「元の文字列」と「反転させた文字列」のうち、辞書順で小さい方をその棒の代表とする。  
これにより、abc も cba も、どちらも abc という一つの形に統一されます。  
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    
    int N;
    cin>>N;
    unordered_set<string> uniqueSticks;
    for(int i=0;i<N;i++)
    {
        string s;
        cin>>s;
        string rev=s;
        reverse(rev.begin(),rev.end());
        string normalized=min(s,rev);
        uniqueSticks.insert(normalized);
    }
    cout<<uniqueSticks.size()<<"\n";
    return 0;
}
```
## F - Socks 2
### https://atcoder.jp/contests/adt_all_20260323_1/tasks/abc334_c
コンテスト本番では $K$ が奇数の場合の処理に迷い、時間内に解ききることができませんでした。悔しいので、改めて整理して解き直しました。  
この問題のポイントは、**「ペアが揃っている靴下は無視して良い」という点です。結局、「1枚しか残っていない $K$ 枚の靴下を、いかに最小コストでペアリングするか」**という問題に集約されます。  
なぜ隣り合うペアなのか？  
数直線上の点を結ぶ際、交差するよりも隣同士を結ぶ方が合計距離 $|i-j|$ は短くなります。  
$K$ が奇数のときの戦略：1つを「除外」する$K$ 枚から 1 枚を除外して、残りの $K-1$ 枚をペアにします。ここで重要なのは、**「どのインデックス $i$ を除外するか」**です。  
左側に残る個数：$i-1$  
右側に残る個数：$K-i$  
残された靴下をすべて隣同士でペアにするには、左右どちらも「偶数個」残らなければなりません。したがって、$i-1$ が偶数、つまり $i$ は奇数番目 $(1, 3, 5, \dots)$ である必要があります。  
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() 
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n, k;
    cin>>n>>k;
    vector<int> a(k);
    for(int i=0;i<k;i++) 
    {
        cin>>a[i];
    }
    if(k%2==0) 
    {
        long long ans=0;
        for(int i=0;i<k;i+=2) 
        {
            ans+=(a[i+1]-a[i]);
        }
        cout<<ans<<"\n";
    } 
    else 
    {
        vector<long long> prefix(k+2,0),suffix(k+2,0);

        for(int i=2;i<=k;i+=2) 
        {
            prefix[i]=prefix[i-2]+(a[i-1]-a[i-2]);
        }

        for(int i=k-1;i>=1;i-=2) 
        {
            suffix[i]=suffix[i+2]+(a[i]-a[i-1]);
        }

        long long ans=2e18;
        for(int i=1;i<=k;i+=2) 
        {
            long long cost=prefix[i-1]+suffix[i+1];
            ans=min(ans,cost);
        }
        cout<<ans<<"\n";
    }

    return 0;
}
```
