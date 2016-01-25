---
title: 'HOJ::Problem : 147 - 海綿寶寶之蟹堡餐飲聯盟(2-SAT)'
tags:
  - C++
  - HOJ
  - 2-SAT
  - SCC
  - 拓撲排序
category:
  - Code 備忘錄
  - Graph
date: 2014-03-23 05:26:01
---


2-STA問題。
http://hoj.twbbs.org.tw/judge/problem/view/147

<!--more-->

2-STA問題是指一些布林值(true & false)，透過設定他們的值讓一串運算是結果為true的方法。這題可以轉成，每個人是否當代表，新增4N個點，分別代表每間餐廳的兩個人和他們的相反情況，因為題目說一間餐廳一個人，所以先把每間餐廳每個人分別連到他們的相反(因為A出去就會導致B不出去)，這些邊是雙向的關係，再來依照題目給的條件建邊，這裡只有單向的導致關係，最後用SCC縮點，先檢查每個人和每個人的相反有沒有在同一個SCC裡，有代表矛盾，沒有的話，因為編號小的SCC會導致編號大的SCC變成true，而我們發現每個人是否出去就是看哪邊SCC編號大(選小的會讓全部變成true，矛盾)。



``` c++
#include <cstdlib>
#include <iostream>
#include <vector>
#define N 8005
using namespace std;
vector<int>GO[4*N],BK[4*N],ved;
bool wed[4*N];
int SC[4*N];
void push(int a,int b){
    GO[a].push_back(b);
    BK[b].push_back(a);
}
int DFS(int now,vector<int> *D,int sc)
{
    wed[now]=true;
    SC[now]=sc;
    //cout<<"DFS2 "<<now<<endl;
    for(int i=0;i<D[now].size();i++){
        if(!wed[D[now][i]])DFS(D[now][i],D,sc);
    }
    //cout<<"!";
    if(D==GO){
        //cout<<"DFS "<<now<<endl;
        ved.push_back(now);
    }
}
int scc(int n)
{
    for(int i=0;i<4*n;i++)
        if(!wed[i])DFS(i,GO,-1);
    fill(wed,wed+4*N,false);
    int sc=0;
    while(!ved.empty()){
        if(!wed[ved.back()]){DFS(ved.back(),BK,sc++);}
        ved.pop_back();
    }
    //cout<<endl;
}
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int n,m;
    cin>>n>>m;
    for(int i=0;i<2*n;i+=2){
        push(i,i+1+n*2);
        push(i+1+2*n,i);
        push(i+1,i+n*2);
        push(i+2*n,i+1);
    }
    for(int i=0;i<m;i++){
        int a,b;
        cin>>a>>b;
        a--;b--;
        push(a,b+2*n);
        push(b,a+2*n);
    }
    scc(n);
    // for(int i=0;i<4*n;i++)cout<<' '<<SC[i];
    // cout<<endl;
    bool tag=false;
    for(int i=0;i<2*n;i++){
        if(SC[i]==SC[i+2*n])tag=true;
    }
    if(tag==true){
        cout<<"-1\n";
    }else{
        for(int i=0;i<2*n;i++){
            if(SC[i]>SC[i+2*n])
                cout<<i+1<<endl;
        }
    }
    //system("pause");
    return 0;
}
```