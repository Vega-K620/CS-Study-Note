# AtCoder Daily Training ALL 2026/04/24 16:00start
今日時間がありませんから、簡単なA-Fを解きました。とても簡単な問題ですから、詳しい解き方法は略します。
## A - 11/22 String
### https://atcoder.jp/contests/adt_all_20260424_1/tasks/abc381_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

bool check(string f,string s)
{
    if(f.size()!=s.size())
    return false;
    else if(f.size()==0)
    return true;
    else
    {
        int len=f.size();

        for(int i=0;i<len;i++)
        {
            if(f[i]!='1'||s[i]!='2')
            return false;
        }
        return true;
    }
}
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string str;
    int n;
    cin>>n>>str;
    if(str.find('/')!=string::npos&&str.size()==n)
    {
        int ip=str.find('/');
        string f=str.substr(0,ip),s=str.substr(ip+1);

        if(check(f,s))
        cout<<"Yes"<<"\n";
        else
        cout<<"No"<<"\n";
    }
    else
    {
        cout<<"No"<<"\n";
    }
    return 0;
}
```
## B - tcdr
### https://atcoder.jp/contests/adt_all_20260424_1/tasks/abc315_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string str,ans="";
    cin>>str;
    for(int i=0;i<(int)str.size();i++)
    {
        if(str[i]!='a'&&str[i]!='e'&&str[i]!='i'&&str[i]!='o'&&str[i]!='u')
        {
            ans+=str[i];
        }
    } 
    cout<<ans<<"\n";
    return 0;
}
```
## C - Full House 3
### https://atcoder.jp/contests/adt_all_20260424_1/tasks/abc398_b
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int count[13]={0};
    for(int i=0;i<7;i++)
    {
        int num;
        cin>>num;
        count[num-1]++;
    }
    int three=0,two=0;
    for(int i=0;i<13;i++)
    {
        if(count[i]>=3)
        three++;
        if(count[i]>=2)
        two++;
    }
    two-=three;
    if(three>=2||(three>=1&&two>=1))
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
    return 0;
}
```
## D - Binary Alchemy
### https://atcoder.jp/contests/adt_all_20260424_1/tasks/abc370_b
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    int A[105][105];
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=i;j++)
        {
            cin>>A[i][j];
        }
    }
    int curr=1;
    for(int i=1;i<=n;i++)
    {
        if(curr>=i) curr=A[curr][i];
        else curr=A[i][curr];
    }
    cout<<curr<<"\n";
    return 0;
}
```
## E - Paint to make a rectangle
### https://atcoder.jp/contests/adt_all_20260424_1/tasks/abc390_c
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int H,W;
    cin>>H>>W;
    vector<string> s(H);
    
    int minR=H,maxR=-1,minC=W,maxC=-1;

    for(int i=0;i<H;i++)
    {
        cin>>s[i];
        for(int j=0;j<W;j++)
        {
            if(s[i][j]=='#')
            {
                minR=min(minR,i);
                maxR=max(maxR,i);
                minC=min(minC,j);
                maxC=max(maxC,j);
            }
        }
    }

    bool possible=true;
    for(int i=minR;i<=maxR;i++)
    {
        for(int j=minC;j<=maxC;j++)
        {
            if(s[i][j]=='.')
            {
                possible=false;
                break;
            }
        }
        if(!possible) break;
    }

    if(possible)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
    return 0;
}
```
## F - Counting 2
### https://atcoder.jp/contests/adt_all_20260424_1/tasks/abc231_c
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,q;
    cin>>n>>q;
    
    vector<int> a(n);
    for(int i=0;i<n;i++)
    {
        cin>>a[i];
    }
    sort(a.begin(),a.end());

    while(q--)
    {
        int x;
        cin>>x;
        auto it=lower_bound(a.begin(),a.end(),x);
        cout<<n-(it-a.begin())<<"\n";
    }
    return 0;
}
```
