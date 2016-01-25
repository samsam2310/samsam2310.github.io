---
title: 'STEP5::Problem 0150 : 花匠'
tags:
  - C++
  - Step5
  - 線段樹
category:
  - Code 備忘錄
  - Data Structure
date: 2014-03-16 05:08:19
---


http://web2.ck.tp.edu.tw/~step5/probdisp.php?pid=0150
這題是個非常邪惡的線段樹......區間更新XOR線段樹。

<!--more-->

這題在校隊補選的時候因為測資爛for迴圈就AC了，但是現在不是了，除了真的需要long long int 之外，還要有區間更新線段樹才行。
題目是給一段數字，要查詢區間和還有對區間XOR。

所謂的區間更新，也就是在每一層線段樹上加一個tag，紀錄更新的狀態，在query或update時，如果不包含直接return，包含就進行運算，並加到tag上，代表上次進行的運算，然後就不遞迴下去了。如果切到，在遞迴下去前，如果tag是有的，就要先把他分到兩個子節點上，並且對兩個子節點做運算處理，然後才遞迴下去(注意query和update都要對tag進行運算，以保證總和的正確性)。

這題的另外一個重點，我用了"J逸!?"的方法。首先線段樹的節點用一個struct，包含sum,B[20],tag，其中sum是long long，B記錄了這個節點以下的0和1的數量和，tag就tag。B的紀錄法是1記1，0記-1，因為我們觀察到XOR是對每一個位元做一個運算：
如果A XOR B，把A和B拆成二進位的話。
以A=1來說，會把B的1變成0，0變成1。
以A=0來說，會把B的1變成1，0變成0。
又
如果0變1總和就會增加 1<< i，1變0總和減少 1<< i ，其中i是位數。
由上面的結論，我們可以發現如果對一個數XOR X的話，位元是0我們就可以忽略那一位，位元是1的話，會把01互換，所以此節點以下的1都變0，0都變1，而我們只要對B[20]的那一位乘以-1，把總和加上 1<< i *B[i]，就可以得到XOR後的總和了。

最後就是實做線段樹時，合併兩格節點時，除了tag是0，其他的值只要加起來就好，因為B加起來剛好可以把1和0的數量抵銷(如果底下有1個1和1個0，那B加起來就是0，事實上我們對這組總和XOR也只是01反轉，總和不變)。
query：不包含回傳0，包含回傳該節點的sum，切到先處理tag在遞迴下去。
update：如果XOR X，不包含傳回該節點的值，包含就對該節點做運算，並把tag XOR X，如果切到就先處理tag在遞迴下去。

寫得好複雜，寫個實例好了：
7  ={00111} ==&gt; {-1,-1,1,1,1}
4 = {00100} ==&gt; {-1,-1,1,-1,-1}
4+7=11   用上面的方法轉移 ==&gt; {-2,-2,2,0,0}
8=  {01000}
4 XOR 8 =12 {01100}
7 XOR 8 =15 {01111}
12+15=27==&gt; {-2,-2,2,0,0} XOR {01000} ==&gt; {-2,2,2,0,0} 發現8那一位的-2變成2，所以原本的總和 11 要加上 2*8=16， 所以11+16=27跟答案一樣。



``` c++
#include <cstdlib>
#include <iostream>
#define LL long long
#define N 100001
#define BB 21  //記錄0和1
using namespace std;
struct MP{
    LL sum;
    int B[BB],tag;
    void clean(){
        sum=tag=0;
        fill(B,B+BB,-1);
    }
    MP in(int b){
        clean();
        sum=b;
        for(int i=0,j=b;j!=0;j>>=1,i++)B[i]=(j&1)? 1:-1;
        return *this;
    }
    MP XOR(int b){  //整個程式的精華
        //cout<<sum<<" XOR:"<<b<<'\n';
        for(int i=0,j=b;j!=0;i++,j>>=1){
            //cout<<"J:"<<j<<' '<<(j&1)<<endl;
            if(j&1){
                // cout<<(1<<i)*B[i]<<' ';
                B[i]*=-1;
                sum+=(1<<i)*B[i];
                //cout<<"SUM:"<<sum<<endl;
            }
        }
        tag^=b;
        return *this;
    }
}BST[4*N];
inline MP operator+(const MP a,const MP b)
{
    MP sum;
    sum.tag=0;
    sum.sum=a.sum+b.sum;
    for(int i=0;i<BB;i++)sum.B[i]=a.B[i]+b.B[i];
    return sum;
}
int S[N];
MP build(int l,int r,int id)
{
    if(l==r)return BST[id].in(S[l]);
    return BST[id]=build(l,(l+r)/2,id*2)+build((l+r)/2+1,r,id*2+1);
}
LL query(int l,int r,int a,int b,int id){
    //cout<<"l-r:"<<l<<' '<<r<<' '<<endl;
    // for(int i=0;i<10;i++)cout<<BST[id].B[i]<<' ';
    // cout<<endl;
    if(l>b||r<a)return 0;
    if(a<=l&&r<=b)return BST[id].sum;
    if(BST[id].tag!=0){
        BST[id*2].XOR(BST[id].tag);
        BST[id*2+1].XOR(BST[id].tag);
        BST[id].tag=0;
    }
    LL sum=query(l,(l+r)/2,a,b,id*2)+query((l+r)/2+1,r,a,b,id*2+1);
    return sum;
}
MP update(int l,int r,int a,int b,int id,int up)
{
    if(r<a||l>b)   return BST[id];
    if(a<=l&&r<=b) return BST[id].XOR(up);
    if(BST[id].tag!=0){
        BST[id*2].XOR(BST[id].tag);
        BST[id*2+1].XOR(BST[id].tag);
        BST[id].tag=0;
    }
    return BST[id]=update(l,(l+r)/2,a,b,id*2,up)+update((l+r)/2+1,r,a,b,id*2+1,up);
}
int main(int argc,char *argv)
{
    ios_base::sync_with_stdio(false);
    int n,a,b,x,q;
    cin>>n;
    for(int i=0;i<n;i++)cin>>S[i];
    build(0,n-1,1);
    cin>>q;
    while(q--){
        cin>>a;
        if(a==1){
            cin>>a>>b;
            cout<<query(0,n-1,--a,--b,1)<<endl;
        }else{
            cin>>a>>b>>x;
            update(0,n-1,--a,--b,1,x);
        }
    }
    // system("pause");
    return 0;
}
```