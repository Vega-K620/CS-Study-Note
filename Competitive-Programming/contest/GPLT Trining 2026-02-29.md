# GPLT Trining 2026-02-29
## L1-1 编程解决一切
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    cout<<"Problem? The Solution: Programming."<<"\n";
    return 0;
}
```
## L1-2 再进去几个人
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int a,b;
    cin>>a>>b;
    cout<<b-a<<"\n";
    return 0;
}
```
## L1-3 帮助色盲
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int a,b;
    cin>>a>>b;
    if(b==0)
    {
        if(a==0)
        cout<<"biii"<<"\n";
        else if(a==1)
        cout<<"dudu"<<"\n";
        else if(a==2)
        cout<<'-'<<"\n";
    }
    else
    cout<<'-'<<"\n";
    if(a==0)
    cout<<"stop"<<"\n";
    else if(a==1)
    cout<<"move"<<"\n";
    else
    cout<<"stop"<<"\n";
    return 0;
}
```
## L1-4 四项全能
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    vector<int> b(m);
    for(int i=0;i<m;i++)
    cin>>b[i];
    int sum=0;
    for(auto i:b)
    sum+=i;
    int ans=sum-n*(m-1);
    if(ans>=0)
    cout<<ans<<"\n";
    else
    cout<<0<<"\n";
    return 0;
}
```
## L1-5 别再来这么多猫娘了！(まだACしていません)
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string str;
    int n;
    cin>>n;
    vector<string> kinnshi(n);
    for(int i=0;i<n;i++)
    cin>>kinnshi[i];
    int v;
    cin>>v;
    cin.ignore();
    getline(cin,str);
    int sum=0;
    for(int i=0;i<n;i++)
    {
        while(str.find(kinnshi[i])!=string::npos)
        {
            if(str.find(kinnshi[i])!=string::npos)
            {
                int finded=str.find(kinnshi[i]);
                sum++;
                str.erase(finded,kinnshi[i].size());
                str.insert(finded,"<censored>");
            }
        }
    }
    
    if(sum>=v)
    {
        cout<<sum<<"\n";
        cout<<"He Xie Ni Quan Jia!"<<"\n";
    }
    else
    cout<<str<<"\n";
    return 0;
}
```
## L1-6 兰州牛肉面
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<double> price(n+1);
    for(int i=1;i<=n;i++)
    cin>>price[i];
    vector<int> dayly(n+1,0);
    while(true)
    {
        int no,wanshu;
        cin>>no;
        if(no==0)
        break;
        cin>>wanshu;
        dayly[no]+=wanshu;
    }
    double sum=0;
    for(int i=1;i<=n;i++)
    {
        cout<<dayly[i]<<"\n";
        sum+=(double)dayly[i]*price[i];
    }
    cout<<fixed<<setprecision(2)<<sum<<"\n";
    return 0;
}
```
## L1-7 整数的持续性
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    long long a,b;
    cin>>a>>b;
    long long maxint=0;
    map<int,vector<long long>> ans;
    for(long long i=a;i<=b;i++)
    {
        long long copy=i;
        long long next=1;
        long long len=0;
        if(i<10)
        {
            ans[len].push_back(i);
            maxint=max(maxint,len);
            continue;
        }
        while(true)
        {
            
            next=1;
            // cout<<copy<<" ";
            while(copy!=0)
            {
                next*=copy%10;
                copy/=10;
            }
            copy=next;
            // cout<<next<<"\n";
            len++;
            if(copy<10)
            {
                ans[len].push_back(i);
                maxint=max(maxint,len);
                break;
            }
        }
    }
    cout<<maxint<<"\n";
    for(int i=0;i<ans[maxint].size();i++)
    {
        cout<<ans[maxint][i]<<(i==ans[maxint].size()-1?"\n":" ");
    }
    return 0;
}
```
## L1-8 九宫格(まだACしていません)
```cpp
#include<bits/stdc++.h>
using namespace std;
int box[10][10];
void solve()
{
    
    for(int i=0;i<9;i++)
    {
        for(int j=0;j<9;j++)
        {
            cin>>box[i][j];
            if(box[i][j]>9||box[i][j]<1)
            {
                cout<<"0\n";
                return;
            }
        }
    }
    for(int j=0;j<9;j++)
    {
        bool check[10]={false};
        for(int i=0;i<9;i++)
        {
            if(check[box[i][j]])
            {
                cout<<0<<"\n";
                return;
            }
            else
            check[box[i][j]]=true;
        }
    }
    for(int j=0;j<9;j++)
    {
        bool check[10]={false};
        for(int i=0;i<9;i++)
        {
            if(check[box[j][i]])
            {
                cout<<0<<"\n";
                return;
            }
            else
            check[box[j][i]]=true;
        }
    }
    for(int i=0;i<9;i+=3)
    {
        for(int j=0;j<9;j+=3)
        {
            for(int k=0;k<3;k++)
            {
                bool check[10]={false};
                for(int l=0;l<3;l++)
                {
                    if(check[box[i+k][j+l]])
                    {
                        cout<<0<<"\n";
                        return;
                    }
                    else
                    check[box[i+k][j+l]]=true;
                }
            }
        }
    }
    cout<<1<<"\n";
    return;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    while(n--)
    {
        solve();
    }
    return 0;
}
```
## L2-1 鱼与熊掌
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;

    vector<vector<int>> hito(n);
    for(int i=0;i<n;i++)
    {
        int k;
        cin>>k;
        for(int j=0;j<k;j++)
        {
            int a;
            cin>>a;
            hito[i].push_back(a);
        }
    }
    // for(int i=0;i<n;i++)
    // {
    //     for(auto j:hito[i])
    //     {
    //         cout<<j<<" ";
    //     }
    //     cout<<"\n";
    // }
    int q;
    cin>>q;
    for(int i=0;i<q;i++)
    {
        int a,b;
        cin>>a>>b;
        int ans=0;
        for(int j=0;j<n;j++)
        {
            if(find(hito[j].begin(),hito[j].end(),a)!=hito[j].end()&&find(hito[j].begin(),hito[j].end(),b)!=hito[j].end())
            {
                ans++;
            }
        }
        cout<<ans<<"\n";
    }
    return 0;
}
```
## L2-2 懂蛇语(まだACしていません)
```cpp
#include<bits/stdc++.h>
using namespace std;

string getAbbr(const string& s)
{
    string res="";
    if(s.empty())return res;
    res+=s[0];
    for(int i=1;i<s.size();i++)
    {
        if(s[i]!=' '&&s[i-1]==' ')
        res+=s[i];
    }
    return res;
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    string dummy;
    getline(cin,dummy);
    map<string,vector<string>> dict;
    for(int i=0;i<n;i++)
    {
        string line;
        getline(cin, line);
        if (line.empty()) continue;
        string abbr = getAbbr(line);
        dict[abbr].push_back(line);
    }
    for(auto& pair:dict)
    {
        sort(pair.second.begin(),pair.second.end());
    }


    
    int m;
    cin>>m;
    getline(cin,dummy);
    while(m--)
    {
        string query;
        getline(cin,query);
        string queryAbbr=getAbbr(query);
        if(dict.count(queryAbbr))
        {
            const vector<string>& res=dict[queryAbbr];
            for(int i=0;i<res.size();i++)
            {
                cout<<res[i]<<(i==res.size()-1?"":"|");
            }
            cout<<"\n";
        }
        else
        {
            cout<<query<<"\n";
        }
    }

    return 0;
}
```
