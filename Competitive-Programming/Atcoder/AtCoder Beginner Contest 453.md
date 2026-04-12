# AtCoder Beginner Contest 453

## A - Trimo
### https://atcoder.jp/contests/abc453/tasks/abc453_a
この問題は「bool」を利用して簡単に解けます。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    string str;
    cin>>str;
    bool flag=false;
    for(int i=0;i<n;i++)
    {
        if(str[i]!='o')flag=true;
        if(flag)cout<<str[i];
    }

    if(!flag)cout<<""<<"\n";
    else
    cout<<"\n";
    return 0;
}
```
## B - Sensor Data Logging
### https://atcoder.jp/contests/abc453/tasks/abc453_b
この問題は状況が満足のをメモして最後に出力します。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int t,x;
    cin>>t>>x;
    vector<int> tempr(t+1);
    vector<pair<int,int>> time;
    for(int i=0;i<=t;i++)
    cin>>tempr[i];
    int now=tempr[0];
    time.push_back({0,tempr[0]});
    for(int i=1;i<=t;i++)
    {
        if(abs(tempr[i]-now)>=x)
        {
            now=tempr[i];
            time.push_back({i,tempr[i]});
        }
    }

    for(int i=0;i<time.size();i++)
    cout<<time[i].first<<" "<<time[i].second<<"\n";
    return 0;
}
```
## C - Sneaking Glances
### https://atcoder.jp/contests/abc453/tasks/abc453_c
この問題の制約が小さいので全探索します。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<double> num(n);
    for(int i=0;i<n;i++)
    cin>>num[i];
    int ans=0;

    for (int i = 0; i < (1 << n); i++) {
        double now=0.5;
        int current_cross=0;

        for (int j=0;j<n;j++)
        {
            double next_pos;
            if((i>>j)&1)
            {
                next_pos=now+num[j];
            }
            else
            {
                next_pos=now-num[j];
            }

            if((now>0&&next_pos<0)||(now<0&&next_pos>0))
            {
                current_cross++;
            }
            now=next_pos;
        }

        ans=max(ans,current_cross);
    }
    cout<<ans<<"\n";
    return 0;
}
```
## D - Go Straight
### https://atcoder.jp/contests/abc453/tasks/abc453_d
この問題は「BFS」を利用して、最短道の基本にちょっと制約を加えて答えが出てきます。
```cpp
#include <bits/stdc++.h>
using namespace std;

int dr[]={-1,1,0,0};
int dc[]={0,0,-1,1};
char dchar[]={'U','D','L','R'};

struct State{
    int r,c,dir;
};

struct Parent {
    int pr,pc,pdir;
    char move;
};

Parent pre[1005][1005][4];
bool visited[1005][1005][4];

int main()
{
    cin.tie(0)->sync_with_stdio(false);
    
    int H,W;
    cin >> H >> W;
    vector<string> S(H);
    int sr,sc,gr,gc;
    for(int i=0;i<H;i++)
    {
        cin>>S[i];
        for(int j=0;j<W;j++)
        {
            if(S[i][j]=='S')
            {
                sr=i;
                sc=j;
            }
            if(S[i][j]=='G')
            {
                gr=i;
                gc=j;
            }
        }
    }

    queue<State> q;
    for(int d=0;d<4;d++)
    {
        int nr=sr+dr[d],nc=sc+dc[d];
        if(nr>=0&&nr<H&&nc>=0&&nc<W&&S[nr][nc]!='#')
        {
            if(!visited[nr][nc][d])
            {
                visited[nr][nc][d]=true;
                pre[nr][nc][d]={sr,sc,-1,dchar[d]};
                q.push({nr,nc,d});
            }
        }
    }

    int end_r=-1,end_c=-1,end_dir=-1;

    while(!q.empty())
    {
        State curr=q.front();
        q.pop();

        if(curr.r==gr&&curr.c==gc)
        {
            end_r=curr.r;
            end_c=curr.c;
            end_dir=curr.dir;
            break;
        }

        char type=S[curr.r][curr.c];
        for(int d=0;d<4;d++)
        {
            if(type=='o'&&d!=curr.dir)continue;
            if(type=='x'&&d==curr.dir)continue;

            int nr=curr.r+dr[d],nc=curr.c+dc[d];
            if(nr>=0&&nr<H&&nc>=0&&nc<W&&S[nr][nc]!='#')
            {
                if(!visited[nr][nc][d])
                {
                    visited[nr][nc][d]=true;
                    pre[nr][nc][d]={curr.r,curr.c,curr.dir,dchar[d]};
                    q.push({nr,nc,d});
                }
            }
        }
    }

    if(end_r==-1)
    {
        cout<<"No"<<"\n";
    }
    else
    {
        cout<<"Yes"<<"\n";
        string res="";
        int cr=end_r,cc=end_c,cd=end_dir;
        while(cr!=sr||cc!=sc)
        {
            Parent p=pre[cr][cc][cd];
            res+=p.move;
            cr=p.pr;
            cc=p.pc;
            cd=p.pdir;
            if(cd==-1)break;
        }
        reverse(res.begin(),res.end());
        cout<<res<<"\n";
    }

    return 0;
}
```
