# AtCoder Daily Training ALL 2026/03/11 18:00start
今日はEASYではなくALLのトレーニングに参加しました。  
AからEまでの5問を解くことができましたが、F問題はTLE（実行時間制限超過）になってしまい、それ以降の問題は時間が足りませんでした。  
## A - Count Takahashi 
### https://atcoder.jp/contests/adt_all_20260311_2/tasks/abc359_a
この問題は簡単です。入力の時に「Takahashi」が出てきた回数をメモします。最後出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);;
    int n;
    cin>>n;
    int ans=0;
    while(n--)
    {
        string str;
        cin>>str;
        if(str=="Takahashi")ans++;
    }
    cout<<ans<<"\n";
    return 0;
}
```
## B - Full House 2
### https://atcoder.jp/contests/adt_all_20260311_2/tasks/abc386_a
この問題の本質は、**「4枚のカードに何種類の数字があるか」**を判定することです。  
思考のプロセス：  
set の活用: set は重複を許さないデータ構造なので、入力された4つの数字を insert するだけで、自動的に種類数が決まります。    
2種類の場合:  
(3枚, 1枚) のパターン（例：7, 7, 7, 1）→ 1枚足せば (3, 2) のフルハウス。  
(2枚, 2枚) のパターン（例：3, 3, 5, 5）→ どちらかを足せば (3, 2) のフルハウス。  
よって、この場合のみ Yes となります。  
それ以外の場合:  
1種類（4枚同じ）：何を足しても (4, 1) か (5, 0) になり、(3, 2) にはなれない。  
3種類以上：1枚足しても数字の種類が多すぎて (3, 2) は作れない。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    set<int>s;
    for(int i=0;i<4;i++)
    {
        int a;
        cin>>a;
        s.insert(a);
    }

    if(s.size()==2)
    cout<<"Yes\n";
    else
    cout<<"No\n";
    return 0;
}
```
## C - Substring
### https://atcoder.jp/contests/adt_all_20260311_2/tasks/abc347_b
この問題は「S は英小文字からなる長さ 1 以上 100 以下の文字列」ですから、全探索をしてもいいです。全部の状況を「set」に格納して答えが出てきます。  
逻辑过程：  
全探索の妥当性: 文字列 $S$ の長さ $N$ が最大 100 と非常に短いため、すべての部分文字列を抽出しても $O(N^2)$、さらに set への挿入を含めても $O(N^3 \log (\text{種類数}))$ 程度の計算量で済みます。これは実行時間制限（2秒）に対して十分に高速です。  
重複の排除: 部分文字列の種類数を数える必要があるため、重複を自動で取り除いてくれる std::set<string> を活用します。  
部分文字列の抽出: s.substr(pos, len) を使い、開始位置 i と長さ len を指定してすべてのパターンを網羅します。  
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);;
    string s;
    cin>>s;
    set<string>ans;
    for(int i=0;i<s.size();i++)
    {
        for(int j=i+1;j<=s.size();j++)
        {
            ans.insert(s.substr(i,j-i));
        }
    }
    cout<<ans.size()<<"\n";
    return 0;
}
```
## D - Broken Rounding
### https://atcoder.jp/contests/adt_all_20260311_2/tasks/abc273_b
この問題は　**「四捨五入（ししゃごにゅう）」を繰り返し行う問題** です。僕は使う方法は注目している位の数字が5以上の場合は切り上げ、それ以外は切り捨てるという操作を $10^{K-1}$ の位まで繰り返します。
```cpp
#include<bits/stdc++.h>
using namespace std;
long long getpow(long long a, long long b) {
    long long res = 1;
    for(int i=0;i<b;i++) res*=a;
    return res;
}
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);;
    long long x,k;
    cin>>x>>k;
    for(int i=0;i<k;i++)
    {
        long long temp=getpow(10,i+1);
        long long num=x%temp;
        if(num>=temp/2)num=temp;
        else num=0;
        x/=temp;
        x*=temp;
        x+=num;
    }
    cout<<x<<"\n";
    return 0;
}
```
## E - Standings
### https://atcoder.jp/contests/adt_all_20260311_2/tasks/abc308_c
解法：__int128 を用いた誤差のない分数比較  
この問題の罠は、浮動小数点数（double）の精度不足にあります。  
思考のプロセス：  
精度の限界: 成功率は $\frac{A_i}{A_i + B_i}$ で計算されますが、$A_i, B_i$ が最大 $10^9$ のとき、分母は $2 \times 10^9$ になります。二つの分数を比較する際、double（有効桁数 約15〜17桁）では微小な差を判定できず、誤った順序でソートされる可能性があります。  
整数への変換（たすき掛け）:分数 $\frac{a}{b} > \frac{c}{d}$ の比較は、両辺に $bd$ を掛けることで $ad > bc$ という整数の積の比較に置き換えられます。  
オーバーフロー対策:$(10^9) \times (2 \times 10^9) = 2 \times 10^{18}$ となり、long long の限界（約 $9 \times 10^{18}$）には収まりますが、計算過程での安全策として、より大きな範囲を扱える __int128 を使用するのが確実です。
```cpp
#include<bits/stdc++.h>
using namespace std;

struct Person{
    long long a, b;
    int id;
};

bool cmp(const Person& p1, const Person& p2)
{
    __int128 left=(__int128)p1.a*(p2.a+p2.b);
    __int128 right=(__int128)p2.a*(p1.a+p1.b);

    if(left!=right)
    {
        return left>right;
    }
    return p1.id<p2.id;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);;

    int n;
    cin>>n;
    vector<Person> persons(n);
    for(int i=0;i<n;i++)
    {
        cin>>persons[i].a>>persons[i].b;
        persons[i].id = i + 1;
    }
    sort(persons.begin(),persons.end(),cmp);
    for(int i=0;i<n;i++)
    {
        cout<<persons[i].id<<(i==n-1?"":" ");
    }
    cout<<"\n";
    return 0;
}
```
## F - Sort
### https://atcoder.jp/contests/adt_all_20260311_2/tasks/abc350_c
解法：逆引き配列（pos配列）による $O(N)$ への高速化  
思考のプロセス：TLEの分析: 単純な二重ループでは、各要素に対して「正しい数字がどこにあるか」を毎回探すため $O(N^2)$ かかります。$N=2 \times 10^5$ では間に合いません。  
位置の記録: 数値 x が今どこにあるかを記録する配列 pos[x] を用意します。  
効率的な交換:  
位置 i にあるべき数字は i です。  
pos[i] を参照すれば、数字 i の場所が $O(1)$ で分かります。  
交换した後は、num 配列だけでなく pos 配列も更新することを忘れないようにします。  
計算量: 全体を1回走査するだけなので $O(N)$ となり、十分に間に合います。実装のポイント：pos[num[i]] = i; で初期位置を記録。swap の際、pos[数字] = 新しい位置; と更新する処理が心臓部です。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<int> num(n+1);
    vector<int> pos(n+1);
    for(int i=1;i<n+1;i++)
    {
        cin>>num[i];
        pos[num[i]]=i;
    }
    vector<pair<int,int>> ans;
    for(int i=1;i<n+1;i++)
    {
        if(num[i]!=i)
        {
            ans.push_back({i,pos[i]});
            int temp=pos[i];
            pos[num[i]] = temp;
            swap(num[i],num[temp]);
            pos[i]=i;
        }
    }
    cout<<ans.size()<<"\n";
    for(int i=0;i<ans.size();i++)
    {
        cout<<ans[i].first<<" "<<ans[i].second<<"\n";
    }
    return 0;
}
```
