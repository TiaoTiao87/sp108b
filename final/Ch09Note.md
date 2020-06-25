Ch9 虛擬記憶體(virtual memory)
===

## 想像的記憶體空間 - virtual memory

所謂virtual memory就是logical memory，

virtual address 就是 logical address。

OS讓每支程式都覺得自己有足夠的空間可以用，

等到程式真的要取用data時，再透過page table，到真實記憶體(physical memory)空間去拿。

## 虛擬記憶體運作流程

OS為了滿足程式資料儲存空間大於真實記憶體的需求，

創造了「虛擬空間」，使每支程式都覺得自己有足夠的空間，

然而，對程式來說，它們世界的記體體位置並不等於真實記憶體位置，

因此真的拿資料時，會先透過Page Table做查詢對應的真實記體體位置，

如果資料不在，OS再把存在硬碟裡的資料搬進真實記體體裡面。

## vaild bit - 判斷資料是否在真實記體體中

![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/IMG/Ch0901.png)

實際上page Table應該還有一個「valid bit」，

記錄資料是否在真實記體體中(v: valid, i: invalid):

![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/IMG/Ch0902.png)

譬如說page0對應到frame4，valid bit是「v」，

資料就好好的放在真實記憶體裡，否則資料應該是沉睡在硬碟上。

## Page fault - 程式要的資料不在記憶體裡

![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/IMG/Ch0903.png)

Step1: 程式看了一下page Table，發現valid bit是i，自己要的資料不在真的記憶體上。

Step2: 發出一個「trap」，呼叫OS。

Step3: OS去巨大的硬碟中尋找資料

Step4: OS把資料帶到physical memory的空位上

Step5: 重設page Table，這時valid bit=v

Step6: 程式拿到資料了，重新開始工作。

**注意注意 - Page fault要付的代價是很高的，應該儘量避免。**

## 先來先忘記憶法- First in first out(FIFO) algorithm

一個很單純的策略，你的大腦先在進行「遺忘」時，優先忘記最早認識的那個人。

## 最優演算法- Optimal algorithm

在數學上，可以證明最優演算法的「尷尬次數」必然最少，但我們無法預知未來。

## 最近最少見優先演算法- LRU (least recently used) algorithm

LRU的刪除原則為「最近最少見到的人優先刪除」。

LRU演算法可說是實務上最常用的演算法，表現普遍也不錯。
