---
title: Codeforces goodbye2016
date: 2016-12-31 15:43:10
tags: dp
categories: Codeforces
---
[cf地址](http://codeforces.com/contest/750)

<!--more-->
退役狗的水平惨不忍睹，goodbye2016打得惨的一逼。  
懒癌发作，前面几题不想写，F也不想写，后面的不会。

### E题
给你一个字符串，然后查询区间，问需要至少需要删掉几个字符使得出现2017的子序列，但不出现2016的子序列。  
先考虑一个简单的问题，只查询一次。那么，这个问题，可以通过一个dp解决，dp[i][5][5]。dp[i][j][k]表示从第一个位置开始，然后从状态i开始匹配到状态j至少需要删掉多少个字符。
![](http://oj1gjp03q.bkt.clouddn.com/blog/Codeforces/cfgoodbye2016E.png-Black)  
状态转移如上图所示。此外，对于状态0，接受一个2之后，可能是到1状态，也可能还保留在0状态，也就是删掉这个字符，那么转移到1状态，用的花费是0（因为没删字符），而保留在0状态，则花费为1（因为删掉了这个字符），0状态对于其他字符，都只能转移到自身，花费为0。对于字符6，比较特殊，注意到状态3和状态4都是不能接受的，在这两个状态下，都必须删掉，所以在状态3，4下，对于字符6，转移的代价是1，而且只能留在自身的状态。其它的自己想清楚就好了。。。  
想清楚这一点后，然后可以发现，其实这个dp，从哪里开始都无所谓。那么，对应区间， 只要做区间上的一个dp。这里用线段树优化，然后搞一下好了。
```cpp
string str;
struct node{
    int rm[5][5];
}T[200010*6];
int match[4] = {2,0,1,7};

node operator + (node A,node B){
    node C;
    for(int i = 0 ; i < 5 ; i ++){
        for(int j = 0;j < 5; j ++){
            C.rm[i][j] = INF;
            for(int k = i ; k <= j; k ++){
                C.rm[i][j] = min(C.rm[i][j], A.rm[i][k] + B.rm[k][j]);
            }
        }
    }
    return C;
}
void build(int l,int r,int cur){
    if(l > r)return;
    if(l == r){
        for(int i = 0 ; i < 5 ; i ++){
            T[cur].rm[i][i] = 0;
            for(int j = i + 1; j < 5 ; j ++){
                T[cur].rm[i][j] = INF;
            }
        }
        int x = str[l] - '0';
        for(int i = 0; i < 4; i ++){
            if(x == match[i]){
                T[cur].rm[i][i] = 1;
                T[cur].rm[i][i + 1] = 0;
            }
        }
        if(x == 6){
            T[cur].rm[3][3] = T[cur].rm[4][4] = 1;
        }
        return;
    }
    int mid = (l + r) >> 1;
    build(l,mid,cur << 1);
    build(mid + 1, r , cur << 1 | 1);
    if(r <= mid)T[cur] = T[cur << 1];
    else T[cur] = T[cur << 1] + T[cur << 1 | 1];
}
node query(int lx,int rx,int l,int r,int cur){
    if(l >= lx && r <= rx)return T[cur];
    int mid = (l + r) >> 1;
    if(rx <= mid)return query(lx,rx,l,mid,cur << 1);
    else if(lx > mid)return query(lx, rx, mid + 1, r, cur << 1 | 1);
    return query(lx,rx,l,mid,cur << 1) + query(lx, rx, mid + 1, r, cur << 1 | 1);
}
int main() {
    ios::sync_with_stdio(0);
    int n,q;
    cin >> n >> q;
    cin >> str;
    build(0,n-1,1);
    int L , R;
    while(q--){
        cin >> L >> R;
        L --;R--;
        node ans = query(L,R,0,n-1,1);
        if(ans.rm[0][4] == INF){
            cout << -1 << endl;
        }else{
            cout << ans.rm[0][4] << endl;
        }
    }
    return 0;
}
```