---
layout: post
title: poj-3280-Cheapest Palindrome
categories: pro-sol
excerpt: "dp"
tag: [poj, dp]

---

## 傳送門

#### [Cheapest Palindrome](http://poj.org/problem?id=3280)  

## 題意
給一字串，求轉成迴文的最小花費 (新增、刪除字元皆有花費)

## 思路
首先簡化一下問題：  
透過觀察發現，需要新增字元時也可以選擇把該字元移除，於是稱新增或刪除一個字元為***操作一個字元***，所需的花費為 ***min(新增該字元, 刪除該字元)***  

令 dp[x][y] ： 區間 ***[x, y)*** 轉成迴文的最小花費  
初始： 所有長度1或0的區間已經是迴文，為0  
接著我們思考如何透過已是迴文的部分來組出更長的迴文，或說  
某區間如何透過已經做好的部分來完成  
結果如下：  

每個區間[x, y)我們可以選擇  
操作最左(x)或最右(y-1)邊的字元來使此區間成為迴文：  

  - ***dp[x][y] = dp[x+1][y] + cost(str[x])***  
  - ***dp[x][y] = dp[x][y-1] + cost(str[y-1])***  

如果該區間最左與最右邊的字元相同：  

  - ***dp[x][y] = dp[x+1][y-1]***  


##### 需依區間長度由小到大建dp表!


## code

{% highlight cpp linenos %}

#include <iostream>
#include <algorithm>
using namespace std;

int T,M,N,K,I,b,c,ans,cnt;
char a;
int cost[200], dp[2002][2002];
string str;

int main(){
  ios::sync_with_stdio(false);cin.tie(0);
  cin>>N>>M>>str;
  for(int i=0;i<N;i++){
    cin>>a>>b>>c;
    cost[a] = min(b, c);
  }
  for(int i=2 ; i<=M ; i++) for(int j=0 ; j+i<=M ; j++){
    dp[j][j+i] = min(dp[j][j+i-1] + cost[str[j+i-1]], dp[j+1][j+i] + cost[str[j]]);
    if(str[j] == str[j+i-1])
      dp[j][j+i] = min(dp[j][j+i], dp[j+1][j+i-1]);
  }
  cout<<dp[0][M]<<endl;
}


{% endhighlight %}
