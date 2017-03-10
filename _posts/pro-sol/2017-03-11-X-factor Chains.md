---
layout: post
title: poj-3421-X-factor Chains 
categories: pro-sol
excerpt: "質數題"
tag: [poj, math]

---

## 傳送門：

#### [X-factor Chains](http://poj.org/problem?id=3421)

## code:

{% highlight cpp linenos %}

int T,M,N,K,I,a,b,c,ans,cnt;
int arr[100000], flag, sum;
ll frac[50];
void sol(){
  flag = sum = 0;
  for(int i=2;i*i<=a;i++)if(a%i == 0){
    int cnt = 0;
    while(a>1 && a%i==0)
      ++cnt, a/=i;
    arr[flag++] = cnt;
    sum += cnt;
  }
  if(a>1) {
    ++sum;
    arr[flag++] = 1;
  }
  ll ans = frac[sum];
  for(int i=0;i<flag;i++) ans /= frac[arr[i]];
  cout << sum << " " << ans << endl;
}
void build(){
  frac[1] = 1;
  for(int i=2;i<50;i++) frac[i] = frac[i-1] * i;
}
int main(){
  build();
  while(cin>>a){
    sol();
  }
}

{% endhighlight %}
