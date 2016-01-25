---
title: 'HOJ::Problem : 192 - 撿肥皂'
tags:
  - C++
  - HOJ
category:
  - Code 備忘錄
  - DP
date: 2014-03-26 05:45:41
---


http://acoj.twbbs.org/blog_article.php?id=418
DP，狀態壓縮。

<!--more-->

這題也是參考別人才寫出來的，以後要多練習自己想出來.....。
題目雖然說可以上下左右走，但是因為要不繞路，所以其實只能走右、下。首先因為有兩個人，DP狀態 DP[k][i][j] 代表走了k步後，第一個人的x座標是i，第二個人的x座標是j，(兩個人的y座標分別是 k-i,k-j，因為只能往下、右走)，並假設第二個人永遠比第一個人右邊，狀態轉移就是
if i==j  (兩個人走到同一格)
DP[k][i][j]= DP[k-1][i][j]  , DP[k-1][i-1][j] , DP[k-1][i-1][j-1]  這三個取max，加上這一格的權重D[k-i][i]。

if  i!=j (兩個人走到不同格)
DP[k][i][j]= DP[k-1][i][j]  , DP[k-1][i-1][j] , DP[k-1][i][j-1] , DP[k-1][i-1][j-1]  這四個取max，加上這兩個人走到的權重 D[k-i][i]+D[k-j][j]。

以上利用從左邊、上面走道這一格取max轉移，然後我把k壓掉，每次i,j從後面跑回來(不然順序會錯)，所以陣列只有二維。

第一次define for迴圈，感覺不錯，可是思考起來不太習慣，比賽還是好好寫for迴圈好了。

付上神人的題解：
http://chiangyo.blogspot.tw/2012/10/hoj-192.html



``` c++
#include <cstdlib>
#include <iostream>
#define N 305
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
#define min(x,y) ((x)<(y)? (x):(y))
#define max(x,y) ((x)>(y)? (x):(y))
using namespace std;
int D[N][N],DP[N][N];
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int n;
    fill((int*)DP,(int*)DP+N*N,(int)-1e9);
    cin>>n;
    F(n)
        Fi(j,n)
            cin>>D[i+1][j+1];
    DP[1][1]=D[1][1];
    Fl(k,3,n*2+1){
        int mk=min(n,k-1);
        for(int i=mk;i>0&&k-i<=n;i--){
            for(int j=mk;j>=i&&k-j<=n;j--){
                if(i==j){
                    DP[i][j]=max(DP[i][j],max(DP[i-1][j],DP[i-1][j-1]))+D[k-i][i];
                }else{
                    DP[i][j]=max(max(DP[i][j],DP[i-1][j]),max(DP[i][j-1],DP[i-1][j-1]))+D[k-i][i]+D[k-j][j];
                }
                //cout<<k<<' '<<i<<' '<<j<<' '<<DP[i][j]<<endl;
            }
        }
    }
    cout<<DP[n][n]<<endl;
    //system("pause");
    return 0;
}
```