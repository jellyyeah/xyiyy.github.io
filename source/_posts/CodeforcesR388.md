---
title: CodeforcesR388
date: 2016-12-21 13:21:56
tags:
	- Fenwick tree
categories: Codeforces
---

[cf入口地址](http://codeforces.com/contest/749)
<!--more-->
### A题
给你一个数，问你最多能由几个质数构成
解法：偶数全部用2，奇数先扣掉一个3，变成偶数，然后用2

### B题
给你平面上三个整点坐标，然后让你求第四个点的坐标使他们构成平行四边形
解法：设A，B为相对顶点，C，D为相对顶点，则\\((A + B) ／ 2 = (C + D) ／ 2\\)即可求出第四个点

### C题
模拟加贪心，不想说题面。
总共两类人，A类和B类，分别一个队列，然后每次拿较小的一个数，将其加n，重新放入队列，另一个队列的首部直接删掉。

### D题
有n个人竞价，进行了n轮的报价，价最高者得。对于这n轮报价，有q次查询，问删掉指定k个人的报价之后，哪个人能竞到价，并且出的价格是多少，如果某个人已经成功竞价，就不会继续报价，即不会对自己出的价格再提升竞价。\\(1 \leq n,q \leq 200000, \sum k \leq 200000 \\)
解法：按照每个人能给出的最高竞价，从高到低排个序，然后，举例来说，对于样例1，能够得到3，2，1（4，5，6）的顺序，括号里的表示没参与，接着，删掉3，那样，刚才排好的序列就变成2，1（4，5，6），那么答案的标号就是剩下序列的第一个数，这个人要出的价只要是他出的价格里面，大于这个第二个的最高竞价就好。$$O(qlogn + \sum k)$$  
```cpp
typedef pair<int,int> PII;
vector<int> G[200010];
PII a[200010];
int dp[200010];
bool vis[200010];
int main() {
    ios::sync_with_stdio(0);
    int n;
    cin >> n;
    int x,y;
    for(int i = 0; i <= n; i ++)G[i].push_back(0);
    for(int i = 0; i < n ; i ++){
        cin >> x >> y;
        G[x].push_back(y);
        a[i + 1].second = i + 1;
        a[x].first = -y;
    }
    sort(a + 1,a + n + 1);
    for(int i = 1; i <= n; i ++){
        dp[a[i].second] = i;
    }
    int q;
    cin >> q;
    while(q--){
        int k;
        cin >> k;
        vector<int> qry(k);
        for(int i = 0 ; i < k ; i ++){
            cin >> qry[i];
            qry[i] = dp[qry[i]];
            vis[qry[i]] = 1;
        }
        vector<int> ans;
        for(int i = 1 ;i <= n ; i ++){
            if(!vis[i]){
                ans.push_back(a[i].second);
            }
            if(ans.size() == 2)break;
        }
        for(int i = 0 ; i < k; i ++){
            vis[qry[i]] = 0;
        }
        if(ans.size() && G[ans[0]][G[ans[0]].size() - 1]){
            ans.push_back(0);
            cout << ans[0] << " " << *upper_bound(G[ans[0]].begin(),G[ans[0]].end(),G[ans[1]][G[ans[1]].size() - 1]) << endl;
        }else{
            cout << 0 << " " << 0 << endl;
        }
    }
    return 0;
}
```
### E题
给你一个1到n的排列，然后他会等概率的随机选一个区间，即对于\\( \frac{n \times (n + 1)}{2}\\)个区间选择是随机的。在随机选好一个区间后，会把区间内随机变成另一种排列，即假设区间长度为\\(len\\)，则任意\\(len !\\)种排列的选择是随机的。问这样操作完一次后，这个排列的逆序对的期望是多少。

解法：  
首先，我们假设选中的区间为X，而区间左边为A，右边为B，那么，我们可以发现，原来A与B内部，以及A与B之间形成的逆序对的数目是不变的。同时，A与X之间以及X与B形成的逆序对的数目都是不变的，变化的只有选中的X区间内部的逆序对的数目。 
 
同时，通过推断，我们可以得知对于一个长度为\\(len\\)的区间，随机选一种的得到的逆序对的期望为\\(\frac{len \times (len - 1)}{4}\\)。这个可以很简单的证明，对于任意一种排列和其相对的排列，这两种排列的逆序对的和为\\(\frac{len \times (len - 1)}{2} \\)。例如对于排列\\(1,3,2,4\\)，其相对的排列就是\\(4,2,3,1\\)。简单来讲，就是如果一种逆序，如果在这个排列中没有出现，那么一定在其相反的排列中出现，从而，两种排列中就会出现所有的逆序对。  

那么，对于这个问题，你只要算清楚原来给你的序列中，已有的每个逆序关系对最终结果的贡献（即该逆序关系会出现在多少个选择中）这个东西，你只要算这对关系会出现在那些区间中，总数扣掉这些就能算出贡献了，这个东西，用一个树状数组搞一搞就能出来了。之后，在加上刚才得到的，选中的区间的期望值，就算完啦。
```cpp
int a[100010];
ll bit[100010];
int sz;
void init(){
    for(int i = 0 ; i <= sz; i ++)bit[i] = 0;
}
void add(int x,int inc){
    while(x <= sz){
        bit[x] += inc;
        x += x & (-x);
    }
}
ll query(int x){
    ll ret = 0;
    while(x){
        ret += bit[x];
        x -= x & (-x);
    }
    return ret;
}


int main() {
    ios::sync_with_stdio(0);
    int n;
    cin >> n;
    ll avg = 0;
    for(int i = 1; i <= n ; i ++){
        avg += 1LL * i * (i - 1) / 2 * (n - i + 1);
    }
    sz = n;
    for(int i = 1 ; i <= n ; i ++){
        cin >> a[i];
    }
    ll inv = 0;
    for(int i = 1; i <= n ; i ++){
        add(a[i],1);
        inv += a[i] - query(a[i]);
    }
    long double ans = inv;
    reverse(a + 1, a + n + 1);
    init();
    ll ret = 0;
    for(int i = 1; i <= n; i ++){
        ret -= 1LL * query(a[i]) * (n - i + 1);
        add(a[i], i);
    }
    long double tot = (1LL * n * (n + 1) / 2);
    ans = inv + avg / tot / 2.0 + ret / tot;
    cout << fixed << setprecision(20) << ans << endl;
    return 0;
}
```

