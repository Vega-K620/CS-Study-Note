# GPLT Exchange competition2026 03 21
## L1-6(can't ac now,debuging)
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
int month[12]={31,28,31,30,31,30,31,31,30,31,30,31};
int ans=0;
bool runnian(int year)
{

    if((year%4==0&&year%100!=0)||year%400==0)
        return 1;
    return 0;
}

void check(int year1,int year2,int month1,int month2,int day1,int day2)
{
    if(year1==year2)
    {
        if(month1==month2)
        {
            ans+=day2-day1;
        }
        else
        {
            if(month2-month1>1)
            {
                for(int i=month1+1;i<month2;i++)
                {
                if(i!=2)
                    ans+=month[i];
                else
                {
                    if(runnian(year1))
                        ans+=29;
                    else
                        ans+=28;
                }
                }
            }
            else
            {
                if(month1!=2)
                {
                    ans+=month[month1]-day1;
                }
                else
                {
                    if(runnian(year1))
                    {
                        ans+=29-day1;
                    }
                    else
                    {
                        ans+=28-day1;
                    }
                }
                ans+=day2;
            }

        }
    }
    else
    {
        for(int i=year1+1;i<year2;i++)
        {
            if(runnian(i))ans+=366;
            else ans+=365;
        }
        for(int i=month1+1;i<=12;i++)
            {
                if(i!=2)
                    ans+=month[i];
                else
                {
                    if(runnian(year1))
                        ans+=29;
                    else
                        ans+=28;
                }
            }
        for(int i=1;i<month2;i++)
            {
                if(i!=2)
                    ans+=month[i];
                else
                {
                    if(runnian(year1))
                        ans+=29;
                    else
                        ans+=28;
                }
            }
        if(month1!=2)
            ans+=month[month1]-day1+1;
        else
        {
            if(runnian(year1))
                ans+=29-day1;
            else
                ans+=28-day1;
        }
        ans+=day2;
    }
}
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string date;
    cin>>date;
    string today="1349-06-28";
    if(date==today)
    {
        cout<<"jiu shi today."<<"\n";
        return 0;
    }
    else
    {
        int todayy=1349,todaym=06,todayd=28;
        int y,m,d;
        y=stoi(date.substr(0,4));
        m=stoi(date.substr(5,2));
        d=stoi(date.substr(8,2));

        if(todaym>y)
        {
            check(y,todayy,m,todaym,d,todayd);
            cout<<"guo qv le "<<ans<<" day?"<<"\n";
        }
        else if(todayy<y)
        {
            check(todayy,y,todaym,m,todayd,d);
            cout<<"hai cha "<<ans<<" day!"<<"\n";
        }
        else
        {
            if(todaym>m)
            {
                check(y,todayy,m,todaym,d,todayd);
                cout<<"guo qv le "<<ans<<" day?"<<"\n";
            }
            else if(todaym<m)
            {
                check(todayy,y,todaym,m,todayd,d);
                cout<<"hai cha "<<ans<<" day!"<<"\n";
            }
            else
            {
                if(todayd>d)
                {
                    check(y,todayy,m,todaym,d,todayd);
                    cout<<"guo qv le "<<ans<<" day?"<<"\n";
                }
                else
                {
                    check(todayy,y,todaym,m,todayd,d);
                    cout<<"hai cha "<<ans<<" day!"<<"\n";
                }
            }
        }
    }
    return 0;
}

```
## L1-7(can't ac now,debuging)
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
bool temp[1000][1000];
vector<pair<int,int>> out;
bool check(int x,int y)
{
    for(int i=0;i<out.size();i++)
    {
        if(x==out[i].first||y==out[i].second)
            return 1;
    }
    return 0;
}
bool cmp(int a,int b)
{
    return a>b;
}
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m,k;
    cin>>n>>m>>k;
    int excel[1000][1000];
    vector<int> num;
    map<int,pair<int ,int>> excelmap;
    for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
                {
                    cin>>excel[i][j];
                    num.push_back(excel[i][j]);
                    excelmap[excel[i][j]]={i,j};
                }
        }
        sort(num.begin(),num.end(),cmp);
    /*while(out.size()<k)
        {
            for(int i=0;i<n;i++)
            {
                for(int j=0;j<m;j++)
                {
                    if(check(i,j))continue;
                    if(excel[i][j]==num[l])
                    {
                        out.push_back({i,j});
                        break;
                    }
                }a
            }
            l++;
        }*/
        int l=0;
        while(out.size()<k)
        {
            if(!check(excelmap[num[l]].first,excelmap[num[l]].second))
            out.push_back(excelmap[num[l]]);
            l++;
        }

    for(int i=0;i<out.size();i++)
    {
        for(int j=0;j<m;j++)
            temp[out[i].first][j]=true;
        for(int j=0;j<n;j++)
            temp[j][out[i].first]=true;
    }
    int outhang=0;
    for(int i=0;i<n;i++)
    {
        bool didwai=false;
        int outline=0;
        for(int j=0;j<m;j++)
        {
            bool didnei=false;
            if(!temp[i][j])
            {
                cout<<excel[i][j];
                outline++;
                didnei=true;
                didwai=true;
            }
            if(didnei&&outline!=m-out.size())cout<<" ";
        }
        if(didwai)
        outhang++;
        if(didwai&&outhang!=n-out.size())
            cout<<"\n";
    }

    return 0;
}

```
