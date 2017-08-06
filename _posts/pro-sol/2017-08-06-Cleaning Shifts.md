---
layout: post
title: poj-3171 Cleaning Shifts
categories: pro-sol
excerpt: "帶權區間覆蓋"
tag: [poj, dp, 線段樹]

---

## 傳送門：

#### [Cleaning Shifts](http://poj.org/problem?id=3171)

## 題意：

給 N 個區間，每個區間[s,t] 有一個權值 v ，求覆蓋整個區間需要的最低權值。

## 思路：

很快我們就可以想到dp方法，令dp[i]為從頭覆蓋到i的最低權值，則我們將所有區間依照t排序後枚舉來做狀態轉移。

對於一個區間 [s,t]:v ，可以接在 [s-1,t) 後方，於是我們掃過 dp[s-1] ~ dp[t-1]，試看看 dp[] + v 是否能更新 dp[t]。

這樣做會超時，但我們如果將所有 dp 用個線段樹來維護，剛剛的操作就變成區間詢問最小，以及單點修改。

詳細往下看 code內註解吧！

## code:

{% highlight cpp linenos %}

typedef long long ll;

struct Node{
  int x, y;
  ll val;
  Node(int a=0, int b=0, ll c=0):
    x(a), y(b), val(c){}
  bool operator < (const Node& a)const{	        // 依照t排序	
    return ((y==a.y) ? (x<a.x) : (y<a.y));
  }
};
const ll INF = 0x7f7f7f7f7f7f7f7fll;
const int VN = 87000;
typedef ll ele;

ele segt_arr[4*VN];
int segt_sz;

inline void segt_pull(int v) {
  segt_arr[v] = min(segt_arr[v<<1], segt_arr[v<<1|1]);
}
inline void segt_push(int v) {

}
void segt_update(int L, int val, int l=0, int r=segt_sz, int v=1) {
  if(l>=L && r<=L+1) segt_arr[v] = val;
  else {
    segt_push(v);
    if((l+r)>>1 >= L+1) segt_update(L, val, l, (l+r)>>1, v<<1);
    if((l+r)>>1 <= L) segt_update(L, val, (l+r)>>1, r, v<<1|1);
    segt_pull(v);
  }
}
ll segt_query(int L, int R, int l=0, int r=segt_sz, int v=1) {
  if(l>=L && r<=R) return segt_arr[v];
  if(l>=R || r<=L) return INF;
  segt_push(v);
  return min( segt_query(L, R, l, (l+r)>>1, v<<1), 
      segt_query(L, R, (l+r)>>1, r, v<<1|1));
}

int N, M, E;

Node inter[VN]; 
ll dp[VN];

void init() {
  int l, r, v, n=0;
  memset(dp, 0x7f, sizeof(dp));
  memset(segt_arr, 0x7f, sizeof(segt_arr));
  scanf("%d %d %d", &N, &M, &E);
  segt_sz = E+1;
  for(int i=0; i<N; ++i) {
    scanf("%d %d %d", &l, &r, &v);
    if(l==M) {                          // 與頭相連的區間就直接更新完扔掉
      if(dp[r] > v) {
        dp[r] = v;
        segt_update(r, v);
      }
    }
    else inter[n++] = Node(l, r, (ll)v);
  }
  N = n;
}

void sol() {
  ll q;
  sort(inter, inter+N);
  for(int i=0; i<N; ++i) {
    q = segt_query(inter[i].x-1, inter[i].y) + inter[i].val;      // 詢問最小
    if(q < dp[inter[i].y]) {
      dp[inter[i].y] = q;
      segt_update(inter[i].y, q);             // 更新
    }
  }
  if(dp[E] == INF) puts("-1");
  else printf("%lld\n", dp[E]);
}

int main() {
  init();
  sol();
}

{% endhighlight %}
