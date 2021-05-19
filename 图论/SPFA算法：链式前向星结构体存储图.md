```c++
//SPFA(Shortest Path Faster Algorithm):解决单源最短路径（可带负权）
1.单源最短路
2.判断是否有负权环，即一个点入队次数超过N
```

```c++
//公交线路：单源最短路
#include<bits/stdc++.h>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=1e6+10;
int n,m,x,y,w,k,sign;
int dis[N],vis[N],indegree[N],head[N];
struct box{
    int x,y,w,next;
}G[N];
void add(int x,int y,int w){
    k++;
    G[k].x=x;
    G[k].y=y;
    G[k].w=w;
    G[k].next=head[x];
    head[x]=k;
}
void SPFA(int s){
    sign=0;
    memset(dis,INF,sizeof(dis));
    memset(vis,0,sizeof(vis));
    queue<int>q;
    dis[s]=0;
    vis[s]=1;
    q.push(s);
    while(!q.empty()){
        int x=q.front();q.pop();vis[x]=0;
        for(int i=head[x];i;i=G[i].next){
            int y=G[i].y;
            int w=G[i].w;
            if(dis[y]>dis[x]+w){
                dis[y]=dis[x]+w;
                indegree[y]++;
                if(indegree[y]>n) sign=1;
                if(!vis[y]){
                    vis[y]=1;
                    q.push(y);
                }
            }
        }
    }
}
int main(){
    memset(head,0,sizeof(head));
    cin>>n>>m>>s>>e;
    for(int i=1;i<=m;i++){
        cin>>x>>y>>w;
        add(x,y,w);
        if(w>=0) add(y,x,w);
    }
    SPFA(s);
    cout<<dis[e];
}
//POJ3259:判断是否存在负环（某点入队的次数大于n，该图必然存在负环）
#include<bits/stdc++.h>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=1e6+10;
int n,m,x,y,w,k,T,v,s;
int dis[N],vis[N],indegree[N],head[N];
struct box{
	int x,y,w,next;
}G[N];
void add(int x,int y,int w){
	k++;
	G[k].x=x;
	G[k].y=y;
	G[k].w=w;
	G[k].next=head[x];
	head[x]=k;
}
int SPFA(){
	memset(dis,INF,sizeof(dis));
	memset(vis,0,sizeof(vis));
	memset(indegree,0,sizeof(indegree));
	queue<int>q;
	dis[1]=0;
	vis[1]=1;
	q.push(1);
	while(!q.empty()){
		int x=q.front();q.pop();vis[x]=0;
		for(int i=head[x];i;i=G[i].next){
			int y=G[i].y;
			if(dis[y]>dis[x]+G[i].w){
				dis[y]=dis[x]+G[i].w;
				indegree[y]++;
				if(indegree[y]>n) return 1;
				if(vis[y]==0){
					vis[y]=1;
					q.push(y);
				} 
			}
		}
	}
	return 0;
}
int main()
{
	cin>>T;
	while(T--){
		memset(head,0,sizeof(head));
		cin>>n>>v>>s;
		for(int i=1;i<=v;i++)
		{
			cin>>x>>y>>w;
			add(x,y,w);
			add(y,x,w);
		}
		for(int i=1;i<=s;i++)
		{
			cin>>x>>y>>w;
			add(x,y,-w);
		}
		if(SPFA()==1) cout<<"YES"<<endl;
		else cout<<"NO"<<endl; 
	}
}
//P3385：判断负环：入度大于n
#include<bits/stdc++.h>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=1e6+10;
struct box{
    int x,y,w,next;
}G[N];
int dis[N],vis[N],indegree[N],head[N];
int n,m,x,y,w,k,T;
void add(int x,int y,int w){
    k++;
    G[k].x=x;
    G[k].y=y;
    G[k].w=w;
    G[k].next=head[x];
    head[x]=k;
}
int SPFA(){
    memset(dis,INF,sizeof(dis));
    memset(vis,0,sizeof(vis));
    memset(indegree,0,sizeof(indegree));
    queue<int>q;
    dis[1]=0;
    vis[1]=1;
    q.push(1);
    while(!q.empty()){
        int x=q.front();q.pop();vis[x]=0;
        for(int i=head[x];i;i=G[i].next){//所有x能走的路 
            int y=G[i].y;
            if(dis[y]>dis[x]+G[i].w)
            {
                dis[y]=dis[x]+G[i].w;
                indegree[y]++;
                if(indegree[y]>n) return 0;
                if(vis[y]==0){
                    vis[y]=1;
                    q.push(y);
                }
            }
        }
    }
    return 1;
}
int main(){
    cin>>T;
    while(T--){
        cin>>n>>m;
        memset(head,0,sizeof(head));
        for(int i=1;i<=m;i++){
            cin>>x>>y>>w;
            add(x,y,w);
            if(w>=0) add(y,x,w);
        }
        if(SPFA()==0) cout<<"YES"<<endl;
        else cout<<"NO"<<endl;
    }
}
```