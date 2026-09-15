# Codeforces Round 1120 (Div. 2)
## A. Min Max Game
### https://codeforces.com/contest/2263/problem/A
この問題は「1」と「0」の数量をメモして、 $ num_1>=num_0 $ の時Bessieの勝ち。
```cpp
void solve()
{
    int n;
    cin>>n;
    int cnt0=0,cnt1=0;
    for(int i=0;i<n;i++)
    {
        int num;
        cin>>num;
        if(num==0)cnt0++;
        else cnt1++;
    }
    if(cnt1>=cnt0)cout<<"Bessie"<<"\n";
    else cout<<"Elsie"<<"\n";
}
```
## B. Min Matrices
### https://codeforces.com/contest/2263/problem/B
できる範囲は $ n<=num<=2*n-1 $ 、  
そして小さい数字からコーナーから入って、  
もし対角線なら使用可能な位置は二つしようされる、  
じゃないと一つしようされる。  
ｎ行列に使用可能の数量は $ 2*n-1 $ 、対角線に入る数量は $ 2*n-k $ 、  
まず対角線に入って他のは使用しません行と列に入ります。
```cpp
void solve()
{
    int n,k;
    cin>>n>>k;
    if(n>k||2*n<=k)
    {
        cout<<-1<<"\n";
        return;
    }
    vector<vector<int>> excel(n,vector<int>(n,-1));
    int freec=0,freer=0;
    int cnt=1,db=2*n-k;
    for(int i=0;i<db;i++)
    {
        excel[i][i]=cnt++;
        freec++;
        freer++;
    }
    for(int i=freec;i<n;i++)
    {
        excel[i][0]=cnt++;
    }
    for(int i=freer;i<n;i++)
    {
        excel[0][i]=cnt++;
    }
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<n;j++)
        {
            if(excel[i][j]==-1)
            excel[i][j]=cnt++;
        }
    }
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<n;j++)
        {
            cout<<excel[i][j]<<" ";
        }
        cout<<"\n";
    }
}
```
## C1. Floor of MEX (Easy Version)
### https://codeforces.com/contest/2263/problem/C1
$ f(S,x)=\text{mex}\left(\left\{\left\lfloor \frac{y}{x}\right\rfloor : y\in S\right\}\right),  $ から $ a[i]*i $から $ a[i]+1*i $ までの数字は悪い数字です、使えるません。これ以外の数字を選んで答えです。
```cpp
void solve()
{
    int n;
    cin>>n;
    vector<int> a(n+1);
    for(int i=1;i<=n;i++)
    {
        cin>>a[i];
    }
    vector<pair<int,int>> bad;
    for(int i=1;i<=n;i++)
    {
        bad.push_back({a[i]*i,(a[i]+1)*i-1});
    }
    vector<int> ans,d(n+1);
    for(auto i:bad)
    {
        d[min(n,i.first)]++;
        d[min(n,i.second+1)]--;
    }
    int temp=0;
    for(int i=0;i<n;i++)
    {
        temp+=d[i];
        if(temp==0)ans.push_back(i);
    }
    cout<<(int)ans.size()<<"\n";
    for(int i=0;i<ans.size();i++)
    {
        cout<<ans[i]<<" ";
    }
    cout<<"\n";
}

```
