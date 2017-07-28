---
layout: post
title: poj-2100 Graveyard Design
categories: pro-sol
excerpt: "2 pointer"
tag: [poj, 2 pointer]

---

## 傳送門：

#### [Graveyard Design](http://poj.org/problem?id=2100)

## 題意：

多少組連續的平方和等於Ｎ。

## 思路：

雙指針爬過去邊維護目前答案即可（也可以先找出全部的前綴和）

## code:

{% highlight cpp linenos %}

void init(){
  ans.clear();
  cnt = 1;
}

void sol(){
  ll f1 = 1, f2 = 1;
  while(f2*f2 <= N){
    while(f1+1<=f2 && cnt > N){
      cnt -= (f1*f1);
      ++f1;
    }
    if(cnt == N) ans.PB(MP(f1,f2));
    ++f2;
    cnt += (f2*f2);
  }
  cout << ans.size() << endl;
  for(int i=0;i<ans.size();i++){
    cout << ans[i].Y-ans[i].X+1 ;
    for(int j=ans[i].X;j<=ans[i].Y;j++)
      cout << " " << j;
    cout << endl;
  }
}

int main(){
  while(cin>>N){
    init();
    sol();
  }
}

{% endhighlight %}
