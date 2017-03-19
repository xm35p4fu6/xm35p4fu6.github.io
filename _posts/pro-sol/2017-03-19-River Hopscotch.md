---
layout: post
title: poj-3258 River Hopscotch
categories: pro-sol
excerpt: "最小值最大化"
tag: [poj, binary search]

---

## 傳送門：

#### [River Hopscotch](http://poj.org/problem?id=3258)

## 題意：

移除和中央的一些石頭，使得最短距離最大。   

## 思路：

二分搜答案即可, 二分搜的判定function仔細寫好就沒啥大問題了。   

## code:

{% highlight cpp linenos %}

inline bool suc(const ll &x){
  a = 0, b = M;
  for(int i=0;i<N;++i){
    while(arr[i]-a < x){
      if(b-- == 0)  return false;
      if(++i == N){
        if(End-a < x)  return false;
        return true;
      }
    }
    a = arr[i];
  }
  return (!(End-a < x and b <= 0) );
}

ll sol(){
  R = L = End;
  R++;
  for(int i=1;i<N;i++)
    L = min(L,arr[i]-arr[i-1]);
  L = min(L,End-arr[N-1]);
  if(R == L) return L;
#define mid ((L+R)>>1)
  while(R-L>1) 
    suc(mid) ? L=mid : R=mid;
  return L;
}
int main(){
  init();
  cout << sol() << endl;
}

{% endhighlight %}
