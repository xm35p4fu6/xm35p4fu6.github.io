---
layout: post
title: poj-3045 Cow Acrobats
categories: pro-sol
excerpt: "最大值最小化"
tag: [poj, greedy]

---

## 傳送門：

#### [Cow Acrobats](http://poj.org/problem?id=3045)

## 題意：

每頭牛有力氣值及重量，現在他們要疊成一座塔，每頭牛的壓力是牠上方所有牛的重量總和減去牠的力氣值，請最小化壓力最大那頭牛的壓力。    


## 思路：

乍看之下好像又要二分搜答案然後去排序，不過我一直想不到好的判斷function，就在我各種推演判斷方法時，突然發現每頭牛的壓力為    
> sum(還沒疊上去的牛的重量和) - 自己的重量 - 自己的力氣    
> !?    
> sum() - (重量＋力氣) 是可以greedy來保證最後得到最佳解的！    

把所有牛依照重量+力氣排序之後，模擬建塔過程邊維護答案即可......      

## code:

{% highlight cpp linenos %}

inline bool cmp(const pii &a, const pii &b){
  return (a.X+a.Y)>(b.X+b.Y);
}

pii arr[50001];

void init(){
  N = getint();
  for(int i=0;i<N;i++){
    arr[i].X = getint(), arr[i].Y = getint();
    cnt += arr[i].X;
  }
  sort(arr, arr+N, cmp);
}
ll sol(){
  ll ans = -INT_MAX;
  for(int i=0;i<N;i++){
    cnt -= arr[i].X;
    ans = max(ans, cnt-arr[i].Y);
  }
  return ans;
}

int main(){
  init();
  cout << sol() << endl;
}

{% endhighlight %}
