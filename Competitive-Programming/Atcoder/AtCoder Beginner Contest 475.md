# AtCoder Beginner Contest 
## A - mnclr
### https://atcoder.jp/contests/abc475/tasks/abc475_a
チェックイン問題のでスキップします。
```cpp
void solve()
{
    string str;
    cin>>str;
    for(int i=0;i<str.size();i++)
    {
        cout<<str[i];
        if(i!=str.size()-1)cout<<'o';
    }
    cout<<"\n";
}
```
## B - Change
### https://atcoder.jp/contests/abc475/tasks/abc475_b
この問題のポイントは、硬貨の数量は増えるばかりです。  
毎回手に入れる硬貨をメモして、答えがでてきます。
```cpp
const int INF=1e18;

void solve()
{
    int n;
    cin>>n;
    int sum=0;
    int a=0,b=0,c=0;
    for(int i=0;i<n;i++)
    {
        int num;
        cin>>num;
        sum=(1000-(num%1000))%1000;
        a+=sum/100;
        b+=(sum%100)/10;
        c+=((sum%100)%10);
    }
    cout<<c<<" "<<b<<" "<<a<<"\n";
}
```
## C - Walk the Line
###https://atcoder.jp/contests/abc475/tasks/abc475_c
この問題は「街 S も訪れた街に含め」と言いました、ですから、  
S から左側と右側の距離を別々で計算して、  
そして「 N<=8000 」、ですから全ての状況を計算して、  
その中で一番大きいのは答えです。
```cpp
void solve()
{
    int n,s,l;
    cin>>n>>s>>l;
    vector<int> num(n+1);
    for(int i=1;i<=n-1;i++)
    {
        cin>>num[i];
    }
    vector<int> sum(n+1);
    sum[s]=0;
    for(int i=s-1;i>=1;i--)
    {
        sum[i]=sum[i+1]+num[i];
    }
    for(int i=s+1;i<=n;i++)
    {
        sum[i]=sum[i-1]+num[i-1];
    }
    int ans=1;
    for(int i=1;i<=s;i++)
    {
        for(int j=s;j<=n;j++)
        {
            int cost=sum[i]+sum[j]+min(sum[i],sum[j]);
            if(cost<=l)
            {
                ans=max(ans,j-i+1);
            }
        }
    }
    cout<<ans<<"\n";
}
```
## D - Alphametic Prime
### https://atcoder.jp/contests/abc475/tasks/abc475_d
この問題は全配列の問題です。  
N 以下全ての素数をチェックして。  
簡単に答えが見つけます。
```cpp
bool check(const string& s,const string& t)
{
    if(s.size()!=t.size()) return false;

    vector<int> s_to_t(26,-1);
    vector<int> t_to_s(10,-1);

    for(int i=0;i<(int)s.size();i++)
    {
        int u=s[i]-'a';
        int v=t[i]-'0';

        if(s_to_t[u]!=-1&&s_to_t[u]!=v) return false;
        if(t_to_s[v]!=-1&&t_to_s[v]!=u) return false;

        s_to_t[u]=v;
        t_to_s[v]=u;
    }
    return true;
}

void solve()
{
    string s;
    cin>>s;

    int len=s.size();
    int MAXN=1;
    for(int i=0;i<len;i++) MAXN*=10;

    vector<bool> is_prime(MAXN,true);
    is_prime[0]=is_prime[1]=false;
    for(int i=2;i*i<MAXN;i++)
    {
        if(is_prime[i])
        {
            for(int j=i*i;j<MAXN;j+=i)
            {
                is_prime[j]=false;
            }
        }
    }

    int MINN=(len==1)?2:MAXN/10;

    for(int p=MINN;p<MAXN;p++)
    {
        if(is_prime[p])
        {
            string t=to_string(p);
            if(check(s,t))
            {
                cout<<p<<"\n";
                return;
            }
        }
    }
    cout<<-1<<"\n";
}
```
