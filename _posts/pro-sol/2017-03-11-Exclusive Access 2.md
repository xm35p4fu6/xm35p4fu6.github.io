---
layout: post
title: poj-3866 poj Exclusive Access 2
categories: pro-sol
excerpt: "Dilworth 進階"
tag: [poj, dilworth, set, dag, graph, dp]

---

## 傳送門：

#### [poj 3866 : Exclusive Access 2](http://poj.org/problem?id=3866)

## 題意：

如果是在ＰＯＪ看懂敘述才來，您真是辛苦了！！    
本題求：如何將無向圖化為有向無環圖(DAG)，並且使得最長路徑儘量小。    


## 思路：

如果只是要來看個***提示***的：   
將路徑視為chain的話，這題使用dp維護minimal antichain cover，antichain是一群無任何路徑的點。    

---   
  
這邊我們忽略繁雜的輸入輸出問題，需要可以參考code（寫的也不甚好就是了）。    

##### 第一步： 先整理出所有 antichain ，使用DP。    

枚舉所有點的選取狀態，對於每個狀態先抽出一點來檢查是否與其他點沒有連線，再檢查不包含此點的狀態來維護目前狀態。    


```

for(int mask=1 ; mask<(1<<cnt) ; mask++){
  withedge[mask] = pre_cal(mask);
}

inline bool pre_cal(int mask){
  int lsb = 0;
  while((1<<lsb) & ~mask) ++lsb;
  for(int pos = lsb+1 ; (1<<pos) < mask ; ++pos)
    if(((1<<pos) & mask) && conn[lsb][pos] ) 
      return true;
  return withedge[mask ^ (1<<lsb)];
}

```

##### 第二步：枚舉每個狀態的所有antichain子集合，找最優的來更新    

子集合找法不知道大家知道嗎，下方code第二行釀，整體複雜度是3^N    

```

for(int mask=1 ; mask<(1<<cnt) ; mask++){
  for(int submask=mask ; submask>0 ; submask = ((submask-1)&mask)){
    if(withedge[submask]) continue;	// 內部有邊就不是antichain
    if(dp[mask-submask] + 1 < dp[mask]){
      dp[mask] = dp[mask-submask] + 1;
      dprecord[mask] = submask;		// 要邊紀錄用哪的子集合更新的
    }
  }
}

```

最後最後，沿著剛剛記錄下來的子集合trace回去，邊幫各團分組，用力一拍就可以印出答案了！！    


## AC code:

{% highlight cpp linenos %}

int T,M,N,K,I,c,ans,cnt;
char to_char[20];
int to_int[200], to_grp[200];
bool conn[20][20], withedge[3000000];
int dp[50000], dprecord[50000];
string in[105];

inline bool pre_cal(int mask){
  int lsb = 0;
  while((1<<lsb) & ~mask) ++lsb;
  for(int pos = lsb+1 ; (1<<pos) < mask ; ++pos)
    if(((1<<pos) & mask) && conn[lsb][pos] ) 
      return true;
  return withedge[mask ^ (1<<lsb)];
}

void init(){
  string s1,s2;
  char a,b;
  memset(to_int, -1, sizeof(to_int));
  cin >> N;
  for(int i=0;i<N;i++){
    cin >> s1 >> s2;
    a = s1[0], b = s2[0];
    if(to_int[a] == -1){      // 沒出現過就開新組別
      to_int[a] = cnt;
      to_char[cnt++] = a;
    } 
    if(to_int[b] == -1){
      to_int[b] = cnt;
      to_char[cnt++] = b;
    }
    conn[to_int[a]][to_int[b]] = 1;     // 建圖
    conn[to_int[b]][to_int[a]] = 1;
    in[i] = a;        // 紀錄 input 
    in[i] += b;
  }
  fill(dp, dp+50000, 30);
  dp[0] = 0;
}
void sol(){
  for(int mask=1 ; mask<(1<<cnt) ; mask++){
    withedge[mask] = pre_cal(mask);
  }

  for(int mask=1 ; mask<(1<<cnt) ; mask++){
    for(int submask=mask ; submask>0 ; submask = ((submask-1)&mask)){
      if(withedge[submask]) continue;
      if(dp[mask-submask] + 1 < dp[mask]){
        dp[mask] = dp[mask-submask] + 1;
        dprecord[mask] = submask;
      }
    }
  }
  
  //    沿著trace回去 順便重新分組
  for(int mask=(1<<cnt)-1, grp=0 ; mask ; ++grp,mask -= dprecord[mask]){
    int sbmsk = dprecord[mask] ; 
    for(int i=0 ; sbmsk>>i ; i++)if((sbmsk>>i) & 1)
      to_grp[i] = grp;
  }
  
  //    用力拍出答案！
  cout << dp[(1<<cnt)-1]-2 << endl;
#define group(x) (to_grp[to_int[in[i][x]]])
  for(int i=0;i<N;i++){
    if( group(0) > group(1)) swap(in[i][0], in[i][1]);
    cout << in[i][0] << " " << in[i][1] << endl;
  }
}

int main(){
  init();
  sol();
}


{% endhighlight %}
