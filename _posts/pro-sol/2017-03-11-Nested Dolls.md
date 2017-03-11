---
layout: post
title: poj-3636 Nested Dolls
categories: pro-sol
excerpt: "Dilworth"
tag: [poj, dilworth, multiset]

---

## 傳送門：

#### [Nested Dolls](http://poj.org/problem?id=3636)

## 題意：

盒子的長寬必須較小才可放入另一個盒子，僅可能收納，求最後生下幾個盒子。    

## 思路：

照Ｘ先小到大排，一樣時大到小排（可以思考看看為什麼），用multiset做完剩下的，不清楚可參考[Dilworth](/articles/poset/)   

## code:

{% highlight cpp linenos %}

multiset<int> s;
multiset<int>::iterator ubi;
void init(){
  N = getint();
  s.clear();
  for(int i=0;i<N;i++){
    a = getint(),
    b = getint();
    arr[i] = Node(a,b);
  }
}

void sol(){
  sort(arr, arr+N);
  for(int i=0;i<N;i++){
    ubi = s.upper_bound(arr[i].Y);
    if(ubi != s.end()) s.erase(ubi);
    s.insert(arr[i].Y);
  }
  cout << s.size() << endl;
}

int main(){
  T = getint();
  while(T--){
    init();
    sol();
  }
}

{% endhighlight %}
