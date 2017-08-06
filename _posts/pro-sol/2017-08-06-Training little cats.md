---
layout: post
title: poj-3735 Training little cats
categories: pro-sol
excerpt: "矩陣快速冪解遞迴"
tag: [poj, matrix]

---

## 傳送門：

#### [Training little cats](http://poj.org/problem?id=3735)

## 題意：

有窩可憐的小貓\\( C_1 \\) ~ \\( C_N \\)，要被命令做一堆操作Ｍ次，包括：  

1. 得到一個堅果  
2. 把堅果全部送進肚子  
3. 把所有堅果與另一隻貓交換  

為了解就可憐的小貓，我們直接算出最後每隻貓有多少個堅果並給牠發上。  

## 思路：

把這些操作想成一個矩陣，這樣做Ｍ次就是矩陣Ｍ次方，我們可以用矩陣快速冪砸他！  

$$

\left[
\begin{matrix}
 C_1    \\
 \vdots \\
 C_N
\end{matrix}
\right]

=

{
\left[
\begin{matrix}
 ?      & \cdots & ?      \\
 \vdots & \ddots & \vdots \\
 ?      & \cdots & ?
\end{matrix}
\right]
}^M

\left[
\begin{matrix}
 C_1    \\
 \vdots \\
 C_N
\end{matrix}
\right]

$$  

第一個操作是得到一個堅果，為了達成這個操作我們擴展一下矩陣，加入一個常數１，這樣可以透過它乘上１來達成此操作。  

第二個操作是歸零，也就是將這隻貓的row全部歸零即可。  

第三個操作是交換，那就把這兩個row，swap一下就好了！  

先上個範例測資：

```
Sample Input:
3 1 6
g 1
g 2
g 2
s 1 2
g 3
e 2
0 0 0

Sample Output:
2 0 1

```

初始矩陣為：

$$

\left[
\begin{matrix}
 C_1    \\
 C_2    \\
 C_3    \\
 1
\end{matrix}
\right]

=

{
\left[
\begin{matrix}

  1 & 0 & 0 & 0 \\
  0 & 1 & 0 & 0 \\
  0 & 0 & 1 & 0 \\
  0 & 0 & 0 & 1

\end{matrix}
\right]
}

\left[
\begin{matrix}
 C_1    \\
 C_2    \\
 C_3    \\
 1
\end{matrix}
\right]

$$  

接著我們開始依據操作來製造方陣：  

``` g 1  // 第一隻貓得一個堅果```  

  $$ 

  \left[
  \begin{matrix}

    1 & 0 & 0 & 1 \\
    0 & 1 & 0 & 0 \\
    0 & 0 & 1 & 0 \\
    0 & 0 & 0 & 1

  \end{matrix}
  \right]

  $$

``` g 2  *2 ```  

  $$ 

  \left[
  \begin{matrix}

    1 & 0 & 0 & 1 \\
    0 & 1 & 0 & 2 \\
    0 & 0 & 1 & 0 \\
    0 & 0 & 0 & 1

  \end{matrix}
  \right]

  $$

``` s 1 2 // 第一隻貓與第二隻貓對調 ```  

  $$ 

  \left[
  \begin{matrix}

    0 & 1 & 0 & 2 \\
    1 & 0 & 0 & 1 \\
    0 & 0 & 1 & 0 \\
    0 & 0 & 0 & 1

  \end{matrix}
  \right]

  $$

``` g 3 ```  

  $$ 

  \left[
  \begin{matrix}

    0 & 1 & 0 & 2 \\
    1 & 0 & 0 & 1 \\
    0 & 0 & 1 & 1 \\
    0 & 0 & 0 & 1

  \end{matrix}
  \right]

  $$

``` e 2 // 第二隻貓堅果清空 ```  

  $$ 

  \left[
  \begin{matrix}

    0 & 1 & 0 & 2 \\
    0 & 0 & 0 & 0 \\
    0 & 0 & 1 & 1 \\
    0 & 0 & 0 & 1

  \end{matrix}
  \right]

  $$

推完方陣後，套進去矩陣快速冪基本上就解完了，有可能吃一個TLE，吃TLE大概是矩陣乘法沒寫好，這矩陣大部分是０，可以對稀疏矩陣優化，詳細看code～  


## code:

{% highlight cpp linenos %}

typedef long long ll;
// ele 為 矩陣內元素
typedef ll ele;
typedef vector<ele> vec;
typedef vector<vec> mat;

int N, M, K;

mat Mat, Base;

mat mul(mat &A, mat &B){              // 矩陣乘法
  //assert(A[0].size() == B.size());        // include cassert
  mat res (N+1, vec(N+1));
  for(size_t i=0; i<=N; ++i)
    for(size_t j=0; j<=N; ++j) if(A[i][j])    // 可在稀疏矩陣時大大加速
      for(size_t k=0; k<=N; ++k)
        res[i][k] += A[i][j]*B[j][k];
  return res;
}

mat pow(mat A, ll n){       // 矩陣快速冪，必須是方陣
  mat res (N+1, vec(N+1));
  for(size_t i=0;i<=N;++i) res[i][i] = 1;
  for(;n; A=mul(A,A), n>>=1) if(n&1) res = mul(res,A);
  return res;
}
void init(){
  int a,b;
  Mat.clear();
  Base.clear();
  Mat = mat(N+1, vec(N+1));
  Base = mat(N+1, vec(N+1));
  for(int i=0; i<=N; ++i) Mat[i][i] = 1;      // 元矩陣
  Base[N][0] = 1;

  while(K--){
    scanf("%s", in);
    if(in[0] == 'g'){
      scanf("%d", &a);
      Mat[a-1][N]++;
    }
    else if(in[0] == 'e'){
      scanf("%d", &a);
      for(int i=0; i<=N; ++i) Mat[a-1][i] = 0;
    }
    else{
      scanf("%d %d", &a, &b);
      swap(Mat[a-1], Mat[b-1]);
    }
  }
}
void sol() {
  mat res = pow(Mat, M);
  res = mul(res, Base);
  for(int i=0; i<N; ++i) cout << res[i][0] << " \n"[i==N-1];
}

int main(){
  while(scanf("%d %d %d", &N, &M, &K) == 3 && (N||M||K)) {
    init();
    sol();
  }
}

{% endhighlight %}
