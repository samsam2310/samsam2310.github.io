---
title: 'C++11 的 Random library, 你還在用rand()嗎?'
category:
  - Note
date: 2016-10-20 23:36:19
tags:
---

最近寫程式作業，要用蒙地卡羅求Pi值，所以要用到 random 函數。
結果使用後發現了各種問題，除了隨機的程度感覺不好之外，由於程式是多執行序，還遇到了 Thread Safe 的問題。
於是查了資料才發現原來C++11早就出了新的 Random 函式庫，提供了各種不同的需求。

<!--more-->

C++11 的 Random 定義在 `<Random>` 這個函式庫裡面，使用前要先 import。
先來看一段產生 1~10 隨機整數的程式碼。
``` c++
#include <iostream>
#include <random>
using namespace std;
int main()
{
  std::random_device rd;
  std::default_random_engine gen = std::default_random_engine(rd());
  std::uniform_int_distribution<int> dis(1,10);

  std::cout << "some random numbers between 1 and 10: ";
  for (int i=0; i<10; ++i)
      std::cout << dis(gen) << " ";

  std::cout << std::endl;

  return 0;
}
```


Random 乍看之下很複雜，但是其實很簡單，基本上亂數產生被拆成了兩個步驟：

1. Generators (產生器、引擎)
2. Distributions (分佈)

第一個產生器的部分，其實就類似 rand() 函數，他會產生一個在 min 和 max 之間的整數，並且會利用一些演算法來保證我們得到的亂數的一些性質等等。

而分布決定了我們的亂數真正的分佈範圍，比如我們想要讓他是 1~10 的整數、 -1~1 的浮點數等等。
分佈決定我們的數值範圍和分散狀態，像是平均分佈、常態(鐘形曲線)分佈等等，依照需求可以很自由的代換，不需要被亂數產生的演算法影響。

所以我們想要產生一個亂數只要選一個引擎和一個分佈，就可以很輕鬆的產生我們想要的亂數了，另外，由於他們都是類似STL的仿函數形式的物件，所以也不用擔心會有 Thread Safe的問題(不同執行序共用同一個變數導致的問題，以 rand() 來說就是有可能會產生大量一樣的亂數而讓多執行序的效果消失)。

下面列出一些引擎和分佈，至於說明就等有空再補吧(也許不會補了QQ)。

### 引擎 Generators:

 * std::random_device
   這是個 亂數引擎，但是他的定義是「非確定性亂數引擎」，也就是他不保證演算法、亂數的性質，單純的產生隨機的位元(看定義，Linux下好像是 32 bit)，因為沒有定義做法，所以也有可能根本就永遠都回傳0 XD(Windows的 Mingw就是這樣實作的)，但基本上新的g++編譯器應該都是有好好實作的，可能需要查一下編譯器的說明。
   因為這個函數沒有保證實做，所以可能根本不亂，但是他是唯一一個可能產生「不一樣」的數字的引擎，其他的引擎都像是 rand() 一樣需要先給定種子，否則會產生一樣的結果。
   這個引擎可以讓我們用來取代 time 函式庫等等，用來產生亂數種子。
 
 * std::default_random_engine
   基本上就是預設的亂數引擎，如果沒有特殊需求就使用這個吧。我(非嚴謹的)測試過後覺得這個在速度和亂度上都很好，除非有特殊需求不然應該都是使用這個。

 * std::linear_congruential_engine
   `template <class UIntType, UIntType a, UIntType c, UIntType m>`
   基本上就是線性同餘算法的模板，可以自己設定a c m。
   同樣使用模板的還有 mersenne_twister_engine std::subtract_with_carry_engine 兩個，不過實際上的內容我沒仔細看了XD。


 * discard_block_engine
 * independent_bits_engine
 * shuffle_order_engine


 * minstd_rand
 * minstd_rand0
 * mt19937
 * mt19937_64
 * ranlux24_base
 * ranlux48_base
 * ranlux24
 * ranlux48
 * knuth_b

### 分佈 Distributions :

#### Uniform:
 * uniform_int_distribution
   整數平均分佈。
 * uniform_real_distribution
   一般的平均分佈，有小數點的。

#### Related to Bernoulli (yes/no) trials:
 * bernoulli_distribution
 * binomial_distribution
 * geometric_distribution
 * negative_binomial_distribution

#### Rate-based distributions:
 * poisson_distribution
 * exponential_distribution
 * gamma_distribution
 * weibull_distribution
 * extreme_value_distribution

#### Related to Normal distribution:
 * normal_distribution
 * lognormal_distribution
 * chi_squared_distribution
 * cauchy_distribution
 * fisher_f_distribution
 * student_t_distribution

#### Piecewise distributions:
 * discrete_distribution
 * piecewise_constant_distribution
 * piecewise_linear_distribution


另外，由於C++11越來越猛，所以他也有提供你合成函數的模板 std::bind()。
``` c++
#include <functional>
#include <random>
int main(){
    std::random_device rd; // 種子產生器
    std::mt19937 gen = std::mt19937(rd()); //使用mt19937引擎
    std::uniform_real_distribution<> dis(-1, 1); //使用平均分佈

    auto randfun = std::bind(dis, gen); //將引擎和分佈綁在一起變成新函數 randfun

    cout<<randfun()<<' '<<randfun()<<' '<<randfun()<<endl; //可以直接重複呼叫
}
```

C++11真的越來越方便了，也和C越走越遠了QQ。

參考資料：http://www.cplusplus.com/reference/random/
