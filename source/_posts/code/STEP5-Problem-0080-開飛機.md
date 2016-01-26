---
title: 'STEP5::Problem 0080 : 開飛機'
tags:
  - C++
  - Step5
  - Deque
category:
  - Code 備忘錄
  - DP
date: 2014-05-23 07:31:10
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0080
這題是deque優化，維護區間最大和最小值。

<!--more-->

所謂的deque，就是可以從前後push和pop的資料結構。
deque優化，就是當我們的區間是持續像右邊爬行時，我們可以將複雜度降到O(n)。
若要找最大值，先將第一個丟進去，他就是最大值，當第二個數進去前，先看看第一個數是不是比他大，如果不是，那最大值一定不會是他了(當區間往右，第一個數一定會比第二個數早pop，所以第一個數一直被第二個數壓住)，所以每次都先把deque後面比較小的pop掉，再塞數字進去，形成一個非嚴格遞減數列，最大值就是deque最前面的值。當左界縮小時，檢查值是否和最大值一樣，如果是就從前面pop掉，由第二大的數字接手(有可能跟原本的最大值一樣大，因為非嚴格遞減)。
每個數字進出deque一次，故複雜度為O(n)。

這題只要開兩個就好。一個最大一個最小。



``` c++
#include <cstdlib>
#include <iostream>
#include <queue>
#define N 3000001
using namespace std;
deque<int>xq,iq;
int S[N];
void push(int r)
{
    while(!xq.empty()&&xq.back()<r)xq.pop_back();
    xq.push_back(r);
    while(!iq.empty()&&iq.back()>r)iq.pop_back();
    iq.push_back(r);
}
void pop(int l)
{
    if(xq.front()==l)xq.pop_front();
    if(iq.front()==l)iq.pop_front();
}
int main(int argc,char *argv[])
{
    ios_base::sync_with_stdio(false);
    int t,n;
    cin>>t>>n;
    for(int i=0;i<n;i++)cin>>S[i];
    int l=0,r=0,mxf=1;
    xq.push_back(S[l]);
    iq.push_back(S[l]);
    while(r<n){
        if(xq.front()-iq.front()<=t){
            mxf=(mxf>r-l+1)? mxf:r-l+1;
            r++;
            push(S[r]);
        }else{
            pop(S[l]);
            l++;
        }
    }
    cout<<mxf<<endl;
    //system("pause");
    return 0;
}
```