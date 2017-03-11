---
layout: post
title: poj-1065 Wooden Sticks
categories: pro-sol
excerpt: "Dilworth"
tag: [poj, lis, dilworth]

---

## 傳送門：

#### [Wooden Sticks](http://poj.org/problem?id=1065)

## 題意：

處理第一塊或與上一塊尺寸沒有滿足非嚴格遞增的木頭，花費１，求最小花費。    

## 思路：

根據Dilworth定理，一串皆滿足非嚴格遞增的木頭為chain，所求minimal chain cover等於maximum antichain，題目等價於ｌ由小到大排序時，ｗ的最長遞減子序列（嚴格），使用類似LIS的ＤＰ即可完工。    

## code:

{% highlight cpp linenos %}

void init(){
  memset(dp, -1, sizeof(dp));
  N = getint();
  for(int i=0;i<N;i++)
    arr[i].X = getint(), arr[i].Y = getint();
  sort(arr, arr+N);
}
void sol(){
  dp[0] = INT_MAX;
  for(int i=0;i<N;i++)
    for(int j=0;j<=N && dp[j]!=-1;j++){
      if(dp[j] > arr[i].Y) 
        dp[j+1] = max(dp[j+1], arr[i].Y);
    }
  for(int i=1;i<=N;i++){
    if(dp[i] != -1) ans = i;
    else break;
  }
  cout<<ans<<endl;
}
int main(){
  T = getint();
  while(T--){
    init();
    sol();
  }
}

{% endhighlight %}
