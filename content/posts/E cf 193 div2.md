---
title: "[Edu CF193 div2]"
date: 2026-08-07
slug: "CF193"
categories: ["CF题解"]
tags: ["思维,贪心"]
draft: false
---

---
##### https://codeforces.com/contest/2253/problem/A
# A   

一张牌要战胜所有牌,容易想到 一段连续数字中 不可能 两两都保持整除关系        
所以 数值小的肯定会被一个大于它的 打败       
因此这张牌只能是最大的,且它在2- (x-1) 中都没有因子 那就是素数            
即最大的为素数就存在        


<details class="code-collapse">
<summary>代码</summary>



```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;

bool judge(int x){
    for(int i=2;i*i<=x;i++){
        if(x%i==0) return false;
    }
    return true;
}

void solve(){ 
    int n;
    cin>>n;
    if(judge(n+1)) cout<<"YES\n";
    else cout<<"NO\n";
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int ttt=1;
    cin>>ttt;
    while(ttt--){
        solve();
    }   
    return 0;
}
```
</details>

---
##### https://codeforces.com/contest/2253/problem/B
# B 
题意为保留最大交替块长度,在删除基础上 加了一个 相邻可交换的操作   
观察到如果 有两块长度都 >=2   1122  交换一次为 1 2 1 2 贡献增加2    
如果只有一块>=2   x1221x  只要x不与2 相同 交换为 x2121x  贡献增加1     
先 求出原字符串 通过删除 能保留的最大串,再看 贡献能不能增加即可    


<details class="code-collapse">
<summary>代码</summary>



```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;

void solve(){ 
    int n;
    cin>>n;
    vector<int> a(n);
    for(int i=0;i<n;i++){
        cin>>a[i];
    }
    vector<int> b; // 记录每个块
    vector<int> cnt; // 每个块的长度

    b.push_back(a[0]);
    cnt.push_back(1);
    for(int i=1;i<n;i++){
        if(a[i]!=a[i-1]){
            b.push_back(a[i]);
            cnt.push_back(1);
        }
        else cnt.back()++;
    }

    // 只有一块
    if(b.size()==1){
        cout<<1<<"\n"; return ;
    }

    // 考虑贡献
    int add=0;
    for(int i=0;i<b.size()-1;i++){
        if(cnt[i]>=2 && cnt[i+1]>=2){
            add=2;
            break;
        }
    }

    if(!add){
        for(int i=0;i<b.size();i++){
            if(cnt[i]>=2){
                bool tl=false; //向左交换
                bool tr=false; //向右交换
                if(i==1 || ( (i>1) && b[i-2]!=b[i]) ) tl=true;
                if(i==b.size()-2 ||  ( (i<b.size()-2) && b[i]!=b[i+2] ) ) tr=true;
                if( tl || tr ){
                    add=1;
                    break;
                }
            }
        }
    }
    cout<<b.size()+add<<"\n";

}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int ttt=1;
    cin>>ttt;
    while(ttt--){
        solve();
    }   
    return 0;
}
```
</details>

---
##### https://codeforces.com/contest/2253/problem/C
# C 
求所有不同数字和的最大值,容易想到 从大到小往里放    
且操作时任意次数,可以重复的,因此我们只要记录能 放进去 的值就行 不管怎么操作    
考虑 a数组 和 b数组 的大小 只要有一个数组的长度小于 行或列数 我们就能把所有数字放进去    
如果都等于呢 我们肯定要少放一个 因为 有一个一定会被覆盖 我们让这个覆盖的是最小的就行     

...我也不知道解释的对不对,至少赛时过了



<details class="code-collapse">
<summary>代码</summary>


```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;

void solve(){ 
    int n,m,x,y;
    cin>>n>>m>>x>>y;
    vector<ll> a(x);
    vector<ll> b(y);
    for(int i=0;i<x;i++){
        cin>>a[i];
    }
    for(int i=0;i<y;i++){
        cin>>b[i];
    }

    ll total=min(n,x)+min(m,y);
    if(min(n,x)==n && min(m,y)==m) total--;

    reverse(a.begin(),a.end());
    reverse(b.begin(),b.end());

    int cnt=0;
    int cnta=0 , cntb=0;
    int ia=0 , ib=0;
    ll sum=0;
    while(cnt<total && ( ia<x || ib<y ) ){
        if(ia<x && ib<y && a[ia]==b[ib]){
            cnt++;
            sum+=a[ia];
            ia++;ib++;
        }
        else if( ib >= y || ia<x && a[ia]>b[ib] ){
            if(cnta < min(n,x)){
                sum+=a[ia];
                cnta++;
                cnt++;
            }
            ia++;
        }
        else{
            if(cntb < min(m,y)){
                sum+=b[ib];
                cntb++;
                cnt++;
            }
            ib++;
        }
    }
    cout<<sum<<"\n";
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int ttt=1;
    cin>>ttt;
    while(ttt--){
        solve();
    }   
    return 0;
}
```
</details>

---
##### https://codeforces.com/contest/2253/problem/D
# D 
拼尽全力无法战胜...


<details class="code-collapse">
<summary>代码</summary>


```cpp

```
</details>

---