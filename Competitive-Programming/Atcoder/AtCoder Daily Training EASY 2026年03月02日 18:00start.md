# AtCoder Daily Training EASY 2026/03/02 18:00start
<details>
  <summary>(今回の感想)</summary>
  今日Daily Training EASYをしました。成功的にAKをもらいました。これが私の問題解決アプローチです。
</details>

## A - Attack
### https://atcoder.jp/contests/adt_easy_20260302_2/tasks/abc302_a
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
### https://atcoder.jp/contests/adt_easy_20260302_2/tasks/abc401_a
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
### https://atcoder.jp/contests/adt_easy_20260302_2/tasks/abc413_b
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
### https://atcoder.jp/contests/adt_easy_20260302_2/tasks/abc310_b
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
## E - Manga
### https://atcoder.jp/contests/adt_easy_20260302_2/tasks/abc271_c
足りない巻を、余った本や後ろの大きな巻を売ることで補い、どこまで連続して読めるかを計算します。  
```cpp
#include <bits/stdc++.h>
using namespace std;
int main()
{
    int N;
    cin>>N;
    vector<int> a(N);
    set<int> has;
    int extra=0;
    for(int i=0;i<N;i++)
    {
        cin>>a[i];
        if(a[i]>N||has.count(a[i]))
        {
            extra++;
        }
        else
        {
            has.insert(a[i]);
        }
    }
    int current_read=0;
    int back=N;
    for(int i=1;i<=N;i++)
    {
        if(has.count(i))
        {
            current_read=i;
        }
        else
        {
            while(extra<2&&back>i)
            {
                if(has.count(back))
                {
                    has.erase(back);
                    extra++;
                }
                back--;
            }
            if(extra>=2)
            {
                extra-=2;
                current_read=i;
            }
            else
            {
                break;
            }
        }
    }
    cout<<current_read<<endl;
    return 0;
}

```
