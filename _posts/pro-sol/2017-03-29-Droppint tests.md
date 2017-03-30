---
layout: post
title: poj-2976 Dropping tests
categories: pro-sol
excerpt: "平均最大化"
tag: [poj, binary search, math]

---

## 傳送門：

#### [Dropping tests](http://poj.org/problem?id=2976)

## 思路：

跟 **[這題](/pro-sol/K-Best/)** 相似，換湯不換藥，參考一下即可。

## code:

{% highlight cpp linenos %}

int N,K;
double eps = 1e-3;
ll sa[1005], sb[1005];
long double sorting[1005];
void init(){
  for(int i=0;i<N;i++)
    sa[i] = getint() * 100;
  for(int i=0;i<N;i++)
    sb[i] = getint();
}
inline bool suc(long double x){
  long double cnt = 0.0;
  for(int i=0;i<N;i++)
    sorting[i] = (long double)sa[i] - x*sb[i];
  sort(sorting, sorting+N, greater<long double>());
  for(int i=0;i<N-K;i++) cnt += sorting[i];
  return cnt > -eps;
}
int sol(){
  long double L = 0.0, R = 101;
#define mid ((L+R)/2)
  while(R-L>eps) suc(mid) ? L=mid : R=mid;
  return int(L+0.5);
}
int main(){
  while(cin >> N >> K && N+K){
    init();
    cout << sol() << endl;
  }
}

{% endhighlight %}
