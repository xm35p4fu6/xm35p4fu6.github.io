---
layout: post
title: 2236_Wireless Network
categories: pro-sol
excerpt: "disjoint set, union find"
tag: [poj, union-find]

---

## 傳送門：

#### [Wireless Network](http://poj.org/problem?id=2236)

## code:

{% highlight cpp linenos %}

int T,M,N,K,I,a,b,c,ans,cnt,d;
ll D;
int par[1005], rk[1005];
pii pos[1005];
bool ok[1005];

inline ll dis(int a, int b){
  return ((pos[a].X-pos[b].X)*(pos[a].X-pos[b].X) + 
    (pos[a].Y-pos[b].Y)*(pos[a].Y-pos[b].Y));
}

int _find(int v){
  return ((par[v] == v) ? v : par[v] = _find(par[v]) );
}

void merge(int a, int b){
  int t1 = _find(a), t2 = _find(b);
  if(t1 == t2) return;
  rk[t1] > rk[t2] ? par[t2] = t1 : par[t1] = t2;
  if(rk[t1] == rk[t2]) rk[t2]++;
}

set<int> s;
set<int>::iterator si;
void repair(int v){
  s.clear();
  for(int i=0;i<N;i++)if(ok[i] && i!=v && dis(i,v)<=D)
    s.insert(_find(i));
  si = s.begin();
  if(si != s.end()){
    for(si++;si!=s.end();si++)
      merge(*(s.begin()), *si);
    merge(*(s.begin()), v);
  }
  ok[v] = 1;
}

int main(){
  scanf("%d %d", &N, &d);
  fill(rk, rk+N, 1);
  D = d*d;
  for(int i=0;i<N;i++){
    scanf("%d %d", &(pos[i].X), &(pos[i].Y));
    par[i] = i;
  }
  char ch[2];
  while(~scanf("%s", ch)){
    if(ch[0] == 'O'){
      cin>>a;
      repair(a-1);
    }
    else{
      cin>>a>>b;
      printf((_find(a-1) == _find(b-1)) ? "SUCCESS\n" : "FAIL\n");
    }
  }
}

{% endhighlight %}
