# AtCoder Beginner Contest 464
## A - Decisive Battle
### https://atcoder.jp/contests/abc464/tasks/abc464_a
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

signed main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string S;
    cin>>S;
    
    int east=count(S.begin(),S.end(),'E');
    int west=count(S.begin(),S.end(),'W');
    
    if(east>west)
    {
        cout<<"East\n";
    }
    else
    {
        cout<<"West\n";
    }
    
    return 0;
}

```
## B - Crop
### https://atcoder.jp/contests/abc464/tasks/abc464_b
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

const int INF=1e18;

void solve()
{
    int h,w;
    cin>>h>>w;
    vector<vector<char>> box(h,vector<char> (w));
    for(int i=0;i<h;i++)
    {
        for(int j=0;j<w;j++)
        {
            cin>>box[i][j];
        }
    }
    int a=0,b=0,c=0,d=0;
    for(int i=0;i<h;i++)
    {
        bool flag=false;
        for(int j=0;j<w;j++)
        {
            if(box[i][j]=='#')
            {
                a=i;
                flag=true;
                break;
            }
        }
        if(flag)break;
    }
    for(int i=h-1;i>=0;i--)
    {
        bool flag=false;
        for(int j=w-1;j>=0;j--)
        {
            if(box[i][j]=='#')
            {
                b=i;
                flag=true;
                break;
            }
        }
        if(flag)break;
    }
    for(int i=0;i<w;i++)
    {
        bool flag=false;
        for(int j=0;j<h;j++)
        {
            if(box[j][i]=='#')
            {
                c=i;
                flag=true;
                break;
            }
        }
        if(flag)break;
    }
    for(int i=w-1;i>=0;i--)
    {
        bool flag=false;
        for(int j=h-1;j>=0;j--)
        {
            if(box[j][i]=='#')
            {
                d=i;
                flag=true;
                break;
            }
        }
        if(flag)break;
    }
    for(int i=a;i<=b;i++)
    {
        for(int j=c;j<=d;j++)
        {
            cout<<box[i][j];
        }
        cout<<"\n";
    }
}

signed main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int t=1;
    //cin>>t;
    while(t--)
    {
        solve();
    }
}
```
## C - Plumage Palette
### https://atcoder.jp/contests/abc464/tasks/abc464_c
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

const int INF=1e18;

void solve()
{
    int n,m;
    cin>>n>>m;

    vector<int> col(n+1);
    vector<vector<pair<int,int>>> events(m+1);

    vector<int> cnt(n+1,0);
    int ans=0;
    
    auto add=[&](int c)
    {
        if(cnt[c]==0)ans++;
        cnt[c]++;
    };
    
    auto del=[&](int c)
    {
        cnt[c]--;
        if(cnt[c]==0)ans--;
    };
    
    for(int i=1;i<=n;i++)
    {
        int a,d,b;
        cin>>a>>d>>b;
        if(d==1)
        {
            col[i]=b;
        }
        else
        {
            col[i]=a;
            events[d].push_back({i,b});
        }
    }

    for(int i=1;i<=n;i++)
    {
        add(col[i]);
    }
    
    vector<int> out(m+1);
    out[1]=ans;

    for(int i=2;i<=m;i++)
    {
        for(auto &ev:events[i])
        {
            int bird=ev.first;
            int new_c=ev.second;
            
            del(col[bird]);
            col[bird]=new_c;
            add(col[bird]);
        }
        out[i]=ans;
    }
    
    for(int i=1;i<=m;i++)
    {
        cout<<out[i]<<"\n";
    }
}

signed main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int t=1;
    //cin>>t;
    while(t--)
    {
        solve();
    }
}
```
## D - Celester
### https://atcoder.jp/contests/abc464/tasks/abc464_d
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

long long N;
string S;
vector<long long> X, Y;
vector<vector<long long>> memo;

long long dfs(int i, int temp)
{
    if(i==N)return 0;
    if(memo[i][temp]!=-1)
    {
        return memo[i][temp];
    }
    long long cost_R=(S[i]=='S')?X[i]:0;
    long long res_R=dfs(i+1,0)-cost_R;
    long long cost_S=(S[i]=='R')?X[i]:0;
    long long bonus_S=(i>0&&temp==0)?Y[i-1]:0;
    long long res_S=dfs(i+1,1)+bonus_S-cost_S;

    return memo[i][temp]=max(res_R,res_S);
}

void solve()
{
    cin>>N>>S;
    X.resize(N);
    Y.resize(N-1);
    for(int i=0;i<N;i++)
    {
        cin>>X[i];
    }
    for(int i=0;i<N-1;i++)
    {
        cin>>Y[i];
    }

    memo.assign(N,vector<long long>(2,-1));

    cout<<dfs(0,1)<<"\n";
}

signed main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int T;
    cin>>T;
    while(T--)
    {
        solve();
    }
    return 0;
}
```
## E - Fill-Rect Query
### https://atcoder.jp/contests/abc464/tasks/abc464_e
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

struct operation{
    int r,c;
    char x;
};

signed main()
{
    cin.tie(NULL)->sync_with_stdio(false);

    int h,w,q;
    cin>>h>>w>>q;

    vector<operation> box(q);
    for(int i=0;i<q;i++)
    {
        cin>>box[i].r>>box[i].c>>box[i].x;
    }

    vector<string> grid(h,string(w,'A'));
    vector<int> max_c(h,0);

    set<int> active_rows;
    for(int r=0;r<h;r++)
    {
        active_rows.insert(r);
    }

    for(int i=q-1;i>=0;i--)
    {
        int R=box[i].r;
        int C=box[i].c;
        char X=box[i].x;

        if(active_rows.empty()) break;

        auto it=active_rows.begin();
        while(it!=active_rows.end()&&*it<R)
        {
            int r=*it;
            
            if(max_c[r]<C)
            {
                for(int c=max_c[r];c<C;c++)
                {
                    grid[r][c]=X;
                }
                max_c[r]=C;
            }
            if(max_c[r]==w)
            {
                it=active_rows.erase(it);
            }
            else
            {
                it++;
            }
        }
    }

    for(int r=0;r<h;r++)
    {
        cout<<grid[r]<<"\n";
    }

    return 0;
}
```
