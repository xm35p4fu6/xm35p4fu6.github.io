---
layout: post
title: poj-2392 Space Elevator
categories: pro-sol
excerpt: "dp"
tag: [poj, dp]

---

## 傳送門：

#### [Space Elevator](http://poj.org/problem?id=2392)

## 題意：

K 種石頭要疊高，第 i 個石頭有 Ci 個、高度 Hi、所在高度不能超過 Ai，求能疊多高？  

## 思路：

有限背包問題的小變化，可以 Greedy 的假設高度要求越嚴格的石頭，越要先擺，所以先依照 Ai 排序，之後在 dp 時記得不要超過目前的高度限制就好。

## code:

{% highlight cpp linenos %}

#include <cstdio>
#include <cstring>
#include <algorithm>

using namespace std;

const int mxn = 405;

int K;
int dp[40001];
struct node {
  int h, a, c;
  bool operator< (const node &x) const {
    return a < x.a;
  }
} n[mxn];

int main() {
  scanf("%d", &K);
  for(int i=0; i<K; ++i) scanf("%d%d%d", &n[i].h, &n[i].a, &n[i].c);
  sort(n, n+K);
  memset(dp, -1, sizeof(dp));
  dp[0] = 0;
  int ans = 0;
  for(int j=0; j<K; ++j) for(int i=0; i<=n[j].a; ++i) {
    if(dp[i] >= 0) dp[i] = n[j].c;
    else if(i-n[j].h >= 0 && dp[i-n[j].h] > 0) 
      dp[i] = dp[i-n[j].h] - 1, ans = max(ans, i);
  }
  printf("%d\n", ans);
}

{% endhighlight %}
