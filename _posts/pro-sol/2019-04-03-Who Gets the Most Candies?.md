---
layout: post
title: poj-2886 Who Gets the Most Candies?
categories: pro-sol
excerpt: "bit"
tag: [poj, bit]

---

## 傳送門：

#### [Who Gets the Most Candies?](http://poj.org/problem?id=2886)

## 題意：

N 個人圍圈，每個人有一個數字 Ai，每個人離開時，下一個離開的是後面第 Ai 個人。  
每個人離開時會有一個函式來計算得分，求最高分的。

## 思路：

函式不重要，主要練習維護位置。  
用序列結構來保存每個人的位置，每次有人離開時，他後方的人位置都 -1，之後，小心的計算下一位要離開的是誰即可。

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

int N, nxt, first;
char name[500005][12];
int card[500005], score[500005];
int bit[1000005], sz, F[500005];
int ans, ans_id;
vector<int> mul;

inline int lowerbit(const int& x){
  return x&-x;
}
inline void add(int pos, const int& val){
  for(; pos <= sz ; pos+=lowerbit(pos))
    bit[pos] += val;
}
inline int query(int pos){
  int res = 0;
  for(; pos ; pos-=lowerbit(pos))
    res += bit[pos];
  return res;
}
inline bool suc(int pos, const int& tar){
  int cnt = 0;
  for(; pos ; pos-=lowerbit(pos)) cnt += bit[pos];
  return cnt>=tar;
}
inline int search(const int& v){
#define mid ((l+r)>>1)
  int l=v-1, r=N;
  while(r-l>1) suc(mid, v) ? r=mid : l=mid;
  return r;
#undef mid
}

void pre(){
  for(int i=1;i<=500000;++i){
    for(int j=1;i*j<=500000;++j)
      ++F[i*j];
  }
}

void init(){
  for(int i=1;i<=N;i++)
    scanf("%s %d", name[i], card+i);

  sz = 1<<(int)ceil(log((double)N)/log(2.));
  memset(bit, 0, sizeof(int)*(sz+1));

  for(int i=1;i<=N;i++) add(i, 1);
  ans = 0;
  first = nxt;
}

void sol(){
  for(int P=1,curr, curr_real, N=::N ; ; --N, ++P){
    curr = nxt;     // 目前的人 在數列中的次序
    curr_real = search(nxt);  // 目前的人 在原數列中的編號

    // 維護答案
    if(F[P] > F[ans]){
      ans = P;
      ans_id = curr_real;
    }
    
    add(curr_real, -1);

    if(N == 1) break;

    // 尋找下個點
    if(card[curr_real] > 0){
      if(curr + card[curr_real] > N){
        nxt = (card[curr_real] + curr - N) % (N-1);
        if(nxt == 0) nxt = N-1;
      }
      else nxt = card[curr_real] + curr - 1;
    }
    else{
      if(curr + card[curr_real] < 1){
        nxt = (card[curr_real] + curr -1) % (N-1) + N;
        if(nxt == N) nxt = 1;
      }
      else nxt = card[curr_real] + curr;
    }

  }

  printf("%s %d\n", name[ans_id], F[ans]);
}

int main(){
  pre();
  while( scanf("%d %d", &N, &nxt) == 2){
    init();
    sol();
  }
}

{% endhighlight %}
