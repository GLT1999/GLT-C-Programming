

```c++
//迪杰斯特拉（Dijkstra）算法：解决单源最短路径
#include<bits/stdc++.h>
using namespace std;
const int N=1001;
const int INF=0x3f3f3f;
int n,m,b,e;
struct box
{
    int post,w,cost;
};
vector<box>G[N];
int visit[N];
int dis[N],cost[N];
void Dijkstra(int B)
{
    memset(dis,INF,sizeof(dis));
    memset(cost,INF,sizeof(cost));
    memset(visit,0,sizeof(visit));
    dis[B]=0;
    cost[B]=0;
    visit[B]=1;
    int sign=B;
    while(sign!=e)
    {
        for(int i=0;i<G[sign].size();i++)
        {
            int post=G[sign][i].post;
            int w=G[sign][i].w;
            int c=G[sign][i].cost;
            if(!visit[post]&&dis[sign]+w<dis[post])
            {
                dis[post]=dis[sign]+w;
                cost[post]=cost[sign]+c;
            }
            else if(!visit[post]&&dis[sign]+w==dis[post])
            {
                if(cost[sign]+c<cost[post]) cost[post]=cost[sign]+c;
            }
        }
        int mindis=INF;
        for(int i=0;i<n;i++)
        {
            if(!visit[i]&&dis[i]<mindis)//FW
            {
                mindis=dis[i];
                sign=i;
            }
        }
        visit[sign]=1;
    }
}
int main()
{
    int pos;
    while(scanf("%d%d%d%d",&n,&m,&b,&e)!=EOF)
    {
        box val;
        memset(G,0,sizeof(G));
        for(int i=0;i<m;i++)
        {
            cin>>pos>>val.post>>val.w>>val.cost;
            G[pos].push_back(val);
            swap(pos,val.post);
            G[pos].push_back(val);
        }
        Dijkstra(b);
        cout<<dis[e]<<" "<<cost[e]<<endl;
    }
    return 0;
}
```

```c++
//PTA城市间紧急救援
#include<bits/stdc++.h>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=1001;
int dis[N],vis[N],res[N],pre[N],teams[N],t[N];
int G[N][N];
int n,m,x,y,w,s,e;
vector<int>v;
void init(){
	memset(G,INF,sizeof(G));
	memset(dis,0,sizeof(dis));
	memset(vis,0,sizeof(vis));
	memset(res,0,sizeof(res));
	memset(pre,-1,sizeof(pre));
	v.clear(); 
} 
void Dijkstra(int s){
	for(int i=0;i<n;i++) dis[i]=G[s][i];
	dis[s]=0;
	vis[s]=1;
	res[s]=1;
	while(s!=e){
		for(int i=0;i<n;i++){
			if(!vis[i]){
				if(dis[i]>dis[s]+G[s][i]){
					dis[i]=dis[s]+G[s][i];
					res[i]=res[s];
					pre[i]=s;
					teams[i]=teams[s]+t[i];
				}
				else if(dis[i]==dis[s]+G[s][i]){
					res[i]+=res[s];
					if(teams[i]<teams[s]+t[i]){
						teams[i]=teams[s]+t[i];
						pre[i]=s;
					}
				} 
			}
		}
		int sign=INF;
		for(int i=0;i<=n;i++){
			if(!vis[i]&&sign>dis[i]){
				sign=dis[i];
				s=i;
			}
		}
		vis[s]=1;
	} 
}
int main(){
	init();
	cin>>n>>m>>s>>e;
	for(int i=0;i<n;i++){
		cin>>t[i];
		teams[i]=t[i];
	} 
	for(int i=0;i<m;i++){
		cin>>x>>y>>w;
		G[x][y]=G[y][x]=w;
	}
	Dijkstra(s);
	printf("%d %d\n",res[e],teams[e]);
	int Begin=e;
	while(Begin!=-1){
		v.push_back(Begin);
		Begin=pre[Begin];
	}
	for(int i=v.size()-1;i>=0;i--){
		if(i==0) cout<<v[i];
		else cout<<v[i]<<" ";
	}
}
```

