---
layout: post
title: poj-2236-Wireless Network
categories: pro-sol
excerpt: "disjoint set, union find"
tag: [poj, union-find]

---

## 傳送門：

#### [Wireless Network](http://poj.org/problem?id=2236)

## 題意：

***poj 2236***  

一場地震後導致電腦全壞了（有沒有這麼脆弱？？），現在給出兩種操作    

1. 修復第k個電腦  
2. 詢問某兩電腦是否能正常通訊  

給定每電腦的位置（Ｘ、Ｙ），只有被修復的電腦可以通訊，且需距離不大於ｄ或之間有另個電腦可通訊。  

1<=Ｎ<=1001、0<=ｄ<=20000、300000左右筆操作

## 思路：

因為**只要Ａ與Ｂ可通訊、Ｂ與Ｃ可通訊，則Ａ與Ｃ可通訊**我們可以發現，若將兩個可通訊的電腦視為同一集合，則詢問是否正常通訊可視為詢問是否在同個集合，可用disjoint
set輾壓。   
現在問題只剩下如何建立起集合了，觀察一下發現Ｎ量級是3個0，換句話說複雜度Ｎ平方的做法是可以接受的，自然而然的想到了對於每個維修電腦的操作，我們都直接枚舉所有電腦來判斷是否能通訊（能合併為一集合），到此，這題八九不離十就已經結束了。  

順帶一提，如果Ｎ的量級不支援到平方等級的複雜度，應該改用ＫＤ樹之類的資料結構來維護集合，目前還沒做到這類型題目～

## code:

{% highlight cpp linenos %}

int T,M,N,K,I,a,b,c,ans,cnt,d;
ll D;
int par[1005], rk[1005];
pii pos[1005];
bool ok[1005];

inline ll dis(int a, int b){
  return ((pos[a].X-pos[b].X)*(pos[a].X-pos[b].X) + 
    (pos[a].Y-pos[b].Y)*(pos[a].Y-pos[b].Y));
}

int _find(int v){
  return ((par[v] == v) ? v : par[v] = _find(par[v]) );
}

void merge(int a, int b){
  int t1 = _find(a), t2 = _find(b);
  if(t1 == t2) return;
  rk[t1] > rk[t2] ? par[t2] = t1 : par[t1] = t2;
  if(rk[t1] == rk[t2]) rk[t2]++;
}

set<int> s;
set<int>::iterator si;
void repair(int v){
  s.clear();
  for(int i=0;i<N;i++)if(ok[i] && i!=v && dis(i,v)<=D)
    s.insert(_find(i));
  si = s.begin();
  if(si != s.end()){
    for(si++;si!=s.end();si++)
      merge(*(s.begin()), *si);
    merge(*(s.begin()), v);
  }
  ok[v] = 1;
}

int main(){
  scanf("%d %d", &N, &d);
  fill(rk, rk+N, 1);
  D = d*d;
  for(int i=0;i<N;i++){
    scanf("%d %d", &(pos[i].X), &(pos[i].Y));
    par[i] = i;
  }
  char ch[2];
  while(~scanf("%s", ch)){
    if(ch[0] == 'O'){
      cin>>a;
      repair(a-1);
    }
    else{
      cin>>a>>b;
      printf((_find(a-1) == _find(b-1)) ? "SUCCESS\n" : "FAIL\n");
    }
  }
}

{% endhighlight %}
