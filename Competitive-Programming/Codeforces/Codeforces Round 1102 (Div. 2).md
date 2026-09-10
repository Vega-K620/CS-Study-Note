# Codeforces Round 1102 (Div. 2)
## A. Euclid, Sequence and Two Numbers
### https://codeforces.com/contest/2234/problem/A
この問題の要点は、もしソートできるなら、一番大きいの二つの数字はそれの始まり。
ですので、一番大きいの二つの数字で正しい配列を計算して、それを元々の配列と比べて判断できます。
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

bool cmp(int a,int b)
{
    return a>b;
}

void solve()
{
    int n;
    cin>>n;
    vector<int> num(n);
    for(int i=0;i<n;i++)
    {
        cin>>num[i];
    }
    sort(num.begin(),num.end(),cmp);
    vector<int> ans;
    ans.push_back(num[0]);
    ans.push_back(num[1]);

    for(int i=2;i<n;i++)
    {
        int temp=ans[i-2]%ans[i-1];
        ans.push_back(temp);
    }

    if(ans==num)
    {
        cout<<ans[0]<<" "<<ans[1]<<"\n";
    }
    else
    {
        cout<<-1<<"\n";
    }
}
signed main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int t=1;
    cin>>t;
    while(t--)
    {
        solve();
    }
}
```
## B. Palindrome, Twelve and Two Terms
### https://codeforces.com/contest/2234/problem/B
12以下「palindrome 」を満足できないのはただの「10」です。
ですから、 $ a=n Mod 12 $ して、
もし「a」は「10」なら「22」に変えて、
それをして「b」はまだ「12」の複数です。 
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long

void solve()
{
    int n;
    cin>>n;
    if(n==10)
    {
        cout<<-1<<"\n";
    }
    else
    {
        int a,b;
        a=n%12;
        if(a==10)
        {
            a=22;
            b=n-22;
        }
        else
        {
            b=n-a;
        }
        cout<<a<<" "<<b<<"\n";
    }
}

signed main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        solve();
    }
}
```
## C. Vessels, Heights and Two Versions (Easy Version)
### https://codeforces.com/contest/2234/problem/C
この問題はまだ理解できないので
```cpp
#include <bits/stdc++.h>
using namespace std;

void solve()
{
    int n;
    cin>>n;
    vector<int> h(n);
    for(int i=0;i<n;i++)
    {
        cin>>h[i];
    }
    vector<int> double_h(2*n);
    for(int i=0;i<2*n;i++)
    {
        double_h[i]=h[i%n];
    }

    for(int s=0;s<n;s++)
    {
        vector<int> w1(n,0),w2(n,0);

        int current_max=0;
        for(int i=0;i<n-1;i++)
        {
            int from_idx=s+i;
            int target_idx=(s+i+1)%n;
            
            current_max = max(current_max, double_h[from_idx]);
            w1[target_idx] = current_max;
        }

        current_max = 0;
        for (int i = 0; i < n - 1; i++) {
            int from_idx = s + n - i;
            int target_idx = (s + n - i - 1) % n;
            
            current_max = max(current_max, double_h[from_idx - 1]);
            w2[target_idx] = current_max;
        }

        long long total_water = 0;
        for(int i=0;i<n;i++)
        {
            if(i==s)continue;
            total_water+=min(w1[i],w2[i]);
        }
        cout<<total_water<<" ";
    }
    cout<<"\n";
}

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        solve();
    }
    return 0;
}
```
