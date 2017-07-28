---
layout: post
title: poj-2739 Sum of Consecutive Prime Numbers
categories: pro-sol
excerpt: "2 pointer"
tag: [poj, 2 pointer]

---

## 傳送門：

#### [Sum of Consecutive Prime Numbers](http://poj.org/problem?id=2739)

## 題意：

給數字，求被連續的質數加總而成的方法數。

## 思路：

做出質數表後爬行即可。

## code:

{% highlight cpp linenos %}

void pre(){
  build();      // 造個質數表出來
  for(int i=1;i<prim.size();i++)
    prim[i] += prim[i-1];
}
void init(){
  ans = 0;
}
void sol(){
  int f1 = 0, f2 = 0;
  while(f2 < prim.size()){
    while(f1+1<f2 && prim[f2]-prim[f1]>N){
      ++f1;
    }
    if(prim[f2]-prim[f1] == N)
      ++ans;
    ++f2;
  }
  cout << ans << endl;
}
int main(){
  pre();
  while( cin >> N && N ){
    init();
    sol();
  }
}

{% endhighlight %}
