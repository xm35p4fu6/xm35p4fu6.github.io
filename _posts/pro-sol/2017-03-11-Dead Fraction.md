---
layout: post
title: poj-1930-Dead Fraction
categories: pro-sol
excerpt: "小數轉分數"
tag: [poj, math]

---

## 傳送門：

#### [Dead Fraction](http://poj.org/problem?id=1930)

## 題意：

小數轉分數，枚舉各循環節找答案。    

## code:

{% highlight cpp linenos %}

string str;
ll num;
int leng ;
pll ans;

pll sol(int x){
  ll ry=0,rx=num;
  for(int i=0;i<x;i++){
    rx /= 10;
    ry *= 10;
    ry += 9;
  }
  for(int i=x;i<leng;i++) ry *= 10;
  rx = num - rx;
  ll gcd = __gcd(rx, ry);
  return MP(rx/gcd, ry/gcd);
}

int main(){
  while(cin >> str && str.length()>1){
    leng = num = 0;
    for(int i=2;i<str.length();i++){
      if(str[i] == '.') break;
      num *= 10;
      num += (str[i]-'0');
      leng ++;
    }
    
    ans = sol(1);
    for(int i=2;i<=leng;i++){
      pll x = sol(i);
      if(x.Y < ans.Y) ans = x;
    }
    cout << ans.X << '/' << ans.Y << endl;
  }
}

{% endhighlight %}
