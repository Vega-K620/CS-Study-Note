# Codeforces Round 1098 (Div. 2)

## A. Marisa Steals Reimu's Takeout
### https://codeforces.com/contest/2228/problem/A
まず0，1，2の数量をメモして、答えは簡単に出てきます。 $ ans=cnt_0+min(cnt_1,cnt_2)+(cnt_1-min(cnt_1,cnt_2))+(cnt_2-min(cnt_1,cnt_2)) $
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

void solve()
{
    int n;
    cin>>n;
    int num[3];
    num[0]=0;
    num[1]=0;
    num[2]=0;
    for(int i=0;i<n;i++)
    {
        int temp;
        cin>>temp;
        num[temp]++;
    }
    int ans=0;
    ans+=num[0];
    int out=min(num[1],num[2]);
    ans+=out;
    num[1]-=out;
    num[2]-=out;
    ans+=num[1]/3;
    ans+=num[2]/3;
    cout<<ans<<"\n";
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
## B. Remilia Plays Soku
### https://codeforces.com/contest/2228/problem/B
〇中の操作ですから、両方の操作回数は注意しなければならない。最後の答えは $ ans=min(n-abs(max(x1,x2)-min(x1,x2)),abs(x1-x2))+k $  
特に:n<=3の時回数は1です。
```cpp
#include<bits/stdc++.h>
using namespace std;
using ll=long long;

void solve()
{
    ll n,x1,x2,k;
    cin>>n>>x1>>x2>>k;
    if(n<=3)
    {
        cout<<1<<"\n";
    }
    else
    {
        ll minnum=1e18;
        minnum=min(n-abs(max(x1,x2)-min(x1,x2)),abs(x1-x2));
        ll ans=minnum+k;
        cout<<ans<<"\n";
    }
}

int main()
{
    cin.tie(nullptr)->sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        solve();
    }
    return 0;
}
```
## C1. Cirno and Number (Easy Version)
### https://codeforces.com/contest/2228/problem/C1
この問題の本質は、「$a$ に近い数 $b$ をいかに効率よく探索するか」です。
大きく分けて3つのケースを考慮する必要があります。  
桁数が減る場合（短くなる）： 全て $D$ の最大値で埋める。  
桁数が増える場合（長くなる）： 全て $D$ の最小値で埋める（先頭が 0 にならないよう注意）。  
同じ桁数の場合： 上位桁から順に「マッチング」を試みる。  
実装のポイント：  
同じ桁数の探索上位桁から $a$ と一致するように数字を選んでいきます。途中で一致しなくなった場合（$a$ のその位置の数字が $D$ に含まれない場合）、「それ以降の桁をどう埋めるか」が重要になります。  
分歧点（Mismatch）の扱い：  
$a$ と一致しなくなった最初の桁で、集合 $D$ に含まれる数字 $d$ を選びます。  
$d$ を選んだ後、残りの桁を「すべて $D$ の最小値」で埋めることで $a$ より小さく、かつ可能な限り大きい値を作れます。  
同様に、「すべて $D$ の最大値」で埋めることで $a$ より大きく、かつ可能な限り小さい値を作れます。  
この「境界付近の候補」を網羅することで、答えを絞り込むことができます。  
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

void solve() {
    string sa;
    ll n;
    cin >> sa >> n;
    vector<ll> num(n);
    for (int i = 0; i < n; i++) cin >> num[i];
    
    ll x = num[0], y = num[1];
    if (x > y) swap(x, y);
    
    ll a = stoll(sa);
    int len = sa.length();
    ll min_diff = 4e18;

    // 情况 1：短一位，全部填最大值 y
    if (len > 1) {
        ll b_short = 0;
        for (int i = 0; i < len - 1; i++) b_short = b_short * 10 + y;
        min_diff = min(min_diff, abs(a - b_short));
    } else if (x == 0) {
        min_diff = min(min_diff, a);
    }

    // 情况 2：长一位，首位尽量小，后续全部填最小值 x
    if (len < 19) {
        ll b_long = (x == 0) ? y : x;
        for (int i = 0; i < len; i++) b_long = b_long * 10 + x;
        min_diff = min(min_diff, abs(a - b_long));
    }

    // 情况 3：相同位数
    for (int i = 0; i <= len; i++) {
        ll prefix = 0;
        bool match = true;
        for (int j = 0; j < i; j++) {
            ll digit = sa[j] - '0';
            if (digit == x || digit == y) {
                prefix = prefix * 10 + digit;
            } else {
                match = false;
                break;
            }
        }
        if (!match) continue;

        if (i == len) {
            min_diff = min(min_diff, abs(a - prefix));
            continue;
        }

        // 核心修正：在分歧点枚举填入 x 或 y，然后分别测试“全 x”和“全 y”后缀
        for (ll d : {x, y}) {
            ll cur = prefix * 10 + d;
            
            ll temp_x = cur, temp_y = cur;
            for (int j = i + 1; j < len; j++) temp_x = temp_x * 10 + x;
            for (int j = i + 1; j < len; j++) temp_y = temp_y * 10 + y;
            
            min_diff = min(min_diff, abs(a - temp_x));
            min_diff = min(min_diff, abs(a - temp_y));
        }
    }

    cout << min_diff << "\n";
}

int main() {
    cin.tie(nullptr)->sync_with_stdio(false);
    int t;
    cin >> t;
    while (t--) solve();
    return 0;
}
```
## C2. Cirno and Number (Hard Version)
### https://codeforces.com/contest/2228/problem/C2
```cpp
#include <bits/stdc++.h>
using namespace std;
using ull = unsigned long long; // 使用无符号避免 19位超大数溢出 UB

// 辅助算绝对值差
ull abs_diff(ull a, ull b) {
    return a > b ? a - b : b - a;
}

void solve() {
    string sa;
    int n;
    cin >> sa >> n;
    vector<ull> num(n);
    for (int i = 0; i < n; i++) cin >> num[i];
    
    ull a = stoull(sa);
    int len = sa.length();
    
    // num 本身题目保证是有序递增的，所以直接取头尾
    ull x = num[0];
    ull y = num[n - 1]; 
    
    // 极端特判：如果集合里只有 0，那么能拼出来的数只能是 0
    if (y == 0) {
        cout << a << "\n";
        return;
    }
    
    ull min_diff = 20e18; // 初始一个极大的差距

    // ================= 情况 1：短一位 =================
    if (len > 1) {
        ull b_short = 0;
        for (int i = 0; i < len - 1; i++) b_short = b_short * 10 + y;
        min_diff = min(min_diff, abs_diff(a, b_short));
    } else if (x == 0) {
        min_diff = min(min_diff, a);
    }

    // ================= 情况 2：长一位 =================
    if (len < 19) {
        // 如果 x 是 0，首位不能是 0，取集合里第二小（肯定是大于0的第一个）
        ull b_long = (x == 0 && n > 1) ? num[1] : x;
        for (int i = 0; i < len; i++) b_long = b_long * 10 + x;
        min_diff = min(min_diff, abs_diff(a, b_long));
    }

    // ================= 情况 3：相同位数 =================
    for (int i = 0; i <= len; i++) {
        ull prefix = 0;
        bool match = true;
        
        // 1. 尝试匹配前 i 位
        for (int j = 0; j < i; j++) {
            ull digit = sa[j] - '0';
            bool found = false;
            for (ull d : num) {
                if (d == digit) { found = true; break; }
            }
            if (found) {
                prefix = prefix * 10 + digit;
            } else {
                match = false; // 前缀都无法凑齐，直接结束
                break;
            }
        }
        if (!match) continue;

        // 2. 如果完美匹配到最后一位
        if (i == len) {
            min_diff = min(min_diff, abs_diff(a, prefix));
            continue;
        }

        // 3. 在分歧点 i，把集合里所有的数字都拿来试探一遍
        for (ull d : num) {
            ull cur = prefix * 10 + d;
            
            ull temp_x = cur; // 后缀全接最小的 x
            ull temp_y = cur; // 后缀全接最大的 y
            
            for (int j = i + 1; j < len; j++) temp_x = temp_x * 10 + x;
            for (int j = i + 1; j < len; j++) temp_y = temp_y * 10 + y;
            
            min_diff = min(min_diff, abs_diff(a, temp_x));
            min_diff = min(min_diff, abs_diff(a, temp_y));
        }
    }

    cout << min_diff << "\n";
}

int main() {
    cin.tie(nullptr)->sync_with_stdio(false);
    int t;
    cin >> t;
    while (t--) solve();
    return 0;
}
```
