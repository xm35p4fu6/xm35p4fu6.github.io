---
layout: post
title: poj-2674 Linear world
categories: pro-sol
excerpt: ""
tag: [poj, ]

---

## 傳送門：

#### [Linear world](http://poj.org/problem?id=2674)

## code:

{% highlight cpp linenos %}

int T,M,N,K,I,a,b,c,ans,cnt;
double Len, V;
bool dir[33000];
string name[33000];
char tmp;
pair<double, int> arr[33000];

void init(){
  cin >> Len >> V;
  for(int i=0;i<N;i++){
    cin >> tmp >> arr[i].X >> name[i];
    dir[i] = (tmp == 'p' || tmp == 'P');
    arr[i].Y = i;
  }
  cnt = 0;
}
void sol(){
  int dir0=-1, dir1=-1;
  sort(arr, arr+N);
  for(int i=0;i<N;i++)
    if(dir[arr[i].Y]){
      dir1 = i;
      break;
    }
  for(int i=N-1;i>=0;i--)
    if(!dir[arr[i].Y]){
      dir0 = i;
      break;
    }
  if(dir0==-1 or (dir1!=-1 and Len-arr[dir1].X > arr[dir0].X)){
    for(int i=dir1;i<N;i++) if(!dir[arr[i].Y])
      cnt++;
    int ans = ((Len-arr[dir1].X)/V * 100);
    cout << setw(13) << fixed << setprecision(2)
      << ((double)ans/100) << " " << name[arr[dir1+cnt].Y] << endl;
  }
  else{
    for(int i=dir0;i>=0;i--) if(dir[arr[i].Y])
      cnt++;
    int ans =  (arr[dir0].X/V)*100 ;
    cout << setw(13) << fixed << setprecision(2)
      << ((double)ans/100) << " " << name[arr[dir0-cnt].Y] << endl;
  }
}

int main(){
  ios::sync_with_stdio(false);cin.tie(0);
  while(cin >> N and N){
    init();
    sol();
  }
}

{% endhighlight %}
