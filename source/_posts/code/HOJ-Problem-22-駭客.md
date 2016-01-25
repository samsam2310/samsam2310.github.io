---
title: 'HOJ::Problem : 22 - 駭客'
tags:
  - C++
  - HOJ
category:
  - Code 備忘錄
  - Math
date: 2014-03-28 05:55:20
---


http://hoj.twbbs.org.tw/judge/problem/view/22
這題是數學題。

<!--more-->

這題題目問說，從小於a和小於b的數中各挑一個來組合，gcd(最大公因數)是d的有幾組。

問題可以轉換成 小於a/d 和小於b/d的整數中，互質的有幾個(gcd==1)，就把這兩個數分別假設為集合A和集合B吧。
在轉換問題，我們可以輕易求出，A和B中gcd大於k的組合數，也就是 (A/k)*(B/k)，也就是說，我們可以直接求出gcd大於1的有多少組，再扣掉大於2、3、4、一直到A或B的極限，就能得到gcd==1的個數了。

其實還少了一點，扣掉2的倍數就等於扣掉4、8、12..的倍數，所以只要是質因數裡有**兩個以上一樣的質因數**的數我們都不考慮（我們希望每個數被篩掉一遍，所以只要篩質數），再來，2和3都會扣掉6的倍數，所以如果相異質因數是偶數個代表多扣一次要加回來，奇數個代表多加一次要扣回來，這是 **排容原理**。

總之，我先建質數表，並預處理哪個數要忽略（標準分解式有次方不是1）(乘0)，哪個數要扣掉（奇數個相異質因數）(乘-1)，哪個數要加回去（偶數個相異質因數）(乘1)，並且對每筆詢問重複從1到A和B小的那個(小的除到剩1就結束了)除完，乘上參數(上面說的那個)，就能得到答案。

但是這樣速度太慢，所以我們又發現到，int 的除法很方便的會自動除到整數位，也因為這樣有些數字除起來答案一樣(10/6 、10/7、 10/8、 10/9 、10/10都一樣)，而我們就可以一起算這段一樣的數字。如果我們的 i 跑到了6，那10/6是1，再用10去除1變10，利用這種算法可以求出同樣答案最高位會到多少，然後在配上剛剛的參數(記得要把他改成前綴和)的區間和相乘，可以把A次加法減少到根號A次。要注意的是，因為我們同時算A和B，所以加速區間(好中二的說法.....)必須要A全部一樣B也全部一樣才行(但是A和B兩個不一定要一樣)，所以最高位的算法變成 A/(A/i)和B/(B/i) 取min。

我解題時遇到的問題，建表時沒有建好，導致答案出錯。還有小心爆int。最後一步的回圈只能跑到小的數字不然會發生除以0。

### 2015/2/27
原來上面那些參數叫做[默比烏斯函數](https://zh.wikipedia.org/wiki/%E9%BB%98%E6%AF%94%E4%B9%8C%E6%96%AF%E5%87%BD%E6%95%B0)，現在才知道...。



``` c++
#include <cstdlib>
#include <iostream>
#define LL long long
#define N 50001
#define sN 225
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
#define min(x,y) ((x)<(y)? (x):(y))
using namespace std;
int ST[sN],st,Pi[N];
bool P[N];
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    P[0]=P[1]=1;
    Fl(i,2,sN){
        if(!P[i]){
            ST[st++]=i;
            for(int j=i*i;j<N;j+=i)P[j]=true;
        }
    }
    Fl(i,2,N){
        if(!P[i])Pi[i]=-1;
        else{
            Pi[i]=1;
            int tmp=i;
            for(int j=0;ST[j]*ST[j]<=tmp;j++){
                if(tmp%ST[j]==0)tmp/=ST[j];
                else continue;
                if(tmp%ST[j]==0){Pi[i]=0;break;}
                else Pi[i]*=-1;
            }
            if(Pi[i]){Pi[i]*=-1;}
        }
        //if(i<100)cout<<i<<' '<<Pi[i]<<endl;
        Pi[i]+=Pi[i-1];
    }
    //Fl(i,1,100)cout<<i<<' '<<Pi[i]-Pi[i-1]<<endl;
    LL a,b,d;
    cin>>a;
    while(cin>>a>>b>>d){
        if(a>b){LL tmp=a;a=b;b=tmp;}
        a/=d;
        b/=d;
        //cout<<a<<' '<<b<<endl;
        LL ans=a*b;
        Fl(i,2,a+1){
            LL cut=min(a/(a/i),b/(b/i));
            ans+=((LL)Pi[cut]-(LL)Pi[i-1])*(a/i)*(b/i);
            i=cut;
            //ans+=Pi[i]*(a/i)*(b/i);
            //cout<<ans<<endl;
        }
        cout<<ans<<endl;
    }
    return 0;
}
```