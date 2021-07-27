---
layout:     post
title:      2021/3/7日题解
subtitle:   题解
date:       2021/3/7
author:    Tim
header-img: img/back_blog.jpg
catalog: true
tags:

    - 题解
---
## 问题E Group Project

#### 题目描述

The big day has finally arrived: today you are going to form groups of two in which you will do the end-of-the-year project.
When you arrive at school, you learn that the teacher of the other class is sick, and that your teacher, Mr. B.A.P. Cee, will also have to make groups for the other class. Mr. B.A.P. Cee is a smart guy and realizes that he can use these unfortunate circumstances to his advantage.
Ending up with groups of one should be avoided at all cost, so mixing the students of the two classes may avoid this situation. 
However, while it is easy to pair up two students from the same class, it is more difficult to match up students from different classes. Throughout the years there has been a lot of rivalry between the two groups, and many students dislike students in the other class. Mr. B.A.P. Cee knows which pairs of students will result in a fight and a failed project.
You are given a list of pairs of students who cannot work together. How many disjoint groups of two can Mr. B.A.P. Cee make that will not result in a failed project?

#### 输入

The input consists of:
• A line with two integers n (1 ≤ n ≤ 105 ), the number of students, and m (0 ≤ m ≤ 2 · 105 ),the number of pairs of students who cannot work together.
• m lines, each with two distinct integers i and j (1 ≤ i, j ≤ n , i ≠ j ), giving a pair of students who cannot work together.
Students are identified by the numbers 1 through n . It is guaranteed that it is possible to split the students into two classes in such a way that all students from the same class get along.

#### 输出

Output the number of pairs of students Mr. B.A.P. Cee can make without making any pair of students who cannot work together.

#### 样例输入

```
【样例1】
3 2
1 2
3 1
【样例2】
5 6
1 4
2 4
3 4
1 5
2 5
3 5
【样例3】
6 6
1 4
2 5
3 6
1 5
3 5
2 6
```

#### 样例输出

```
【样例1】
1
【样例2】
2
【样例3】
3
```

#### 题解

题目大意为两个班之间要进行两两组队合作，其中有些人不能和另一班级中一些人进行组队，并且班级内部都可以进行组队，问最多可以组队的队数。

先用DFS划分出两个班的人，然后判断两个班级之间是否两两都有矛盾，分两种情况讨论。

1. 如果任何两个人都有矛盾，则应当班级内部进行分组合作。
2. 如果不是所有人之间都有矛盾，则答案应为总人数除以2，分以下三种情况讨论：
   1. 两个班中人数均为偶数，则两个班级内部之间直接进行分组，答案为n/2
   2. 两个班中人数一奇一偶，两个班级内部直接分组，答案仍为n/2
   3. 两个班中人数均为奇数，则两个班级之间肯定存在一对跨班级的人没有矛盾，可以组成一对，其他内部之间分组，答案仍未n/2
   4. 综上，答案为总人数除以2，即为n/2

#### 代码

```c++
#include <bits/stdc++.h>
 
using namespace std;
vector<int>edge[100500];
int depth[100500]={0};
int vis[100500]={0};
int dfs(int k,int dep)
{
    depth[k]=dep;
    for(int to:edge[k])
    {
        if(depth[to]==0)
        {
            dfs(to,3-dep);
        }
    }
    return 0;
}
int main()
{
    int n,m,s,e;
    scanf("%d%d",&n,&m);
    for(int i=1;i<=m;i++)
    {
        scanf("%d%d",&s,&e);
        edge[s].push_back(e);
        edge[e].push_back(s);
    }
    dfs(1,1);
    int sum1=0,sum2=0;
    for(int i=1;i<=n;i++)
    {
        if(depth[i]==1)
            sum1++;
        if(depth[i]==2)
            sum2++;
    }
    if(sum1+sum2==n&&sum1*sum2==m)
        printf("%d\n",sum1/2+sum2/2);
    else
        printf("%d\n",n/2);
}
```

![image-20210307235951967](https://raw.githubusercontent.com/wcx1024979076/image1/master/img/image-20210307235951967.png)

#### 题解

用multiset维护质因数个数即可。

#### 代码

```c++
#pragma GCC optimize(1)
#pragma GCC optimize(2)
#pragma GCC optimize(3, "Ofast", "inline")
#include <bits/stdc++.h>

using namespace std;
typedef long long ll;
const ll mod = 1e9 + 7;
ll qpow(ll a, ll n)
{
    ll r = 1;
    while (n)
    {
        if (n & 1)
            r = (r * a) % mod;
        n >>= 1;
        a = (a * a) % mod;
    }
    return r;
}
multiset<ll> st[200500];
map<ll, ll> mp[200500];
vector<ll> prime;
int vis[200500] = {0};
ll last[200500] = {0};
int main()
{
    ll n, m, k, pos;
    scanf("%lld%lld", &n, &k);
    for (int i = 2; i <= 200000; i++)
    {
        if (!vis[i])
            prime.push_back(i);
        for (int j = 2; j * i <= 200000; j++)
            vis[i * j] = 1;
    }
    for (ll i = 1; i <= n; i++)
    {
        scanf("%lld", &m);
        for (ll j : prime)
        {
            if (m == 1)
                break;
            if (!vis[m])
            {
                mp[i][m] = 1;
                st[m].insert(1);
                break;
            }
            ll t = 0;
            while (m % j == 0)
            {
                m /= j;
                t++;
            }
            if (t)
            {
                mp[i][j] = t;
                st[j].insert(t);
            }
        }
    }
    ll ans = 1;
    for (ll j : prime)
    {
        if (st[j].size() == n)
        {
            int tem = *st[j].begin();
            if (tem != last[j])
            {
                ans = ans*qpow(j, tem - last[j])%mod;
                last[j] = tem;
            }
        }
    }
    while (k--)
    {
        scanf("%lld%lld", &pos, &m);
        for (ll j : prime)
        {
            if (m == 1)
                break;
            if (!vis[m])
            {
                ll &tem = mp[pos][m];
                if (tem)
                    st[m].erase(st[m].lower_bound(tem));
                tem++;
                st[m].insert(tem);
                if (st[m].size() == n)
                {
                    int tem1 = *st[m].begin();
                    if (tem1 != last[m])
                    {
                        ans = ans*qpow(m, tem1 - last[m])%mod;
                        last[m] = tem1;
                    }
                }
                break;
            }
            ll t = 0;
            while (m % j == 0)
            {
                m /= j;
                t++;
            }
            if (t)
            {
                ll &tem = mp[pos][j];
                if (tem)
                    st[j].erase(st[j].lower_bound(tem));
                tem+=t;
                st[j].insert(tem);
                if (st[j].size() == n)
                {
                    int tem1 = *st[j].begin();
                    if (tem1 != last[j])
                    {
                        ans =ans* qpow(j, tem1 - last[j])%mod;
                        last[j] = tem1;
                    }
                }
            }
        }
        printf("%lld\n",ans);
    }
}
```

