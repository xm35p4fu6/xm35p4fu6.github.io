---
layout: post
title: poj-2184 Cow Exhibition
categories: pro-sol
excerpt: "dp"
tag: [poj, dp]

---

## 傳送門：

#### [Cow Exhibition](http://poj.org/problem?id=2184)

## 題意：

N 頭牛，每頭有兩個數值 Ai, Bi，可能為負，選任意數量頭牛來最大化 Ai 與 Bi 的和，Ai 的和必須非負，Bi 的和也必須非負。  

## 思路：

01 背包問題的特化版本，因為有兩個值要處理，我們把其中一個當作 index，此外，可能為負，所以要把座標範圍位移一下。  
之後就依照 01 背包問題求解，有些小變化可以自己思考怎麼做。

## code:

{% highlight cpp linenos %}

#include <cstdio>
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int mxn = 200000;
const int inf = 0x80808080;

int N;
int dp[mxn + 1];
int s[101], f[101], n;

int main() {
  scanf("%d", &N);
  for(int i=0; i<N; ++i) {
    scanf("%d%d", s+n, f+n);
    if(s[n]>0 || f[n]>0) ++n;
  }
  N = n;
  if(N == 0) {
    puts("0");
    return 0;
  }
  memset(dp, 0x80, sizeof(dp));
  dp[mxn/2] = 0;

  int ans = 0;
  for(int i=0; i<N; ++i) {
    if(s[i] >= 0) {
      for(int j=mxn; j-s[i]>=0; --j) if(dp[j-s[i]] != inf)
        dp[j] = max(dp[j], dp[j-s[i]] + f[i]);
    }
    else {
      for(int j=0; j-s[i]<=mxn; ++j) if(dp[j-s[i]] != inf)
        dp[j] = max(dp[j], dp[j-s[i]] + f[i]);
    }
  }
  for(int i=mxn/2; i<=mxn; ++i) if(dp[i] >= 0)
    ans = max(ans, dp[i] - mxn/2 + i);

  printf("%d\n", ans);
}

{% endhighlight %}
