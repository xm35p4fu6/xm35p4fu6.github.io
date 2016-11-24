---
layout: post
title: poj-3616-Milking Time
categories: pro-sol
excerpt: "dp"
tag: [poj, dp]

---

## 傳送門

#### [Milking Time](http://poj.org/problem?id=3616)  

## 題意
給一些時間區間[L, R] 以及該區間能獲得的牛奶產量，在區間不重疊的前提下求最大產量。

## 思路
每個區間以結束時間當index，存放開始時間以及產量 (注意可能有多個區間結束時間相同)
dp[t] 表示到時間t之前所能獲得的最大產量  

於是我們有以下維護dp[t]的方法  
  1. dp[t] = dp[t-1] // t 時間點的最大產量同t-1  
  2. dp[t] = dp[ x ] + E // 放入一個結束時間為t的區間， x 表起始時間-1，E 表示產量   

## code

{% highlight cpp linenos %}

#include <iostream>
#include <algorithm>
#define safe(x) max(0,x)
using namespace std;

int T,M,N,K,I,a,b,c,ans,cnt;
vii v[1000001];
ll dp[1000001];

int main(){
  //ios::sync_with_stdio(false);cin.tie(0);
  scanf("%d%d%d", &M, &N, &K);
  for(int i=0;i<N;i++){
    scanf("%d%d%d", &a, &b, &c);
    v[b].PB(MP(a, c));
  }
  for(int i=1;i<=M;i++){
    dp[i] = dp[i-1];
    for(int j=0;j<v[i].size();j++)
      dp[i] = max(dp[i], dp[safe(v[i][j].X - K)] + v[i][j].Y);
  }
  printf("%lld\n", dp[M]);
}

{% endhighlight %}
