---
title: 'TIOJ::1051 . G.考古問題'
tags:
  - C++
  - TIOJ
  - NPSC
  - LIS
  - LCS
  - DP回溯
category:
  - Code 備忘錄
  - DP
date: 2016-02-25 12:33:44
---


http://tioj.ck.tp.edu.tw/problems/1051

動態規劃題，最後還是有點被捏才寫出來QQ。
這種有點小複雜的DP要是不能快速列出轉移式，TOI真的會不太好打...。

<!--more-->

題目給你兩串一樣長的數列，兩串數列中各自不會有重複的數字。
題目問你最長遞增共同子序列是什麼，請你印出來。

題目一開始會被當作單純的共同子序列去做，不過我不知道怎麼證明答案會再最長共同子序列中，所以不知道怎麼寫。

而其他人的寫法看起來也都是什麼N^3的寫法，或是不知道用到了什麼特殊性質，總之，想不出來。

然後，我就發現，原來題目只是單純的N^2最長遞增子序列而已！！！
作法就是利用把最長共同子序列轉成遞增子序列的方法，也就是紀錄一個同時出現在兩個數列的數字，在A數列和B數列中的位置，並轉化成最長遞增子序列去作（依照A數列的位置排序，只要B數列的位置的最長遞增子序列就是原本的最長共同子序列）。
現在我們要再加一個條件，也就是把原本的數字放進去，變成3個數字一組，依照A數列的位置排序，之後定義小於代表B數列的位置比較小且題目給的數字比較大（因為題目要遞減），因為定義了小於，所以其他就一樣，做遞增子序列，並紀錄前一個數字是誰，藉此回溯出答案。

要注意因為數組是二維的（三維轉二維），所以不可以用NlgN的最長遞增子序列的算法，因為二維不符合貪心的單調性。
因為都是用一些N^2的作法，整體code很好寫，總複雜度N^2。


``` c++
#include <bits/stdc++.h>
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
using namespace std;
const int N = 513;
struct MP{
    int x,y,z;

    // 定義三維數組的小於
    bool operator<(MP b){
        return x<b.x&&y<b.y&&z>b.z;
    }
}S[N];
int D[2][N], st, DP[N], PRE[N];
main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0);
    int t,n;

    // 輸出答案的遞迴函數
    function<void(int)> printANS = [&](int x)->void{
        if(PRE[x]!=-1){
            printANS(PRE[x]);
            cout<<' ';
        }
        cout<<S[x].z;
    };

    cin>>t;
    while(t--){
        // 初始化
        memset(DP,0,sizeof(DP));
        memset(PRE,-1,sizeof(PRE));
        
        // 輸入數列
        cin>>n;
        F(2)Fi(j,n)cin>>D[i][j];
        
        // 共同子序列轉化成遞增子序列問題
        st = 0;
        F(n)Fi(j,n){
            if(D[0][i]==D[1][j]){
                S[st++] = (MP){i,j,D[0][i]};
                break;
            }
        }
        
        // N^2 最長遞增子序列
        F(st)Fl(j,i+1,st){
            if(S[i] < S[j] && DP[i]+1 > DP[j]){
                DP[j] = DP[i]+1;
                PRE[j] = i;
            }
        }

        // 找出最長的答案
        int mx = -1,pos = 0;
        F(st)if(DP[i] > mx){
            mx = DP[i];
            pos = i;
        }

        // print 出來
        if(st)printANS(pos);
        cout<<endl;
    }
}
```