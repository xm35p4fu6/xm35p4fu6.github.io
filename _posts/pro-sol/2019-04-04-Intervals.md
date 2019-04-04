---
layout: post
title: poj-1201 Intervals
categories: pro-sol
excerpt: "greedy+線段樹"
tag: [poj, greedy, 線段樹]

---

## 傳送門：

#### [Intervals](http://poj.org/problem?id=1201)

## 題意：

給一些區間，以及各區間中最少要幾個點，求問最少共有幾個點。

## 思路：

這種區間問題先很自然的依照區間結束的部分做排序，如此一來，我們可以保證每次拿出的區間，要是沒有把需要的點給滿，之後就有可能這個區間沒有滿足，也就是說，必須每次都把目前的區間給滿。  
因為後方區間可能包含前面的，所以我們再給點時，從區間尾部開始給，這樣能最大程度重複利用這些點。  
現在問題轉換為，每次給一個區間，詢問目前區間有多少點，之後再該區間內儘量靠左的地方，繼續補上缺的點，這些動作可以用一些序列結構來做。

## code:

{% highlight cpp linenos %}

#include <cstdio>
#include <algorithm>

using namespace std;

const int mxn = 50001;
int N;

struct Node {
  int l, r, c;
} n[mxn];

bool cmp(const Node& a, const Node& b) {
  return (a.r==b.r) ? (a.l<b.l) : (a.r<b.r);
}

int st[mxn*3], tag[mxn*3];    // st, tag:=st已經加了 要往下壓
void update(int &v, int L, int R, int l=0, int r=mxn, int i=0) {
  if(l >= R || r <= L) return ;
  if(l >= L && r <= R) {
    int t = min(r-l-st[i], v);
    st[i] += t, tag[i] += t, v -= t;
    return;
  }
    
  int mid = (l+r)/2, lc = i*2+1, rc = i*2+2;
  // push
  if(tag[i]) {
    int tr = (r-mid) - st[rc];
    tr = min(tag[i], tr);
    int tl = tag[i] - tr;
    st[rc] += tr, tag[rc] += tr;
    if(tl > 0) st[lc] += tl, tag[lc] += tl;
    tag[i] = 0;
  }
  update(v, L, R, mid, r, rc);
  if(v) update(v, L, R, l, mid, lc);
  st[i] = st[lc] + st[rc];
}
int query(int L, int R, int l=0, int r=mxn, int i=0) {
  if(l >= R || r <= L) return 0;
  if(l >= L && r <= R) return st[i];
  if(st[i] == r-l) return min(R, r) - max(L, l);
    
  int mid = (l+r)/2, lc = i*2+1, rc = i*2+2;
  // push
  if(tag[i]) {
    int tr = (r-mid) - st[rc];
    tr = min(tag[i], tr);
    int tl = tag[i] - tr;
    st[rc] += tr, tag[rc] += tr;
    if(tl > 0) st[lc] += tl, tag[lc] += tl;
    tag[i] = 0;
  }

  int res = query(L, R, l, mid, lc) + query(L, R, mid, r, rc);
  st[i] = st[lc] + st[rc];
  return res;
}
int main() {
  scanf("%d", &N);
  for(int i=0; i<N; ++i)
    scanf("%d%d%d", &n[i].l, &n[i].r, &n[i].c);
  sort(n, n+N, cmp);

  int ans = 0;

  for(int i=0; i<N; ++i) {
    int q = query(n[i].l, n[i].r+1);
    q = n[i].c - q;
    if(q <= 0) continue;
    ans += q;
    update(q, n[i].l, n[i].r+1);
  }

  printf("%d\n", ans);
}

{% endhighlight %}
