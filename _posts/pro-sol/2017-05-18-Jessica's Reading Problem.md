---
layout: post
title: poj-3320 Jessica's Reading Problem
categories: pro-sol
excerpt: "2 pointer"
tag: [poj, 2 pointer]

---

## 傳送門：

#### [Jessica's Reading Problem](http://poj.org/problem?id=3320)

## 題意：

在一組由某幾種數構成的序列中，求出一段連續的區間，而區間內包含所有數且長度儘量短。  

## 思路：

首先統計好有幾種數字，用 map 隨便喇一喇取size()就是了。    

{% highlight cpp %}

void init(){
  N = getint();
  for(int i=1;i<=N;i++) {
    arr[i] = getint();
    m[arr[i]] ++;
  }
  cnt = m.size();     // cnt = 有幾種數字
  m.clear();
}

{% endhighlight %}

接著跑two-pointer，如果前方新進的數字是第一次加入則目前數字種類加一，以及後方出去的是最後一個就減一，加一時維護答案。    

{% highlight cpp %}

void sol(){
  int f1 = 0, f2 = 0, ans = N;    // f2跑在前方
  while(f2 <= N){
    if(c == cnt){                 // 需要維護答案
      ans = min(ans, f2-f1);
      m[arr[++f1]] --;            // 後方指針前進並維護數量
      if( m[arr[f1]] == 0 ) c--;  // 如果剛剛pop掉的是最後一個了 種類減一
    }
    else{
      m[arr[++f2]] ++;
      if( m[arr[f2]] == 1 ) c++;
    }
  }
  cout << ans << endl;
}

{% endhighlight %}


## code:

{% highlight cpp linenos %}


int T,M,N,K,I,a,b,c,ans,cnt;
int arr[1000005];
map<int, int> m;

void init(){
  N = getint();
  for(int i=1;i<=N;i++) {
    arr[i] = getint();
    m[arr[i]] ++;
  }
  cnt = m.size();
  m.clear();
}

void sol(){
  int f1 = 0, f2 = 0, ans = N;
  while(f2 <= N){
    if(c == cnt){
      ans = min(ans, f2-f1);
      m[arr[++f1]] --;
      if( m[arr[f1]] == 0 ) c--;
    }
    else{
      m[arr[++f2]] ++;
      if( m[arr[f2]] == 1 ) c++;
    }
  }
  cout << ans << endl;
}

int main(){
  init();
  sol();
}

{% endhighlight %}
