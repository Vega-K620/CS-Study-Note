# AtCoder Daily Training ALL 2026/05/19 16:00start
## A - Online Shopping
### https://atcoder.jp/contests/adt_all_20260519_1/tasks/abc332_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,s,k;
    cin>>n>>s>>k;
    vector<pair<int,int>> p(n);
    for(int i=0;i<n;i++)
    {
        cin>>p[i].first>>p[i].second;
    }
    ll sum=0;
    for(int i=0;i<n;i++)
    {
        sum+=p[i].first*p[i].second;
    }
    if(sum<s)
    {
        sum+=k;
    }
    cout<<sum<<'\n';
    return 0;
}
```
## B - Odd Position Sum
### https://atcoder.jp/contests/adt_all_20260519_1/tasks/abc403_a
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    int sum=0;
    for(int i=1;i<=n;i++)
    {
        int temp;
        cin>>temp;
        if(i%2==1)
        sum+=temp;
    }
    cout<<sum<<"\n";
    return 0;
}
```
## C - AtCoder Janken 2
### https://atcoder.jp/contests/adt_all_20260519_1/tasks/abc354_b
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

bool cmp(pair<string,int> a,pair<string,int> b)
{
    return a.first<b.first;
}
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n;
    cin>>n;
    vector<pair<string,int>> name(n);
    int sum=0;
    for(int i=0;i<n;i++)
    {
        cin>>name[i].first>>name[i].second;
        sum+=name[i].second;
    }
    sort(name.begin(),name.end(),cmp);
    
    int out=sum%n;
    cout<<name[out].first<<"\n";
    return 0;
}
```
## D - Default Price
### https://atcoder.jp/contests/adt_all_20260519_1/tasks/abc308_b
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    vector<string> c(n+1);
    unordered_map<string,int> price;
    for(int i=1;i<=n;i++)
    {
        cin>>c[i];
    }
    vector<pair<string,int>> temp(m+1);
    for(int i=1;i<=m;i++)
    {
        cin>>temp[i].first;
    }
    int other;
    cin>>other;
    for(int i=1;i<=m;i++)
    {
        cin>>temp[i].second;
    }
    for(int i=1;i<=m;i++)
    {
        price[temp[i].first]=temp[i].second;
    }
    ll sum=0;
    for(int i=1;i<=n;i++)
    {
        if(price[c[i]]!=0)
        {
            sum+=price[c[i]];
        }
        else
        sum+=other;
    }
    cout<<sum<<"\n";
    return 0;
}
```
## E - Snake Numbers
### https://atcoder.jp/contests/adt_all_20260519_1/tasks/abc387_c
```cpp
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

// 计算 base 的 exp 次方
long long power(long long base, int exp) {
    long long res = 1;
    for (int i = 0; i < exp; i++) res *= base;
    return res;
}

// 计算在最高位为 t 的限制下，小于等于 N 的蛇数个数
long long count_with_top(long long n, int t) {
    string s = to_string(n);
    int len = s.length();
    
    // 如果 N 只有 1 位数，且我们强制最高位是 t，这不可能构成 >= 10 的蛇数
    if (len < 2) return 0; 

    long long count = 0;

    // 1. 先把所有位数比 N 短的、最高位为 t 的蛇数直接算出来
    // 比如 N 是 4 位数，那 2 位数和 3 位数中最高位为 t 的蛇数，直接公式计算
    for (int i = 2; i < len; i++) {
        count += power(t, i - 1);
    }

    // 2. 接下来处理和 N 位数相同的情况，开始纯粹的数位遍历
    int first_limit = s[0] - '0'; // N 的最高位限制
    
    if (t < first_limit) {
        // 如果我们假设的最高位 t 比 N 的最高位还要小
        // 那后面所有位都可以放飞自我，在 0 ~ t-1 里随便填
        count += power(t, len - 1);
    } 
    else if (t == first_limit) {
        // 如果 t 恰好等于 N 的最高位，我们需要从左到右精确遍历 N 的每一个数位
        bool reached_end = true;
        
        for (int i = 1; i < len; i++) {
            int limit = s[i] - '0'; // N 当前数位的数字
            
            // 当前位能够填入的数字，最大不能超过 limit，同时必须严格小于最高位 t
            int max_val = min(limit, t - 1);
            
            // 数位遍历核心：尝试在当前位填一个比 limit 小的数 d
            // 一旦填了比 limit 小的数，后面剩下的位就全部自由了
            for (int d = 0; d < max_val; d++) {
                int remaining_digits = len - 1 - i;
                count += power(t, remaining_digits);
            }
            
            // 如果 N 的这一位数字本身就大于等于 t
            // 说明我们这一位不能填 limit（因为蛇数要求必须 < t）
            if (limit >= t) {
                // 我们只能最大填到 t-1。如果填了 t-1，由于 t-1 < limit，后面也直接自由了
                int remaining_digits = len - 1 - i;
                count += power(t, remaining_digits);
                
                reached_end = false; // 被挡住了，无法一路保持和 N 完全一致
                break;               // 后面不需要再遍历了
            }
        }
        
        // 如果成功遍历完了所有数位，说明 N 本身就是个合法的蛇数，把它自己加上
        if (reached_end) {
            count++;
        }
    }
    // 如果 t > first_limit，说明最高位超出了 N 的最高位，个数为 0，不作处理

    return count;
}

long long solve(long long n) {
    if (n < 10) return 0;
    
    long long total = 0;
    // 在外层直接遍历最高位
    for (int t = 1; t <= 9; t++) {
        total += count_with_top(n, t);
    }
    return total;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    long long L, R;
    if (cin >> L >> R) {
        cout << solve(R) - solve(L - 1) << "\n";
    }
    return 0;
}
```
