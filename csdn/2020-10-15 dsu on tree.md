[http://icpc.upc.edu.cn/problem.php?cid=2597&pid=10](http://icpc.upc.edu.cn/problem.php?cid=2597&pid=10)
##### 题目描述
Christmas is still one month away, but Mr. Panda already starts the Christmas preparation. Mr. Panda is decorating a Christmas tree with a set of Russian dolls. There are n Russian dolls numbered 1, 2, . . . , n. The ith doll is designed to be perfected nested inside the i + 1th doll for all 1 ≤ i ≤ n − 1. Nesting dolls are stable only if they have neighboring ordinal numbers, otherwise the smaller one will slide out from the larger one. Dolls can be nested recursively. For example, the n dolls can be nested all the way up from smallest to largest until there is only one doll left.
The Christmas tree happens to have n nodes wit one Russian roll dangling on each node. Doll number 1 is put at the tree root. Mr. Panda will invite his friend Mr. Sheep to collect some dolls from the Christmas tree as gifts on Christmas Eve. Mr. Sheep will pick a tree node and collects all the dolls in the sub-tree with the selected node as the sub-tree root.
As there could be a lot of dolls, Mr. Sheep want to nest the dolls he collects for easy carrying. The dolls should be stably nested. He wonders for each tree node, how many dolls will be ended up if he nests them as much as possible.
##### 输入
The first line of input gives the number of test cases T (1 ≤ T ≤ 10). T test cases follow.
Each test case starts with a line containing an integer n (1 ≤ n ≤ 2 × 105), the number of dolls and also the number of tree nodes.
The next n − 1 lines each contains two integers x and y (1 ≤ x, y ≤ n), denoting doll number x and doll number y are neighbors in the Christmas tree.
It is guaranteed that the sum of n in all cases is not greater than 106.
##### 输出
For each test case, the output consists one line starts with “Case #x:”, where x is the test case number (starting from 1), followed by next n integers. The ith (1 ≤ i ≤ n) integer indicates the number of dolls will be ended up if Mr. Sheep selects tree node that contains doll i, collects all the dolls in the sub-tree,and nests the dolls stably as much as possible.
##### 样例输入
```
1
7
1 2
2 4
2 6
1 3
3 5
3 7
```
##### 样例输出
```
Case #1: 1 3 3 1 1 1 1
```
##### 解析
dsu on tree算法，详情见：
[https://blog.csdn.net/qq_43472263/article/details/104150940](https://blog.csdn.net/qq_43472263/article/details/104150940)

```cpp
#include <bits/stdc++.h>

using namespace std;
struct node
{
    int from;
    int to;
    int next;
};
vector<node>edge;
int head[200050]= {0};
int hson[200050]= {0};
//int dep[200500]= {0};
//int father[200500]= {0};
int siz[200050]= {0};
int ans[200050]= {0};
int add_edge(int from,int to)
{
    edge.push_back({from,to,head[from]});
    head[from]=edge.size()-1;
}
int dfs1(int now,int depth,int fa)
{
    //dep[now]=depth;
    //father[now]=fa;
    siz[now]=1;
    for(int i=head[now]; i; i=edge[i].next)
    {
        if(edge[i].to!=fa)
        {
            dfs1(edge[i].to,depth+1,now);
            siz[now]+=siz[edge[i].to];
            if(hson[now]==-1||siz[edge[i].to]>siz[hson[now]])
                hson[now]=edge[i].to;
        }
    }
}
bool flag[200050]= {0};
bool vis[200050]= {0};
int cnt=0;
int update(int now,int fa,bool flg)
{
    int sum=flag[now-1]+flag[now+1];
    if(flg) ///插入
    {
        if(sum==2)
            cnt--;
        else if(sum==0)
            cnt++;
    }
    else ///移除
    {
        if(sum==0)
            cnt--;
        else if(sum==2)
            cnt++;
    }
    flag[now]=flg;
    for(int i=head[now]; i; i=edge[i].next)
    {
        if(edge[i].to!=fa&&!vis[edge[i].to])
        {
            update(edge[i].to,now,flg);
        }
    }
}
int dfs2(int now,int fa,bool kep)
{
    for(int i=head[now]; i; i=edge[i].next)
    {
        if(edge[i].to!=fa&&edge[i].to!=hson[now])
        {
            dfs2(edge[i].to,now,false);
        }
    }
    if(hson[now]!=-1)
    {
        dfs2(hson[now],now,true);
        vis[hson[now]]=1;
    }
    update(now,fa,true);
    ans[now]=cnt;
    if(hson[now]!=-1)
        vis[hson[now]]=0;
    if(!kep)
        update(now,fa,false);
}
int main()
{
    int t;
    scanf("%d",&t);
    for(int t1=1; t1<=t; t1++)
    {
        memset(hson,-1,sizeof(hson));
        memset(head,0,sizeof(head));
        edge.clear();
        edge.push_back({-1,-1,-1});
        int n;
        scanf("%d",&n);
        for(int i=1; i<=n-1; i++)
        {
            int x,y;
            scanf("%d%d",&x,&y);
            add_edge(x,y);
            add_edge(y,x);
        }
        dfs1(1,0,-1);
        memset(flag,0,sizeof(flag));
        memset(vis,0,sizeof(vis));
        dfs2(1,-1,false);
        printf("Case #%d:", t1);
        for(int i=1; i<=n; i++)
            printf(" %d", ans[i]);
        printf("\n");
    }
}
```

