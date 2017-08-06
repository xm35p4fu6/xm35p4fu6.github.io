---
layout: post
title: poj-3185 The Water Bowls 
categories: pro-sol
excerpt: ""
tag: [poj]

---

## 傳送門：

#### [The Water Bowls ](http://poj.org/problem?id=3185)

## code:

{% highlight cpp linenos %}

int T=1,M,N,K,I,a,b,c,ans,cnt;
int di[] = {-1,0};
bool arr[30], flip[30], tmp_flip;

void init(){
  for(int i=0;i<20;i++) arr[i] = getint();
  ans = INT_MAX;
}

inline bool ok(int &v){
  return (not(v<0 || v>=20));
}

inline bool need(int &i){
  int ii;
  tmp_flip = 0;
  for(int j=0;j<2;j++)if(ok(ii=i+di[j]))
    tmp_flip ^= flip[ii];
  tmp_flip ^= arr[i];
  return tmp_flip;
}
void cal(){
  int times = flip[0], last = 19;
  
  for(int i=0;i<19;i++){
    if(need(i)){
      flip[i+1]=1;
      ++times;
    }
  }
  
  if(!need(last)) ans = min(ans, times);
}
void sol(){
  for(int I=0;I<2;I++){ // 假設第一個碗的flip狀況
    memset(flip, 0, sizeof(flip));
    flip[0] = I;
    cal();
  }
  cout << ans << endl;
}

int main(){
  while(T--){
    init();
    sol();
  }
}

{% endhighlight %}
