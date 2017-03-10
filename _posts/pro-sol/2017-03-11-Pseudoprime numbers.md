---
layout: post
title: poj-3641-Pseudoprime numbers
categories: pro-sol
excerpt: "fast power"
tag: [poj, math]

---

## 傳送門：

#### [Pseudoprime numbers](http://poj.org/problem?id=3641)

## 題意：   

給定 p, a ，在p非質數的前提下是否 \\( a^{p}=a\; \left( \mbox{mod}\; p \right) \\) 。   

## 思路：

看code不解釋。    

## code:

{% highlight cpp linenos %}

vector<ll> prim;
bool prim_[46345];

void build(){
  fill(prim_, prim_ + 46345, 1);
  prim_[0] = prim_[1] = 0;
  for(int i=2;i<46345;i++)if(prim_[i]){
    prim.push_back(i);
    for(int j = i*i ; j<46345 ; j+=i)
      prim_[j] = 0;
  }
}

inline bool is_pr(ll x){
  if(x < 46345) return prim_[x];
  for(int i=0;i<prim.size() && prim[i]*prim[i]<=x ; i++) if(x % prim[i] == 0)
    return 0;
  return 1;
}
inline ll mul( ll _x , ll _y , ll _mod ){
  _x *= _y;
  return _x >= _mod ? _x % _mod : _x;
}
ll mypow( ll _a , ll _x , ll _mod ){
  if( _x == 0 ) return 1ll;
  ll _ret = mypow( mul( _a , _a , _mod ) , _x >> 1 , _mod );
  if( _x & 1 ) _ret = mul( _ret , _a , _mod );
  return _ret;
}
////////  template end   //////////

ll T,M,N,K,I,a,b,c,ans,cnt;

bool sol(){
  if(is_pr(a)) return false;
  return (mypow(b,a,a) == b);
}

int main(){
  build();
  while(cin>>a>>b && (a+b)){
    cout << ((sol()) ? "yes\n" : "no\n");
  }
}

{% endhighlight %}
