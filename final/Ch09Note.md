Ch9 虛擬記憶體(virtual memory)
===

##想像的記憶體空間- virtual memory

所謂virtual memory就是logical memory，

virtual address 就是 logical address。

OS讓每支程式都覺得自己有足夠的空間可以用，

等到程式真的要取用data時，再透過page table，

到真實記憶體(physical memory)空間去拿。

##虛擬記憶體運作流程

OS為了滿足程式資料儲存空間大於真實記憶體的需求，

創造了「虛擬空間」，使每支程式都覺得自己有足夠的空間，

然而，對程式來說，它們世界的記體體位置並不等於真實記憶體位置，

因此真的拿資料時，會先透過Page Table做查詢對應的真實記體體位置，

如果資料不在，OS再把存在硬碟裡的資料搬進真實記體體裡面。

##vaild bit- 判斷資料是否在真實記體體中

https://ithelp.ithome.com.tw/upload/images/20191109/20117114MgLylf8RH4.png

實際上page Table應該還有一個「valid bit」，

記錄資料是否在真實記體體中(v: valid, i: invalid):

https://ithelp.ithome.com.tw/upload/images/20191109/20117114X8XFGthd7w.png

譬如說page0對應到frame4，valid bit是「v」，

資料就好好的放在真實記憶體裡呢，

否則資料應該是沉睡在硬碟上。

##Page fault- 程式要的資料不在記憶體裡

https://ithelp.ithome.com.tw/upload/images/20191109/20117114nItqv8benB.png

Step1: 程式看了一下page Table，發現valid bit是i，自己要的資料不在真的記憶體上。

Step2: 發出一個「trap」(打擾源)，呼叫OS。

Step3: OS去巨大的硬碟中尋找資料

Step4: OS把資料帶到physical memory的空位上(如果沒有空位就先把別人的資料丟回硬碟)

Step5: 重設page Table，這時valid bit=v

Step6: 程式拿到資料了，重新開始工作。

**注意注意- Page fault要付的代價是很高的，應該儘量避免。**

##Page replacement algorithm

以下範例記憶體外紅字代表「尷尬」，

12天會遇到這些人「1，2，3，4，1，2，5，1，2，3，4，5」。

##先來先忘記憶法- First in first out(FIFO) algorithm

一個很單純的策略，你的大腦先在進行「遺忘」時，優先忘記最早認識的那個人。

以下畫個圖示表示:

https://ithelp.ithome.com.tw/upload/images/20191110/20117114SciCbiQ1P9.png

https://ithelp.ithome.com.tw/upload/images/20191110/20117114DELeM7Kj99.png

https://ithelp.ithome.com.tw/upload/images/20191110/20117114Oru7fvS6ZK.png

這結果看起來並不是很好，短短12天便發生了9次「尷尬」。

在我們繼續介紹其它記憶策略之前，

我們先來看一個不可思議的現象叫做「Belady's anormaly」。

##Belady's anormaly- 記住更多人，「尷尬」反而還更多?

大腦記憶體容量可以記住四個人了，且完全相同的人，相同的記憶策略。

https://ithelp.ithome.com.tw/upload/images/20191110/20117114FMiMRhVU29.png

https://ithelp.ithome.com.tw/upload/images/20191110/20117114gukxNW0r7a.png

https://ithelp.ithome.com.tw/upload/images/20191110/20117114jlBKhtgKqr.png

原本大腦記憶體容量只能記住三個人只有9次尷尬，

當大腦記憶體容量升級能記住四個人時竟然有10次尷尬。

這種現象叫做「Belady's anormaly」。

## 最優演算法- Optimal algorithm

https://ithelp.ithome.com.tw/upload/images/20191110/20117114mBd8mBuQef.png

在前4天時，發生的事情與FIFO演算法相同，

都因初次見面需要第一次記住他，「尷尬」不可免。

https://ithelp.ithome.com.tw/upload/images/20191110/20117114D4pNTyANae.png

在第7天的時候，我們遇見了「5號」，

怎麼知道應該刪除「1號、2號、3號、4號」哪個人呢?

我們往未來看

我們將於第8天再次遇見1號

我們將於第9天再次遇見2號

我們將於第10天再次遇見3號

我們將於第11天再次遇見4號

「4號」是最久以後才會再次遇見的人，將其刪除，

這樣一來，等一下第8~10天碰到1~3號都不會有「尷尬」。

https://ithelp.ithome.com.tw/upload/images/20191110/20117114ccSdupgcK0.png

一直到第11天，我們再次碰到「4號」，

要決定該刪除「1號、2號、3號、4號」哪個人的長相記憶，

此時再看一次未來

結果來看尷尬次數 : 6次。

在數學上，可以證明最優演算法的「尷尬次數」必然最少。

##最近最少見優先演算法- LRU (least recently used) algorithm

LRU的刪除原則為「最近最少見到的人優先刪除」。

在前4天時，發生的事情與前兩個演算法都相同:

https://ithelp.ithome.com.tw/upload/images/20191110/20117114usSQe9V7TR.png

接下來注意看第5天，因為遇見「1號」，

將「1號」從記憶體中更新到new的位置了。(建議跟FIFO比較以了解其差異)

https://ithelp.ithome.com.tw/upload/images/20191110/20117114qe5rBccyW5.png

https://ithelp.ithome.com.tw/upload/images/20191110/20117114xAwVvv1Tjx.png

尷尬次數 : 8次。

LRU演算法可說是實務上最常用的演算法，表現普遍也不錯。