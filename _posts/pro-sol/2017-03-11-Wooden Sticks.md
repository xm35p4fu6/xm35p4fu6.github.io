---
layout: post
title: poj-1065 Wooden Sticks
categories: pro-sol
excerpt: "Dilworth"
tag: [poj, lis, dilworth]

---

## 傳送門：

#### [Wooden Sticks](http://poj.org/problem?id=1065)

## 題意：

機器要處理的木頭有兩個參數 A, B，如果上次處理的木頭，這兩個參數有任何一個大於這次要處理的，時間 + 1，求處理完所有木頭的最短時間。    

## 思路：

很容易就可以把題目化簡為，排序後，求看看可以找出幾條 LIS，可以直接做，或是反過來求 LDS 的長度（[Dilworth 定理](/articles/poset/)）。

## code:

{% highlight cpp linenos %}

void init(){
  memset(dp, -1, sizeof(dp));
  N = getint();
  for(int i=0;i<N;i++)
    arr[i].X = getint(), arr[i].Y = getint();
  sort(arr, arr+N);
}
void sol(){
  dp[0] = INT_MAX;
  for(int i=0;i<N;i++)
    for(int j=0;j<=N && dp[j]!=-1;j++){
      if(dp[j] > arr[i].Y) 
        dp[j+1] = max(dp[j+1], arr[i].Y);
    }
  for(int i=1;i<=N;i++){
    if(dp[i] != -1) ans = i;
    else break;
  }
  cout<<ans<<endl;
}
int main(){
  T = getint();
  while(T--){
    init();
    sol();
  }
}

{% endhighlight %}
