---
title: 'Standard Template Library(STL)&&常用工具&&黑魔法'
category:
  - Note
date: 2014-03-28 06:01:21
tags:
- C++
- STL
- GNU-PBDS
---

STL 標準模板庫，是一些很好用的常見的資料結構的模板，如果要打競賽或是寫程式的時候懶得自己寫就可以用這個很方便的工具庫。

#### 2015/02/14
加了一點黑魔法。

<!--more-->

STL是物件，容器、資料結構等等基本上都有差不多的函式可以用，比如 push()  pop()  push_back()   pop_back()  top()  back()  front()  insert() erase() 等等，每個容器、物件的同一個函式功能也都類似，但是複雜度、實作方式可是完全不一樣。想要善用STL一定要好好了解它的內容。(我都不懂一直亂用，程式常常爛掉)。


## STL總覽：

* `vector<type>` ：動態陣列，容量不足時會自動增加，其他資料結構很多都用這個容器當存放的容器，基本上宣告完可以直接當陣列用。注意容量不夠時，會O(n)搬移所有元素。POP後記憶體不會釋放。iterator有可能溢位，只要vector改變不保證指到一樣的值。 常用函式---push_back()/pop_back()  O(1) ， push_front()/pop_front() O(n)。
* `list<type>`
* `stack<type>`   push(),pop(),top(),empty() O(1)，不能修改裡面的值。
* `queue<type>`   push(),pop(),front(),empty() O(1)
* `priority_queue<type>`
* `deque<type>`  複雜度較高，但是 random access O(1)
* `set<type>`
* `map<type,type>`
* `unordered_map<type,type>`


## 常用函式：

一些常用的內建函式。

##### `<iostream>`
* std::fill(*p,*p+N,value);  ： 指定一段區域付值，速度其實跟for迴圈差不多....。

##### `<cstring>`
* memset(*p, value, size);  ：這其實原本用來區間付值，用來改字元的。因為一個字元 1byte，如果拿來付值int 1會變成 (1<<24)+(1<<16)+(1<<8)+1 = 16843009，(一個byte一個1)，平常拿來把陣列改成0就好，速度比較快，可以搭配sizeof(int)取得型別的記憶體大小。通常一個陣列D[]就用  memset(D,0,sizeof(D))  就可以初史化囉!!

##### `<Algorithm>`
* std::sort(*p,*p+N,function_name) ： 很方便的內建排序，綜合了多種排序法，會依情況更換演算法，效率不錯。不穩定，基本上是快速排序，由小排到大。平常可以直接用兩個指標將區間排序，如果不想用他的排序法，可以塞一個自訂函式給他(回傳bool就好)，他會照你的方法排，而且這樣甚麼都能丟(struct也是)。
* std::__gcd(x,y)   ：回傳最大公因數，應該是用輾轉相除法。
* min(x,y) max(x,y) 。 傳回小的、大的。通常我都自己define。

##### `<cstdlib>`
* system(char*)  ：很常見的函式，用來呼叫系統命令，他吃字串，在windows用就當塞字給cmd。  

##### `<cmath>`
* sqrt(double)    開根號。
* 三角函數、反三角函數。(弧度，非360角度) acos(-1) = pi(圓周率)


## #define：

* define N 1000：這樣要改範圍比較好改。
* define min(x,y) (x)<(y) (x):(y)   ：define可以取代字元外，如果括號內有符合格式的變數也可以取代，有點像函式，依此類推，也可以define MAX。
* define F(n) Fi(i,n)
* define Fi(i,n) Fl(i,0,n)
* define Fl(i,l,n)  for(int i=l;i<n;i++)   ：這是學一個學長的，很好用的for回圈取代法，可以節省很多打for迴圈的時間，但是不熟悉會容易出問題，要小心用。


## C++ 黑魔法：
GNU-PBDS: Policy-Based Data Structures. GNU GCC的函式庫之一。有一些很神奇的資料結構。


``` c++
/*C++11 支援，要引入以下標頭檔才能用*/
#include <ext/pb_ds/assoc_container.hpp>
//__gnu_pbds::cc_hash_table
#include <ext/pb_ds/priority_queue.hpp>
//__gnu_pbds::priority_queue
using namespace std;
using namespace __gnu_pbds;
int main(){
    cc_hash_table<int,int> Hash;
}
```

* __gnu_pbds::cc_hash_table：Hash，比用map做離散化快，key只能丟原本的資料形態，因為unorderde_map有些問題的關係，這東西在離散化時很好用~~。

* __gnu_pbds::gp_hash_table：跟cc_hash_table很像，但是試了幾次都比他快，記憶體也比較少。但是離散化最省記憶體的方式還是排序二分搜(low_bound)最小。

* __gnu_pbds::priority_queue；有多種不同的實作方式可以選擇，效率各有優劣。

[這篇](http://gaotianyu1350.gitcafe.io/2015/02/17/pbds/)裡面講的非常清楚，有空來整理一下。可是他有錯誤，尤其裡面提到的binary_heap_tag打死都不要用，慢死......。

C++11 新增好用語法：

``` c++
/*C++11 編譯參數 g++ a.cpp -o a -O2 -std=C++11*/
#include <algorithm>
#include <iostream>
using namespace std;
int main(){
    auto x=5;
    /*自動形別，需要賦值否則無法運作，
      可以少打字很方便。*/
    auto fun = [](int x){return x+5;};
    /*定義匿名函數，也就是lambda，
      中括號中是定義把外面的全域變數傳入的方法(參考或複製)，
      細節不在這邊說明。
      因為每個函數對於C++而言都是不同的型態，所以變數形態用auto，
      但是重點是 [](){};
      遞回等等操作比較複雜，建議只有取值、比大小或是define功能的函式
      再使用這種語法~_~。
    */
    int D[10]={...}
    sort(D, D+10, [](int a,int b){return a>b} );
    /*可以這樣傳函數進入sort函式，改變排序方式*/

    for(int i:D){
        cout<<i<<endl;
    }
    /*很像其他語言的 for i in ...
      變數會遍歷整個陣列，
      後面也可以是vector等等可以遍歷的結構。
      可以省掉iterator~~
      for(int i : vector<int>){...}
    */
}
```

關於auto 還有 decltype() 可以參考[這裡](https://kheresy.wordpress.com/2010/05/19/c0x-auto-%E5%92%8C-decltype/)。
