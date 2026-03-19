# GPLT 2026 Popular competition
今回のコンテストでは、全問のうち P-1, P-2, P-3, P-4, P-6 の5問を時間内に解くことができましたが、残りの4問（P-5, P-7, P-8, P-9）はAC（正解）に至りませんでした。  
振り返ってみると、最後の P-9（DSU） 以外は、数学的な精度やシミュレーションの細かい条件、あるいはデータ構造の活用に慣れていれば十分に解ける問題でした。今回のミスや分からなかった部分をしっかりと吸収し、次のコンテストではさらに多くのACを勝ち取れるよう、これからも努力を続けていきます！  
## P-1 失败乃成功之母
P-1は挨拶代わりの出力問題です。
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    cout<<"If people never did silly things, nothing intelligent would ever get done."<<"\n";
    return 0;
}
```
## P-2 长颈鹿的身高
この問題も簡単の出力する問題です。
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int x,n,y;
    cin>>x>>n>>y;
    cout<<x*n+y<<"\n";
    return 0;
}
```
## P-3 这瓜保熟吗
この問題はシンプルな if 文の条件分岐です。状況は三つあります。一つ一つで判断してとけます。
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int H,h1,h2;
    cin>>H>>h1>>h2;
    if(H<h1)
    cout<<H<<"\n"<<"Bu Kan"<<"\n";
    else if(h<h2)
    cout<<H<<"\n"<<"Zhe Gua Bao Shu Ma"<<"\n";
    else
    cout<<H<<"\n"<<"Chi Gua"<<"\n";
    return 0;
}
```
## P-4 单曲循环
この問題は、与えられた $n$ 個の乱数シーケンスのうち、偶数番目にある数値をすべて合計する問題です。  
好きな曲は常に偶数番目に配置されるというルールなので、単純に偶数番目のループ回数を加算するだけで解けます。  
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;

    long long sum=0;
    for (int i=1;i<=n;i++)
    {
        int temp;
        cin>>temp;
        if(i%2==0)
        sum+=temp;
    }
    cout<<sum<<"\n";
    return 0;
}
```
## P-5 骗分
この問題は、競技プログラミングで正解が分からない参加者が、適当な数値を送り続けて正解（ $A+B$ ）を当てるまでの時間をシミュレーションする問題です。  競技開始（00:00:01）から 2秒おき に数値を提出します。正解が出るか、または制限時間の 3時間（03:00:00）に達した時点で判定を出力します。  
ポイント: 各提出ごとの「経過時間」を正確に計算し、正解が制限時間内に現れたか、それとも時間切れになったかを判断します。  
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int A,B;
    cin>>A>>B;
    int target=A+B;

    int guess;
    int count=0;
    bool finished=false;

    int val=0;
    string msg="";
    int time=0;

    while(cin>>guess)
    {
        count++;
        int t_time=1+(count-1)*2;

        if(!finished&&t_time<=10800)
        {
            val=guess;
            time=t_time;
            
            if(guess==target)
            {
                msg="Accepted";
                finished = true;
            }
            else
            {
                msg="Wrong Answer";
            }
        }
    }

    int h=time/3600;
    int m=(time%3600)/60;
    int s=time%60;

    cout<<val<<" "<<msg <<" " 
        <<setfill('0')<<setw(2)<<h<< ":" 
        <<setfill('0')<<setw(2)<<m<< ":" 
        <<setfill('0')<<setw(2)<<s<<"\n";
    return 0;
}
```
## P-6 从前有座山
この問題は、与えられた数値の列から、重複して現れる数値を見つける問題です。  
ルール: 数値が順番に読み込まれ、2回目に登場した数値を「重複」とみなします。  
出力: 1番目に重複が確認された数値と、2番目に重複が確認された数値を出力します。  
特殊条件: 重複が1つもない場合は NONE を出力します。  
ポイント: 各数値が「すでに現れたかどうか」を記録するために、配列や std::set, std::map などのデータ構造を使用します。  
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    void solve()
    {
    unordered_map<int, int> counts;
    vector<int> nums;
    int x;
    while(cin>>x&&x!=-1)
    nums.push_back(x);

    vector<int> repeats;
    unordered_map<int, bool> visited;
    
    for(int val : nums)
    {
        if(visited[val])
        {
            bool flag=false;
            for(int r : repeats)
            if(r==val)
            flag=true;
            repeats.push_back(val);
            if (repeats.size()==1)
            break;
        }
        visited[val]=true;
    }

    if(repeats.empty())
    cout << "NONE" << endl;
    else
    {
        for(int i=0;i<repeats.size();i++)
        cout<<repeats[i]<<(i==repeats.size()- 1?"":" ");
        cout<<"\n";
    }
}

int main()
{
    int n;
    cin>>n;
    while(n--)
    solve();
    return 0;
}
```
## P-7 双花与三花
この問題は、与えられた正整数 $z$ が特定の数学的条件を満たすかどうかを判定する問題です。  
ダブルフラワー数 (Double Flower): $z = 2 \times k^2$ （ある整数の平方の2倍）  
トリプルフラワー数 (Triple Flower): $z = \frac{1}{3} \times k^3$ （ある整数の立方の $\frac{1}{3}$ 倍、つまり $3z = k^3$）  
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

bool check2(ll z,ll &k_out)
{
    if(z%2!=0)
    return false;
    ll m=z/2;
    ll k=(ll)round(sqrt((double)m));
    if (k*k==m)
    {
        k_out=k;
        return true;
    }
    return false;
}

bool check3(ll z)
{
    if(z%3!=0) return false;
    ll target=z/3;
    ll m=(ll)round(pow((double)target,1.0/3.0));
    return(m*m*m==target);
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    while(n--)
    {
        ll z;
        cin>>z;

        ll k;
        if(check2(z,k))
        {
            if(check3(z))
            cout<<z<<" is a triple flower"<<"\n";
            else
            cout<<z<<" is a double flower"<<"\n";
        }
        else
        cout<<z<<" is "<<z<<"\n";
    }
    return 0;
}
```
## P-8 从前有座山 - 大数据版
この問題は、単なる数値の重複ではなく、**3つ連続した数値のパターン（トリプレット）**が最初に重複して現れる箇所を探す問題です。  
ルール: 数値 $x$ が再び現れたとき、その直後の2つの数値（$y, z$）も前回と同じ順序で現れる必要があります。つまり、$(x, y, z)$ というパターンが2回目に登場した瞬間、その $x, y, z$ を出力します。  
データ: 数値は最大 $2^{30}$（int では足りるが long long 推奨）、1行に最大 $10^4$ 個の数値があります。  
出力: 最初に条件を満たした 3 つの数値をスペース区切りで出力します。存在しない場合は NONE です。  
```cpp
#include<bits/stdc++.h>
using namespace std;

void solve()
{
    vector<ll> v;
    ll temp;
    while(cin>>temp&&temp!=-1)
    v.push_back(temp);

    if(v.size()<3)
    {
        cout<<"NONE"<<endl;
        return;
    }
    map<vector<ll>,bool> seen;
    bool found=false;
    for(int i=0;i<=(int)v.size()-3;i++)
    {
        vector<ll> triplet={v[i],v[i+1],v[i+2]};

        if(seen.count(triplet))
        {
            cout<<triplet[0]<<" "<<triplet[1]<<" "<<triplet[2]<<"\n";
            found=true;
            break;
        }
        seen[triplet]=true;
    }
    if(!found)
    cout<<"NONE"<<endl;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    while(n--)
    solve();
    return 0;
}
```
## P-9 梆梆不梆梆
この問題は、複数の単語のグループ（文）が与えられ、同じグループに属する単語を「関連あり」と見なして、互いに全く関連のない単語の最大セットを見つける問題です。  
同じ文に含まれる全ての単語を union 操作で一つの集合にまとめます。  
全ての文を処理した後、いくつの独立した集合があるか数えます。  
各集合の中から最小の単語番号を選びます。  
選んだ単語を昇順に並べて出力します。  
```cpp
#include<bits/stdc++.h>
using namespace std;

struct DSU{
    vector<int> parent;
    DSU(int n)//初
    {
        parent.resize(n+1);
        iota(parent.begin(),parent.end(),0); 
    }
    int find(int i)//查
    {
        if(parent[i]==i)
        return i;
        return parent[i]=find(parent[i]);//更新
    }
    void unite(int i,int j)//并
    {
        int root_i=find(i);
        int root_j=find(j);
        if(root_i!=root_j)
        parent[root_i]=root_j;
    }
};

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n, m;
    cin>>n>>m;

    DSU dsu(m);
    vector<bool> appeared(m+1,false);

    for(int i=0;i<n;i++)
    {
        int k;
        cin>>k;
        int first_word;
        if(k>0)
        {
            cin>>first_word;
            appeared[first_word]=true;
            for(int j=1;j<k;j++)
            {
                int next_word;
                cin>>next_word;
                appeared[next_word]=true;
                dsu.unite(first_word,next_word);
            }
        }
    }


    vector<int> min_in_set(m+1,0);
    for (int i=1;i<=m;i++)
    {
        int root=dsu.find(i);
        if(min_in_set[root]==0||i<min_in_set[root])
        min_in_set[root]=i;

    }

    vector<int> result;
    for(int i=1;i<=m;i++)
    {
        if(min_in_set[i]!=0)
        result.push_back(min_in_set[i]);
    }
    sort(result.begin(), result.end());

    for(int i=0;i<result.size();i++)
    cout<<result[i]<<(i==result.size()-1?"":" ");

    cout<<endl;
    return 0;
}
```
今回の復盤で 「浮動小数点の精度（round）」「3元組のパターンマッチング（map<vector>）」「グループ分け（DSU）」 という3つの強力な武器を手に入れましたね！  
この調子で、次回のコンテストも「梆梆就两拳」でACを量産しちゃいましょう！応援しています！
