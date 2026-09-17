# Codeforces Round 1107 (Div. 3)
## A. Divide and Conquer
### https://codeforces.com/contest/2241/problem/A
この問題は $ x MOD y=0 $ の時「Yes」を出力します。
```cpp
void solve()
{
    int x,y;
    cin>>x>>y;
    if(x%y==0)
    cout<<"Yes"<<"\n";
    else
    cout<<"No"<<"\n";
}
```
## B. Good times Good times
### https://codeforces.com/contest/2241/problem/B
yとx*yがいい数字です、そしてｘもいい数字ので、簡単の方法は $ x * y = x * x_length + x $ $ y = 10 ^ x_length + 1 $ 
```cpp
int getpow(int a,int b)
{
    int ans=1;
    for(int i=0;i<b;i++)
    {
        ans*=a;
    }
    return ans;
}

void solve()
{
    string x;
    cin>>x;
    cout<<getpow(10,x.size())+1<<"\n";
}
```
## C. RemovevomeR
### https://codeforces.com/contest/2241/problem/C
この問題の結果はただ「1」と「2」です。もし配列の中でただ「0」あるいは「1」なら、答えは「1」です。他の状況は「2」です。
```cpp
void solve()
{
    int n;
    cin>>n;
    string str;
    cin>>str;
    int cnt=0;
    for(int i=0;i<n-1;i++)
    {
        if(str[i]!=str[i+1])cnt++;
    }
    if(cnt==1)cout<<2<<"\n";
    else cout<<1<<"\n";
}
```
## D. An Alternative Way
### https://codeforces.com/contest/2241/problem/D
この問題の操作は二種類あります。一つ目はこの数字に1を加える、二つ目はこの数字に1を減ってまえの数字に1を加えて。ですからa配列の累積和はb配列の累積和より小さいの時b配列になれる。
```cpp
void solve()
{
    int n;
    cin>>n;
    vector<int> num1(n),num2(n);
    for(int i=0;i<n;i++)
    {
        cin>>num1[i];
    }
    for(int i=0;i<n;i++)
    {
        cin>>num2[i];
    }
    int check=0;
    for(int i=0;i<n;i++)
    {
        // if(num1[i]==num2[i])check=0;
        if(num1[i]<num2[i])check+=abs(num1[i]-num2[i]);
        if(num1[i]>num2[i])check-=abs(num1[i]-num2[i]);
        if(check<0)break;
    }
    if(check>=0)cout<<"Yes"<<"\n";
    else cout<<"No"<<"\n";
}
```
