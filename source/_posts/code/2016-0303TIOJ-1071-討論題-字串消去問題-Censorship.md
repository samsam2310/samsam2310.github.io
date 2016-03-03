---
title: 'TIOJ::1071 . [討論題] 字串消去問題(Censorship)'
tags:
  - C++
  - TIOJ
  - Trie
category:
  - Code 備忘錄
  - DP
date: 2016-03-03 14:30:57
---


複雜的字串DP問題。
但是沒有很難寫。

<!--more-->

題目給你一個字串S，還有一堆字串集合A，每個長度都是N。如果你可以在S找到某個子字串等於某個A的話，你就可以把他刪掉，剩下的字串可以再重複做這件事，問你S可以被刪到最少剩下幾個字元。

從題目中可以看到，刪掉字串是可以遞迴重複刪的，也就是baab可以先刪掉aa再刪掉剩下的bb。

一開始寫了一個暴力解50分，就是直接遞迴刪字串，然後把到過的字串丟進map裡紀錄到過的狀態，可是這樣會MLE。


這題看起來也很DP可是想不到怎麼寫，最後只好偷看[CBD的解答](http://cbdcoding.blogspot.tw/2015/07/tioj-1071-censorship.html)，然後才大概想到怎麼寫。

想了幾種作法之後，最後還是只能buttom-up。
這題需要兩層DP，第1層先轉換狀態，假設一個CD[i][j]表格，代表[i,j)可以完全刪掉，這樣我們就可以用N^2 DP答案，ANS[i]代表[0,i)最多可以刪掉多少字元，這樣就可以知道答案。

問題變成怎麼求CD陣列，想知道一個區間[i,j)可不可以刪除，可以用DP去求。
我們可以發現，一個需要抹掉兩次的字串，等價於我們可以找到很多段在i,j之間的子字串可以被抹掉，然後被抹掉後的剩餘字串也可以被抹掉。
i,j之間可以被抹掉的字串是DP結果，而剩下的字串一定是一個存在於A集合中的一個字串（如果不是，這個字串就不能在兩步驟之內被抹去）。於是一個在N步內可以被消去（最多只要N步一定可以消完），可以重複這個DP N次即可（其實就是滾動）。
至於實作，就是開一個DP表格DP[p][k]，先建一棵TRIE（字典樹），把原本的集合A裡的字串都加進去，p代表的是TRIE上節點的編號，k代表的是字串S的位置，要是DP[p][k]是true，代表DP[p][k]是可以到達的。
狀態轉移有兩種，遍歷每個p和k，如果這個點可以到達就在轉移掉其他點，可以轉移的點有，如果第k格的字元在TRIE上走訪存在下一個節點p'的話，DP[p'][k+1]=true，再來就是如果k到後面某一個位置h的區間[k,h)可以抹去，則可以轉移到DP[p][h]=true，至於細節思考一下就可以理解。
之後再檢查所有TRIE上的結尾節點p''和k之後的位置h（集合A字串的結尾），如果DP[p''][h]是true則代表h可以到達而且是一個可以刪掉的字串結尾，所以CD[k][h]=true。

重複檢查直到沒有CD可以更新就跳出回圈，因為狀態轉移的時候要遍歷一次會比較慢，可以先用一個vector陣列跑一次把答案快取起來，速度會快一些。

TRIE上的節點最多會有N^2個，所以這個DP的複雜度是O(N^5)，N=50所以可以接受。


``` c++
#include <bits/stdc++.h>
#define F(n) Fi(i,n)
#define Fi(i,n) Fl(i,0,n)
#define Fl(i,l,n) for(int i=l;i<n;i++)
using namespace std;
const int N = 51, TM = 1001;
bool CD[N][N], DP[TM][N];
int ANS[N], TRI[TM][26], tri, END[N];
char S[N];
vector<int>cache[N];
inline int build_trie(string s){
    int now = 0;
    for(char c:s){
        int p = c-'a';
        if(TRI[now][p] == -1){
            memset(TRI[tri],-1,sizeof(TRI[tri]));
            TRI[now][p] = tri++;
        }
        now = TRI[now][p];
    }
    return now;
}
main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0);
    int n,m;
    string s;
    while(cin>>m,m){
        cin>>S;
        n = strlen(S);
        tri = 1;
        memset(TRI[0],-1,sizeof(TRI[0]));
        F(m){
            cin>>s;
            END[i] = build_trie(s);
        }
        memset(CD,0,sizeof(CD));
        bool stop = false;
        while(!stop){
            stop = true;
            fill(cache,cache+n, vector<int>());
            F(n)Fl(j,i+1,n+1)if(CD[i][j]){
                cache[i].push_back(j);
            }
            F(n){
                memset(DP,0,sizeof(DP));
                DP[0][i] = 1;
                Fi(j,tri)Fl(k,i,n)if(DP[j][k]){
                    int nex = S[k]-'a';
                    if(TRI[j][nex] != -1){
                        DP[TRI[j][nex]][k+1] = true;
                    }
                    for(int p:cache[k])DP[j][p] = true;
                }
                Fi(j,m)Fl(k,i,n+1)if(DP[END[j]][k] && !CD[i][k]){
                    CD[i][k] = true;
                    stop = false;
                }
            }
        }
        memset(ANS,0,sizeof(ANS));
        F(n+1){
            if(i)ANS[i] = max(ANS[i], ANS[i-1]);
            Fl(j,i+1,n+1)if(CD[i][j]){
                ANS[j] = max(ANS[j], (i?ANS[i-1]:0)+(j-i));
            }
        }
        cout<<(n-ANS[n])<<endl;
    }
}
```