---
layout: post
title: poj-3268-Silver Cow Party
categories: pro-sol
excerpt: "最短路徑"
tag: [poj, dijkstra]

---

## 傳送門：

#### [Silver Cow Party](http://poj.org/problem?id=3268)

## 題意：

有一群牛都要走最短路徑到同一點，之後再走最短路徑折返，求總花費最高者。    

## 思路：

Ａ點到Ｂ點的最短路徑，在所有邊反轉之後等同於Ｂ點到Ａ點的最短路徑，由此可知當我們欲求所有點到某一點的最短路徑，可以將邊反轉後求此點到所有點最短路徑；所以這題直接跑一次dijkstra，再將邊反轉後也跑一次即可。    

## code:

{% highlight cpp linenos %}

#define X first
#define Y second
#define PB push_back
#define MP make_pair

using namespace std;
typedef long long ll;
typedef pair<int, int> pii;
typedef vector<pii> vii;
typedef priority_queue<pii, vii, greater<pii> > pqQAQ;

int T,M,N,K,I,a,b,c,ans,cnt;
int dis[1005], disr[1005];
bool visit[1005];
vii G[1005], G2[1005];
pqQAQ pq;

void sol(int ds[], vii g[]){
  pq = pqQAQ ();
  memset(visit, 0, sizeof(visit));
  pq.push(MP(0,K));
  while(!pq.empty()){
    int w = pq.top().X, v = pq.top().Y;
    pq.pop();
    if(visit[v]) continue;
    visit[v] = 1;
    for(int i=0;i<g[v].size();i++)if(!visit[g[v][i].X]){
      int vv = g[v][i].X, ww = g[v][i].Y;
      if(ds[vv] > w + ww){
        ds[vv] = w + ww;
        pq.push(MP(ds[vv], vv));
      }
    }
  }
}

int main(){
  cin>>N>>M>>K;
  --K;
  for(int i=0;i<N;i++) dis[i] = disr[i] = (1<<30);
  while(M--){
    cin>>a>>b>>c;
    --a,--b;
    G[a].PB(MP(b,c));	// 實作時直接建兩張圖即可
    G2[b].PB(MP(a,c));
  }
  dis[K] = disr[K] = 0;
  sol(dis, G);
  sol(disr, G2);
  for(int i=0;i<N;i++)if(i!=K)
    ans = max(ans, dis[i]+disr[i]);
  cout<<ans<<endl;
}

{% endhighlight %}
