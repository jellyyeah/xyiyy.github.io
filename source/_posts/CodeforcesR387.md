---
title: Codeforces Round 387 (Div. 2)
date: 2016-12-19 18:44:36
tags: dp
categories: Codeforces
---


[cf入口地址](http://codeforces.com/contest/747)
<!--more-->
### A题  
给你一个整数n，\\( 0 \leq n \leq 10^6 \\)，要你求出a，b，使得\\( a \leq b \\)，并且a，b的差值最小  
解法： 暴力枚举

### B题  
给你一个只包含A，G，C，T和？的字符串str，问你能否通过将？变成A，G，C，T使得字符串中只出现A，G，C，T并且四种字符出现的次数相同。  
解法：当前仅当，\\( len(str) % 4 == 0\\) 并且当前A，G，C，T的个数，均小于等于\\(len(str) / 4\\)才有解，随便替换到满足条件即可

### C题
有n台机器，每个机器的编号为1到n，和q个工作任务(\\(1 \leq n \leq 100,1\leq q \leq 10^5\\))，每个任务都有对应的请求时间，需要工作的时间，以及需要占用的机器数，并且每次占用机器会优先占用编号小的机器，所有任务的请求时间都不相同  
然后要你求每次任务所占用的机器的编号（如果当前剩下的机器不够请求，则输出-1）。
解法：暴力\\(O(n*q)\\)

### D题
有n天，每天都对应一个温度，然后有两种轮胎，A轮胎只能用于大于等于0度的时候，B轮胎随便什么温度都能用，但是最多只能用k天。一开始汽车上装的是A轮胎，问最少要换多少次轮胎。\\(1 \leq n \leq 2*10^5,1 \leq k \leq n\\)  
解法：贪心。  
首先，对于温度低于0度的那几天，必须使用B轮胎，如果不够用，那就不合法，否则，能够得到一种相对较坏的安排。  
那么接下来，我需要尽量减少换轮胎的次数。可以看到，这n天被划分成了若干个正数段和负数段，对于每一个正数段，如果我全部使用B轮胎，就会使得换轮胎的次数减少2.那么，我只需要尽量取尽可能多的正数段来使用B轮胎，即可。这是一个很典型的贪心，只要优先取长度最少的正数段来使用B轮胎即可。  
这里还有两个点需要注意一下，那就是如果这些段是以正数段开头的，那么第一个正数段是不会需要直接可以用一开始的A轮胎的，所以不需要去用B轮胎。同时，还有如果是以正数段作为最后一段的，那么如果将对最后一段使用B轮胎之后，不需要再换回来，所以在这段使用B轮胎，换轮胎的次数只减少了1.  
所以，当最后一段是正数段的时候，只要枚举其是否用B轮胎替换。然后贪心就好了。  
复杂度\\(O(nlog(n))\\)
```cpp
//  main.cpp
//  codeforces
//
//  Created by Yi Xu on 12/19/16.
//  Copyright © 2016 Yi Xu. All rights reserved.
//
int a[200010];
priority_queue<int,vector<int>,greater<int> >q;
priority_queue<int,vector<int>,greater<int> >q2;
int main() {
    ios::sync_with_stdio(0);
    int n,m;
    cin >> n >> m;
    int cur = 0;
    int f = 1;
    int ans = 0;
    for(int i = 0; i< n;i++){
        cin >> a[i];
        if(a[i] >= 0 && f){
            cur++;
        }else if(a[i] < 0 && f == 0){
            m--;
        }else if(a[i] >= 0 && f == 0){
            cur = 1;
            ans++;
            f = 1;
        }else if(a[i] < 0 && f){
            ans++;
            if(i == cur);
            else q.push(cur),q2.push(cur);
            m--;
            f= 0;
        }
    }
    bool gao = 0;
    if(f){
        if(cur != n)gao = 1;
    }
    if(m >= 0){
        int tmp = m;
        int ret = ans;
        if(gao && m >= cur){
            m -= cur;
            ret--;
            while(!q2.empty() && m >= q2.top()){
                m -= q2.top();
                q2.pop();
                ret -= 2;
            }
        }
        m = tmp;
        while(!q.empty() && m >= q.top()){
            m -= q.top();
            q.pop();
            ans -= 2;
        }
        ans = min(ans,ret);
        cout << ans << endl;
    }else{
        cout << -1 << endl;
    }
    return 0;
}
```

### E题
模拟题，没啥好说的

```cpp
//  main.cpp
//  codeforces
//
//  Created by Yi Xu on 12/19/16.
//  Copyright © 2016 Yi Xu. All rights reserved.
//
string str;
vector<char>G[500010];
stack<PII> s;
int main() {
    ios::sync_with_stdio(0);
    cin >> str;
    string tmp = "";
    int now = 0;
    int maxx = 0;
    PII cur = mp(INF,0);
    int ans = 0;
    str = str + ",";
    for(int i = 0 ; i < str.length() ; i ++){
        if(str[i] == ','){
            if(now == 0){
                maxx = 0;
            }else{
                G[cur.second].push_back(' ');
                if(maxx > 0){
                    s.push(cur);
                    cur.first = maxx;
                    cur.second ++;
                    ans = max(ans,cur.second);
                }else{
                    cur.first --;
                    while(cur.first == 0 && !s.empty()){
                        cur = s.top();
                        s.pop();
                        cur.first--;
                    }
                }
            }
            now = now ^ 1;
            continue;
        }
        if(now == 0){
            G[cur.second].push_back(str[i]);
        }else{
            maxx = maxx * 10 + str[i] - '0';
        }

    }
    cout << ans + 1 << endl;
    for(int i = 0 ; i <= ans; i ++){
        for(int j = 0 ; j < G[i].size(); j++){
            cout << G[i][j];
        }
        cout << endl;
    }
    return 0;
}
```

### F题
让你求十六进制下每个数字使用次数不超过t次的第k小的数（0不算，即从1开始计数）\\(1 \leq k \leq 2 \cdot  10^9,1 \leq t \leq 10\\)
解法：先枚举位数,然后从1到F枚举最高位x（因为最高不能为0），在枚举了最高位之后，后面的位数在每个数只能限定的次数的情况下，能够构成多少数字，如果发现长度为len，最高位为x时，能构成的数加上最小位小于x或者长度小于len，小于这种的数位cnt个的时候，那说明最高位你已经找对了。  
然后对于剩下的低位，只要从0到F枚举，然后重复之前的方法即可。  
复杂度为\\(O(16^4 \cdot t^3) \\)，但由于 \\(k \leq 2 \cdot  10^9\\)，所以并不会达到这么大的范围
```cpp
//  main.cpp
//  codeforces
//
//  Created by Yi Xu on 12/19/16.
//  Copyright © 2016 Yi Xu. All rights reserved.
//
int can[110];
ll C[170][11];
vector<ll> dp(170);
const ll maxx = 2e9 + 7;
ll DP(int n){
    for(int i = 0; i <= n; i++)dp[i] = 0;
    dp[0] = 1;
    for(int i = 0 ; i < 16 ; i ++){
        for(int len = n ; len >= 0; len --){
            for(int j = 1 ; j <= can[i]; j ++){
                dp[len + j] += dp[len] * min(maxx,C[len + j][j]);
                dp[len + j] = min(dp[len + j],maxx);
            }
        }
    }
    return dp[n];
}
void output(int p){
    if(p < 10)cout << p;
    else cout << char(p - 10 + 'a');
}
void dfs(int n,int p,ll k){
    output(p);
    if(!n){
        cout << endl;
        return;
    }
    for(int i = 0; i < 16 ; i ++){
        if(can[i] == 0)continue;
        can[i]--;
        ll cnt = DP(n - 1);
        if(cnt >= k){
            dfs(n - 1, i, k);
            return;
        }
        k -= cnt;
        can[i]++;
    }
}
int main() {
    ios::sync_with_stdio(0);
    ll k;
    int t;
    for(int i = 0 ; i < 170 ; i ++){
        C[i][0] = 1;
        for(int j = 1;  j <= min(i,10) ; j ++){
            C[i][j] = C[i - 1][j] + C[i - 1][j - 1];
            C[i][j] = min(C[i][j],maxx);
        }
    }
    cin >> k >> t;
    for(int i = 0 ; i < 16 ; i++){
        can[i] = t;
    }
    for(int len = 1 ; ; len ++){
        for(int i = 1; i < 16 ; i ++){
            can[i]--;
            ll cnt = DP(len - 1);
            if(cnt >= k){
                dfs(len - 1,i,k);
                return 0;
            }
            k-= cnt;
            can[i]++;
        }
    }
    return 0;
}
```
