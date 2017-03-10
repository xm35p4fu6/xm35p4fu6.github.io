---
layout: post
title: poj-1995-Raising Modulo Numbers
categories: pro-sol
excerpt: "fast power"
tag: [poj, math]

---

## 傳送門：

#### [Raising Modulo Numbers](http://poj.org/problem?id=1995)

## 題意：    

Ｔ筆測資，給一個MOD，以及Ｎ項ａ、ｂ，求 \\( \left( a1^{b1}+a2^{b2}+a3^{b3}+...+an^{bn} \right) \\) %MOD   

## code:

{% highlight cpp linenos %}

ll T,M,N,K,I,a,b,c,ans,cnt;

int main(){
  cin >> T;
  while(T--){
    ans = 0;
    cin >> M >> N;
    while(N--){
      cin >> a >> b;
      ans = add(ans, mypow(a,b,M), M); 
    }
    cout << ans << endl;
  }
}

{% endhighlight %}
