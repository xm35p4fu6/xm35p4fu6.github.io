---
layout: post
title: poj-3292-Semi-prime H-numbers
categories: pro-sol
excerpt: "4k+1質數題"
tag: [poj, math]

---

## 傳送門：

#### [Semi-prime H-numbers](http://poj.org/problem?id=3292)

## code:

{% highlight cpp linenos %}

bool is_pr[1000005];
int rev[1000005];
vector<ll> h, sp, pr;
vector<ll>::iterator iv;
void build(){
  fill(is_pr, is_pr+1000000, 1);
  for(int i=0;(i<<2)+1<1000005;i++){
    rev[(i<<2)+1] = h.size();
    h.PB((i<<2)+1);
  }
  for(int i=1;i<h.size();i++)if(is_pr[i]){
    pr.PB(h[i]);
    for(int j=i;h[i]*h[j]<1000005ll;j++){
      is_pr[rev[h[i]*h[j]]] = 0;
    }
  }
  for(int i=0;i<pr.size();i++)for(int j=0;j<pr.size() && pr[i]*pr[j] < 1000005ll;j++)
    sp.PB(pr[i]*pr[j]);
  sort(sp.begin(), sp.end());
  iv = unique(sp.begin(), sp.end());
  sp.erase(iv, sp.end());
  
}
#define mid ((L+R)>>1)
inline bool suc(int x){
  return (sp[x] <= a);
}
int main(){
  build();
  while(cin>>a && a){
    int L=0, R=sp.size();
    while(R-L>1)
      suc(mid) ? L=mid : R=mid;
    cout << a << " " << (L>0 ? L+1 : 0) << endl;
  }
}

{% endhighlight %}
