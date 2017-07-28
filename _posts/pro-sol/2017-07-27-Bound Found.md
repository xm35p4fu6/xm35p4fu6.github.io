---
layout: post
title: poj-2566 Bound Found
categories: pro-sol
excerpt: "sort + 2 pointer"
tag: [poj, 2 pointer]

---

## 傳送門：

#### [Bound Found](http://poj.org/problem?id=2566)

## 題意：

題目會給一個有正有負的序列和一些詢問，每筆詢問包含一個數字ｔ，求出序列中一段連續區間總和絕對值最接近ｔ的區間，並印出該區間總和絕對值及範圍，區間不能為空。


## 思路：

先將區間前綴求出，綁上原本位置之後做排序，這樣任選兩個就代表了原序列上的一段連續區間，雙指標爬過去一邊更新答案即可。

## code:

{% highlight cpp linenos %}

void init(){
  arr[0] = MP(0,0);
  for(int i=1;i<=N;i++){
    arr[i].Y = i;		      // 原位置
    arr[i].X = getint() + arr[i-1].X;     // 前綴合
  }
}
void sol(){
  sort(arr, arr+N+1);
  int f1 = 0, f2 = 1;
  while(M--){
    q = getint();
    f1 = 0, f2 = 1;         // f2 : 下一個要被收進來的  f1 : 下一個要被移出的  range:[f1,f2)
    ans = MP(f1,f2);
    while(f2 <= N){
      if(abs(arr[ans.Y].X - arr[ans.X].X - q) > abs(arr[f2].X - arr[f1].X - q))
        ans = MP(f1,f2);
      if(f1+1<f2 && arr[f2].X - arr[f1].X > q) ++f1;
      else ++f2;
    }

    if( arr[ans.X].Y > arr[ans.Y].Y)
      printf("%lld %lld %lld\n", arr[ans.Y].X-arr[ans.X].X, arr[ans.Y].Y+1, arr[ans.X].Y);
    else
      printf("%lld %lld %lld\n", arr[ans.Y].X-arr[ans.X].X, arr[ans.X].Y+1, arr[ans.Y].Y);
  }
}

int main(){
  while(cin>>N>>M && (N+M)){
    init();
    sol();
  }
}

{% endhighlight %}
