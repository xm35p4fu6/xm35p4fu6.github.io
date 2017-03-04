---
layout: post
title: poj-2377-Bad Cowtractors
categories: pro-sol
excerpt: "有點變化的MST"
tag: [poj, mst]

---

## 傳送門：

#### [Bad Cowtractors](http://poj.org/problem?id=2377)

## 思路：

有點變化的MST，權重轉負套上一題 code 殺掉即可。  

## code:

{% highlight cpp linenos %}

struct Node{
  int f, t, l;
  Node(int F=0, int T=0, int L=0):
    f(F), t(T), l(L){}
  inline bool operator < (const Node& a)const{
    return l<a.l;
  }
}e[20005];

int T,M,N,K,I,a,b,c,ans,cnt;
int par[1005];

void init(){
  ans = cnt = 0;
  N = getint(), M = getint();
  for(int i=0;i<N;i++) par[i] = i;
  for(int i=0;i<M;i++){
    a = getint(), b = getint(), c = getint();
    e[i] = Node(a-1,b-1,-c);
  }
}
int _find(int v){
  return ( par[v]==v ? v : par[v]= _find(par[v]));
}
void sol(){
  int t1, t2;
  sort(e, e+M);
  for(int i=0;i<M;i++){
    t1 = _find(e[i].f), t2 = _find(e[i].t);
    if(t1 == t2) continue;
    cnt ++ ;
    ans -= e[i].l;
    (i&1) ? par[t1] = t2 : par[t2] = t1;
  }
  if(cnt < N-1) puts("-1");
  else printf("%d\n", ans);
}
int main(){
  init();
  sol();
}

{% endhighlight %}
