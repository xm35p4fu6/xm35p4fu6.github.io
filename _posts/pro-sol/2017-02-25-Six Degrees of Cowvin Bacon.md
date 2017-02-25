---
layout: post
title: poj-2139-Six Degrees of Cowwin Bacon
categories: pro-sol
excerpt: "最短路徑問題"
tag: [poj, floyd-warshall, shortest path]

---

## 傳送門：

#### [Six Degrees of Cowvin Bacon](http://poj.org/problem?id=2139)

## 題意：

求每頭牛到其他頭牛的最短路徑總和最大者。    

## 思路：

使用floyd-warshall演算法求出每頭牛之間的距離，再統計最大者。    

## code:

{% highlight cpp linenos %}

int T,M,N,K,I,a,b,c,ans,cnt;
int dis[301][301], buf[301];

int main(){
  cin >> N >> M;
  for(int i=1;i<=N;i++)	
    for(int j=1;j<=N;j++)
      dis[i][j] = (i==j?0:(1<<20));

  while(M--){
    cin >> K;
    for(int i=0;i<K;i++) {
      cin >> buf[i];
      for(int j=0;j<i;j++){
        dis[ buf[i] ][ buf[j] ] = 1;
        dis[ buf[j] ][ buf[i] ] = 1;
      }
    }
  }

  for(int k=1;k<=N;k++)
    for(int i=1;i<=N;i++)
      for(int j=1;j<=N;j++)
        dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);

  ll tot,mn=(1ll<<40);
  for(int i=1;i<=N;i++){
    tot = 0;
    for(int j=1;j<=N;j++) 
      tot += dis[i][j];
    if(tot<mn) ans = i, mn = tot;
  }
  cout << mn*100/(N-1) << endl;
}




{% endhighlight %}
