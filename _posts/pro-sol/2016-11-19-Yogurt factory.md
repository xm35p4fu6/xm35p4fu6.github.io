---
layout: post
title: poj-2393-Yogurt factory
categories: pro-sol
excerpt: "greedy"
tag: [greedy]

---

## 傳送門：

#### [Yogurt factory](http://poj.org/problem?id=2393)

## code:

{% highlight cpp linenos %}

#include <iostream>
using namespace std;
int N,M, mn, a,b;
long long cnt;
int main(){
  scanf("%d %d", &N, &M);
  mn = 9999;
  for(int i=0;i<N;i++){
    scanf("%d %d", &a, &b);
    mn = (a < mn + M) ? a : mn+M;
    cnt+=(mn*b);
  }
  printf("%lld\n", cnt);
}

{% endhighlight %}
