---
layout: post
title: spoj-subst1 字串練習
categories: pro-sol
excerpt: "求相異子字串個數"
tag: [spoj, string, sa]

---

## 傳送門：

#### [SUBST1 - New Distinct Substrings](http://www.spoj.com/problems/SUBST1/)

## 題意：
    
輸入字串Ｓ，求Ｓ的相異子字串數量。    

## 思路：

使用 suffix array + height array 求出。    
    
經過仔細觀察，可發現相異子字串的數量即為 suffix tree 上節點的數量，接著我們又發現 suffix tree
上節點的數量即為每個suffix的長度扣除與上一個suffix的LCA。    

suffix tree 上的 LCA 也就是 lcp(height) array，於是題目所求等同    
    
***子字串數量-LCP陣列的值***    

## code:

{% highlight cpp linenos %}

#include <bits/stdc++.h>
#define rank rk
using namespace std;
const int MXN = 1e5 + 5;
int n, k, T;
int rank[MXN], tmp[MXN];
int suffix[100010], lcp[100010];
bool cmp_sa(int i, int j){
  if(rank[i] != rank[j])
    return rank[i] < rank[j];
  int ii = i+k<=n ? rank[i+k] : -1;
  int jj = j+k<=n ? rank[j+k] : -1;
  return ii < jj ;
}

void build_sa(string s, int* sa){ // O(nlg2n)
  n = s.length();
  for(int i=0;i<=n;i++){
    sa[i] = i;      // 先填入sa
    rank[i] = i<n ? s[i] : -1;    // ascii當排名用
  }
  for(k=1;k<=n;k<<=1){
    sort(sa, sa+n+1, cmp_sa);     // 依照排名sort sa
    tmp[sa[0]] = 0;             // 初始化第0名
    for(int i=1;i<=n;i++)         // 依照sa重新排名
      tmp[sa[i]] = tmp[sa[i-1]] + (cmp_sa(sa[i-1], sa[i]) ? 1 : 0);
    for(int i=0;i<=n;i++)         // 儲存排名結果
      rank[i] = tmp[i];
  }
}

void build_lcp(string s, int* sa, int* lcp){
  int n = s.length(), h = 0;
  lcp[0] = 0;
  for(int i=0;i<n;i++){
    int j = sa[rank[i] - 1];    // 存下排名在i之前
    if(h>0) h--;
    for(;j+h<n && i+h<n; h++)
      if(s[j+h] != s[i+h])
        break;
    lcp[rank[i] - 1] = h;
  }
}
int main(){
  ios::sync_with_stdio(0);
  string str;
  cin>>T;
  while(T--){
    long long ans=0;
    cin>>str;
    build_sa(str, suffix);
    build_lcp(str, suffix, lcp);
    for(int i=0;i<=str.length();i++){
      ans+=(suffix[i]-lcp[i]);
    }
    cout<<ans<<endl;
  }
}

{% endhighlight %}
