---
layout: post
title: poj-3176-Cow Bowlong
categories: pro-sol
excerpt: "dp"
tag: [poj, dp]

---

## 傳送門

#### [Cow Bowlong](http://poj.org/problem?id=3176)  

## 思路
基本的dp，每個點可往正下方或右下方走  
dp[i][j]：到此點時的最高分  
邊讀測資邊維護陣列即可

## code

{% highlight cpp linenos %}

#include <iostream>
#include <algorithm>
using namespace std;
int N,ans,a;
int dp[400][400];

int main(){
  //ios::sync_with_stdio(false);cin.tie(0);
  scanf("%d", &N);
  for(int i=0;i<N;i++)for(int j=0;j<=i;j++){
    scanf("%d", &a);
    dp[i][j] += a;
    ans = max(dp[i][j], ans);
    dp[i+1][j] = max(dp[i+1][j], dp[i][j]);
    dp[i+1][j+1] = max(dp[i+1][j+1], dp[i][j]);
  }
  printf("%d\n", ans);
}

{% endhighlight %}
