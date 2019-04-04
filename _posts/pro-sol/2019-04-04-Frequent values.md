---
layout: post
title: poj-3368 Frequent values
categories: pro-sol
excerpt: "線段樹"
tag: [poj, 線段樹]

---

## 傳送門：

#### [Frequent values](http://poj.org/problem?id=3368)

## 題意：

給你一個非遞減數列，以及一系列詢問，每次詢問一個區間內出現頻率最高的數字的出現頻率。

## 思路：

因為數列是具有單調性的，所以我們可以知道每個數值，一定都會排在一起。  
也就是說一個區間的答案可以藉由左子區間、右子區間、左右子區間鄰接處相同的數字中找出答案。  
使用線段樹來維護這些資料吧！  

## code:

{% highlight cpp linenos %}
#include <iostream>
using namespace std;
typedef long long ll;
typedef pair<int, int> pii;
typedef vector<int> vi;
typedef vector<pii> vii;
inline ll getint(){
  ll _x=0,_tmp=1; char _tc=getchar();    
  while( (_tc<'0'||_tc>'9')&&_tc!='-' ) _tc=getchar();
  if( _tc == '-' ) _tc=getchar() , _tmp = -1;
  while(_tc>='0'&&_tc<='9') _x*=10,_x+=(_tc-'0'),_tc=getchar();
  return _x*_tmp;
}

////////  template end   //////////

#define mid ((l+r)/2)
#define lc  (ind*2+1)
#define rc  (ind*2+2)

int N, Q, L, R;
int arr[100005];
pii lb[400025], rb[400025], mx[400025];   // 線段樹維護左右邊界以及答案 {key, cnt}
pii ans, Rb;

inline pii operator + (const pii& x, const pii& y) {
  return {x.X, x.Y+y.Y};
}
inline const pii& max(const pii& x, const pii& y) {
  return (x.Y > y.Y ? x : y);
}

void build(const int& l, const int& r, const int& ind){   // [ )
  if(r-l == 1){
    lb[ind] = rb[ind] = mx[ind] = {arr[l], 1};
    return;
  }
  build(l, mid, lc);    // left_child
  build(mid, r, rc);    // right_child
  
  if( lb[rc].X == lb[lc].X ) lb[ind] = lb[lc] + lb[rc];   // left_bound
  else lb[ind] = lb[lc];

  if( rb[rc].X == rb[lc].X ) rb[ind] = rb[lc] + rb[rc];   // right_bound
  else rb[ind] = rb[rc];

  mx[ind] = max( mx[rc], mx[lc] );
  if( rb[lc].X == lb[rc].X ) mx[ind] = max( mx[ind], rb[lc]+lb[rc] );
}

void query(const int& l, const int& r, const int& ind){   // [ )
  if( l>=L and r<=R ){
    if( lb[ind].X == Rb.X ) ans = max( ans, lb[ind]+Rb );
    if( rb[ind].X == Rb.X ) Rb = Rb + rb[ind];
    else Rb = rb[ind];
    ans = max( ans, mx[ind] );
    return;
  }
  if( mid>L ) query(l, mid, lc);
  if( mid<R ) query(mid, r, rc);
}

void init(){
  Q = getint();
  for(int i=0;i<N;i++)
    arr[i] = getint();
  build(0, N, 0);   // 0-base
}

void sol(){
  while(Q--){   // input 1-base
    L = getint()-1;
    R = getint();

    Rb = ans = {INT_MAX, -1};
    query(0, N, 0);   // 0-base
    printf("%d\n", ans.Y);
  }
}

int main(){
  while( cin >> N and N ){
    init();
    sol();
  }
}

{% endhighlight %}
