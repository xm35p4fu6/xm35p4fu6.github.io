---
layout: post
title: poj-2385-Apple Catching
categories: pro-sol
excerpt: "dp"
tag: [poj, dp]

---

## 傳送門

#### [Apple Catching](http://poj.org/problem?id=2385)  

## 題意
給一串數列表示第1或2棵樹正在掉蘋果，起始在第1棵樹，最多移動W次，求最多接到幾次

## 思路

dp[x][y] 表示目前在第x棵樹且已經移動y次最多接到幾次  
初始值：  
* dp[0][0] = 0
* dp[1][0] = -10 (使最佳狀態不成立於起始在第2棵樹)  


## code

{% highlight cpp linenos %}

#include <iostream>
#include <algorithm>
using namespace std;

int M,N,a,ans;
int dp[2][35];

int main(){
  ios::sync_with_stdio(false);cin.tie(0);
  cin>>N>>M;
  dp[1][0] = -10;
  while(N--){
    cin>>a; --a;
    dp[a][0]++;
    for(int i=1;i<=M;i++){
      dp[a][i] = max(dp[a][i], dp[!a][i-1])+1;
      ans = max(dp[a][i], ans);
    }
  }
  cout<<max(ans, max(dp[1][0], dp[0][0]))<<endl;
}

{% endhighlight %}
