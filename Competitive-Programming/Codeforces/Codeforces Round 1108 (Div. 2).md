# Codeforces Round 1108 (Div. 2)
## A. farmpiggie and Subset Sum
### https://codeforces.com/contest/2246/problem/A
一番簡単の構築の方法は直接ｎから1までの配列出力します。
```cpp
void solve()
{
    int n;
    cin>>n;
    vector<int> num(n);
    for(int i=n;i>=1;i--)
    {
        cout<<i<<" ";
    }
    cout<<"\n";
}
```
## B. ezraft and Array
### https://codeforces.com/contest/2246/problem/B
$ a_i $ の数字は前の数字の累積和、こうすると問題文を満たす。
```cpp
void solve()
{
    int n;
    cin>>n;
    if(n==1)cout<<1<<"\n";
    else if(n==2)cout<<-1<<"\n";
    else
    {
        vector<int> num;
        num.push_back(1);
        num.push_back(2);
        num.push_back(3);
        for(int i=0;i<n-3;i++)
        {
            num.push_back(num.back()*2);
        }
        for(int i:num)
        {
            cout<<i<<" ";
        }
        cout<<"\n";
    }
}
```
## C. 0mar and Alternating Sums
### https://codeforces.com/contest/2246/problem/C

```cpp
int power(int base,int exp)
{
    int res=1;
    base%=MOD;
    while(exp>0)
    {
        if(exp%2==1)
        res=(res*base)%MOD;
        
        base=(base*base)%MOD;
        exp/=2;
    }
    return res;
}

void solve()
{
    int n;
    cin>>n;
    vector<int> a(n);
    int c=0;
    
    for(int i=0;i<n;i++)
    {
        cin>>a[i];
        if(a[i]==-1)
        {
            c++;
        }
    }
    
    vector<pair<int,int>> cntnum;
    for(int i=c;i<n;i++)
    {
        if(cntnum.empty()||cntnum.back().first!=a[i])
        {
            cntnum.push_back({a[i],1});
        }
        else
        {
            cntnum.back().second++;
        }
    }
    
    int total_even=1;
    for(auto const& p:cntnum)
    {
        int cnt=p.second;
        total_even=(total_even*power(2,cnt-1))%MOD;
    }
    
    int P=0;
    for(int i=0;i+1<cntnum.size();i++)
    {
        if(cntnum[i].first+1==cntnum[i+1].first)
        {
            P++;
        }
    }
    
    int ans=0;
    if(c==0)
    {
        ans=total_even;
    }
    else
    {
        int ways_minus1=power(2,c-1);
        ans=ways_minus1*total_even%MOD*(1+P)%MOD;
    }
    
    cout<<ans<<"\n";
}
```
## D. diss_quack and Array Game
### https://codeforces.com/contest/2246/problem/D

```cpp
const int INF=1e18;

int cnt1(int num)//计算1的个数
{
    int ans=0;
    while(num!=0)
    {
        if((num&1)==1)
        {
            ans++;
        }
        num>>=1;
    }
    return ans;
}

int cnt2(int num)//计算长度
{
    int ans=0;
    while(num>1)
    {
        ans++;
        num>>=1;
    }
    return ans;
}

int cnt3(int num)//计算后缀0长度
{
    if(num==0) return 0;
    int ans=0;
    while((num&1)!=1)
    {
        ans++;
        num>>=1;
    }
    return ans;
}

int getone(int num)//计算单个操作次数
{
    if(num==0) return 0;
    return cnt1(num)+cnt2(num);
}

void solve()
{
    int n;
    cin>>n;
    vector<int> a(n);
    for(int i=0;i<n;i++)
    {
        cin>>a[i];
    }
    
    int ans=INF;
    
    for(int k=0;k<=30;k++)//枚举后面0的个数
    {
        int cnt=0;
        int temp=1LL<<k;
        
        for(int i=0;i<n;i++)//赛前处理这个数的次数加上每个数的单独次数
        {
            int minnum=0;
            int yushu=a[i]%temp;
            int beforcnt=0;
            if(yushu!=0)
            {
                minnum=temp-yushu;
            }

            int mincnt=INF;
            for(int ttemp=minnum;ttemp<=minnum+64;ttemp+=temp)
            {
                mincnt=min(mincnt,ttemp+getone(a[i]+ttemp));
            }
            cnt+=mincnt;
        }
        
        cnt-=k*(n-1);//减去一起除以2省下的次数
        ans=min(ans,cnt);
    }
    
    cout<<ans<<"\n";
}
```
