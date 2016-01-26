---
title: 'IOI::HOJ::TIOJ::Problem : 292 - Game'
tags:
  - C++
  - TIOJ
  - IOI
  - HOJ
  - RMQ
category:
  - Code 備忘錄
  - Data Structure
date: 2015-03-17 09:18:02
---


http://hoj.twbbs.org/judge/problem/view/292
這題是IOI 2013年的題目，老實說還滿詭異的.....，應該說會AC是因為測資爛吧。

<!--more-->

題目是這樣的，你有一個非常大的平面（**10^9**）。你現在需要：
1.改某個點的值。
2.查詢某個平面區域的全部數字的GCD。（對了任何數跟0 做GCD是自己喔！！）
其中改某個值的次數是**22000**，查詢是250000。

這題的複雜度很詭異，基本上因為測資的更新點數不多，又很散，所以可以透過**線段樹套線段樹**AC掉，不過其實有特殊測資會讓他MLE。另外一種是**線段樹套平衡樹**（也許treap可以吧？！）。

這題的瓶頸其實是**空間**，時間上O（Nlog^2N）就會過了。第一次傳一次拿了5種狀態QAQ。實作上要利用**動態加點**的技巧。

大部分的人要是沒寫過樹套樹，都會無法理解，這題我一開始也誤會了。

首先第一步，建構一個資料結構，他可以插入點，查詢區間的GCD，這是**一維**的。這可以是平衡樹或是線段樹，差不多拉~~。

然後第二維是一棵線段樹，裡面的值已經不是整數，而是一個上面提到的資料結構。
對於查詢，先照一般的線段樹查找，找到包含第二維的那幾塊，然後繼續在那幾塊「大線段樹」裡查找第一維的區間（上面的資料結構），**大線段樹裡每一個節點都是一棵「小線段樹」**。

在寫線段樹的時候，要維護一個值，常常透過左右子節點來維護（比如求和就是左子節點的和+右子節點的和），而這裡我們要維護的是**「一個資料結構」**，當然不能直接加起來。
一個好方法是，我們可以把「大線段樹」的每個節點，當作一個一維容器維護某個座標的GCD，每次加入一個點時，會遞迴下去，之後，直接**query「大線段樹」的左右子節點裡的「小線段樹」**，查出剛剛加入的點的Y座標那排的值，做GCD運算之後，就得到某個Y座標，寬度是這個「大線段樹」節點寬度的**點，問題變回一維**，我們直接將這個點丟進這個節點的容器裡。

做到這裡，應該得到一個MLE了，這時有一個優化，我們要加在第一維的「小線段樹」上。
小線段樹只會做插入點，跟查詢區間，插入點的時候，我們先加一個懶惰標記，代表這個**區間只有一個值**，並紀錄他的位置（某個一維的座標），查詢時也可以直接判斷有沒有包含這個值，直到下一次插入值時，如果位置一樣，直接改掉值繼續用，不然就把標記推下去，然後繼續做。

至於動態加點，我們的線段樹查詢前可以先讓左右子節點（指標型線段樹）指向NULL，查詢時如果是NULL就直接當作0，插入點**有需要再New**一個節點出來。這也是上面那個優化會節省記憶體的原因，因為這樣可以整整省掉一排記憶體，細節和卡掉的測資可以自己思考~~。

2015/03/18
寫了線段樹套Treap，概念不變，反正第一維就是一個容器，也有人用黑魔法Tree做。
時間變成兩倍，但是空間剩下一半XDD。
聽說這題有線段樹[正解](https://gist.github.com/johnchen902/efb9a4a5211c35f06397)，可以有效解決空間問題，但是覺得難寫....。

#### 線段樹套線段樹：

``` c++
#include <cstdio>
#include <algorithm>
#define F(n) Fi(i,n)
#define Fi(i,n) for(int i=0;i<n;i++)
#define LL long long
using namespace std;
struct Seg{
    LL val;
    int tag;
    Seg *lt,*rt;
    Seg():lt(NULL),rt(NULL),val(-1),tag(-1){}
    inline void push(int l,int r,int x,LL k){
        if(x<=(l+r)/2){
            if(!lt)lt=new Seg();
            lt->insert(l,(l+r)/2,x,k);
        }else{
            if(!rt)rt=new Seg();
            rt->insert((l+r)/2+1,r,x,k);
        }
    }
    void insert(int l,int r,int x,LL k){
        if(l==r){
            val=k;
            return;
        }
        if(x==tag)val=k;
        else if(~tag){
            push(l,r,tag,val);
            tag=-1;
        }else if(!~val){
            tag=x;val=k;
            return;
        }
        push(l,r,x,k);
        val=__gcd((lt?lt->val:0),(rt?rt->val:0));
    }
    LL query(int l,int r,int a,int b){
        if(a<=l&&r<=b)return val;
        if(l>b||r<a)return 0;
        if(~tag)return a<=tag&&tag<=b?val:0;
        int mid=(l+r)/2;
        return __gcd((lt?lt->query(l,mid,a,b):0),(rt?rt->query(mid+1,r,a,b):0));
    }
};
struct SegSeg{
    static int max_n;
    Seg *val;
    SegSeg *lt,*rt;
    SegSeg():lt(NULL),rt(NULL),val(NULL){}
    void insert(int l,int r,int x,int y,LL k){
        if(l==r){
            if(!val)val=new Seg();
            val->insert(0,max_n-1,y,k);
            return;
        }
        if(x<=(l+r)/2){
            if(!lt)lt=new SegSeg();
            lt->insert(l,(l+r)/2,x,y,k);
        }else{
            if(!rt)rt=new SegSeg();
            rt->insert((l+r)/2+1,r,x,y,k);
        }
        if(!val)val=new Seg();
        val->insert(0,max_n-1,y,__gcd((lt&&lt->val?lt->val->query(0,max_n-1,y,y):0),(rt&&rt->val?rt->val->query(0,max_n-1,y,y):0)));
    }
    LL query(int l,int r,int a,int b,int x,int y){
        if(a<=l&&r<=b)return val?val->query(0,max_n-1,x,y):0;
        if(l>b||r<a)return 0;
        int mid=(l+r)/2;
        return __gcd((lt?lt->query(l,mid,a,b,x,y):0),(rt?rt->query(mid+1,r,a,b,x,y):0));
    }
};
int SegSeg::max_n;
int main(){
    // freopen("02.in","r",stdin);
    // freopen("0.out","w",stdout);
    int r,n,a,b,c,d;
    LL k;
    scanf("%d%d%d",&r,&SegSeg::max_n,&n);
    SegSeg *segseg=new SegSeg();
    F(n){
        scanf("%d",&a);
        if(a==1){
            scanf("%d%d%lld",&a,&b,&k);
            segseg->insert(0,r-1,a,b,k);
        }else{
            scanf("%d%d%d%d",&a,&b,&c,&d);
            if(a>c)swap(a,c);
            if(b>d)swap(b,d);
            printf("%lld\n",segseg->query(0,r-1,a,c,b,d));
        }
    }
    scanf("%*d");
}
```

#### Treap版：
``` c++
#include <cstdio>
#include <cstdlib>
#include <algorithm>
#define F(n) Fi(i,n)
#define Fi(i,n) for(int i=0;i<n;i++)
#define LL long long
using namespace std;
struct Treap{
    Treap *l,*r;
    int key,pri;
    LL val,g;
    Treap(){}
    Treap(int _k,LL _v):l(NULL),r(NULL),key(_k),val(_v),g(_v),pri(rand()){}
};
inline LL get_g(Treap *a){
    return a?a->g:0;
}
inline void pull(Treap *a){
    a->g=__gcd(a->val,__gcd(get_g(a->l),get_g(a->r)));
}
Treap* merge(Treap *a,Treap *b){
    if(!a||!b)return a?a:b;
    if(a->pri > b->pri){
        a->r=merge(a->r,b);
        pull(a);
        return a;
    }else{
        b->l=merge(a,b->l);
        pull(b);
        return b;
    }
}
void split(Treap *t,int k,Treap *&a,Treap *&b){
    if(!t)a=b=NULL;
    else if(t->key<=k){
        a=t;
        split(t->r,k,a->r,b);
        pull(a);
    }else{
        b=t;
        split(t->l,k,a,b->l);
        pull(b);
    }
}
bool update(Treap *a,int k,LL x){
    if(!a)return 0;
    if(a->key==k)a->val=x,x=1;
    else if(a->key > k)x=update(a->l,k,x);
    else x=update(a->r,k,x);
    pull(a);
    return x;
}
LL Tquery(Treap *&a,int k){
    if(!a)return 0;
    if(a->key==k)return a->val;
    else if(a->key > k)return Tquery(a->l,k);
    else return Tquery(a->r,k);
}
inline LL queryLR(Treap *&a,int l,int r){
    if(!a)return 0;
    Treap *lt,*rt;
    split(a,l-1,lt,a);
    split(a,r,a,rt);
    LL ans=a?a->g:0;
    a=merge(lt,merge(a,rt));
    return ans;
}
inline void Tinsert(Treap *&a,int k,LL x){
    if(update(a,k,x))return;
    Treap *tmp,*tt;
    split(a,k,a,tmp);
    a=merge(a,merge(new Treap(k,x),tmp));
}
struct SegSeg{
    Treap *val;
    SegSeg *lt,*rt;
    SegSeg():lt(NULL),rt(NULL),val(NULL){}
    void insert(int l,int r,int x,int y,LL k){
        if(l==r){
            if(!val)val=new Treap(y,k);
            else Tinsert(val,y,k);
            return;
        }
        if(x<=(l+r)/2){
            if(!lt)lt=new SegSeg();
            lt->insert(l,(l+r)/2,x,y,k);
        }else{
            if(!rt)rt=new SegSeg();
            rt->insert((l+r)/2+1,r,x,y,k);
        }
        // printf(" I %d %d (%d %d) %lld %lld\n",l,r,x,y,Tquery(val,y),__gcd((lt?Tquery(lt->val,y):0),(rt?Tquery(rt->val,y):0)));
        if(!val)val=new Treap(y,__gcd((lt?Tquery(lt->val,y):0),(rt?Tquery(rt->val,y):0)));
        else Tinsert(val,y,__gcd((lt?Tquery(lt->val,y):0),(rt?Tquery(rt->val,y):0)));
        // printf(" H %d %d (%d %d) %lld %lld\n",l,r,x,y,Tquery(val,y),__gcd((lt?Tquery(lt->val,y):0),(rt?Tquery(rt->val,y):0)));
    }
    LL query(int l,int r,int a,int b,int x,int y){
        if(a<=l&&r<=b)return queryLR(val,x,y);
        if(l>b||r<a)return 0;
        int mid=(l+r)/2;
        return __gcd((lt?lt->query(l,mid,a,b,x,y):0),(rt?rt->query(mid+1,r,a,b,x,y):0));
    }
};
int main(){
    // freopen("01.set-zero.in","r",stdin);
    // freopen("0.out","w",stdout);
    srand(7122);
    int r,n,a,b,c,d;
    LL k;
    scanf("%d%d%d",&r,&c,&n);
    SegSeg *segseg=new SegSeg();
    F(n){
        scanf("%d",&a);
        if(a==1){
            scanf("%d%d%lld",&a,&b,&k);
            segseg->insert(0,r-1,a,b,k);
        }else{
            scanf("%d%d%d%d",&a,&b,&c,&d);
            if(a>c)swap(a,c);
            if(b>d)swap(b,d);
            printf("%lld\n",segseg->query(0,r-1,a,c,b,d));
        }
    }
    scanf("%*d");
}
```