# AtCoder Daily Training EASY 2026/03/04 18:00start
<details>
  <summary>(今回の感想)</summary>
  今日は用事があって本番には参加できず、後から解きました。
  AKでしたが。途中ケアレスミスが多いすぎます。
</details>

## A - Swap Odd and Even
### https://atcoder.jp/contests/adt_easy_20260304_2/tasks/abc293_a
この問題は文字列操作の問題です。
僕はペア（隣り合う文字）ごとの入れ替えました。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
  string s;
  cin>>s;
  int len;
  len=s.size();
  for(int i=0;i<len;i+=2)
  {
    cout<<s[i+1]<<s[i];
  }
  return  0;
}
```
後から気づいたのですが、swap 関数を使う方法もあります。
```cpp
for (int i = 0; i < len; i += 2) {
    swap(s[i], s[i+1]);
}
```
## B - N-choice question
### https://atcoder.jp/contests/adt_easy_20260304_2/tasks/abc300_a
この問題は線形探索の基本問題です。
配列を順番に走査して、条件に合う要素を探します。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
  ios::sync_with_stdio(false);cin.tie(NULL);
  int n;
  long long a,b;
  cin>>n>>a>>b;
  vector<long long> num(n);
  for(int i=0;i<n;i++)
  cin>>num[i];
  int ans=a+b;
  for(int i=0;i<n;i++)
  {
    if(ans==num[i])
    {
      cout<<i+1<<"\n";
      break;
    }
  }
  return  0;
}
```
## C - Pizza
### https://atcoder.jp/contests/adt_easy_20260304_2/tasks/abc238_b
この問題シミュレーションです。累積和的な考え方です。
回転角度を累積させて、360で割った余りをとるのがポイントです。
実装自体は単純ですが、コーナーケース（0度や360度）の扱いに注意が必要です。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    ios::sync_with_stdio(false);cin.tie(NULL);
    int n;
    cin>>n;
    vector<int> cuts;
    int lastcut=0;
    int newcut;
    cuts.push_back(0);
    for(int i=0;i<n;i++)
    {
        cin>>newcut;
        lastcut=(lastcut+newcut)%360;
        cuts.push_back(lastcut);
    }
    cuts.push_back(360);
    int ans=0;
    sort(cuts.begin(),cuts.end());
    for(int i=0;i<n+1;i++)
    {
        ans=max(ans,cuts[i+1]-cuts[i]);
    }
    cout<<ans<<"\n";
    return 0;
}
```
## D - Perfect String
### https://atcoder.jp/contests/adt_easy_20260304_2/tasks/abc249_b
この問題は複数の条件（大文字の有無、小文字の有無、重複の有無）
をすべて同時に満たしているかを正確に判定する。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    string s;
    cin>>s;
    int len=(int)s.size();
    vector<bool> check1(26,false);
    vector<bool> check2(26,false);
    bool flag=true;
    bool have1=false,have2=false;
    for(int i=0;i<len;i++)
    {
        if(s[i]>='a'&&s[i]<='z')
        {
            if(check1[s[i]-'a']==false)
            {
                check1[s[i]-'a']=true;
                have1=true;
            }
            else
            {
                flag=false;
                break;
            }
        }
        else
        {
            if(check2[s[i]-'A']==false)
            {
                check2[s[i]-'A']=true;
                have2=true;
            }
            else
            {
                flag=false;
                break;
            }
        }
    }
    if(flag&&have1&&have2)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
}
```
## E - abc285_brutmhyhiizp
### https://atcoder.jp/contests/adt_easy_20260304_2/tasks/abc285_c
Excelの列番号のように「0」がない26進法の変換。
累乗（pow）を使わず、左から順に26倍して足すことで精度と速度を確保する。
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    string s;
    cin>>s;
    int len=(int)s.size();
    long long ans=0;
    for(int i=0;i<len;i++)
    {
        ans=ans*26+(s[i]-'A'+1);
    }
    cout<<ans<<"\n";
}
```
