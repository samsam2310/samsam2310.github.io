---
title: 'TIOJ::1041 . D.運河'
tags:
  - C++
  - TIOJ
  - NPSC
  - 凸包
  - 旋轉卡尺
category:
  - Code 備忘錄
  - 計算幾何
date: 2016-01-31 13:31:47
---


http://tioj.ck.tp.edu.tw/problems/1041
這題是非常非常靠北的計算幾和題。
有很多的特殊狀況，浮點數誤差也要仔細考量。

<!--more-->

因為很久沒有寫題目了，所以採了很多很爛的地雷...。

這題題目是，你有兩組點集，你要用兩條平行線將平面分成左右兩個部份（平行線中間的區域是運河），而這兩組點集要剛好個別完全分佈在河的兩側，要是沒辦法分開兩組點集的話就輸出IMPOSSIBLE，不然輸出運河最大的寬度。

這個題目很容易就可以發現，我們要對兩個點集做凸包，之後問題就是兩個多邊形之間的問題。
因為兩個點集要被分開，所以兩個多邊形不可以相交。

於是我們可以利用旋轉卡尺來解這一題。
旋轉卡尺就是沿著多邊形的邊，不停的旋轉兩條平行線，而且他們一直和多邊形相切。
這題我們必須個別沿著兩個多邊形的編做旋轉卡尺，實作上就是兩條向量。

{% asset_img 001.png %}
{% asset_img 002.png %}

要計算答案，可以用高中數學中教的兩平行線距離公式來解，但是有趣的是，原本的公式有加絕對值，但是只要我們仔細的推一下，就可以利用這個公式的正負號來當作方向。
一開始先找到兩個多邊形的最左下跟最右上的點，分別給他們（0,-1）（0,1）兩個向量，這樣絕對是合法的兩條平行線。
然後開始旋轉，並計算距離，當距離是正數的時候就是合法的距離，就更新答案（或是負數，依照不同的算法會有差異）。

旋轉的過程中，為了轉到所有的角度，必須要看轉1號向量還是2號向量，旋轉量較小。
因為要切到兩個多邊形的話，所有平行線一定有一邊有切到多邊形的邊，所以我們只要比較1號多邊形跟2好多邊形接下來的那條邊，哪個需要轉的旋轉量比較小，這可以透過cross這兩條邊，大於180和小於180分別代表要轉1號和要轉2號。

只是在作距離的時候有個例外，就是最佳解可能沒有切到多邊形的邊，而是分別切在頂點上，這個case，距離就是這兩個頂點的距離（畫個圖就可以證明了），而符合這個case的條件就是，垂直這兩個頂點連線的兩條向量，斜率介於兩個多邊形的前後兩條邊之間，反正就是畫個圖就明白了～。


重點來了，這題有很多陷阱，第一個就是，點的數量可能只有1到2個，這種情況下會出現很多特殊狀況讓你RE。
首先兩個點比較簡單，只要注意在計算要旋轉1號向量還是2號向量的時候，如果平行的話要兩個一起轉，就可以避免不停的轉同一個多邊形而WA的狀況。
再來一個點的狀況，如果兩組點都是一個，那就直接計算距離（小心點可能會重疊，要輸出IMPOSSIBLE），如果一組是一個點一組不是，那在計算要旋轉那個向量的時候要特判，永遠轉1號並讓2號跟著1號轉（2號是一個點的）。

還有再做凸包的時候，小心一兩個點的情況，而且不管如何都要記得排序所有點，再做凸包（三角形記得做，不然有機會變成順時針排列的3個點），才會符合多邊形的點是逆時針排列。

最後就是點有可能重複，記得先做unique把重複的點去掉，再開始檢查點的數量喔。


這裡提一下浮點數誤差，如果要判斷符點數的大於小於等於，都要加一個誤差值EXP，大約10^-9，因為浮點數用二進位表示分數有可能不完整到至有很小很小的誤差，讓你再判斷等於的時候比錯。0的範圍要當作是-EXP到EXP之間。


``` c++
#include <bits/stdc++.h>
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
using namespace std;
const int N = 51;
const double EXP = 1e-9;
typedef pair<double,double> PDD;
PDD D[N], E[N], ST[N];
int st;
struct V{
    double x,y;
    V(){}
    V(double _x,double _y):x(_x),y(_y){}
    V(PDD a,PDD b):x(b.first - a.first), y(b.second - a.second){}
    V turn(){
        return V(-x,- y);
    }
};
double cross(V a, V b){
    return a.x*b.y - a.y*b.x;
}
inline bool biger(double x){
    return x>EXP;
}
inline bool equal(double x){
    return -EXP<x&&x<EXP;
}
void my_unique(PDD *P, int &_n){
    int pre = 0;
    F(_n-1){
        if(P[pre]!=P[i+1]){
            P[++pre] = P[i+1];
        }
    }
    _n = pre+1;
}
void hull(PDD *P, int &_n){
    sort(P,P+_n);
    my_unique(P, _n);
    if(_n<=2)return;
    st=0;
    F(_n){
        while(st>=2 && !biger(cross(V(ST[st-2], ST[st-1]), V(ST[st-1], P[i]))) ){
            st--;
        }
        ST[st++] = P[i];
    }
    st--;
    int tag = st+2;
    F(_n){
        while(st>=tag && !biger(cross(V(ST[st-2], ST[st-1]), V(ST[st-1], P[_n-i-1]))) ){
            st--;
        }
        ST[st++] = P[_n-i-1];
    }
    st--;
    F(st)P[i] = ST[i];
    _n = st;
}
inline double countC(PDD p, V v){
    //cout<<"coutC: "<<p.first*v.y + p.second*(-v.x)<<endl;
    return p.first*v.y + p.second*(-v.x);
}
double distance(PDD a, V v1, PDD b, V v2){
    //cout<<"V "<<v1.x<<' '<<v1.y<<' '<<v2.x<<' '<<v2.y<<endl;
    double dis = -countC(a,v1) - countC(b,v2);
    //cout<<"-C-C: "<<dis<<endl;
    if(!biger(dis)){
        return 0.0;
    }else{
        //cout<<"DD         "<<a.first<<' '<<a.second<<' '<<b.first<<' '<<b.second<<endl;
        return dis / sqrt(v1.x*v1.x + v1.y*v1.y);
    }
}
inline bool check_middle(V a, V b, V c){
    return biger(cross(a,b)) && biger(cross(b,c));
}
inline double distanceP(PDD a, PDD b){
    return sqrt((a.first-b.first)*(a.first-b.first) + (a.second-b.second)*(a.second-b.second));
}
void solve(PDD *P1, int _n, PDD *P2, int _m){
    V v1 = V(0,-1), v2 = V(0,1);
    int p1 = 0,p2;
    F(_m)if(P2[(i+1)%_m]<P2[i]){
        p2 = i;
        break;
    }
    double ans = 0;
    F(_n+_m){
        //cout<<"D "<<p1<<' '<<p2<<endl;
        double tmp = distance(P1[p1], v1, P2[p2], v2);
        if(biger(tmp)){
            ans = max(tmp, ans);
        }
        V PtP = V(P1[p1].second-P2[p2].second, P2[p2].first-P1[p1].first),
            v1n = V(P1[p1],P1[(p1+1)%_n]), v2n = V(P2[p2],P2[(p2+1)%_m]);
        //cout<<"PtP: "<<PtP.x<<' '<<PtP.y<<endl;
        if(_m==1)v2n = v1n.turn(); // dot!!!!
        if(check_middle(v1,PtP,v1n) && check_middle(v2,PtP.turn(),v2n)){
            tmp = distanceP(P1[p1], P2[p2]);
            ans = max(tmp, ans);
        }
        if(equal(cross(v1n, v2n)) ){
            v1 = v1n;
            v2 = v1.turn();
            p1 = (p1+1)%_n;
            p2 = (p2+1)%_m;
        }else if(biger(cross(v1n, v2n)) ){
            v2 = v2n;
            v1 = v2.turn();
            p2 = (p2+1)%_m;
        }else{
            v1 = v1n;
            v2 = v1.turn();
            p1 = (p1+1)%_n;
        }
    }
    if(biger(ans))cout<<ans<<endl;
    else cout<<"IMPOSSIBLE"<<endl;
}
bool dot_in_middle(PDD a, PDD b, PDD c){
    return a<b && b<c;
}
main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0);
    cout<<fixed<<setprecision(3);
    int n,m;
    while(cin>>n>>m, n||m){
        F(n)cin>>D[i].first>>D[i].second;
        F(m)cin>>E[i].first>>E[i].second;
        hull(D, n);
        hull(E, m);
        if(n==1 && m==1){
            double ans = distanceP(D[0],E[0]);
            if(biger(ans))cout<<ans<<endl;
            else cout<<"IMPOSSIBLE"<<endl;
            continue;
        }
        // F(n)if(!biger(cross(V(D[i],D[(i+1)%n]), V(D[(i+1)%n], D[(i+2)%n])))){
        //     cout<<"ERROR :"<<i<<endl;
        // }
        // cout<<endl;
        // F(n)cout<<D[i].first<<' '<<D[i].second<<endl;
        // cout<<endl;
        // F(m)cout<<E[i].first<<' '<<E[i].second<<endl;
        if(m==1){
            solve(D,n,E,m);
        }else{
            solve(E,m,D,n);
        }
    }
}
```