---
layout: post
title: poj-3126-Prime Path 
categories: pro-sol
excerpt: "質數題"
tag: [poj, math]

---

## 傳送門：

#### [Prime Path ](http://poj.org/problem?id=3126)

## 題意：

一次改變一個digit，求起始到終點的距離。    

## 思路：

BFS即可。   

## code:

{% highlight cpp linenos %}

int T,M,N,K,I,a,b,c,ans,cnt, head, tail;
int visit[10000];
queue<int> que;
void init(){
  memset(visit, 0, sizeof(visit));
  cin >> head >> tail;
  que.push(head);
  visit[head] = 1;
}
stringstream ss;
string str,ts;
void sol(){
  int x, num;
  if(tail != head)
  while(!que.empty()){
    x = que.front();
    que.pop();
    ss.clear();
    ss << x;
    ss >> str;
    for(int i=0;i<10;i++){
      for(int j=0;j<4;j++)if(str[j]-'0' != i){
        ts = str;
        ts[j] = '0'+i;
        ss.clear();
        ss << ts;
        ss >> num;
        if(num == tail){
          cout << visit[x] << endl;
          while(!que.empty()) que.pop();
          return;
        }
        if(!visit[num] && is_pr(num)){
          visit[num] = visit[x]+1;
          que.push(num);
        }
      }
    }
  }
  cout << 0 << endl;
}
int main(){
  build();
  cin >> T;
  while(T--){
    init();
    sol();
  }
}

{% endhighlight %}
