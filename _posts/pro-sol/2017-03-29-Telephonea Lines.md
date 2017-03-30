---
layout: post
title: poj-3662 Telephone Lines
categories: pro-sol
excerpt: "路徑上的最大值最小化"
tag: [poj, binary search, dijkstra]

---

## 傳送門：

#### [Telephone Lines](http://poj.org/problem?id=3662)

## 題意：

從第１點走到第Ｎ點（不必經過所有點）經過的邊，權重最大的儘量小，並且有Ｋ條道路可當作權重為０，求最大權重。    

## 思路：

二分搜最大權重，跑一個特殊的dijkstra算法，Key值是該點剩餘的Ｋ，需注意最後答案可能為０。   

## code:

{% highlight cpp linenos %}

int N,P,K, a,b,l, mx;

vii G[1005];
bool visit[1005];
void init(){
  N = getint(), P = getint(), K = getint();
  mx = 0;
  while(P--){
    a = getint(), b = getint(), l = getint();
    mx = max(mx, l);
    G[a].PB(MP(b,l));
    G[b].PB(MP(a,l));
  }
}

typedef priority_queue<pii> pq;
pq q;
int karr[1005];

inline bool suc(int x){
  memset(visit, 0, sizeof(visit));
  fill(karr, karr+N+1, -INT_MAX);
  q = pq();
  karr[1] = K;
  q.push(MP(K,1));
  while(!q.empty()){
    int k = q.top().X, v = q.top().Y;
    q.pop();
    if(visit[v]) continue;
    visit[v] = 1;
    for(int i=0;i<G[v].size();i++){
      int vv = G[v][i].X, len = G[v][i].Y;
      if(!visit[vv] and ((len<=x and k>karr[vv]) or (k>0 and k-1>karr[vv]))){
        if(vv == N) return true;
        if(len <= x){
          q.push(MP(k, vv));
          karr[vv] = k;
        }
        else{
          q.push(MP(k-1, vv));
          karr[vv] = k-1;
        }
      }
    }
  }
  return false;
}

void sol(){
  visit[1] = 1;
#define mid ((L+R)>>1)
  int L = -1, R = mx;   // 注意L為-1才能做到 suc(0) 的case
  if(!suc(mx)){
    puts("-1");
    return;
  }
  while(R-L>1) suc(mid) ? R=mid : L=mid;
  cout << R << endl;
}

int main(){
  init();
  sol();
}

{% endhighlight %}
