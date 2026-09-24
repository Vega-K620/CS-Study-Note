# Codeforces Round 1109 (Div. 3)
## A. Iskander and Drawings
### https://codeforces.com/contest/2244/problem/A
一番長い「#」をメモして、答えは $ (長さ+1)/2 $ 。
```cpp
void solve()
{
    int n;
    cin>>n;
    string str;
    cin>>str;
    int longest=0;
    int len=0;
    for(int i=0;i<n;i++)
    {
        if(str[i]=='#')
        {
            len++;
            longest=max(longest,len);
        }
        else
        {
            longest=max(longest,len);
            len=0;
        }
    }
    cout<<(longest+1)/2<<"\n";;
}
```
## B. Nikita and Books
### https://codeforces.com/contest/2244/problem/B
一番構築やすいの方法は自然の数字の配列です。
```cpp
void solve()
{
    int n;
    cin>>n;
    vector<int> num(n);
    for(int i=0;i<n;i++)
    {
        cin>>num[i];
    }
    int sum=0;
    for(int i=0;i<n;i++)
    {
        sum+=(num[i]-(i+1));
        if(sum<0)
        {
            cout<<"No"<<"\n";
            return;
        }
    }
    cout<<"Yes"<<"\n";
}
```
## C. Stepan and Permutation
### https://codeforces.com/contest/2244/problem/C

```cpp
void solve()
{
    int n,x,y;
    cin>>n>>x>>y;
    vector<int> num(n);
    for(int i=0;i<n;i++)
    {
        cin>>num[i];
    }

    int g=__gcd(x,y);
    vector<vector<int>> check(g);
    for(int i=0;i<n;i++)
    {
        check[i%g].push_back(num[i]);
    }

    for(int i=0;i<g;i++)
    {
        sort(check[i].begin(),check[i].end());
    }

    vector<int> temp(g,0);
    vector<int> copy(n);
    for(int i =0;i<n;i++)
    {
        int point=i%g;
        copy[i]=check[point][temp[point]++];
    }

    if(is_sorted(copy.begin(),copy.end()))
    {
        cout<<"Yes"<<"\n";
    }
    else
    {
        cout<<"No"<<"\n";
    }
}
```
## D. Yaroslav and Productivity
### https://codeforces.com/contest/2244/problem/D

```cpp
void solve()
{
    int n,m;
    cin>>n>>m;
    vector<int> a(n+1);
    vector<int> sum(n+1,0);
    for(int i=1;i<=n;i++)
    {
        cin>>a[i];
        sum[i]=sum[i-1]+a[i];
    }
    vector<int> b(m+1);
    b[0]=0;
    for(int i=1;i<=m;i++)
    {
        cin>>b[i];
    }
    sort(b.begin(),b.end());
    
    int ans=0;
    if(b[m]<n)
    {
        ans+=(sum[n]-sum[b[m]]);
    }

    bool flag=false; 
    for (int i=m;i>=1;i--)
    {
        int l=b[i-1]+1;
        int r=b[i];
        
        if(l<=r)
        {
            int tempsum=sum[r]-sum[l-1];
            
            int temp=flag?(-tempsum):(tempsum);
            
            if(temp<0)
            {
                ans-=temp;
                flag=!flag; 
            }
            else
            {
                ans+=temp;
            }
        }
    }
    
    cout<<ans<<"\n";
}
```
## E. Masha and the Garland
### https://codeforces.com/contest/2244/problem/E

```cpp
void solve()
{
    int n,q;
    cin>>n>>q;
    string str;
    cin>>str;
    string temp1="",temp2="";
    for(int i=0;i<n;i++)
    {
        if(i%2==0)
        {
            temp1+='0';
            temp2+='1';
        }
        else
        {
            temp1+='1';
            temp2+='0';
        }
    }
    if(str==temp1||str==temp2)
    {
        for(int i=0;i<q;i++)
        {
            int a,b,c;
            cin>>a>>b>>c;
            cout<<"Yes"<<"\n";
        }
        return;
    }
    vector<int> diff1(n+1,0),diff2(n+1,0),sum1(n+1,0),sum2(n+1,0);
    
    for(int i=0;i<n;i++)
    {
        if(str[i]!=temp1[i]) diff1[i+1]=1;
        if(str[i]!=temp2[i]) diff2[i+1]=1;
    }
    
    bool flag1=false,flag2=false;
    for(int i=1;i<=n;i++)
    {
        sum1[i]=sum1[i-1];
        if(diff1[i]==1)
        {
            if(!flag1)
            {
                flag1=true;
                sum1[i]++;
            }
        }
        if(diff1[i]==0)
        {
            flag1=false;
        }
        
        sum2[i]=sum2[i-1];
        if(diff2[i]==1)
        {
            if(!flag2)
            {
                flag2=true;
                sum2[i]++;
            }
        }
        if(diff2[i]==0)
        {
            flag2=false;
        }
    }
    
    for(int i=0;i<q;i++)
    {
        int l,r,cnt;
        cin>>l>>r>>cnt;
        
        int op1=sum1[r]-sum1[l];
        if(diff1[l]==1)op1++;
        
        int op2=sum2[r]-sum2[l];
        if(diff2[l]==1)op2++;
        
        if(op1<=cnt||op2<=cnt)
        {
            cout<<"Yes"<<"\n";
        }
        else
        {
            cout<<"No"<<"\n";
        }
    }
}
```
## F. Anya Loves Trees!
### https://codeforces.com/contest/2244/problem/F

```cpp
int n;
vector<vector<int>> road;
vector<int> a;
vector<int> cnt;
vector<int> minnum;
vector<int> maxnum;
bool possible;

void dfs(int u)
{
    if(!possible)
    {
        return;
    }

    if(a[u]>0)
    {
        cnt[u]=1;
        minnum[u]=a[u];
        maxnum[u]=a[u];
        return;
    }

    for(int i=0;i<road[u].size();i++)
    {
        int v=road[u][i];
        dfs(v);
    }

    if(!possible)
    {
        return;
    }

    int tempcnt=0;
    int tempmin=2e18;
    int tempmax=-2e18;

    for(int i=0;i<road[u].size();i++)
    {
        int v=road[u][i];
        tempcnt+=cnt[v];
        
        if(minnum[v]<tempmin)
        {
            tempmin=minnum[v];
        }
        if(maxnum[v]>tempmax)
        {
            tempmax=maxnum[v];
        }
    }

    cnt[u]=tempcnt;
    minnum[u]=tempmin;
    maxnum[u]=tempmax;

    if(tempmax-tempmin+1!=tempcnt)
    {
        possible=false;
        return;
    }

    int m=road[u].size();
    if(m>1)
    {
        int err=0;
        for(int i=0;i<m;i++)
        {
            int curr=road[u][i];
            int next=road[u][(i+1)%m];
            
            if(minnum[curr]>minnum[next])
            {
                err++;
            }
        }
        
        if(err>1)
        {
            possible=false;
            return;
        }
    }
}

void solve()
{
    cin>>n;
    road.assign(n+1, vector<int>());
    a.assign(n+1,0);
    cnt.assign(n+1,0);
    minnum.assign(n+1,0);
    maxnum.assign(n+1,0);
    possible=true;

    for(int i=2;i<=n;i++)
    {
        int p;
        cin>>p;
        road[p].push_back(i);
    }

    for(int i=1;i<=n;i++)
    {
        cin>>a[i];
    }

    dfs(1);

    if(possible)
    {
        cout<<"Yes"<<"\n";
    }
    else
    {
        cout<<"No"<<"\n";
    }
}
```
