---
layout: post
title: codeforces-755D-PolandBall and Polygon
categories: pro-sol
excerpt: "BIT"
tag: [codeforces, BIT]

---

## 傳送門：

#### [PolandBall and Polygon](http://codeforces.com/contest/755/problem/D)

## 題意：

有Ｎ個點的凸多邊形且對角線任三條不交於同點，每點依順時針方向編號，現在從任一點開始往後數Ｋ個連一條對角線，再由此點繼續出發重複上個步驟共畫Ｎ次，每畫一條對角線就輸出***目前多邊形被切成幾個區塊***。    
   
\\( 5<=N<=10^{6} \\)，\\( 2<=K<=N-2 \\)，\\( GCD(N,K)=1 \\) 


## 思路：

這題看上去還挺難的，嚇得我當時直接往後面一題看，不過又被嚇回來就是了ＸＤ  
  
首先把圖畫出來觀察一下   
    
    
![凸五邊形](http://codeforces.com/predownloaded/5c/d0/5cd059d6a34bc4d34e9980ea56b9394f6ea929b6.png)  
        
發現以下幾點：  

1. 起始時共有１個區塊（ans=1）  
2. 從Ａ點連到Ｂ點的線段與從Ａ＋１、Ａ＋２、⋯⋯、Ｂ−１連到不包含Ａ、Ａ＋１、⋯⋯、Ｂ相交   
    
接著我們再畫幾條線來發現直線與Ｍ條**線**相交時，將平面切出Ｍ＋１個新的區塊，現在，我們剩下一個問題。    

##### 從區間（Ａ，Ｂ）連出去的線到底有多少？  

因為每點都是固定與往後數第Ｋ點連線，所以區間（Ａ，Ｂ）內的點一定只往區間外連，畢竟內部自己連就＜Ｋ了，所以我們只需要知道區間（Ａ，Ｂ）連幾條線了，之後每次連線時就把Ａ點與Ｂ點的連線數量＋１。    
    
至此，問題已被剝去層層外衣（咦？），我們需要做的是提供
***區間詢問***、***單點更新***，有一大把的方法來對付這種問題，這邊我是用BIT(Binary Indexed Tree/Fenwick Tree)來喇過的，之後，我們來繼續分析如何做詢問以及更新。  
    
因為ＧＣＤ（Ｎ，Ｋ）＝１，得知在前Ｎ次必定不會畫出起點終點相同的線段，導致畫了線卻沒有切出新區塊，原因類似取 mod Ｎ，互質所以前Ｎ次不會碰撞。    
    
接著，往後數第Ｋ個點等價於往後數第Ｎ−Ｋ個點，挑小的做吧！    

最後，把區間以Ｂ有無大於Ｎ來分類，再仔細思考區間該如何下即可結束這回合，以下講解code。    

## code:

{% highlight cpp linenos %}

//hi~ :)
#include <bits/stdc++.h>
#define fastio ios::sync_with_stdio(false);cin.tie(0)
using namespace std;
typedef long long ll;

ll N,M,a,b,c;
ll bit[3000000];
inline int lowbit(int x) {
  return x&(-x);
}
void update(int pos, int val){
  for(int i=pos; i<=N;i+=lowbit(i))
    bit[i] += val;
}
ll sum(int k){
  ll ans=0;
  for(int i=k;i>0;i-=lowbit(i))
    ans+=bit[i];
  return ans;
}

ll query(int l, int r){
  ll ans=0;
  if(r<l){            // 分成有無＞Ｎ處理
    ans += sum(N);
    ans -= query(r-1, l+1);
  }
  else{
    ans += sum(r-1);
    ans -= sum(l);
  }
  return ans;
}

int main(){
  fastio;
  int ncase=1;
  while(ncase--){
    cin>>N>>M;
    if(2*M > N) M=N-M;  //  挑小的做省麻煩
    memset(bit, 0, sizeof(bit));
    ll ans=1;
    for(int i=0,l=1,r;i<N;i++){
      r = ((l+M>N)?(l+M-N):(l+M));  // ＞Ｎ記得從頭開始
      ans += (query(l, r)+1);       // 區間詢問來更新答案
      update(l,1);                  // 維護BIT
      update(r, 1);
      l=r;              //  從這個點繼續開始
      cout<<ans;
      if(i+1<N)cout<<" ";
    }
    cout<<endl;
  }
}

{% endhighlight %}
