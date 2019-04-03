---
layout: post
title: poj-3666 Making the Grade
categories: pro-sol
excerpt: "dp"
tag: [poj, dp]

---

## 傳送門：

#### [Making the Grade](http://poj.org/problem?id=3666)

## 題意：

給 N 個數字，改動每個數字要花費 |Ai-Ai'|，將這 N 個數字修改成非嚴格遞增或遞減的數列，需要的最小花費是？

## 思路：

觀察發現，一個數字一定只會不改動或改成其他數列中的數字。  
現在思考，數列一個一個數字加入時，只能在前一個數字為某些特定值，才能夠造出一個合法數列。所以我們要把數列中，第 i 個位置更改成 Ai' 且前 i 個都合法時的情況的花費都算好，這樣才能計算第 i+1 個位置。  

共有 N*N 個狀態，可以使用一些技巧來壓縮空間 (滾動陣列)、時間 (漸進式優化) 複雜度。

## code:

{% highlight cpp linenos %}

#include <cstdio>
#include <vector>
#include <algorithm>

using namespace std;

const int mxn = 2002;

int N, n;
vector<int> v, v2, dp[2];

inline int dis(int a, int b) {
  return abs(v[a] - v2[b]);
}

int main() {
  scanf("%d", &N);
  v.resize(N);
  for(int i=0; i<N; ++i)
    scanf("%d", &v[i]);
  v2 = v;
  sort(v2.begin(), v2.end());
  v2.resize(unique(v2.begin(), v2.end()) - v2.begin());
  n = v2.size();

  dp[0].resize(N);
  dp[1].resize(N);

  for(int i=0; i<N; ++i) {
    for(int j=0; j<n; ++j) {
      dp[1][j] = dis(i, j);
      if(i) dp[1][j] += dp[0][j];
      if(j) dp[1][j] = min(dp[1][j], dp[1][j-1]);
    }
    swap(dp[0], dp[1]);
  }
  int ans = dp[0][n-1];
  for(int i=0; i<N; ++i) {
    for(int j=n-1; j>=0; --j) {
      dp[1][j] = dis(i, j);
      if(i) dp[1][j] += dp[0][j];
      if(j+1<n) dp[1][j] = min(dp[1][j], dp[1][j+1]);
    }
    swap(dp[0], dp[1]);
  }

  printf("%d\n", min(ans, dp[0][n-1]));
}

{% endhighlight %}
