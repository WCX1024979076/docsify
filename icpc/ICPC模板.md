### 快读&O2优化

```c++
#pragma GCC optimize(1)
#pragma GCC optimize(2)
#pragma GCC optimize(3,"Ofast","inline")
std::ios::sync_with_stdio(false);
std::cin.tie(0);
```
####   __int128 输入和输出

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
__int128 int128read()
{
	__int128 x=0;
	int flag=1;
	string a;
	cin>>a;
	for(char c:a)
	{
		if(c=='-')
			flag=-1;
		else
			x=x*10+c-'0';
	}
	return flag*x;
}
void int128print(__int128 x)
{
	if(x<0)
	{
		putchar('-');
		x=-x;
	}
	if(x>9)
		int128print(x/10);
	putchar(x % 10 + '0');
}
int main()
{
	__int128 a=int128read(),b=int128read();
	int128print(a+b);
}
```
####  java ACM 基础

````java
import java.io.*;
import java.math.*;
import java.util.*;

public class Main {
    public static void main(String args[])
    {
        //BigDecimal大数Double类
        //读入
        Scanner cin = new Scanner (new BufferedInputStream(System.in));
        int a; double b; BigInteger c; String d;
        a = cin.nextInt(); b = cin.nextDouble(); c = cin.nextBigInteger(); 
        d = cin.nextLine(); // 每种类型都有相应的输入函数.
        System.out.printf("输入的为%d %f %s %s\n",a,b,c.toString(),d);
        
        c=cin.nextBigInteger(2); //大数以2进制读入
        String tmp=c.toString(2); ///将大数以二进制形式输出
		
        System.out.print(1); // cout << …;
        System.out.println(1); // cout << … << endl;
        System.out.printf("%d",1); // 与C中的printf用法类似.
		
        ///字符串处理
        String st = "abcdefg";
        System.out.println(st.charAt(0)); // st.charAt(i)就相当于st[i].
        char [] ch;
        ch = st.toCharArray(); // 字符串转换为字符数组.
        for (int i = 0; i < ch.length; i++) ch[i] += 1;
        System.out.println(ch); // 输入为“bcdefgh”.
        if (st.startsWith("a")) // 如果字符串以'0'开头.
            st = st.substring(1); // 则从第1位开始copy(开头为第0位).
        
        ///进制转化
        int num=15,base=2;
        System.out.printf("15转2进制为%s\n",Integer.toString(num, base));
        st="1111";
        System.out.printf("2进制的1111转10进制为%d\n",Integer.parseInt(st, base)); 
        
        // 把st当做base进制，转成10进制的int(parseInt有两个参数,第一个为要转的字符串,第二个为进制). 
        BigInteger m = new BigInteger(st, base); // st是字符串，base是st的进制.
        
        ///排序
        int n=cin.nextInt();
        Integer[] arr=new Integer[n];
        for(int i=0;i<n;i++)
            arr[i]=cin.nextInt();
        
        Arrays.sort(arr,new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2-o1;     		 ///从大到小排序
            }
        });
        
        for(int i=0;i<n;i++)
            System.out.printf("%d ",arr[i]);
        System.out.println();
       	
        ///清空
        Arrays.fill(arr,5);
        for(int i=0;i<n;i++)
            System.out.printf("%d ",arr[i]);
        System.out.println();
        
        ///二分查找
        System.out.println(Arrays.binarySearch(arr, 5));
        ///如果key在数组中，则返回搜索值的索引；否则返回-1或者”-“(插入点)。
        ///插入点是索引键将要插入数组的那一点，即第一个大于该键的元素索引。
    }
}
````

####  java BigInteger 运算

```java
import java.io.*;
import java.math.BigInteger;
import java.util.*;

public class Main {
    public static void main(String args[])
    {
        Scanner cin=new Scanner(System.in);
        while(cin.hasNext())  ///相当于 scanf()!=EOF
        {
            BigInteger a,b,c;
            a=cin.nextBigInteger();
            b=cin.nextBigInteger();
            c=a.add(b);        //把a与b相加并赋给c
            c=a.subtract(b);   //把a与b相减并赋给c
            c=a.multiply(b);   //把a与b相乘并赋给c
            c=a.divide(b);     //把a与b相除并赋给c
            c=a.mod(b);        // 相当于a%b
            a.pow(2);         //相当于a^b
            a.compareTo(b);  //根据该数值是小于、等于、或大于a 返回 -1、0 或 1
            a.equals(b);     //判断两数是否相等，也可以用compareTo来代替
            a.min(b);
            a.max(b);  //取两个数的较小、大者
            System.out.println(c);
        }
    }
}

```

### 图论

####  SPFA判断负环

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <cmath>
#include <queue>
#include <algorithm>
#define INF 0x3f3f3f3f
 
using namespace std;
const int MAXN = 5500;
int n,m,w;
 
struct Edge
{
    int v,w,next;
}edge[MAXN];
int head[MAXN],dis[MAXN],vis[MAXN],t;
 
void Init()
{
    memset(head,-1,sizeof(head));
    t = 0;
}
void Add_edge(int u,int v,int w)
{
    edge[t].v = v;edge[t].w = w;edge[t].next = head[u];head[u] = t++;
}
 
bool SPFA()
{
    int mark[MAXN];//记录每个点如队列的次数
    for(int i = 1;i <= n;i ++)
    {
        mark[i] = 0;dis[i] = INF;vis[i] = 0;
    }
    queue<int> q;
    q.push(1);  //我们只需要判断负环，随便找一个起点就好
    dis[1] = 0;
    vis[1] = 1;//入队列
    mark[1] ++;
    while(!q.empty())
    {
        int  u = q.front();
        q.pop();
        vis[u] = 0;//出队列
        for(int i = head[u];i != -1;i = edge[i].next)
        {
            int v = edge[i].v;
            if(dis[v] > dis[u] + edge[i].w)
            {
                dis[v] = dis[u] + edge[i].w;
                if(!vis[v])//不在队列中的时候出队
                {
                    q.push(v);mark[v] ++;vis[v] = 1;
                }
                if(mark[v] >= n)//如果不存在负环，那么最多更新n-1次就可以得到最终的答案，因为一次最少更新一个节点，那么如果出现了更新n次，那么就一定出现了负环
                    return false;
            }
        }
    }
    return true;
}
int main()
{
    int T;
    scanf("%d",&T);
    while(T--)
    {
        Init();
        int u,v,z;
        scanf("%d%d%d",&n,&m,&w);
        for(int i = 0;i < m;i ++)
        {
            scanf("%d%d%d",&u,&v,&z);
            Add_edge(u,v,z);
            Add_edge(v,u,z);
        }
        for(int i = 0;i < w;i ++)
        {
            scanf("%d%d%d",&u,&v,&z);
            Add_edge(u,v,-z);
        }
        if(!SPFA())
            printf("YES\n");
        else
            printf("NO\n");
    }
    return 0;
}
```

####  dijikastra第k短路

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <queue>
using namespace std;
const int INF = 0x3f3f3f3f;
const int MAX = 1005;
int n,m;
int start,end,k;
struct Edge
{
    int w;
    int to;
    int next;
};
Edge e[100005];
int head[MAX],edgeNum;
int dis[MAX];   //dis[i]表示从i点到end的最短距离
bool vis[MAX];
int cnt[MAX];
vector<Edge> opp_Graph[MAX];
 
struct Node
{
    int f,g;    //f = g+dis[v]
    int v;      //当前到达的节点
    Node(int a, int b,int c):f(a),g(b),v(c){}
    bool operator < (const Node& a) const
    {
        return a.f < f;
    }
};
 
void addEdge(int from, int to, int w)
{
    e[edgeNum].to = to;
    e[edgeNum].w = w;
    e[edgeNum].next = head[from];
    head[from] = edgeNum++;
}
 
void dijikastra(int start)
{
    int i;
    memset(vis,0,sizeof(vis));
    for(i = 1; i <= n; i++)
        dis[i] = INF;
    dis[start] = 0;
    priority_queue<Node> que;
    que.push(Node(0,0,start));
    Node next(0,0,0);
    while(!que.empty())
    {
        Node now = que.top();
        que.pop();
        if(vis[now.v])              //从集合T中选取具有最短距离的节点
            continue;
        vis[now.v] = true;          //标记节点已从集合T加入到集合S中
        for(i = 0; i < opp_Graph[now.v].size(); i++)    //更新从源点到其它节点(集合T中)的最短距离
        {
            Edge edge = opp_Graph[now.v][i];
            if(!vis[edge.to] && dis[now.v] + edge.w < dis[edge.to])     //加不加前面的判断无所谓
            {
                dis[edge.to] = dis[now.v] + edge.w;
                next.f = dis[edge.to];
                next.v = edge.to;
                que.push(next);
            }
        }
    }
}
 
int A_Star()
{
    int i;
    priority_queue<Node> que;
    if(dis[start] == INF)
        return -1;
    que.push(Node(dis[start],0,start));
    Node next(0,0,0);
    while(!que.empty())
    {
        Node now = que.top();
        que.pop();
        cnt[now.v]++;
        if(cnt[end] == k)
            return now.f;
        if(cnt[now.v] > k)
            continue;
        for(i = head[now.v]; i != -1; i = e[i].next)
        {
            next.v = e[i].to;
            next.g = now.g + e[i].w;
            next.f = next.g + dis[e[i].to];
            que.push(next);
        }
    }
    return -1;
}
 
int main()
{
    int i;
    int from,to,w;
    edgeNum = 0;
    memset(head,-1,sizeof(head));
    memset(opp_Graph,0,sizeof(opp_Graph));
    memset(cnt,0,sizeof(cnt));
    scanf("%d %d",&n,&m);
    Edge edge;
    for(i = 1; i <= m; i++)
    {
        scanf("%d %d %d",&from,&to,&w);
        addEdge(from,to,w);
        edge.to = from;
        edge.w = w;
        opp_Graph[to].push_back(edge);
    }
    scanf("%d %d %d",&start,&end,&k);
    if(start == end)
        k++;
    dijikastra(end);
    int result = A_Star();
    printf("%d\n",result);
    return 0;
}
```

####  LCA+ST倍增算法

```c++
#include <bits/stdc++.h>
using namespace std;
//lca板子题,求俩个点最短距离
//树上两点最短路径:从根节点出发dis[u]+dis[v]-dis[lca]*2
const int maxn=1e5+5;
struct node
{
    int to,nex,w;
} road[maxn*2];
int n,q,cnt;
int pre[maxn][32],head[maxn],depth[maxn];
int dis[maxn];
void add(int u,int v,int w)
{
    road[cnt].to=v;
    road[cnt].w=w;
    road[cnt].nex=head[u];
    head[u]=cnt++;
}
void dfs(int u,int fa)
{
    pre[u][0]=fa;
    depth[u]=depth[fa]+1;
    for(int i=1; (1<<i)<=depth[u]; i++) //倍增
        pre[u][i]=pre[pre[u][i-1]][i-1];
    for(int i=head[u]; ~i; i=road[i].nex)
    {
        int v=road[i].to;
        if(v!=fa)
        {
            dis[v]=dis[u]+road[i].w;
            dfs(v,u);
        }
    }
}
int lca(int u,int v)
{
    if(depth[u]<depth[v])
    {
        swap(u,v);
    }
    int i=-1,j;
    while((1<<(i+1))<=depth[u])
        i++;
    for(j=i; j>=0; j--)
    {
        if(depth[u]-(1<<j)>=depth[v])
        {
            u=pre[u][j];
        }
    }
    if(u==v)
        return u;
    for(int j=i; j>=0; j--)
    {
        if(pre[u][j]!=pre[v][j])
        {
            u=pre[u][j];
            v=pre[v][j];
        }
    }
    return pre[u][0];
}
int main()
{
    scanf("%d %d",&n,&q);
    memset(head,-1,sizeof(head));
    memset(depth,0,sizeof(depth));
    cnt=0;
    for(int i=1; i<n; i++)
    {
        int u,v,w;
        scanf("%d %d %d",&u,&v,&w);
        add(u,v,w);
        add(v,u,w);
    }
    dis[1]=0;
    dfs(1,0);
    while(q--)
    {
        int u,v;
        scanf("%d %d",&u,&v);
        int lc=lca(u,v);
        int ans=dis[u]+dis[v]-2*dis[lc];
        printf("%d\n",ans);
    }
}
```

####  树的直径

```c++
#include <bits/stdc++.h>
 
using namespace std;
struct node
{
    int to,val,next;
};
node edge[200500]={0};
int head[200500]={0};
int cnt=0;
int add_edge(int from,int to,int val)
{
    edge[++cnt].next=head[from];
    edge[cnt].to=to;
    edge[cnt].val=val;
    head[from]=cnt;
}
int dp[200500][4]={0};
int down[200500]={0};
int up[200500]={0};
int len[200500][3]={0};
int dfs1(int now,int fa)
{
    for(int i=head[now];i;i=edge[i].next)
    {
        int to=edge[i].to,val=edge[i].val;
        if(to==fa)
            continue;
        dfs1(to,now);
        int tmp=dp[to][0]+val;
        if(tmp>dp[now][0])swap(dp[now][0],tmp);
        if(tmp>dp[now][1])swap(dp[now][1],tmp);
        if(tmp>dp[now][2])swap(dp[now][2],tmp);
        down[now]=max(down[now],down[to]);
    }
    down[now]=max(down[now],dp[now][0]+dp[now][1]);
}
int dfs2(int now,int fa)
{
    for(int i=head[now];i;i=edge[i].next)
    {
        if(edge[i].to==fa)
            continue;
        int tem=down[edge[i].to];
        if(tem>len[now][0])
            swap(tem,len[now][0]);
        if(tem>len[now][1])
            swap(tem,len[now][1]);
    }
    for(int i=head[now];i;i=edge[i].next)
    {
        int to=edge[i].to,val=edge[i].val;
        if(to==fa)
            continue;
        if(dp[now][0]==dp[to][0]+val)
        {
            dp[to][3]=max(dp[now][1],dp[now][3])+val;
            up[to]=max(dp[now][2],dp[now][3])+dp[now][1];
        }
        else if(dp[now][1]==dp[to][0]+val)
        {
            dp[to][3]=max(dp[now][0],dp[now][3])+val;
            up[to]=max(dp[now][2],dp[now][3])+dp[now][0];
        }
        else
        {
            dp[to][3]=max(dp[now][0],dp[now][3])+val;
            up[to]=max(dp[now][1],dp[now][3])+dp[now][0];
        }
        if(len[now][0]==down[to])
            up[to]=max(up[to],len[now][1]);
        else
            up[to]=max(up[to],len[now][0]);
        dfs2(to,now);
    }
}
int main()
{
    int n,from,to,val;
    scanf("%d",&n);
    for(int i=1;i<=n-1;i++)
    {
        scanf("%d%d%d",&from,&to,&val);
        add_edge(from,to,val);
        add_edge(to,from,val);
    }
    dfs1(1,-1);
    dfs2(1,-1);
    int ans=0;
    for(int i=1;i<=n;i++)
        ans=max(ans,up[i]+down[i]);
    cout<<ans<<endl;
}
```

####  最大流算法

```c++
#pragma GCC optimize(1)
#pragma GCC optimize(2)
#pragma GCC optimize(3,"Ofast","inline")
#include <bits/stdc++.h>
#define inf 0x3f3f3f3f3f3f3f3f
using namespace std;
typedef long long ll;
struct node
{
    ll to,val,next;
};
node edge[805]={0};
ll head[205]={0};
ll cnt=0;
ll s=1,e=1;
void add_edge(ll from,ll to,ll val)
{
    edge[cnt].next=head[from];
    edge[cnt].to=to;
    edge[cnt].val=val;
    head[from]=cnt++;
}
ll pre[205]={0},tag[205]={0},vis[205]={0};
ll bfs()
{
    queue<ll>que;
    memset(tag,0,sizeof(tag));
    memset(pre,0,sizeof(pre));
    memset(vis,0,sizeof(vis));
    que.push(s);
    vis[s]=1;
    while(!que.empty())
    {
        ll now=que.front();
        que.pop();
        for(ll i=head[now];i!=-1;i=edge[i].next)
        {
            ll to=edge[i].to,val=edge[i].val;
            if(!vis[to]&&val>0)
            {
                vis[to]=1;
                pre[to]=now;
                tag[to]=i;
                if(to==e)
                    return 1;
                que.push(to);
            }
        }
    }
    return 0;
}
ll EK()
{
    ll ans=0;
    while(bfs())
    {
        ll min1=inf;
        for(ll i=e;i!=s;i=pre[i])
        {
            min1=min(min1,edge[tag[i]].val);
        }
        for(ll i=e;i!=s;i=pre[i])
        {
            edge[tag[i]].val-=min1;
            edge[tag[i]^1].val+=min1;
        }
        ans+=min1;
    }
    return ans;
}
int main()
{
    ll n,m,from,to,val;
    while(scanf("%lld%lld",&m,&n) == 2&&n){
        e=n;
        cnt=0;
        for(int i=1;i<=n;i++)
            head[i]=-1;
        for(ll i=1;i<=m;i++)
        {
            scanf("%lld%lld%lld",&from,&to,&val);
            add_edge(from,to,val);
            add_edge(to,from,0);
        }
        printf("%lld\n",EK());
    }
}
```

####  最小费用最大流算法

```c++
#pragma GCC optimize(1)
#pragma GCC optimize(2)
#pragma GCC optimize(3,"Ofast","inline")
#include<bits/stdc++.h>
using namespace std;
struct node
{
    int next;
    int flow;
    int dis;
    int to;
};
int head[100500]= {0};
node edge[100500]= {0};
int dis[100500]= {0};
int pre[100500]= {0};
int vis[100500]= {0};
int last[100500]= {0};
int flow[100500]={0};
char g[1000][1000]= {0};
int n,m,s,t,cnt=-1,maxflow=0,mincost=0;
int num(int i,int j)
{
    return i*m+j;
}
int add(int from,int to,int flow,int dis)
{
    edge[++cnt].next=head[from];
    edge[cnt].to=to;
    edge[cnt].flow=flow;
    edge[cnt].dis=dis;
    head[from]=cnt;
}
queue<int>w;
bool spfa(int start,int end1)
{
    w.push(start);
    memset(dis,0x3f3f3f,sizeof(dis));
    memset(vis,0,sizeof(vis));
    memset(flow,0x3f3f3f,sizeof(flow));
    pre[end1]=-1;
    vis[start]=1;
    dis[start]=0;
    while(!w.empty())
    {
        int now=w.front();
        w.pop();
        vis[now]=0;
        for(int i=head[now]; i!=-1; i=edge[i].next)
        {
            //cout<<i<<endl;
            if(edge[i].flow>0&&dis[edge[i].to]>dis[now]+edge[i].dis)
            {
                dis[edge[i].to]=dis[now]+edge[i].dis;
                pre[edge[i].to]=now;
                last[edge[i].to]=i;
                flow[edge[i].to]=min(flow[now],edge[i].flow);
                if(!vis[edge[i].to])
                {
                    vis[edge[i].to]=1;
                    w.push(edge[i].to);
                }
            }
        }
    }
    return pre[end1]!=-1;
}
int MCMF()
{
    while(spfa(s,t))
    {
        maxflow+=flow[t];
        mincost+=dis[t]*flow[t];
        int now=t;
        while(now!=s)
        {
            edge[last[now]].flow-=flow[t];
            edge[last[now]^1].flow+=flow[t];
            now=pre[now];
        }
    }
}
int main()
{
    memset(head,-1,sizeof(head));
    scanf("%d%d",&n,&m);
    s=n*m+200,t=n*m+100;
    for(int i=1; i<=n; i++)
    {
        scanf("%s",g[i]+1);
    }
    for(int i=1; i<=n; i++)
    {
        for(int j=1; j<=m; j++)
        {
            if(g[i][j]=='x')
                continue;
            if(j>1&&g[i][j-1]!='x')
            {
                add(num(i,j),num(i,j-1),1,1);
                add(num(i,j-1),num(i,j),0,-1);
            }
            if(i>1&&g[i-1][j]!='x')
            {
                add(num(i,j),num(i-1,j),1,1);
                add(num(i-1,j),num(i,j),0,-1);
            }
            if(j<m&&g[i][j+1]!='x')
            {
                add(num(i,j),num(i,j+1),1,1);
                add(num(i,j+1),num(i,j),0,-1);
            }
            if(i<n&&g[i+1][j]!='x')
            {
                add(num(i,j),num(i+1,j),1,1);
                add(num(i+1,j),num(i,j),0,-1);
            }
            if(g[i][j]=='F')
            {
                add(s,num(i,j),2,0);
                add(num(i,j),s,0,0);
            }
            if(g[i][j]=='C'||g[i][j]=='B')
            {
                add(num(i,j),t,1,0);
                add(t,num(i,j),0,0);
            }
        }
    }
    MCMF();
   // cout<<maxflow<<" "<<mincost<<endl;
    if(maxflow!=2)
        printf("-1\n");
    else
        printf("%d\n",mincost);
}


```

####  二分图模板

```c++
#include <bits/stdc++.h>

using namespace std;
char a[1000][300] = {0};
int mp[1000][1000] = {0};
map<int, int> mp1, vis;
int n;
int dfs(int k)
{
    for (int i = 1; i <= n; i++)
    {
        if (mp[i][k] &&!vis[i])
        {
            vis[i] = 1;
            if (mp1[i] == 0 || dfs(mp1[i]))
            {
                mp1[i] = k;
                return 1;
            }
        }
    }
    return 0;
}
int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i++)
        scanf("%s", a[i] + 1);
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            int sum = 0;
            for (int k = 1; a[i][k]; k++)
            {
                if (a[i][k] != a[j][k])
                    sum++;
            }
            if (sum == 2)
                mp[i][j] = 1;
        }
    }
    //cout<<mp[1][2]<<endl;
    int ans = 0;
    for (int i = 1; i <= n; i++)
    {
        vis.clear();
        ans += dfs(i);
    }
    cout << n - ans / 2 << endl;
}
```

### 数据结构

####  树状数组前缀和

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
ll n,m;
ll a[100500]={0};
ll c[100500]={0};
ll lowbit(ll x)
{
    return x&(-x);
}
ll update(ll k,ll x)
{
    for(ll i=k;i<=n;i+=lowbit(i))
        c[i]+=x;
}
ll sum1(ll k)
{
    ll ans=0;
    for(ll i=k;i>=1;i-=lowbit(i))
        ans+=c[i];
    return ans;
}
int main()
{
    ll a1,b1,c1;
    scanf("%lld%lld",&n,&m);
    for(ll i=1;i<=m;i++)
    {
        scanf("%lld%lld%lld",&a1,&b1,&c1);
        if(a1==0)
            update(b1,c1;
        else
            printf("%lld\n",sum1(c1)-sum1(b1-1));
    }
}
```

####  线段树维护区间和

```c++
#include <stdio.h>
using namespace std;
typedef long long ll;
ll num[100500]={0};
ll lazy[800500]={0};
ll sum[800500]={0};
void push_up(ll t)
{
    sum[t]=sum[2*t]+sum[2*t+1];
}
void push_down(ll t,ll l,ll r)
{
    if(lazy[t]==0)
        return ;
    lazy[2*t]+=lazy[t];
    lazy[2*t+1]+=lazy[t];
    ll mid=(l+r)/2;
    sum[2*t]+=lazy[t]*(mid-l+1);
    sum[2*t+1]+=lazy[t]*(r-mid);
    lazy[t]=0;
}
void build(ll t,ll l,ll r)
{
    if(l==r)
    {
        sum[t]=num[l];
        return ;
    }
    ll mid=(l+r)/2;
    build(2*t,l,mid);
    build(2*t+1,mid+1,r);
    push_up(t);
}
void update(ll t,ll l,ll r,ll L,ll R,ll add) ///l,r为更新区间，L,R为线段树区间
{
    if(l<=L&&r>=R)
    {
        sum[t]+=add*(R-L+1);
        lazy[t]+=add;
        return ;
    }
    push_down(t,L,R);
    ll mid=(L+R)/2;
    if(l<=mid)
        update(2*t,l,r,L,mid,add);
    if(mid<r)
        update(2*t+1,l,r,mid+1,R,add);
    push_up(t);
}
ll query_sum(ll t,ll l,ll r,ll L,ll R) ///l,r为更新区间，L,R为线段树区间
{
    if(l<=L&&r>=R)
    {
        return sum[t];
    }
    push_down(t,L,R);
    ll mid=(L+R)/2,sum=0;
    if(l<=mid)
        sum+=query_sum(2*t,l,r,L,mid);
    if(mid<r)
        sum+=query_sum(2*t+1,l,r,mid+1,R);
    return sum;
}
int main()
{
    ll n,m;
    scanf("%lld%lld",&n,&m);
    for(ll i=1;i<=n;i++)
        scanf("%lld",&num[i]);
    build(1,1,n);
    char s[10];
    ll l,r,x;
    for(ll i=1;i<=m;i++)
    {
        scanf("%s%lld%lld",s,&l,&r);
        if(s[0]=='Q')
        {
            printf("%lld\n",query_sum(1,l,r,1,n));
        }
        else
        {
            scanf("%lld",&x);
            update(1,l,r,1,n,x);
        }
    }
}
```

####  划分树求中位数

```c++
#pragma GCC optimize(1)
#pragma GCC optimize(2)
#pragma GCC optimize(3,"Ofast","inline")
#include <bits/stdc++.h>

using namespace std;
int tree[20][100500]={0};
int to[20][100500]={0};
int num[100500]={0};
int sorted[100500]={0};
int build(int l,int r,int deep)
{
    if(l==r)
        return 0;
    int mid=(l+r)/2;
    int midd=sorted[mid];
    int suppose=mid-l+1;
    for(int i=l;i<=r;i++)
    {
        if(tree[deep][i]<midd)
            suppose--;
    }
    int sleft=l,sright=mid+1;
    for(int i=l;i<=r;i++)
    {
        if(i==l)
            to[deep][l]=0;
        else
            to[deep][i]=to[deep][i-1];
        if(tree[deep][i]<midd)
        {
            to[deep][i]++;
            tree[deep+1][sleft++]=tree[deep][i];
        }
        else if(tree[deep][i]==midd&&suppose>0)
        {
            suppose--;
            to[deep][i]++;
            tree[deep+1][sleft++]=tree[deep][i];
        }
        else
        {
            tree[deep+1][sright++]=tree[deep][i];
        }
    }
    build(l,mid,deep+1);
    build(mid+1,r,deep+1);
}
int query(int l,int r,int L,int R,int k,int deep)
{
    if(L==R)
        return tree[deep][L];
    int mid=(l+r)/2;
    int lef;
    int toleft;
    if(l==L)
        lef=0,toleft=to[deep][R];
    else
        lef=to[deep][L-1],toleft=to[deep][R]-lef;
    if(k<=toleft)
    {
        return query(l,mid,l+lef,l+toleft+lef-1,k,deep+1);
    }
    else
    {
        return query(mid+1,r,mid+L-l-lef+1,mid+R-l-toleft-lef+1,k-toleft,deep+1);
    }
}
int main()
{
    int n,m,summ=0;
    while(scanf("%d",&n)!=EOF)
    {
        summ++;
        for(int i=1;i<=n;i++)
        {
            scanf("%d",&num[i]);
            sorted[i]=num[i];
            tree[0][i]=num[i];
        }
        sort(sorted+1,sorted+n+1);
        build(1,n,0);
        printf("Case %d:\n",summ);
        scanf("%d",&m);
        for(int i=1;i<=m;i++)
        {
            int a,b,c; /// a,b,c代表查询a到b区间内第c大的数
            scanf("%d%d",&a,&b);
            c=(b-a)/2+1;
            printf("%d\n",query(1,n,a,b,c,0));
        }
    }
}
```

### 字符串

####  KMP算法

```c++
#pragma GCC optimize(1)
#pragma GCC optimize(2)
#pragma GCC optimize(3,"Ofast","inline")
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll mod=1e9+7;
const ll N = 1005000;
ll Next[N];
char s[N];
char t[N];
ll  tlen,len;
void getNext(char s[N])
{
    ll j=0,k=-1;
    Next[0]=-1;
    while(j<tlen)
    {
        if(k==-1||s[j]==s[k])
        {
            Next[++j]=++k;
        }
        else
            k=Next[k];
    }
}
int main()
{
    while(1)
    {
        scanf("%s",s);
        if(s[0]=='.')
        break;
        tlen=strlen(s);
        len=tlen;
        getNext(s);
        ll kk=tlen-Next[tlen];
        if(strlen(s)%kk==0)
        {
            printf("%d\n",strlen(s)/kk);
        }
        else
        { 
            printf("1\n");   //如果不能除尽，说明有后缀，例如abababa，这种情况只能为1
        }
    }
    return 0;
}
```

####  字符串哈希

  给一个长度为n的字符串(1<=n<=200000),他只包含**小写**字母 

  找到这个字符串多少个前缀是M形字符串. 

  M形字符串定义如下: 

  他由两个相同的回文串拼接而来,第一个回文串的结尾字符和第二个字符串的开始字符可以重叠,也就是以下都是M 形字符串.  

  abccbaabccba(由abccba+abccba组成) 

  abcbaabcba(有abcba+abcba组成) 

  abccbabccba(由abccba+abccba组成组成,但是中间的1是共用的) 

  a(一个单独字符也算) 

```c++
#include <bits/stdc++.h>
using namespace std;
typedef unsigned long long int ULL;
const int N = 200010;

int P = 131;
ULL p[N], h[N], ed[N];
int ask1(int l, int r)
{
    return h[r] - h[l - 1] * p[r - l + 1];
}
int ask2(int l, int r)
{
    return ed[l] - ed[r + 1] * p[r - l + 1];
}
int main()
{
    int ans = 0;
    char str[N];
    cin >> str + 1;
    int n = strlen(str + 1);
    p[0] = 1;
    for (int i = 1; i <= n; i++)
    {
        h[i] = h[i - 1] * P + str[i];
        p[i] = p[i - 1] * P;
    }
    for (int i = n; i >= 0; i--)
    {
        ed[i] = ed[i +1] * P + str[i];
    }
    for (int i = 1; i <= n; i++)
    {
        if (i % 2 == 1)
        {
            int x = (i + 1) / 2;
            if (ask1(1, x) == ask1(x, i) && ask1(1, x) == ask2(1, x))
                ans++;
        }
        else
        {
            int x = i / 2;
            if (ask1(1, x) == ask1(x + 1, i) && ask1(1, x) == ask2(1, x))
                ans++;
        }
    }
    cout << ans ;
}
```

####  回文串匹配算法（马拉车算法）

```c++
#include <bits/stdc++.h> ///求解最长回文串，对p[i]/2求和即可得回文串个数
using namespace std;
typedef long long ll;
char a[500500]={0};
int l[500500]={0};
int r[500500]={0};
string str="$#";
vector<int>p;
void manacher(char *c)
{
    int max_id=0,id=0;
    for(int i=1;c[i];i++)
        str+=c[i],str+="#";
    p.push_back(1);
    for(int i=1;i<(int)str.size();i++)
    {
        if(max_id>i)
            p.push_back(min(max_id-i,p[2*id-i]));  
        else
            p.push_back(1);
        while(i+p[i]<(int)str.size()&&str[i+p[i]]==str[i-p[i]])
            p[i]++;
        if(i+p[i]>max_id){
            max_id=i+p[i],id=i;
        }
    }
}
int main()
{
    scanf("%s",a+1);
    manacher(a);
    int now=0,n=str.size(),ans=0;
    for(int i=0;i<n;i++)
        ans=max(ans,p[i]-1);
    printf("%d",ans);
}
```

####  字典树

从一组数据中选取两个数求最大异或值。

```c++
#include <iostream>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int a[1005000]={0};
int tree[5000500][3]={0};
int tot=1;///必须从1开始
int in(int x)
{
    int p=1;
    for(int i=30;i>=0;i--)
    {
        int k=x>>i&1;
        if(tree[p][k]==0)
            tree[p][k]=++tot;
        p=tree[p][k];
    }
}
int out(int x)
{
    int p=1,ans=0;
    for(int i=30;i>=0;i--)
    {
        int k=x>>i&1;
        if(tree[p][!k])
        {
            ans=ans*2+!k;
            p=tree[p][!k];
        }
        else
        {
            ans=ans*2+k;
            p=tree[p][k];
        }
    }
    return ans;
}
int main()
{
    int n;
    scanf("%d",&n);
    for(int i=1;i<=n;i++)
        scanf("%d",&a[i]);
    int max1=0;
    for(int i=1;i<=n;i++)
    {
       in(a[i]);
       int num=out(a[i]);
       max1=max(max1,num^a[i]);
    }
    cout<<max1<<endl;
    return 0;
}
```

### 数论

####  欧拉函数模板

```c++
#include <bits/stdc++.h>
using namespace std;
int is_prime[100500]={0};
vector<int>prime;
int oula_table[100500]={0};
void init(int n)  ///打素数表
{
    for(int i=2;i<=n;i++)
    {
        if(!is_prime[i])
            prime.push_back(i);
        for (int j = 0; j <=prime.size() && i*prime[j] <= n; j++) {
            is_prime[i*prime[j]] = 1;
            if (i % prime[j] == 0)
                break;
        }
    }
}
int oula1(int n) ///根据素数表求解欧拉函数
{
    int ans=n;
    for(int i=0;prime[i]*prime[i]<=n;i++)
    {
        if(n%prime[i]==0)
            ans=ans-ans/prime[i];
        while(n%prime[i]==0)
            n/=prime[i];
    }
    return ans;
}
int oula2(int n)  ///分解质因数法求解欧拉函数
{
    int m=n;
    int ans=n;
    for(int i=2;i<=sqrt(n);i++)
    {
        if(m%i==0)
            ans=ans-ans/i;
        while(m%i==0)
            m/=i;
    }
    if(m!=1)
        ans=ans-ans/m;
    return ans;
}
void get_oula_table(int n) ///欧拉打表
{
    for(int i=1;i<=n;i++)
        oula_table[i]=0;
    oula_table[1]=1;
    for(int i=2;i<=n;i++)
    {
        if(!oula_table[i]) ///说明i是质数
        {
            for(int j=i;j<=n;j+=i) //去找i的倍数
            {
                if(!oula_table[j])
                    oula_table[j]=j;
                oula_table[j]=oula_table[j]/i*(i-1);
            }
        }
    }
}
int main()
{
    int n;
    cin>>n;
    init(n);
    cout<<"找素数求解欧拉函数值为"<<oula1(n)<<endl;
    cout<<"分解质因数法求解欧拉函数值为"<<oula2(n)<<endl;
    get_oula_table(n);
    cout<<"打表法求解欧拉函数值为";
    for(int i=1;i<=n;i++)
        cout<<oula_table[i]<<' ';
}
```

####  逆序对

```c++
#include <iostream>
#include <bits/stdc++.h>
using namespace std;
int a[2005000]= {0};
int b[2005000]= {0};
long long int merge_sort(long long int l,long long int r)
{
    if (l >= r)
        return 0;
    long long int mid = (l + r) >> 1, res = 0;
    res += merge_sort(l, mid);
    res += merge_sort(mid + 1, r);
    long long int i = l, j = mid + 1, cnt = 0;
    while (i <= mid && j <= r)
        if (a[i] <= a[j])
            b[cnt++] = a[i++];
        else
        {
            res += mid - i + 1;
            b[cnt++] = a[j++];
        }
    while (i <= mid)
        b[cnt++] = a[i++];
    while (j <= r)
        b[cnt++] = a[j++];
    for (long long int i = l, j = 0; j < cnt; ++i, ++j)
        a[i] = b[j];
    return res;
}
int main()
{
    int n;
    cin>>n;
    for(long long int i=0;i<n; i++)
    {
        cin>>a[i];
    }
    long long int sum=merge_sort(0,n-1);
    cout<<sum<<endl;
    return 0;
}
```

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
ll n,m;
ll a[100500]={0};
ll c[100500]={0};
ll maxn=10050,k,ans=0;
ll lowbit(ll x)
{
    return x&(-x);
}
ll add(ll k,ll x)
{
    for(ll i=k;i<=maxn;i+=lowbit(i))
        c[i]+=x;
}
ll getsum(ll k)
{
    ll ans=0;
    for(ll i=k;i>=1;i-=lowbit(i))
        ans+=c[i];
    return ans;
}
int main()
{
    scanf("%lld",&n);
    for(ll i=1;i<=n;i++)
    {
        scanf("%lld",&k);
        add(k,1);
        ans+=i-getsum(k);
    }
    printf("%d\n",ans);
}
```

####  二维前缀和

```c++
#include<bits/stdc++.h>
using namespace std;
const int maxn = 5010;
int g[maxn][maxn];
int main(void)
{
    int N,R;
    cin >> N >> R;
    int n = R, m = R;
    for(int i = 0,x,y,w;i < N;++i)
    {
        cin >> x >> y >> w;
        x++,y++;
        n = max(n,x);
        m = max(m,y);
        g[x][y] += w;
    }
    for(int i = 1; i <= n; i++)
        for(int j = 1;j <= m; j++)
            g[i][j] += g[i-1][j] + g[i][j-1] - g[i-1][j-1];
    int ans = 0;
    for(int i = R;i <= n;i++)
        for(int j = R;j <= m;j++)
            ans = max(ans,g[i][j]-g[i-R][j]-g[i][j-R]+g[i-R][j-R]);
    cout << ans ;
    return 0;
}
```

####  扩展欧几里得定理

1. 求解a * x + b * y = gcd(a,b) 方程的通解x和y

   ```c++
   #include <bits/stdc++.h>
   using namespace std;
   typedef long long ll;
   const ll N=1e9+7;
   ll exgcd(ll a, ll b, ll &x, ll &y)
   {
       if(b==0)
       {
           x=1;
           y=0;
           return a;
       }
       ll d = exgcd(b, a%b, x, y);
       ll t = x;
       x = y;
       y = t-(a/b)*y;
       return d;
   }
   int main()
   {
       ll a=3,b=5,x=0,y=0;
       ll g=exgcd(a,b,x,y);
       cout<<"特解为"<<"x="<<x<<",y="<<y<<endl;
       for(int i=1;i<=5;i++)
       {
           x+=b/g;
           y-=a/g;
           cout<<"通解为"<<"x="<<x<<",y="<<y<<endl;
       }
       return 0;
   }
   ```

2. 求解一般方程a * x+b * y=c的通解x和y

   将方程转变为：a * x+b * y=gcd(a, b) * c/gcd(a, b);

   最终即为：a * x/(c/gcd(a, b))+b * y/(c/gcd(a, b))=gcd(a, b);

   即把结果调整为：x1=x0 * c/gcd(a, b); y1=y0 * c/gcd(a, b);

   ```c++
   #include<stdio.h>
   long long exgcd(long long a, long long b, long long &x, long long &y)
   {
       if(b==0)
       {
           x=1;
           y=0;
           return a;
       }
       long long r=exgcd(b, a%b, x, y), t;
       t=x;
       x=y;
       y=t-(a/b)*y;
       return r;
   }
   int main()
   {
       long long a, b, c, ans, x, y;
       while(scanf("%lld%lld%lld", &a, &b, &c)!=EOF)
       {
           ans=exgcd(a, b, x, y);
           if(c%ans==0)
           {
               x=-x*c/ans;
               y=-y*c/ans;
               printf("%lld %lld\n", x, y);
           }
           else
               printf("-1\n");
       }
       return 0;
   }
   ```

####  逆元

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
ll a,q;
ll ksm(ll n,ll k)
{
    ll sum1=1,sum2=n;
    while(k!=0)
    {
        if(k%2==1)
            sum1=sum1*sum2%q;
        sum2=sum2*sum2%q;
        k/=2;
    }
    return sum1;
}
int main()
{
    cout<<"请输入a和q的值"<<endl;
    cin>>a>>q;
    cout<<"a关于模q的逆元为"<<ksm(a,q-2)<<endl;
}
```

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
ll exgcd(ll a,ll b,ll& x,ll& y) ///后两个参数为引用
{
    if(b==0)
    {
        x=1;
        y=0;
        return a;
    }
    ll gcd_ans=exgcd(b,a%b,x,y);
    ll tem=x;
    x=y;
    y=tem-a/b*y;
    return gcd_ans;
}
int main()
{
    cout<<"请输入a和q的值"<<endl;
    ll a,q,x=0,y=0;
    cin>>a>>q;
    ll gcd_ans=exgcd(q,a,x,y);
    ll mul=q/gcd_ans;
    y=(y%mul+mul)%mul;
    cout<<"a关于模q的逆元为"<<y<<endl;
}
```
####  表达式求值（中缀表达式转后缀表达式）

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
stack<char> sc;
stack<ll> sll;
char a[100500] = {0};
inline ll qpow(ll a, ll b)
{
    ll ans = 1;
    while (b)
    {
        if (b & 1)
            ans *= a;
        a *= a, b >>= 1;
    }
    return ans;
}

int cmp1(char a1, char a2)
{
    if (a2 == '(')
        return -1;

    if (a2 == ')')
    {
        if (a1 == '(')
            return 0;
        else if (a1 == '#')
            return 0;
        else
            return 1;
    }
    if (a2 == '+' || a2 == '-')
    {
        if (a1 == '+' || a1 == '-' || a1 == '*' || a1 == '/' || a1 == '^')
            return 1;
        else
            return -1;
    }
    if (a2 == '*' || a2 == '/')
    {
        if (a1 == '*' || a1 == '/' || a1 == '^')
            return 1;
        else
            return -1;
    }
    if (a2 == '^')
    {
        if (a1 == '^')
            return 1;
        else
            return -1;
    }
    if (a2 == '#')
    {
        if (a1 == '#')
            return 0;
        else if (a1 == '(' || a1 == ')')
            return 0;
        else
            return 1;
    }
    return 0;
}
int main()
{
    scanf("%s", a + 1);
    a[strlen(a + 1) + 1] = '#';
    ll tem = 0, tem2, tem1;
    sll.push(0);
    sc.push('#');
    for (int i = 1; a[i]; i++)
    {
        if (a[i] <= '9' && a[i] >= '0')
        {
            tem = tem * 10 + a[i] - '0';
            if (a[i + 1] < '0' || a[i + 1] > '9')
                sll.push(tem), tem = 0;
        }
        else
        {
            while (!sc.empty())
            {
                ll tem_cmp = cmp1(sc.top(), a[i]);
                if (tem_cmp == 1)
                {
                    tem2 = sll.top();
                    sll.pop();

                    tem1 = sll.top();
                    sll.pop();

                    if (sc.top() == '+')
                        sll.push(tem1 + tem2);
                    else if (sc.top() == '-')
                        sll.push(tem1 - tem2);
                    else if (sc.top() == '*')
                        sll.push(tem1 * tem2);
                    else if (sc.top() == '/')
                        sll.push(tem1 / tem2);
                    else if (sc.top() == '^')
                        sll.push((ll)qpow(tem1, tem2));
                    sc.pop();
                }
                else if (tem_cmp == 0)
                {
                    if (a[i] != ')' || (a[i] == ')' && sc.size() > 1))
                        sc.pop();
                    break;
                }
                else if (tem_cmp == -1)
                {
                    sc.push(a[i]);
                    break;
                }
            }
        }
    }
    cout << sll.top() << endl;
}
```

### 动态规划

####  最长上升子序列

如果你需要移动一样东西，显然接触或者使用磁场电场之类的可以解决。但是有没有办法进行超越距离的随心所欲的移动？
对于物体或者文字进行超距离移动一直是人类的梦想，有一天这个难题终于被我们的大牛解决了！他现在需要的就是整理数列。数列就是所谓的写在纸上或者在电脑品目上的数列...
整理数列需要一个叫做swap的操作，swap操作就是指大牛通过超距离的控制把数列中的某一位直接插入某两位的中间或者数列的开始或者终止的操作。这个操作的关键在于超距离控制，显然这种事情不能干太多次，不但降RP，而且很耗体力。你的任务就是从初始状态到目标状态所需要做swap的最少次数。

输入
三行，第一行一个整数 n（n<600000）
第二行，n 个整数（1-n），表示初始数列。
第三行，n 个整数（1-n），表示目标数列。
保证整数不重复。

输出
一行 表示最少swap次数。
样例输入
10
1 2 3 4 5 6 7 8 9 10
10 9 8 7 6 5 4 3 2 1
样例输出
9

```c++
#pragma GCC optimize(1)
#pragma GCC optimize(2)
#pragma GCC optimize(3,"Ofast","inline")
#include<iostream>
#include<cstdio>
#include <bits/stdc++.h>
using namespace std;
const int inf=0x7fffffff;
int b[1005001],mp[1005001],c[1005001];
int main()
{
    int n,x;
    cin>>n;
    for(int i=1; i<=n; i++)
        scanf("%d",&x),mp[x]=i;
    for(int i=1; i<=n; i++)
        scanf("%d",&b[i]),c[i]=inf;
    int len=0;
    c[0]=0;
    for(int i=1; i<=n; i++)
    {
        int l=0,r=len,mid;
        if(mp[b[i]]>c[len])
            c[++len]=mp[b[i]];
        else
        {
            while(l<r)
            {
                mid=(l+r)/2;
                if(c[mid]>mp[b[i]])
                    r=mid;
                else
                    l=mid+1;
            }
            c[l]=min(mp[b[i]],c[l]);
        }
    }
    cout<<n-len<<endl;
    return 0;
}
```

####  最长公共子序列

```c++
for(int i = 1; i <= strlen(a); i ++)
		for(int j = 1; j <= strlen(b); j ++)
			if(a[i] == b[j])
				dp[i][j] = dp[i-1][j-1] + 1;
			else
				dp[i][j] = max(dp[i-1][j],dp[i][j-1]);
```

####  状压DP

```c++
#include <bits/stdc++.h>
using namespace std;
typedef unsigned long long ull;
typedef long long ll;
 
const int inf=0x3f3f3f3f;
int n,m;
 
struct node{
     int cost,num;
}nd[100005];
 
int f[1005][(1<<12)+5];//1左移12位再加5
 
int main()
{
    cin>>n>>m;
    for(int i=1;i<=m;i++)
    {
        int b;
        scanf("%d%d",&nd[i].cost,&b);
        int t=0;
        for(int j=1;j<=b;j++)
        {
              int temp;
              scanf("%d",&temp);
              t|=(1<<(temp-1));///1本来就在第一个位，所以要到第temp位，只需要向左移动temp-1位
        }
        nd[i].num=t;
    }
    memset(f,inf,sizeof(f));
    f[0][0]=0;///很重要
    for(int i=1;i<=m;i++)
    {
        for(int j=0;j<(1<<n);j++)///n个锁
        {
            int kk=j|nd[i].num;///对应每个锁选不选
            f[i][kk]=min(f[i][kk],f[i-1][j]+nd[i].cost);
            f[i][j]=min(f[i][j],f[i-1][j]);///
        }
    }
    if(f[m][(1<<n)-1]!=inf)///1向左移动12，相当于1000000000000，然而12个位置都为1，即(111111111111)=(1000000000000)-1
        printf("%d\n",f[m][(1<<n)-1]);
    else printf("-1");
    return 0;
}
```

### 计算机图形学

####  凸包算法

```c++
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<cmath>
#define PI 3.1415926535
using namespace std;
struct node
{
    int x,y;
};
node vex[1000];//存入的所有的点
node stackk[1000];//凸包中所有的点
int xx,yy;
bool cmp1(node a,node b)//排序找第一个点
{
    if(a.y==b.y)
        return a.x<b.x;
    else
        return a.y<b.y;
}
int cross(node a,node b,node c)//计算叉积
{
    return (b.x-a.x)*(c.y-a.y)-(c.x-a.x)*(b.y-a.y);
}
double dis(node a,node b)//计算距离
{
    return sqrt((a.x-b.x)*(a.x-b.x)*1.0+(a.y-b.y)*(a.y-b.y));
}
bool cmp2(node a,node b)//极角排序另一种方法，速度快
{
    if(atan2(a.y-yy,a.x-xx)!=atan2(b.y-yy,b.x-xx))
        return (atan2(a.y-yy,a.x-xx))<(atan2(b.y-yy,b.x-xx));
    return a.x<b.x;
}
bool cmp(node a,node b)//极角排序
{
    int m=cross(vex[0],a,b);
    if(m>0)
        return 1;
    else if(m==0&&dis(vex[0],a)-dis(vex[0],b)<=0)
        return 1;
    else return 0;
    /*if(m==0)
        return dis(vex[0],a)-dis(vex[0],b)<=0?true:false;
    else
        return m>0?true:false;*/
}
int main()
{
    int t,L;
    while(~scanf("%d",&t),t)
    {
        int i;
        for(i=0; i<t; i++)
        {
            scanf("%d%d",&vex[i].x,&vex[i].y);
        }
        if(t==1)
            printf("%.2f\n",0.00);
        else if(t==2)
            printf("%.2f\n",dis(vex[0],vex[1]));
        else
        {
            memset(stackk,0,sizeof(stackk));
            sort(vex,vex+t,cmp1);
            stackk[0]=vex[0];
            xx=stackk[0].x;
            yy=stackk[0].y;
            sort(vex+1,vex+t,cmp2);//cmp2是更快的，cmp更容易理解
            stackk[1]=vex[1];//将凸包中的第两个点存入凸包的结构体中
            int top=1;//最后凸包中拥有点的个数
            for(i=2; i<t; i++)
            {
                while(i>=1&&cross(stackk[top-1],stackk[top],vex[i])<0)   //对使用极角排序的i>=1有时可以不用，但加上总是好的
                    top--;
                stackk[++top]=vex[i];                                    //控制<0或<=0可以控制重点，共线的，具体视题目而定。
            }
            double s=0;
            //for(i=1; i<=top; i++)//输出凸包上的点
            //cout<<stackk[i].x<<" "<<stackk[i].y<<endl;
            for(i=1; i<=top; i++)   //计算凸包的周长
                s+=dis(stackk[i-1],stackk[i]);
            s+=dis(stackk[top],vex[0]);//最后一个点和第一个点之间的距离
            /*s+=2*PI*L;
            int ans=s+0.5;//四舍五入
            printf("%d\n",ans);*/
            printf("%.2lf\n",s);
        }
    }
}
```

####  求任意多边形面积

```c++
#include <bits/stdc++.h>
using namespace std;
struct Point{
   double x,y;
}p[100500];
int n;
double polygonarea()
{
    int i,j;
    double area = 0;
    for(i = 1;i <= n;++i){
        if(i<n)
            j=i+1;
        else
            j=1;
        area += p[i].x*p[j].y;
        area -= p[i].y*p[j].x;
    }
    area /= 2.0;
    return (area<0?-area:area);
}
int main()
{
    while(scanf("%d",&n)!=EOF&&n!=0)
    {
        for(int i=1;i<=n;i++)
            scanf("%lf%lf",&p[i].x,&p[i].y);
        printf("%.1f\n",polygonarea());
    }
}
```

####  线段树+扫描线求正方形颜色反转

输入：

```c++
1
5 2  
2 4 1 3
1 5 3 5
```

代表1组测试样例，给定5*2的正方形，将2<=x<=4&&1<=y<=3的范围内的小正方形进行黑白反转，求最终黑色正方形的个数，答案为18.

![](https://img-blog.csdnimg.cn/img_convert/dbec3f15263780627c1708daa011f31a.png#pic_center)

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
struct node
{
    int h,a,b,num;
};
int mark[100500]={0};
int sum[100500]={0};
node edge[100500]={0};
bool cmp(node a,node b)
{
    if(a.h==b.h)
        return a.num>b.num;
    else
        return a.h<b.h;
}
int nodeupdate(int root,int l,int r,ll num)
{
    mark[root]^=1;
    sum[root]=r-l+1-sum[root];
}
int pushdown(int root,int l,int r)
{
    if(mark[root]==0)
        return 0;
    int mid=(l+r)/2;
    nodeupdate(2*root,l,mid,mark[root]);
    nodeupdate(2*root+1,mid+1,r,mark[root]);
    mark[root]=0;
}
int update(int root,int l,int r,int L,int R,ll num)
{
    if(l>=L&&r<=R)
    {
        nodeupdate(root,l,r,num);
        return 0;
    }
    pushdown(root,l,r);
    int mid=(l+r)/2;
    if(mid>=L)
        update(root*2,l,mid,L,R,num);
    if(mid<R)
        update(root*2+1,mid+1,r,L,R,num);
    sum[root]=sum[2*root]+sum[2*root+1];
}
int main()
{
    int t,cnt;
    scanf("%d",&t);
    while(t--)
    {
        memset(sum,0,sizeof(sum));
        memset(mark,0,sizeof(mark));
        int n,m,ans=0;
        cnt=0;
        scanf("%d%d",&n,&m);
        for(int i=1;i<=m;i++)
        {
            int x1,y1,x2,y2;
            scanf("%d%d%d%d",&x1,&x2,&y1,&y2);
            edge[cnt++]=node{y1,x1,x2,1};
            edge[cnt++]=node{y2,x1,x2,-1};
        }
        sort(edge,edge+cnt,cmp);
        for(int i=1,j=0;i<=n;i++)
        {
            while(j<cnt&&edge[j].h<=i&&edge[j].num==1)
            {
                update(1,1,n,edge[j].a,edge[j].b,1);
                j++;
            }
            ans+=sum[1];
            while(j<cnt&&edge[j].h<=i)
            {
                update(1,1,n,edge[j].a,edge[j].b,1);
                j++;
            }
        }
        cout<<ans<<endl;
    }
    return 0;
}
```

### 博弈论

####  巴什博弈

只有一堆n个物品，两个人从轮流中取出（1~m）个；最后取光者胜。

若n=k*(m+1) 那么先取者必输。

```c++
 int Bash_Game(int n,int m)//是否先手有必赢策略
 {
     if (n%(m+1)!=0) return 1;
     return 0;
 }
```

####  尼姆博弈

有若干堆各若干个物品，两个人轮流从某一堆取任意多的物品，规定每次至少取一个，多者不限，最后取光者得胜。

XOR 判断:

```c++
int Nimm_Game(int n)//假设n个数存在数组f[]中,有必胜策略返回1
{
    int flag=0;
    for(int i=1;i<=n;i++)
    flag^=f[i];
    if(flag) return 1;
    return 0;
}
```

####  威佐夫博奕

有两堆各若干个物品，两个人轮流从某一堆或同时从两堆中取同样多的物品，规定每次至少取一个，多者不限，最后取光者得胜。

 判断是否为奇异局势

设x=![\frac{\sqrt{5}-1}{2}](https://private.codecogs.com/gif.latex?%5Cfrac%7B%5Csqrt%7B5%7D-1%7D%7B2%7D)   k=[a*x]，向上取整

如果a+k=b，则（a，b）为奇异局势，后手胜，反之为先手胜

对应的代码在这里：

```c++
int Wythoff_Game(int a,int b)
{
    if(a>b)
        swap(a,b);
    double x=(sqrt(5.0)-1.0)/2.0;
    int k=ceil(1.0*a*x);
    if(a+k==b)
        return 0;//奇异局势，后手胜！
    else
        return 1;//非奇异局势，先手胜！
}
```
####  SG函数和NIM博弈
[题目题解](https://blog.csdn.net/jk_chen_acmer/article/details/83477602?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_title-0&spm=1001.2101.3001.4242)
[题目链接](http://icpc.upc.edu.cn/problem.php?cid=2810&pid=1)
[SG函数详解](https://blog.csdn.net/strangedbly/article/details/51137432)
```c++
#include <bits/stdc++.h>
using namespace std;
bool vis[300]={0};
int sg[110][110]={0};
void init()
{
    for(int i=0;i<=100;i++)
        sg[i][i]=sg[i][0]=sg[0][i]=199;
    for(int i=0;i<=100;i++)
    {
        for(int j=0;j<=100;j++)
        {
            if(i==j||i==0||j==0)
                continue;
            memset(vis,0,sizeof(vis));
            for(int a=0;a<i;a++)
                vis[sg[a][j]]=1;
            for(int b=0;b<j;b++)
                vis[sg[i][b]]=1;
            for(int c=min(i,j);c>=1;c--)
                vis[sg[i-c][j-c]]=1;
            for(int k=0;;k++)
            {
                if(!vis[k])
                {
                    sg[i][j]=k;
                    break;
                }
            }
        }
    }
}
int main()
{
    int n;
    scanf("%d",&n);
    int a,b;
    int nim=0;
    init();
    for(int i=1;i<=n;i++)
    {
        scanf("%d%d",&a,&b);
        if(a==b)
        {
            printf("Y\n");
            return 0;
        }
        nim^=sg[a][b];
    }
    if(nim!=0)
        printf("Y\n");
    else
        printf("N\n");
}
```

