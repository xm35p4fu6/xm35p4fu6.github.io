---
layout: post
title: poj-3273 Monthly Expense
categories: pro-sol
excerpt: "最大值最小化"
tag: [poj, binary search]

---

## 傳送門：

#### [Monthly Expense](http://poj.org/problem?id=3273)

## 題意：

將數列切成連續的幾個區間，使得總和最大的區間儘量小。   

## 思路：

二分搜答案即可, 二分搜的判定function仔細寫好就沒啥大問題了。   

## code:

{% highlight cpp linenos %}

void init(){
  N = getint(), M = getint();
  for(int i=0;i<N;i++){
    arr[i] = getint();
    L = max(L,arr[i]);
    R += arr[i];
  }
  --L;
}

inline bool suc(const ll &x){
  cnt = sum = 0;
  for(int i=0;++cnt<=M;){
    while(i<N and sum+arr[i]<=x)
      sum += arr[i++];
    if(i==N) return true;
    sum = arr[i++];
  }
  return false;
}

inline ll sol(){
#define mid ((R+L)>>1)
  while(R-L>1)
    suc(mid) ? R=mid : L=mid;
  return R;
}

int main(){
  init();
  cout << sol() << endl;
}


{% endhighlight %}
