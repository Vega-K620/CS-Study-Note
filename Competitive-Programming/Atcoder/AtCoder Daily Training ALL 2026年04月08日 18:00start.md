# AtCoder Daily Training ALL 2026/04/08 18:00start
今日のトレーニングはEFを解きました。最初G問題も解きたいですが、動の計画の問題でまだ勉強していませんから、最後できませんでした。
## E - Monotonically Increasing
### https://atcoder.jp/contests/adt_all_20260408_2/tasks/abc263_c
この問題は「DFS」を使って答えが出てきます。でもその中で僕は小さいミスがあります。「DFS」中の「FOR」の開始場所は「0」じゃなくて「最後出力の数字」です。
複雑度: $O(C_M^N \cdot M)$
```cpp
#include<bits/stdc++.h>
using namespace std;
vector<bool> used;
void dfs(int n,int m,int outed)
{
    if(outed==n)
    {
        for(int i=0;i<m;i++)
        {
            if(used[i]) cout<<i+1<<" ";
        }
        cout<<"\n";
        return;
    }
    int last=0;
    for(int i=m-1;i>=0;i--)//ここで、最初間違いました。
    {
        if(used[i])
        {
            last=i+1;
            break;
        }
    }
    for(int i=last;i<m;i++)
    {
        if(!used[i])
        {
            used[i]=true;
            dfs(n,m,outed+1);
            used[i]=false;
        }
    }
}
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    used.assign(m+1, false);

    dfs(n,m,0);
    return 0;
}
```
## F - Insert and Erase A
### https://atcoder.jp/contests/adt_all_20260408_2/tasks/abc447_c
この問題最初僕が使い方法の時間複雑度が高いすきで「TLE」しました。  
正解:  
まず、二つの文字列中の「A」を抜いて、同じなら変換できます。  
そして、「A」じゃないの二つの英大文字の中に「A」の数量を確認して、操作回数は二つの文字列の「A」数量を別々の相差を加えて、答えが出てきます。
複雑度: $ O(N+M) $
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    cin.tie(NULL)->sync_with_stdio(false);
    string s,t;
    cin>>s>>t;
    int ans=0;
    if(s==t)
    {
        cout<<ans;
        return 0;
    }
    string s1="",t1="";
    for(int i=0;i<(int)s.size();i++)
    {
        if(s[i]!='A')
        s1+=s[i];
    }
    for(int i=0;i<(int)t.size();i++)
    {
        if(t[i]!='A')
        t1+=t[i];
    }
    if(s1!=t1)
    {
        cout<<-1<<"\n";
        return 0;
    }
    vector<int> shave((int)t.size()+1,0),thave((int)t.size()+1,0);
    int temp=0;
    for(int i=0;i<(int)s.size();i++)
    {
        if(s[i]=='A') shave[temp]++;
        else 
        {
            temp++;
        }
    }
    temp=0;
    for(int i=0;i<(int)t.size();i++)
    {
        if(t[i]=='A') thave[temp]++;
        else 
        {
            temp++;
        }
    }
    // for(auto i:shave)
    // {
    //     cout<<i<<" ";
    // }
    // cout<<"\n";
    // for(auto i:thave)
    // {
    //     cout<<i<<" ";
    // }
    for(int i=0;i<(int)shave.size();i++)
    {
        ans+=abs(shave[i]-thave[i]);
    }
    cout<<ans<<"\n";
    return 0;
}
```
