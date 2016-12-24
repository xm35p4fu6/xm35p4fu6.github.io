---
layout: post
title: poj-2010-Moo University-Financial Aid
categories: pro-sol
excerpt: "priority_queue"
tag: [poj, priority_queue, prefix, postfix]

---

## 傳送門：

#### [Moo University-Financial Aid](http://poj.org/problem?id=2010)

## 題意：

有Ｃ頭牛考試入學，而牠們需要被補助學費，學校只收剛好Ｎ頭牛以及Ｆ總資金來補助這些牛  
求入學這Ｎ頭牛考試分數的中位數，中位數要盡可能最大

## 思路：

因為分數跟需要被補助的金額沒有相關性，所以分數低的牛（中位數小的牛兒們）Ｆ無法負擔不代表分數高的牛也無法負擔，簡而言之，答案沒有單調性，無法二分搜。  
既然無法二分，我們勢必要枚舉每頭牛是否能當中位數了。  
***第ｉ頭牛是否能成為答案（中位數）***  

#### 先依照分數排序

而ｉ為Ｎ（Ｎ為奇數）個數的中位數是指ｉ的左右各有Ｎ／２個數字。  
於是我們需要快速地回答出［１, ｉ)、（ｉ, Ｃ］ 內取Ｎ／２個數字相加的最小值。  
令［１, ｉ）為 pre[1], pre[2], ... , pre[i-1] ，只能花ＮｌｇＮ的時間建出這個表格。  
把 pre[i] 依照ｉ是否＞Ｎ／２分成兩部分  
否：pre[i] = pre[i-1] + cost[i] , pq.push(cost[i])  
  
其餘的需要判斷 cost[i] 有無小於 max(cost[1] ~ cost[i-1]) ， max 用 priority_queue(pq) 來維護。  

1. cost[i] < pq.top() then pre[i] is pre[i-1]  
+ 這隻牛的花費 比 花費最小的那群牛中花費最大的那隻牛的花費還高（有點拗口，看不懂多看幾次想想唄） 
+ 所以這隻牛無法加入花費最小的那群牛。  

2. else then pre[i] is pre[i-1] - pq.top() + cost[i] , pq.pop(), pq.push(cost[i])   
+ 這隻牛可以加入花費最小的那群牛，把那群牛中花費最大的換成這隻牛  


之後枚舉牛牛們看Ｆ能否支付來更新答案即可。  

## code:

{% highlight cpp linenos %}

pii arr[100005];
ll pre[100005], pof[100005];
priority_queue<int> pq, pq2;
int main(){
  //ios::sync_with_stdio(false);cin.tie(0);
  cin>>N>>C>>F;
  for(int i=0;i<C;i++)
    cin>>arr[i].X>>arr[i].Y;
  sort(arr, arr+C);

  pre[0] = arr[0].Y;
  pq.push(pre[0]);
  pof[C-1] = arr[C-1].Y;
  pq2.push(pof[C-1]);
  for(int i=1;i<(N/2);i++){
    pre[i] = pre[i-1]+arr[i].Y;
    pq.push(arr[i].Y);
    pof[C-1-i] = pof[C-i]+arr[C-1-i].Y;
    pq2.push(arr[C-1-i].Y);
  }
  for(int i=(N/2);i<C;i++){
    pre[i] = pre[i-1];
    pof[C-1-i] = pof[C-i];
    if(pq.top() > arr[i].Y){
      pre[i] -= (pq.top() - arr[i].Y);
      pq.pop();
      pq.push(arr[i].Y);
    }
    if(pq2.top() > arr[C-1-i].Y){
      pof[C-i-1] -= (pq2.top() - arr[C-1-i].Y);
      pq2.pop();
      pq2.push(arr[C-1-i].Y);
    }
  }
  ans = -1;
  for(int i=(N/2);i+(N/2)<C;i++)if(pre[i-1]+pof[i+1]+arr[i].Y<=F){
      ans = arr[i].X;
  }
  cout<<ans<<endl;
}

{% endhighlight %}
