---
layout: post
title: poj-3484 Showstopper
categories: pro-sol
excerpt: "二分搜函式加總範圍"
tag: [poj, binary search, prefix]

---

## 傳送門：

#### [Showstopper](http://poj.org/problem?id=3484)

## 題意：

給３個參數ＸＹＺ來定義一集合，分別為Ｘ、Ｘ＋Ｚ、Ｘ＋２Ｚ ...  Ｘ＋ｋＺ <= Ｙ。   
每筆測資是一些集合，必須找出哪一個元素的出現次數為奇數，或皆為偶數，題目保證只有此兩種情況。   

## 思路：

一個有趣的觀察是：    
如果將所有集合的所有元素出現次數加總，為偶數則保證所有元素出現次數皆為偶數。    

有了此前提，我們就可以二分搜上界來取代每個集合的Ｙ，檢查出現次數為奇數的元素是否在內。    

## code:

{% highlight cpp linenos %}

ll T,M,N,K,I,a,b,c,ans,cnt;
string in;
stringstream ss;
vector< piii > vec;

inline bool suc(ll x){
  cnt = 0;
  for(int i=0;i<vec.size();i++){
    if(vec[i].X > x) continue;
    cnt += (((min(vec[i].Y.X, x) - vec[i].X)/vec[i].Y.Y)+1);
  }
  return (cnt&1);
}
void sol(){
  ll R=0,L=0;
  vec.clear();
  do{
    ss.clear();
    ss.str(in);
    ss >> a >> b >> c;
    R = max(R,b);
    vec.PB(MP(a,MP(b,c)));
  }while(getline(cin, in) and in != "");
#define mid ((L+R)>>1)
  if(!suc(R)){
    cout << "no corruption" << endl;
    return;
  }
  while(R-L>1) suc(mid) ? R=mid : L=mid;
  suc(R);
  ll c2 = cnt;
  suc(R-1);
  cout << R << " "<< c2-cnt << endl;
}

int main(){
  while(getline(cin, in)){
    if(in != ""){
      sol();
    }
  }
}


{% endhighlight %}
