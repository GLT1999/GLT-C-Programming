```c++
//弗洛伊德算法：解决多源最短路径(顶点间最长中的最短)
1.多源最短路（任意两点最短/最大路径长度/中间关键点/任意两点路径条数）
2.最小的最大边权值/最大边权的最小值
3.最大的最小边权值/最小边权的最大值
4.最小环问题
/*
void init(){
	memset(G,INF,sizeof(G));
	memset(path,-1,sizeof(path));
	for(int i=1;i<=m;i++){
		cin>>x>>y>>w;
		G[x][y]=G[y][x]=w;
	}
}
void Floyd(){
	for(int k=1;k<=n;k++){
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++){
				G[i][j]=min(G[i][j],G[i][k]+G[k][j]);
				//G[i][j]=max(G[i][j],G[i][k]+G[k][j]);
			}
		}
	}
}
void Floyd(){
	for(int k=1;k<=n;k++){
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++){
				G[i][j]=max(G[i][j],min(G[i][k],G[k][j]));
			}
		}
	}
}
void Floyd(){
	for(int k=1;k<=n;k++){
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++){
				G[i][j]=min(G[i][j],max(G[i][k],G[k][j]));
			}
		}
	}
}
*/
```

```c++
//P2935：多源最短路（任意两点最短路径）
#include<bits/stdc++.h>
using namespace std;
const int INF=0x3f3f3f3f;//INF = 1e8;
const int N=501;
int n,r,m,x,y,w,sign[N],G[N][N],path[N][N],res[N][N],mp[N][N];
int main()
{
	while(cin>>n>>r>>m)
	{
		int pos=0,ans=INF;
		memset(path,0,sizeof(path));
		memset(res,0,sizeof(res));
/*		memset(G,INF,sizeof(G));
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++){
				if(i!=j) G[i][j]=G[j][i]=mp[i][j]=mp[j][i]=INF;
			}
		}
*/
		for(int i=1;i<=r;i++) cin>>sign[i];
		for(int i=1;i<=m;i++){
			cin>>x>>y>>w;
			G[x][y]=G[y][x]=min(G[x][y],w);
		}
		for(int k=1;k<=n;k++){
			for(int i=1;i<=n;i++){
				for(int j=1;j<=n;j++){
					G[i][j]=min(G[i][j],G[i][k]+G[k][j]);
				}
			}
		}
		for(int i=1;i<=n;i++){
			int sum=0;
			for(int j=1;j<=r;j++) sum+=G[i][sign[j]];
			if(ans>sum){
				ans=sum;
				pos=i;
			} 
		}
		cout<<pos<<endl;
	}
}
//P1841： 多源最短路（中间关键城市）
#include<bits/stdc++.h>
using namespace std;
const int INF=0x3f3f3f;//INF = 1e8
const int N=201;
int n,m,x,y,w,G[N][N],path[N][N],res[N][N],mp[N][N];
int main()
{
	while(cin>>n>>m)
	{
		int box[N],flag=0;
		memset(G,INF,sizeof(G));
		memset(path,0,sizeof(path));
		memset(res,0,sizeof(res));
		for(int i=1;i<=m;i++){
			cin>>x>>y>>w;
			G[x][y]=G[y][x]=min(G[x][y],w);
		}
		for(int k=1;k<=n;k++){
			for(int i=1;i<=n;i++){
				for(int j=1;j<=n;j++){
					if(G[i][j]>G[i][k]+G[k][j])
					{
						G[i][j]=G[i][k]+G[k][j];
						path[i][j]=k;
					}
					else if(G[i][j]==G[i][k]+G[k][j]) path[i][j]=0;
				}
			}
		}
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++) box[path[i][j]]=1;
		}
		for(int i=1;i<=n;i++) if(box[i]==1) printf("%d ",i),flag=1;
		if(!flag) printf("No important cities.\n");
	}
}
//NOI2007：多源最短路（任意两点路径条数）
#include<bits/stdc++.h>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=105;
int n,m,x,y,w,G[N][N],path[N][N],mp[N][N];
double res[N][N];
int main(){
	while(cin>>n>>m)
	{
		memset(G,INF,sizeof(G));
		memset(path,0,sizeof(path));
		memset(res,0,sizeof(res));
		for(int i=1;i<=m;i++){
			cin>>x>>y>>w;
			G[x][y]=G[y][x]=min(G[x][y],w);
			res[x][y]=res[y][x]=1;
		}
		for(int k=1;k<=n;k++){
			for(int i=1;i<=n;i++){
				for(int j=1;j<=n;j++){
					if(G[i][j]>G[i][k]+G[k][j])
					{
						G[i][j]=G[i][k]+G[k][j];
						res[i][j]=res[i][k]*res[k][j];
					}
					else if(G[i][j]==G[i][k]+G[k][j]) res[i][j]+=res[i][k]*res[k][j];
				} 
			}
		}
        /*
        for(int i=1;i<=n;i++){
        	for(int j=i+1;j<=n;j++){
        		cout<<res[i][j]<<" ";
        	}
        }
        */
		for(int k=1;k<=n;k++){
			double ans=0;
			for(int i=1;i<=n;i++){
				for(int j=1;j<=n;j++){
					if(i==j||i==k||j==k) continue;
					if(G[i][j]==G[i][k]+G[k][j]) ans+=res[i][k]*res[k][j]*1.0/res[i][j];
				}
			}
			printf("%.3f\n",ans); 
		}
	}
} 
```

```c++
//UVA - 10048:最小的最大边权值/最大边权的最小值
#include<bits/stdc++.h>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=101;
int n,m,k,x,y,w,G[N][N],path[N][N],res[N][N],mp[N][N];
void Floyd(){
	for(int k=1;k<=n;k++){
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++){
				G[i][j]=min(G[i][j],max(G[i][k],G[k][j]));
			} 
		}
	}
}
int main(){
	int sign=1;
	while(cin>>n>>m>>k,n!=0&&m!=0&&k!=0)
	{
		memset(G,INF,sizeof(G));
		memset(path,0,sizeof(path));
		for(int i=1;i<=m;i++){
			cin>>x>>y>>w;
			G[x][y]=G[y][x]=min(G[x][y],w);
		}
		Floyd();
		printf("Case #%d\n",sign++);
		for(int i=1;i<=k;i++){
			cin>>x>>y;
			if(G[x][y]==INF) printf("no path\n");
			else printf("%d\n",G[x][y]);
		}
		cout<<endl; 
	}
}
//POJ2253 Frogger：最小的最大边权值/最大边权的最小值
#include<bits/stdc++.h>
using namespace std;
const int N=201;
const int INF=0x3f3f3f3f;
struct box{
	double x,y;
}point[1100];
double G[N][N],path[N][N],res[N][N],mp[N][N];
int n,t=0;
void Floyd()
{
	for(int k=1;k<=n;k++){
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++){
				G[i][j]=min(G[i][j],max(G[i][k],G[k][j]));
			}
		}
	}
}
int main()
{
	while(scanf("%d",&n)&&n)
	{
		t++;
		memset(G,INF,sizeof(G));
		memset(path,-1,sizeof(path));
		for(int i=1;i<=n;i++) cin>>point[i].x>>point[i].y;
		for(int i=1;i<=n;i++)
		{
			for(int j=1;j<=n;j++){
				double a=point[i].x-point[j].x;
				double b=point[i].y-point[j].y;
				G[i][j]=G[j][i]=sqrt(pow(a,2)+pow(b,2));		
			}
		}
		Floyd(); 
		printf("Scenario #%d\nFrog Distance = %.3lf\n\n",t,G[1][2]);
	}
}
```

```c++
//最小环问题:find the mincost route
#include<bits/stdc++.h>
using namespace std;
const int N=110;
const int INF=1e8;
int n,m,x,y,w,G[N][N],mp[N][N],res=INF;
void Floyd(){
	res=INF;
	for(int k=1;k<=n;k++){
		for(int i=1;i<k;i++){
			for(int j=i+1;j<k;j++){
				res=min(res,G[i][j]+mp[i][k]+mp[k][j]);
			} 
		}
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++){
				G[i][j]=min(G[i][j],G[i][k]+G[k][j]);
			}
		}
	}
}
int main(){
	while(cin>>n>>m){
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++){
				if(i!=j) G[i][j]=mp[i][j]=INF;
			}
		}
		while(m--){
			cin>>x>>y>>w;
			G[x][y]=G[y][x]=mp[x][y]=mp[y][x]=min(G[x][y],w); 
		}
		Floyd();
		if(res==INF) cout<<"It's impossible."<<endl;
		else cout<<res<<endl;
	}
}
```

