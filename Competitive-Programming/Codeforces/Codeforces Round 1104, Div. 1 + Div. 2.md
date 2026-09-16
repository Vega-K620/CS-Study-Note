# Codeforces Round 1104, Div. 1 + Div. 2
## A. Destroying Towers
### https://codeforces.com/contest/2237/problem/A
左側から一番小さい高さを維持して、配列を走査してもし今の高さは一番小さい高さより高いなら答えに維持している高さを加えて。
```cpp
void solve()
{
    int n,ans=0;
    cin>>n;
    vector<int> num(n);
    for(int i=0;i<n;i++)
    {
        cin>>num[i];
    }
    int now=num[0];
    for(int i=0;i<n;i++)
    {
        if(num[i]>=now)
        {
            ans+=now;
        }
        else
        {
            now=num[i];
            ans+=now;
        }
    }
    cout<<ans<<"\n";
}
```
## B. Annoying the Ghost
### https://codeforces.com/contest/2237/problem/B
この問題 $ n<=2000 $ ですから、 $ O(n^2) $ の方法ができます。配列をｎ回走査して、 $ a[i] $ と $ b[i] $ 大きさを対比して答えを出力します。
```cpp
void solve()
{
    int n;
    cin>>n;
    vector<int> a(n),b(n);
    for(int i=0;i<n;i++)
    {
        cin>>a[i];
    }
    for(int i=0;i<n;i++)
    {
        cin>>b[i];
    }
    bool flag=true;
    for(int i=0;i<n;i++)
    {
        if(a[i]>b[i])
        {
            flag=false;
            break;
        }
    }
    if(flag)
    {
        cout<<0<<"\n";
        return;
    }
    int cnt=0;
    for(int i=0;i<n-1;i++)
    {
        for(int j=0;j<n-1;j++)
        {
            if(a[i]>b[i])
            {
                cnt++;
                swap(a[i],a[i+1]);
            }
        }
    }
    cout<<cnt<<"\n";
}
```
