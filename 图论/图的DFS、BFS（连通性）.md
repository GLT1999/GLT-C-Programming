#### 图的连通性（DFS、BFS、并查集）

```c++
/*
判断是否有欧拉回路/汉密尔顿回路？
无向欧拉图：当且仅当Graph连通且没有奇度顶点
有向欧拉图：当且仅当Graph所有顶点属于同一个强连通分量且每个顶点的入度和出度相同
汉密尔顿图：1：序列首尾元素相同 2：相邻顶点连通 3：各顶点度不为0且小于2（首顶点除外）
*/
```

```c++
void init()
{
    memset(G,0,sizeof(G));
    memset(visit,0,sizeof(visit));
}
void DFS(int x)
{
    visit[x]=1;
    cout<<x<<" ";
    //sign++;
    for(int i=0;i<n;i++){
        if(!visit[i]&&G[x][i]) DFS(i);
    }
}
void BFS(int x)
{
    queue<int>q;
    q.push(x);
    visit[x]=1;
    while(!q.empty())
    {
        int x=q.front();q.pop();
        cout<<x<<" ";
        //sign++;
        for(int i=0;i<n;i++)
        {
            if(!visit[i]&&G[x][i]){
                q.push(i);
                visit[i]=1;
            }
        }
    }
}
```

```c++
//所有节点可能的连通性
#include<bits/stdc++.h>
using namespace std;
const int N=101;
int G[N][N],visit[N];
int v,e;
void DFS(int x)
{
    visit[x]=1;
    printf("%d ",x);
    for(int i=0;i<v;i++)
    {
        if(!visit[i]&&G[x][i]) DFS(i);
    }
}
void BFS(int x)
{
    queue<int>q;
    q.push(x);
    visit[x]=1;
    while(!q.empty())
    {
        int k=q.front();q.pop();
        printf("%d ",k);
        for(int i=0;i<v;i++)
        {
            if(!visit[i]&&G[k][i])
            {
                visit[i]=1;
                q.push(i);
            }
        }
    }
}
int main()
{
    int x,y;
    while(scanf("%d %d",&v,&e)!=EOF)
    {
        memset(G,0,sizeof(G));
        memset(visit,0,sizeof(visit));
        for(int i=0;i<e;i++)
        {
            cin>>x>>y;
            G[x][y]=G[y][x]=1;
        }
        for(int i=0;i<v;i++)
        {
            if(!visit[i])
            {
                printf("{ ");
                DFS(i);
                printf("}\n");
            }
        }
        memset(visit,0,sizeof(visit));
        for(int i=0;i<v;i++)
        {
            if(!visit[i])
            {
                printf("{ ");
                BFS(i);
                printf("}\n");
            }
        }
    }
    return 0;
}
```

```C++
/*
欧拉回路：若图中从某点出发，经过所有边一次后又重新回到起点
有向图：图连通，所有的顶点出度=入度
无向图：图连通，所有顶点都是偶数度(前提)
*/
#include<bits/stdc++.h>
using namespace std;
const int N=1001;
int G[N][N],visit[N],A[N];
int v,e,sign=0;
vector<int>ans;
//void BFS(int x)
//{
//	queue<int>q;
//	q.push(x);
//	visit[x]=1;
//	ans.push_back(x);
//	while(!q.empty())
//	{
//		int k=q.front();q.pop();
//		for(int i=1;i<=v;i++)
//		{
//			if(!visit[i]&&G[k][i])
//			{
//				visit[i]=1;
//				q.push(i);
//				ans.push_back(i);
//			}
//		}
//	}
//}
void DFS(int x)
{
	visit[x]=1;
	ans.push_back(x);
	for(int i=1;i<=v;i++)
	{
		if(!visit[i]&&G[x][i]) DFS(i);
	}
 } 
int main()
{
	int x,y;
	while(scanf("%d %d",&v,&e)!=EOF)
	{
		memset(G,0,sizeof(G));
		memset(visit,0,sizeof(visit));
		for(int i=1;i<=e;i++)
		{
			cin>>x>>y;
			G[x][y]=G[y][x]=1;
			A[x]++;
			A[y]++;
		}
		for(int i=1;i<=v;i++)
		{
			if(A[i]%2!=0)
			{
				sign=1;
				break;
			}
		}
//		BFS(1);
		DFS(1);
		if(ans.size()==v&&sign==0) cout<<1<<endl;
		else cout<<0<<endl;
	}
	return 0;
}

解法二：（一个点A不了）
#include<bits/stdc++.h>
using namespace std;
const int N=1001;
int G[N][N],visit[N],box[N];
int parent[N],ranks[N];
int v,e;
void init()
{
	for(int i=1;i<=N;i++)
	{
		parent[i]=i;
		ranks[i]=1;
	}
}
int find_root(int x)
{
	if(x==parent[x]) return x;
	else return parent[x]=find_root(parent[x]);
}
int main()
{
	int x,y;
	while(scanf("%d %d",&v,&e)!=EOF)
	{
		int sign=0;
//		memset(G,0,sizeof(G));
//		memset(visit,0,sizeof(visit)); 
		memset(box,0,sizeof(box));
		for(int i=1;i<=e;i++)
		{
			cin>>x>>y;
//			G[x][y]=G[y][x]=1;
			int a=find_root(x);
			int b=find_root(y);
			if(a!=b) parent[a]=b;
			box[x]++;box[y]++;	
		}
		for(int i=1;i<=v;i++)
		{
			if(box[i]%2!=0)
			{
				cout<<0<<endl;
                return 0;
			}
		}
        for(int i=2;i<=v;i++)
        {
            if(find_root(1)!=find_root(i))
            {
                cout<<0<<endl;
                return 0;
            }
        }
        cout<<1<<endl;
	} 
}

/*
汉密尔顿回路：若图中从某点出发，经过所有点一次后又重新回到起点
1.数列起点与终点相同
2.数列每相邻两个点间为通路
3.除起点外，其他所有点都出现过一次（0次或2次以上都不行）
*/
#include<bits/stdc++.h>
using namespace std;
const int N=201;
const int INF=0x3f3f3f3f;
int G[N][N],visit[N];
int n,m,k;
int main()
{
	int x,y,val;
	while(scanf("%d%d",&n,&m)!=EOF)
	{
		memset(G,0,sizeof(G));
		for(int i=1;i<=m;i++)
		{
			cin>>x>>y;
			G[x][y]=G[y][x]=1;
		}
		cin>>m;
		while(m--)
		{
			int sign=1;
			memset(visit,0,sizeof(visit));
			vector<int>v;
			cin>>k;
			for(int i=0;i<k;i++)
			{
				cin>>val;
				v.push_back(val);
			}
			if(v[0]!=v[v.size()-1]) sign=0;
			visit[v[0]]++;
			for(int i=1;i<v.size();i++)
			{
				visit[v[i]]++;
				if(G[v[i-1]][v[i]]!=1) sign=0;
			}
			for(int i=1;i<=n;i++)
			{
				if(visit[i]==0||(visit[i]>=2&&i!=v[0])) sign=0;
			}
			if(sign==1) cout<<"YES"<<endl;
			else cout<<"NO"<<endl;
		}
	}
}
```

```c++
//图的连通性+改善BFS：求与原结点距离不超过k的所有结点
const int N=1001;
int G[N][N],visit[N];
int v,e;
int BFS(int x)
{
	int ans=1;
	queue<int>q;
	q.push(x);
	visit[x]=1;
	for(int i=0;i<6;i++)
	{
		vector<int>res;
		while(!q.empty()){
			int sign=q.front();q.pop();
			res.push_back(sign);
		}
		for(int i=0;i<res.size();i++){
			int sign=res[i];
			for(int j=1;j<=v;j++)
			{
				if(!visit[j]&&G[sign][j]){
					visit[j]=1;
					ans++;
					q.push(j);
				}
			}
		}
	}
	return ans;
}
int main()
{
	int x,y;
	while(scanf("%d%d",&v,&e)!=EOF)
	{
		memset(G,0,sizeof(G));
		memset(visit,0,sizeof(visit));
		for(int i=0;i<e;i++)
		{
			cin>>x>>y;
			G[x][y]=G[y][x]=1;
		}
		for(int i=1;i<=v;i++)
		{
			memset(visit,0,sizeof(visit));
			printf("%d: %.2f%%\n",i,BFS(i)*1.0/v*100);
		}
	}
}
```

```c++
//判断图的连通性，并回溯
#include<bits/stdc++.h>
using namespace std;
const int N=1001;
int n,m,sign,res;
int G[N][N],visit[N],box[N];
void DFS(int x)
{
    visit[x]=1;
    if(sign==0)
    {
        sign=1;
        cout<<x;
    }
    else cout<<" "<<x;
    for(int i=1;i<=n;i++)
    {
        if(!visit[i]&&G[x][i])
        {
            visit[i]=1;
            res++;
            DFS(i);
            cout<<" "<<x;
        }
    }
}
void BFS(int x)//过不了
{
    queue<int>q;
    visit[x]=1;
    q.push(x);
    while(!q.empty())
    {
        int g=q.front();q.pop();
        if(sign==0) cout<<g;
        else cout<<" "<<g;
        box[sign++]=g;
        for(int i=1;i<=n;i++)
        {
            if(!visit[i]&&G[g][i])
            {
                visit[i]=1;
                q.push(i);
            }
        }
    }
}
int main()
{
    int x,y,k;
    while(scanf("%d%d%d",&n,&m,&k)!=EOF)
    {
        memset(G,0,sizeof(G));
        memset(visit,0,sizeof(visit));
        res=1;
        for(int i=1;i<=m;i++)
        {
            cin>>x>>y;
            G[x][y]=G[y][x]=1;
        }
        DFS(k);
        //BFS(k);
        //for(int i=sign-1;i>=0;i--) cout<<" "<<box[i];
        if(res<n) cout<<" "<<0;
    }
    return 0;
}
```

