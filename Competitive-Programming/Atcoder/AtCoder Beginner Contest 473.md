# AtCoder Beginner Contest 473
## A - Second Half Sum
### https://atcoder.jp/contests/abc473/tasks/abc473_a
この問題はチェックイン問題から、スキップする。
```cpp
void solve()
{
    int n;
    cin>>n;
    vector<int> num(n+1);
    for(int i=1;i<=n;i++)
    {
        cin>>num[i];
    }
    int sum=0;
    for(int i=n/2+1;i<=n;i++)
    {
        sum+=num[i];
    }
    cout<<sum<<"\n";
}
```
## B - Old Maid
### https://atcoder.jp/contests/abc473/tasks/abc473_b
この問題は数字の回数をメモして、奇の数字を加えて答えです。
```cpp
void solve()
{
    int n;
    cin>>n;
    map<int,int> mp;

    for(int i=1;i<=n;i++)
    {
        int num;
        cin>>num;
        mp[num]++;
    }
    int ans=0;
    for(auto i:mp)
    {
        if(i.second%2!=0)
        {
            ans+=i.first;
        }
    }
    cout<<ans<<"\n";
}
```
## C - Change Schools
### https://atcoder.jp/contests/abc473/tasks/abc473_c
この問題はまずクラスメートの回数をメモして、一番多いのもメモして、 $ bigest<=num[i]+1&&num[i]!=0 $ の時答え+1。
```cpp
void solve()
{
    int n,k;
    cin>>n>>k;
    vector<int> num(k+1,0);
    int bigest=-1;
    for(int i=1;i<=n;i++)
    {
        int temp;
        cin>>temp;
        num[temp]++;
        bigest=max(num[temp],bigest);
    }
    debug(num);
    debug(bigest);
    int cnt=0;
    for(int i=1;i<=k;i++)
    {
        if(bigest<=num[i]+1&&num[i]!=0)
        {
            cnt++;
        }
    }
    cout<<cnt<<"\n";
}
```
## D - Coefficient Stair
### https://atcoder.jp/contests/abc473/tasks/abc473_d
この問題のポイントは「dfs」を使って、後ろから書きます。
簡単に答えがでてきます。
```cpp
int n,k;
vector<vector<int>> ans;
vector<int> num;

void dfs(int last,int now)
{
    if(now==1)
    {
        num[1]=last;
        ans.push_back(num);
        return;
    }
    for(int i=last/now;i>=0;i--)
    {
        num[now]=i;
        if((last-now*i)>=0)
        dfs(last-now*i,now-1);
    }
}

void solve()
{
    cin>>n>>k;
    num.clear();
    num.resize(n+1);
    dfs(k,n);
    sort(ans.begin(),ans.end());
    for(int i=0;i<ans.size();i++)
    {
        for(int j=1;j<=n;j++)
        {
            cout<<ans[i][j]<<" ";
        }
        cout<<"\n";
    }
}
```
