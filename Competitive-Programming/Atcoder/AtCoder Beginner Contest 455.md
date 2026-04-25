# AtCoder Beginner Contest 454

## A - 455
### https://atcoder.jp/contests/abc455/tasks/abc455_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int a,b,c;
    cin>>a>>b>>c;
    if(a!=b&&b==c)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
    return 0;
}
```
## B - Spiral Galaxy
### https://atcoder.jp/contests/abc455/tasks/abc455_b
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int h,w;
    cin>>h>>w;
    vector<vector<char>> excel(h);
    for(int i=0;i<h;i++)
    {
        for(int j=0;j<w;j++)
        {
            char c;
            cin>>c;
            excel[i].push_back(c);
        }
    }
    int ans=0;
    for(int zuo=0;zuo<w;zuo++)
    {
        for(int you=zuo;you<w;you++)
        {
            for(int shang=0;shang<h;shang++)
            {
                for(int xia=shang;xia<h;xia++)
                {
                    bool flag=true;
                    for(int i=shang;i<=xia;i++)
                    {
                        for(int j=zuo;j<=you;j++)
                        {
                            int ni=shang+xia-i;
                            int nj=zuo+you-j;
                            if(excel[i][j]!=excel[ni][nj])
                            {
                                flag=false;
                                break;
                            }
                        }
                        if(!flag)
                        break;
                    }
                    if(flag)ans++;
                }
            }
        }
    }
    cout<<ans<<"\n";
    return 0;
}
```
## C - Vanish
### https://atcoder.jp/contests/abc455/tasks/abc455_c
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,k;
    cin>>n>>k;
    vector<int> num(n);
    map<ll,int> check;
    priority_queue<ll> ans;
    ll sum=0;
    for(int i=0;i<n;i++)
    {
        cin>>num[i];
        sum+=num[i];
        if(check.count(num[i])!=0)
        check[num[i]]++;
        else
        check[num[i]]=1;
    }
    for(auto i:check)
    {
        ans.push(i.first*i.second);
    }
    for(int i=0;i<k;i++)
    {
        if(!ans.empty())
        {
            sum-=ans.top();
            ans.pop();
        }
    }
    cout<<sum<<"\n";
    return 0;
}
```
## D - Card Pile Query
### https://atcoder.jp/contests/abc455/tasks/abc455_d
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);

    int N,Q;
    cin>>N>>Q;

    vector<int> up(N+1,0);
    vector<int> down(N+1,0);

    for(int i=0;i<Q;i++)
    {
        int c,p;
        cin>>c>>p;
        if(down[c]!=0)
        {
            up[down[c]]=0; 
        }
        down[c]=p;
        up[p]=c;
    }

    vector<int> ans(N+1,0);
    
    for(int i=1;i<=N;i++)
    {
        if(down[i]==0)
        {
            int count=0;
            int curr=i;
            while(curr!=0)
            {
                count++;
                curr=up[curr];
            }
            ans[i]=count;
        }
    }

    for(int i=1;i<=N;i++)
    {
        cout<<ans[i]<<(i==N?"\n":" ");
    }

    return 0;
}
```
## E - Unbalanced ABC Substrings
### https://atcoder.jp/contests/abc455/tasks/abc455_e
```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);

    long long n;
    string s;
    cin >> n >> s;

    map<int, int> mAB, mBC, mCA;
    map<pair<int, int>, int> mABC;

    int a = 0, b = 0, c = 0;
    mAB[0]++; mBC[0]++; mCA[0]++;
    mABC[{0, 0}]++;

    for (char ch : s) {
        if (ch == 'A') a++;
        else if (ch == 'B') b++;
        else c++;

        mAB[a - b]++;
        mBC[b - c]++;
        mCA[c - a]++;
        mABC[{a - b, b - c}]++;
    }

    long long total = n * (n + 1) / 2;
    long long sAB = 0, sBC = 0, sCA = 0, sABC = 0;

    for (auto const& [val, count] : mAB) sAB += (long long)count * (count - 1) / 2;
    for (auto const& [val, count] : mBC) sBC += (long long)count * (count - 1) / 2;
    for (auto const& [val, count] : mCA) sCA += (long long)count * (count - 1) / 2;
    for (auto const& [val, count] : mABC) sABC += (long long)count * (count - 1) / 2;

    cout << total - (sAB + sBC + sCA) + 2 * sABC << endl;
    return 0;
}
```
