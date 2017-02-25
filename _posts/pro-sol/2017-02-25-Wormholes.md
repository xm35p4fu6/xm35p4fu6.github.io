---
layout: post
title: poj-3259-Wormholes
categories: pro-sol
excerpt: "找負環"
tag: [poj, bellman-ford, spfa]

---

## 傳送門：

#### [Wormholes](http://poj.org/problem?id=3259)

## 題意：

給定一些邊，輸出有無負環。    

## 思路：

跑跑spfa就可以了。    

## code:

{% highlight cpp linenos %}
  // spfa template

const int NV = 505, NE = 250005, INF = 1<<30;

struct SPFA{
  int n, size, dis[NV], head[NV];
  bool in[NV];

  struct edge {
    int v,w,nxt;
    edge(){}
    edge(int V, int NXT, int W=0): v(V), nxt(NXT), w(W){}
  }E[NE];

  void init(int nn){
    n = nn, size = 0;
    for(int i=0;i<=n;i++)
      head[i]=-1, in[i]=0, dis[i]=INF;
  }

  inline void insert(int u, int v, int w){
    E[size] = edge(v, head[u], w);
    head[u] = size++;
  }
  inline bool relax(int u, int v, int w){
    if(dis[v] == INF or dis[u]+w < dis[v]){
      dis[v] = dis[u] + w;
      return true;
    }
    return false;
  }
  int spfa(int src, int des){   
    int vs[NV] = {0};
    vs[src] = 1;
    queue<int> que;
    dis[src] = 0;
    que.push(src);
    in[src] = true;
    while(!que.empty()){
      int u = que.front();
      que.pop();
      in[u] = false;
      
      for(int i = head[u]; i!=-1; i=E[i].nxt){
        if(relax(u, E[i].v, E[i].w)){
          vs[E[i].v] ++;
          if(vs[E[i].v] == n) return INT_MAX;
          if(!in[E[i].v]){
            in[E[i].v] = true;
            que.push(E[i].v);
          }
        }
      }

    }
    return dis[des];
  }
}G;

int T, a,b,c, N,M,K,graph[505][505];

int main(){
  scanf("%d", &T);
  while(T--){
    scanf("%d %d %d", &N,&M,&K);
    G.init(N);
    for(int i=0;i<=N;i++)for(int j=0;j<=N;j++)
      graph[i][j] = 1<<30;
    while(M--){
      scanf("%d %d %d", &a,&b,&c);
      graph[a][b] = min(graph[a][b], c);
      graph[b][a] = min(graph[b][a], c);
    }
    while(K--){
      scanf("%d %d %d", &a, &b, &c);
      graph[a][b] = min(graph[a][b], -c);
    }
    for(int i=1;i<=N;i++)for(int j=1;j<=N;j++)
      if(i!=j and graph[i][j] != 1<<30)
        G.insert(i,j,graph[i][j]);
    printf(G.spfa(1,N) == INT_MAX ? "YES\n" : "NO\n");
  }
}

{% endhighlight %}

{% highlight cpp linenos %}
#define X first
#define Y second
#define PB push_back
#define MP make_pair
using namespace std;
typedef long long ll;
typedef pair<int, int> pii;
typedef pair<pii, int> p2i;
typedef vector<pii> vii;

ll T,M,N,K,I,a,b,c,ans,cnt;
vector<p2i> edge;
ll dis[501][501];
int ds[2501];
void init(){
  for(int i=1;i<=N;i++)for(int j=1;j<=N;j++)
    dis[i][j] = (i==j?0:(1ll<<30));
  edge.clear();
}
bool suc(){
  memset(ds, 0, sizeof(ds));
  for(int I=0;I<N;I++){		// spfa
    for(int i=0;i<edge.size();i++){
      p2i x = edge[i];
      if(ds[x.X.Y] > ds[x.X.X] + x.Y){
        ds[x.X.Y] = ds[x.X.X] + x.Y;
        if(I == N-1) return true;
      }
    }
  }
  return false;
}
int main(){
  scanf("%lld", &T);
  while(T--){
    scanf("%lld %lld %lld", &N, &M, &K);
    init();
    while(M--){
      scanf("%lld %lld %lld", &a, &b, &c);
      dis[a][b] = min(dis[a][b], c);
      dis[b][a] = min(dis[b][a], c);
    }
    while(K--){
      scanf("%lld %lld %lld", &a, &b, &c);
      dis[a][b] = min(dis[a][b], -c);
    }
    for(int i=1;i<=N;i++){	// 建圖, build graph
      for(int j=1;j<=N;j++)if(i!=j && dis[i][j] != 1ll<<30){
        edge.PB(MP(MP(i,j), dis[i][j])),
        edge.PB(MP(MP(j,i), dis[j][i]));
      }
    }
    printf(suc()?"YES\n":"NO\n");
  }
}

{% endhighlight %}
