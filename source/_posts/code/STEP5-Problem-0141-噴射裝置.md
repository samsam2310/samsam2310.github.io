---
title: 'STEP5::Problem 0141 : 噴射裝置'
tags:
  - C++
  - Step5
  - SCC
category:
  - Code 備忘錄
  - Graph
date: 2014-03-18 05:14:22
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0141
這題其實是圖論題，scc縮點後DFS最大值。

<!--more-->

scc，強連通元件(或分量??)，是指有向圖中一段圖任兩點互有路徑可以相通的狀態，如果在上面DFS可能會進入無限循環，所以可以用一些演算法把它們縮成一個點，一張爛圖上DFS就會轉換成樹DFS(縮點完一定是有權無環圖)。
先講一下題目，題目是說有一堆房子，你可以往比現在房子矮或一樣高的房子上走，如果房子上有噴射裝置，則可以順移到任意點，所有房子和噴射裝置都可以重複經過。題目要知道能走到最多幾間不一樣的房子。

可以轉成一張圖，可以從a房子->b房子就建一條有向邊，噴射裝置則是連到一個萬能點，萬能點連到全部的點，然後對這張圖縮點，每個scc的權重等於點權重的和(初始每個點權重都是1，萬能點是0)

我寫的時候遇到一些問題，第一就是因為不熟，code很醜，演算法還是要多練習。第二就是對於DFS概念不太正確，在搜有向無還圖時，有些點因為叉路，比如1到2、3，2、3再到4，那4以下就要DFS兩遍，多幾個這種叉路就TLE了，一定要注意優化(記錄走過的點的總權重，第二次走到直接取值)。

一些對scc理解有幫助的連結：
Kosaraju：
     http://blog.csdn.net/zzran/article/details/8939851
     http://greatpowerlaw.wordpress.com/2013/05/03/scc-kosarajus-algorithm/
Tarjan：
     https://www.byvoid.com/blog/scc-tarjan



``` c++
#include <cstdlib>
#include <iostream>
#include <vector>
#include <queue>
#define N 300002
using namespace std;
vector<int>go[N],back[N],tree[N];
int hu[N],ST[N],st=0,scc[N],scCo[N],scmx[N];
bool wed[N];
int DFS_go(int now){
    //cout<<now<<" DFS ";
    wed[now]=true;
    for(int i=0;i<go[now].size();i++){
        if(!wed[go[now][i]])
            DFS_go(go[now][i]);
    }
    ST[st++]=now;
    return 0;
}
int DFS_back(int now,int id){
    wed[now]=true;
    scc[now]=id;
    int sum=1;
    if(now==0)sum=0;
    for(int i=0;i<back[now].size();i++){
        if(!wed[back[now][i]])
            sum+=DFS_back(back[now][i],id);
    }
    return sum;
}
int DFS_tree(int now)
{
    if(scmx[now]!=0)return scmx[now];
    int mx=0,tmp;
    for(int i=0;i<tree[now].size();i++){
        tmp=DFS_tree(tree[now][i]);
        mx=(mx>tmp)? mx:tmp;
    }
    scmx[now]=mx+scCo[now];
    return mx+scCo[now];
}
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int n,k;
    char c;
    cin>>n>>k>>hu[1];
    go[0].push_back(1);
    back[1].push_back(0);
    for(int i=2;i<=n;i++){
        cin>>hu[i];
        if(hu[i]>=hu[i-1]){
            go[i].push_back(i-1);
            back[i-1].push_back(i);
        }
        if(hu[i-1]>=hu[i]){
            go[i-1].push_back(i);
            back[i].push_back(i-1);
        }
        go[0].push_back(i);
        back[i].push_back(0);
    }
    for(int i=1;i<=n;i++){
        cin>>c;
        if(c=='T'){
            go[i].push_back(0);
            back[0].push_back(i);
        }
    }
    for(int i=0;i<=n;i++)
        if(!wed[i])DFS_go(i);
    //cout<<endl;
    fill((bool*)wed,(bool*)wed+N,false);
    int tsc=0;
    // for(int i=0;i<st;i++)cout<<ST[i]<<" HH ";
    // cout<<endl;
    while(st!=0)
        if(!wed[ST[--st]]){
            scCo[tsc]=DFS_back(ST[st],tsc);
            tsc++;
        }
    // for(int i=0;i<N;i++)
    //     while(!back[i].empty())back[i].pop_back();
    for(int i=0;i<=n;i++)
        for(int j=0;j<go[i].size();j++){
            if(scc[i]!=scc[go[i][j]]){
                tree[scc[i]].push_back(scc[go[i][j]]);
            }
        }
    // for(int i=0;i<=n;i++)cout<<scc[i]<<" BB ";
    // cout<<endl;
    // for(int i=0;i<tsc;i++)cout<<scCo[i]<<" GG ";
    cout<<DFS_tree(scc[k])<<endl;
    //system("pause");
    return 0;
}
```