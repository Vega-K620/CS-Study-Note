# CCPC nanchang2025
今日、チームで CCPC Nanchang 2025 のバーチャル参加（VP）を行いました。  
結果は A, G, J の3問正解でしたが、非常に悔やまれるのが K問題 です。  
構築のロジックはほぼ完成しており、あと一歩で AC というところでしたが、実装上の些細なミス（スタックの空チェック漏れ）で RE（Runtime Error）を出してしまいました。  
もしこれが通っていれば、順位が数十位上がっていたはずなので、反省を込めてここにメモを残します。  
## K. 不許偷吃 (K. つまみ食い禁止)
### https://vjudge.net/contest/811443#problem/K
問題概要  
$n$ 個の料理があり、それぞれ $a_i$ 個の点心が入っている。
料理が1つ運ばれるたびに、テーブル上の点心の合計が4以上であれば、4人が1個ずつ食べる（合計4個減る）。これをテーブルの点心が4未満になるまで繰り返す。  
「つまみ食い」の条件： 料理を食べた後、テーブルに点心が ちょうど1個 残っていると、小猪（XiaoZhu）がそれを食べてしまい、クレームに繋がる。  
目標：つまみ食いが発生しないような料理の提供順序を1つ出力せよ。不可能な場合は -1 を出力する。  
制約：点心の総数は必ず4の倍数である。  
構築の思考プロセス  
点心の数を $4$ で割った余り（$0, 1, 2, 3$）に着目します。  
基本戦略：余りが $0$ の料理は、いつ出しても現在の余りの状態に影響を与えないため、最初にすべて出し切る。  
ペアリング：  
余り $1$ と 余り $3$ をペア（$1+3=4$）にすれば、余りは $0$ に戻り、つまみ食い（余り $1$）を回避できる。  
残った余りの処理：  
余り $1$ と $3$ のペアを作った後、どちらかが残る場合：  
余り $2$ があるなら、「$2 + 1 + 1$」 または 「$3 + 3 + 2$」 という組み合わせで余りを $0$ にできる。
不可能な条件：  
余り $1$ がどうしても残ってしまう場合。  
余り $3$ が $3$ つ以上余ってしまう場合（$3$ が $1$ つだけ余るのは OK、なぜなら $3 < 4$ なのでつまみ食い条件の $1$ にはならない）。
実装の反省  
実装では std::stack を使って余りごとのインデックスを管理しました。  
VP中に提出したコードでは、残った余り $1$ や $3$ を処理する際の if(!mod1.empty()) などの空チェックが甘く、要素がないのに top() や pop() を呼び出したため RE になりました。
## コード比較
VP中に出したコード（RE発生）
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

stack<int> mod0,mod1,mod2,mod3;
int cnt0=0,cnt1=0,cnt2=0,cnt3=0;
int sum=0;
void out()
{
    while(!mod0.empty())
    {
        cout<<mod0.top()<<" ";
        mod0.pop();
    }
    while(!mod1.empty()&&!mod3.empty())
    {
        cout<<mod3.top()<<" "<<mod1.top()<<" ";
        mod1.pop();
        mod3.pop();
    }
    if(mod1.empty())
    {
        while(!mod2.empty()&&!mod3.empty())
        {
            cout<<mod3.top()<<" ";
            mod3.pop();
            if(!mod3.empty())
            {
                cout<<mod3.top()<<" ";
                mod3.pop();    
            }
            cout<<mod2.top()<<" ";
            mod2.pop();
        }
        while(!mod3.empty())
        {
            cout<<mod3.top()<<" ";
            mod3.pop();
        }
        while(!mod2.empty())
        {
            cout<<mod2.top()<<" ";
            mod2.pop();
        }
    }
    else
    {
        while(!mod2.empty()&&!mod1.empty())
        {
            cout<<mod2.top()<<" ";
            mod2.pop();
            cout<<mod1.top()<<" ";
            mod1.pop();
            cout<<mod1.top()<<" ";
            mod1.pop();
        }
        while(!mod2.empty())
        {
            cout<<mod3.top()<<" ";
            mod3.pop();
        }
    }
}

void solve()
{
    int n;
    cin>>n;
    sum=0;
    cnt0=0;
    cnt1=0;
    cnt2=0;
    cnt3=0;
    while(!mod0.empty())
    {
        mod0.pop();
    }
    while(!mod1.empty())
    {
        mod1.pop();
    }
    while(!mod2.empty())
    {
        mod2.pop();
    }
    while(!mod3.empty())
    {
        mod3.pop();
    }
    for(int i=0;i<n;i++)
    {
        int num;
        cin>>num;
        sum+=num;
        if(num%4==0)
        {
            cnt0++;
            mod0.push(i+1);
        }
        else if(num%4==1)
        {
            cnt1++;
            mod1.push(i+1);
        }
        else if(num%4==2)
        {
            cnt2++;
            mod2.push(i+1);
        }
        else if(num%4==3)
        {
            cnt3++;
            mod3.push(i+1);
        }
    }
    //cout<<cnt0<<" "<<cnt1<<" "<<cnt2<<" "<<cnt3<<" "<<"\n";
    if(sum%4!=0)
    {
        cout<<-1<<"\n";
        return;
    }
    if(cnt1>cnt3)
    {
        if((cnt1-cnt3)-cnt2*2>0)
        {
            cout<<-1<<"\n";
        }
        else
        {
            out();
            cout<<"\n";
        }
    }
    else if(cnt1<cnt3)
    {
        if((cnt3-cnt1)-cnt2*2>2)
        {
            cout<<-1<<"\n";
        }
        else
        {
            out();
            cout<<"\n";
        }
    }
    else
    {
        out();
        cout<<"\n";
    }
//    while(!mod0.empty())
//    {
//        cout<<mod0.top()<<" ";
//        mod0.pop();
//    }
//    while(!mod1.empty())
//    {
//        cout<<mod1.top()<<" ";
//        mod1.pop();
//    }
//    while(!mod1.empty())
//    {
//        cout<<mod1.top()<<" ";
//        mod1.pop();
//    }
//    while(!mod1.empty())
//    {
//        cout<<mod1.top()<<" ";
//        mod1.pop();
//    }
//    cout<<"\n";
//    cout<<mod0.size()<<" "<<mod1.size()<<" "<<mod2.size()<<" "<<mod3.size()<<"\n";
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        solve();
    }
    return 0;
}
```
正解コード
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

stack<int> mod0,mod1,mod2,mod3;
int cnt0=0,cnt1=0,cnt2=0,cnt3=0;
int sum=0;
void out()
{
    while(!mod0.empty())
    {
        cout<<mod0.top()<<" ";
        mod0.pop();
    }
    while(!mod1.empty()&&!mod3.empty())
    {
        cout<<mod3.top()<<" "<<mod1.top()<<" ";
        mod1.pop();
        mod3.pop();
    }
    if(mod1.empty())
    {
        while(!mod2.empty()&&!mod3.empty())
        {
            cout<<mod3.top()<<" ";
            mod3.pop();
            if(!mod3.empty())
            {
                cout<<mod3.top()<<" ";
                mod3.pop();    
            }
            cout<<mod2.top()<<" ";
            mod2.pop();
        }
        while(!mod3.empty())
        {
            cout<<mod3.top()<<" ";
            mod3.pop();
        }
        while(!mod2.empty())
        {
            cout<<mod2.top()<<" ";
            mod2.pop();
        }
    }
    else
    {
        while(!mod2.empty()&&!mod1.empty())
        {
            cout<<mod2.top()<<" ";
            mod2.pop();
            cout<<mod1.top()<<" ";
            mod1.pop();
            if(!mod1.empty())
            {
                cout<<mod1.top()<<" ";
                mod1.pop();    
            }
        }
        while(!mod2.empty())
        {
            cout<<mod2.top()<<" ";
            mod2.pop();
        }
    }
}

void solve()
{
    int n;
    cin>>n;
    sum=0;
    cnt0=0;
    cnt1=0;
    cnt2=0;
    cnt3=0;
    while(!mod0.empty())
    {
        mod0.pop();
    }
    while(!mod1.empty())
    {
        mod1.pop();
    }
    while(!mod2.empty())
    {
        mod2.pop();
    }
    while(!mod3.empty())
    {
        mod3.pop();
    }
    for(int i=0;i<n;i++)
    {
        int num;
        cin>>num;
        sum+=num;
        if(num%4==0)
        {
            cnt0++;
            mod0.push(i+1);
        }
        else if(num%4==1)
        {
            cnt1++;
            mod1.push(i+1);
        }
        else if(num%4==2)
        {
            cnt2++;
            mod2.push(i+1);
        }
        else if(num%4==3)
        {
            cnt3++;
            mod3.push(i+1);
        }
    }
    //cout<<cnt0<<" "<<cnt1<<" "<<cnt2<<" "<<cnt3<<" "<<"\n";
    if(sum%4!=0)
    {
        cout<<-1<<"\n";
        return;
    }
    if(cnt1>cnt3)
    {
        if((cnt1-cnt3)-cnt2*2>0)
        {
            cout<<-1<<"\n";
        }
        else
        {
            out();
            cout<<"\n";
        }
    }
    else if(cnt1<cnt3)
    {
        if((cnt3-cnt1)-cnt2*2>2)
        {
            cout<<-1<<"\n";
        }
        else
        {
            out();
            cout<<"\n";
        }
    }
    else
    {
        out();
        cout<<"\n";
    }
//    while(!mod0.empty())
//    {
//        cout<<mod0.top()<<" ";
//        mod0.pop();
//    }
//    while(!mod1.empty())
//    {
//        cout<<mod1.top()<<" ";
//        mod1.pop();
//    }
//    while(!mod1.empty())
//    {
//        cout<<mod1.top()<<" ";
//        mod1.pop();
//    }
//    while(!mod1.empty())
//    {
//        cout<<mod1.top()<<" ";
//        mod1.pop();
//    }
//    cout<<"\n";
//    cout<<mod0.size()<<" "<<mod1.size()<<" "<<mod2.size()<<" "<<mod3.size()<<"\n";
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        solve();
    }
    return 0;
}
```
## 終わりに  
今回の失敗は、実装の細部に対する注意不足が原因でした。構築問題はロジックがシンプルに見えても、コーナーケースや境界条件でバグを生みやすいです。  
次回のコンテストでは、実装前に一度冷静に「このスタックは本当に空にならないか？」を確認する癖をつけたいと思います。  
