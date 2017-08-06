---
layout: post
title: poj-1222 Extended Lights Out
categories: pro-sol
excerpt: ""
tag: [poj, ]

---

## 傳送門：

#### [Extended Lights Out](http://poj.org/problem?id=1222)

## code:

{% highlight cpp linenos %}

int T=1,M,N,K,I,a,b,c,ans,cnt;
bool arr[10][10], flip[10][10], tmp_flip;
int di[] = {0, 0, 0, 1,-1};
int dj[] = {0,-1, 1, 0, 0};
void init(){
  for(int i=0;i<5;i++)
    for(int j=0;j<6;j++)
      arr[i][j] = getint();
}
inline bool ok(int i, int j){
  return (not(i<0 || j<0 || i>=5 || j>=6));
}
inline bool need(int i, int j){
  tmp_flip = 0;
  for(int k=0;k<5;k++)if(ok(i+di[k], j+dj[k]))
    tmp_flip ^= flip[i+di[k]][j+dj[k]];
  tmp_flip ^= arr[i][j];
  return tmp_flip;
}
inline bool cal(){
  for(int i=1;i<5;i++)
    for(int j=0;j<6;j++)
      if(need(i-1, j)) flip[i][j] = 1;
  for(int i=0;i<6;i++) if(need(4, i))
    return false;
  return true;
}
void sol(){
  for(int i=0 ; i<1<<6 ; i++){
    memset(flip, 0, sizeof(flip));
    for(int j=0;j<6;j++)if(i&(1<<j))
      flip[0][j] = 1;
    if(cal()) return;
  }
}

int main(){
  T = getint();
  for(int i=1;i<=T;i++){
    init();
    sol();
    cout << "PUZZLE #" << i << endl;
    for(int i=0;i<5;i++)for(int j=0;j<6;j++)
      cout << flip[i][j] << " \n"[j==5];
  }
}

{% endhighlight %}
