---
title: 'TIOJ::1684 . 圓桌武士 Knights of the Round Table'
tags:
  - C++
  - TIOJ
  - POJ
  - BCC
  - Tarjan
category:
  - Code 備忘錄
  - Graph
date: 2015-02-27 09:05:18
---


http://tioj.ck.tp.edu.tw/problems/1684

有趣的圖論題，感覺很複雜，結果難得傳一次就AC~~。
跟點雙連通有關。

<!--more-->

題目是有一堆圓桌武士，它們之間可能會互相仇恨。
每次聚會時必須符合 1. 人數是奇數 2.互相仇恨的人不能做在相鄰的位置。
如果依照此規則，有些人沒辦法參加任何聚會，輸出不能參加任何聚會的人數。

我們先把題目給的仇恨關係建成**反圖**，也就是互相不仇恨關係，所以 A 跟 B 有邊若且為若 **A 跟 B 可以坐一起**。
然後我們發現只要一個點**屬於一個奇環**(有奇數個點的環)，那他一定有辦法參加聚會(那個奇環剛好可以湊一桌，滿足奇數個人、每個相鄰的都不討厭對方)。

所以題目變成有幾個點不屬於任何奇環。

這題要用到一個很神奇的性值：**一個點雙連通分量中存在奇環**，若且為若**這個分量中所有點都在奇環的附蓋下**。

這個性質感覺不太難證，因為點雙連通分量中任意兩個點間都有至少兩條路徑，如果有一個奇環，不在奇環上的某個點 A，一定會連到奇環上的至少兩個相異點(連到同一個點的話那個點就是割點，點雙連通中不存在割點)，於是那兩個點把奇環切成奇數長路徑跟偶數長路徑，這樣一定可以用這兩條路徑跟A到奇環的兩條路徑湊出一個包含A的奇環。

所以只要一個點雙連通裡有奇環就標記掉所有在雙連通裡的點，沒被標到的點的數量就是答案。

至於怎麼找雙連通，運用 [Tarjan's Alogrithm找割點](/code/TOJ-183-高棕櫚傳遞鏈/)的概念，並且在每次進去DFS的時候把點加入Stack裡，如果一個子節點的 low 值大於等於自己的 dfs值，就代表自己是割點，也代表剛剛進去DFS那一群點是一個雙連通分量(屬於其他雙連通分量的點都在遞迴時被拔掉了)，那就把Stack裡的點POP出來(POP到剛剛的子節點)，同時在DFS的時候進行著色，如果戳到一個走過的點就跟她進行比較，如果顏色一樣代表有奇環，return 時 回傳 true，在POP點時如果剛剛DFS時是true代表此雙連通有奇環，全部標記掉(記得標記現在的點)。
然後root記得也要判，其實也就是root有第二個子節點以後的子節點全都是獨立的雙連通分量。
最後DFS完剩下的一群點(還在Stack裡的)也是一個雙連通，不要忘記標記了。

好像有點複雜，其實就是DFS時維持這個DFS Tree沒有割點，有就把會被割掉的部分拔掉，這樣就沒有割點了。

有沒有奇環的變數，如果這個點是割點就做上面做的事，不是割點了話要記錄起來，最後return回去。

PS.其實可以找出雙連通後再DFS一次著色，但是我喜歡壓成一個DFS。




``` c++
#include <cstdio>
#include <cstring>
#include <algorithm>
#define F(n) Fi(i,n)
#define Fi(i,n) for(int i=0;i<n;i++)
#define N 1002
using namespace std;
int ST[N],st,dfs[N],dfsn,low[N],n,child;
bool color[N],E[N][N],ANS[N];
int gin(){
    int a;char c;
    while(c=getchar(),c<'0'||c>'9')if(c==-1)return -1;
    a=c-'0';
    while(c=getchar(),c>='0'&&c<='9')a=a*10+c-'0';
    return a;
}
bool DFS(int now,bool clr,bool root=false){
    bool odd=false,tmp;
    dfs[now]=low[now]=++dfsn;
    color[now]=clr;
    ST[st++]=now;
    F(n)if(!E[now][i+1]){
        child++;
        E[now][i+1]=E[i+1][now]=1;
        if(!dfs[i+1]){
            tmp=DFS(i+1,!clr);
            low[now]=min(low[now],low[i+1]);
            if(!root&&low[i+1]>=dfs[now]||root&&child>1){
                do{
                    st--;
                    if(tmp)ANS[ST[st]]=true;
                }while(ST[st]!=i+1);
                if(tmp)ANS[now]=true;
            }else odd|=tmp;
        }else{
            low[now]=min(low[now],dfs[i+1]);
            if(color[now]==color[i+1])odd=true;
        }
    }
    return odd;
}
main(){
    int m,a,b;
    while(n=gin(),m=gin(),n||m){
        memset(E,0,sizeof(E));
        F(N)E[i][i]=1;
        memset(ST,0,sizeof(ST));
        memset(dfs,0,sizeof(dfs));
        memset(ANS,0,sizeof(ANS));
        st=dfsn=0;
        F(m)a=gin(),b=gin(),E[a][b]=E[b][a]=1;
        F(n)if(!dfs[i+1]){
            child=0;
            bool tmp=DFS(i+1,false,true);
            while(st)ANS[ST[--st]]|=tmp;
        }
        int ans=0;
        F(n)ans+=!ANS[i+1];
        printf("%d\n",ans);
    }
}
```