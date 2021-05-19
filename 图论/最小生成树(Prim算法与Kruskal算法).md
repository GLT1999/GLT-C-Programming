### 最小生成树：无向连通图边权和最小的生成树

```c++
/*
Prim算法：存点,不断更新任意两点最小距离
存储：G[N][N]存(x,y,w),初始化：INF
	 visit[N]标记，初始化：0
	 lowcost[N]，初始化：0
Kruskal算法：存边(结构存储+排序+并查集)

注意：最小生成树能够保证整个拓扑图的所有路径之和最小，但不能保证任意两点之间是最短路径
	 最短路径是从一点出发，到达目的地的路径最小
```

```c++
/*
void init()
{
	memset(G,INF,sizeof(G));
	memset(visit,0,sizeof(visit));
	memset(lowcost,0,sizeof(lowcost));
	for(int i=1;i<=m;i++){
		cin>>x>>y>>w;
		G[x][y]=G[y][x]=w;
	}
}
int Prim()
{
	int ans=0,pos=1,minx;
	visit[pos]=1;
	for(int i=1;i<=n;i++) lowcost[i]=G[pos][i];
	for(int i=1;i<n;i++)
	{
		minx=INF;
		for(int j=1;j<=n;j++)
		{
			if(!visit[j]&&minx>lowcost[j])
			{
				minx=lowcost[j];
				pos=j;
			}
		}
		if(minx==INF)
		{
			printf("-1\n");
			return 0;
		}
		visit[pos]=1;
		ans+=minx;
		for(int j=1;j<=n;j++)
		{
			if(!visit[j]&&lowcost[j]>G[pos][j]) lowcost[j]=G[pos][j];
		}
	}
	return ans;
}
*/
```

```c++
//最小生成树唯一性判断与次最小生成树求解
using namespace std;
const int N=501;
const int INF=0x3f3f3f3f;
int G[N][N],vis[N],lowcost[N];
int used[N][N],path[N][N],pre[N];
int n,m,x,y,w;
void init()
{
    memset(G,INF,sizeof(G));
    memset(vis,0,sizeof(vis));
    memset(used,0,sizeof(used));
    memset(path,0,sizeof(path));
    memset(pre,0,sizeof(pre));
    for(int i=1;i<=m;i++)
    {
        cin>>x>>y>>w;
        G[x][y]=G[y][x]=w;
        int a=find_root(x);
        int b=find_root(y);
		if(a!=b) parent[a]=b; 
    }
}
int Prim()
{
    int pos=1,ans=0,sign=1;
    vis[pos]=1;
    for(int i=1;i<=n;i++)
    {
        lowcost[i]=G[pos][i];
        pre[i]=pos;
    }
    for(int i=1;i<n;i++)
    {
        int minx=INF;
        for(int j=1;j<=n;j++)
        {
            if(!vis[j]&&minx>lowcost[j])
            {
                minx=lowcost[j];
                pos=j;
            }
        }
        sign++;
        vis[pos]=1;
        ans+=minx;
        used[pos][pre[pos]]=used[pre[pos]][pos]=1;
        for(int j=1;j<=n;j++)
        {
            if(vis[j]&&j!=pos)
            {
                path[j][pos]=path[pos][j]=max(path[j][pre[pos]],lowcost[pos]);
            }
            if(!vis[j]&&lowcost[j]>G[pos][j])
            {
                lowcost[j]=G[pos][j];
                pre[j]=pos;
            }
        }
    }
    return sign==n?ans:-1;
}
int sec_Prim(int t)
{
    int ans=INF;
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=n;j++)
        {
            if(i!=j&&!used[i][j])
            {
                ans=min(ans,t+G[i][j]-path[i][j]);
            }
        }
    }
    return ans;
}
int main()
{
    while(scanf("%d%d",&n,&m)!=EOF)
    {
    	init();
        int A=Prim();
        int B=sec_Prim(A);
		if(A==B) cout<<A<<endl<<"No"<<endl;
    	else cout<<A<<endl<<"Yes"<<endl;	
	}
}
```

```c++
/*
int parent[N],ranks[N];
int n,m,sign=1,ans=0;
struct box{
	int x,y,w;
	bool operator <(const box &x) const{
		return w<x.w;
	}
}G[3*N];
void init(){
	sign=1,ans=0;
	for(int i=1;i<=N;i++)
	{
		parent[i]=i;
		ranks[i]=0;
	}
}
int find_root(int x){
	if(x==parent[x]) return x;
	else return parent[x]=find_root(parent[x]);
}
int Kruskal(){
	sort(G+1,G+1+m);
	for(int i=1;i<=m;i++){
		int x=G[i].x;
		int y=G[i].y;
		int w=G[i].w;
		int a=find_root(x),b=find_root(y);
		if(a!=b){
			parent[a]=b;
			ans+=w;
			sign++;
		}
	}
	return sign==n?ans:-1;
}
```

```c++
#include<bits/stdc++.h>
using namespace std;
const int N=1e3+10;
int parent[N],ranks[N];
int n,m,sign=1,ans=0;
struct box{
	int x,y,w;
	bool operator <(const box &x) const{
		return w<x.w;
	}
}G[3*N];
void init()
{
	for(int i=1;i<=N;i++)
	{
		parent[i]=i;
		ranks[i]=0;
	}
}
int find_root(int x)
{
	if(x==parent[x]) return x;
	else return parent[x]=find_root(parent[x]);
}
int Kruskal()
{
	sort(G+1,G+m+1);
	for(int i=1;i<=m;i++)
	{
		int x=G[i].x;
		int y=G[i].y;
		int w=G[i].w;
		int a=find_root(x),b=find_root(y);
		if(a!=b){
			parent[a]=b;
			ans+=w;
			sign++;
		}
	}
	return sign==n?ans:-1;
}
int main()
{
	while(scanf("%d%d",&n,&m)!=EOF)
	{
		init();
		for(int i=1;i<=m;i++)
		{
			int x,y,w;
			cin>>x>>y>>w;
			G[i]={x,y,w};
		}
		cout<<Kruskal()<<endl;
	}
	return 0;
}
```

