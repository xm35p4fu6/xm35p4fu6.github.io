---
layout: post
title: poj-3061 Subsequence
categories: pro-sol
excerpt: "2 pointer"
tag: [poj, 2 pointer]

---

## 傳送門：

#### [Subsequence](http://poj.org/problem?id=3061)

## 題意：

給一個正整數序列，求出一段連續的區間和大於某數，而區間長度儘量短，無解時輸出零。  

## 思路：

使用爬行法（2 pointer），並且使用prefix-sum儲存資料。  


prefix-sum：以前我喜歡邊讀input邊製造，現在覺得分開做比較好。  

{% highlight cpp %}

void init(){
  N = getint(), M = getint();
  for(int i=1;i<=N;i++)
    arr[i] = getint();
  for(int i=2;i<=N;i++)
    arr[i] += arr[i-1];
}

{% endhighlight %}

爬行法：通常我寫爬行法時會根據當下各種因素而產生不同風格，不過爬行法大家各自有自己喜歡的寫法，
我認為只要能夠正確work即可。    

這題我寫的是簡約風格（？），很容易就能夠想出sum不夠時要讓前方的指針往前跑，於是可以繼續推論得出迴圈的中止條件只需要focus在前方的指針即可，因為他會先超界，當然還因為本題不可能後方的指針超越前方，而迴圈內部則是判斷當下需要移動哪個指針。    

{% highlight cpp %}

void sol(){
  int l=0, r=1, ans=INT_MAX;
  while(r<=N){
    if(arr[r]-arr[l] >= M){
      ans = min(ans, r-l);
      ++l;
    }
    else{
      ++r;
    }
  }
  if( ans == INT_MAX ) puts("0");
  else cout << ans << endl;
}

{% endhighlight %}

大概就這樣，似乎沒什麼雷點。   

## code:

{% highlight cpp linenos %}

ll T,M,N,K,I,a,b,c,ans,cnt;
ll arr[100005];

void init(){
  N = getint(), M = getint();
  for(int i=1;i<=N;i++)
    arr[i] = getint();
  for(int i=2;i<=N;i++)
    arr[i] += arr[i-1];
}

void sol(){
  int l=0, r=1, ans=INT_MAX;
  while(r<=N){
    if(arr[r]-arr[l] >= M){
      ans = min(ans, r-l);
      ++l;
    }
    else{
      ++r;
    }
  }
  if( ans == INT_MAX ) puts("0");
  else cout << ans << endl;
}

int main(){
  int ncase = getint();
  while(ncase -- ){
    init();
    sol();
  }
}

{% endhighlight %}
