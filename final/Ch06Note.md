Ch6 什麼是「同步化」議題?Critical section problem的各種解法
===

以Ch3程式生命週期的程式生命週期圖來說，

一次只會有一支程式在running state執行，

其餘程式在ready state等候scheduler選擇它進入running state。

平時你在電腦上感覺程式可以同時執行，

只不過是程式快速交替換到running state的結果。

所以說，既然程式本來就不會「同時」執行，

那麼「同步化」問題從何而來?

## 程式語言的同步化問題

假設我們有process A, B，它們共用變數sum，

process A 做的事是 「sum= sum+3」

process B 做的事是 「sum= sum+2」

從程式碼來看你會以為只有一個步驟，

但簡單的「sum= sum+3」這個指令翻譯成組合語言時卻有三個步驟。

```
(程式A做的指令)
move ax sum (將記憶體中變數sum的值，讀進register ax)
add ax 3 (將ax的值加3，存放在register ax裡) 
move sum ax (將ax的值寫回記憶體中的變數sum) 

(程式B做的指令)
move bx sum (將記憶體中變數sum的值，讀進register bx)
add bx 2 (將bx的值加2，存放在register bx裡) 
move sum bx (將bx的值寫回記憶體中的變數sum)
```

也就是說，如果這些指令中途被其它程式打斷的話，

那麼便會發生上述描述的「race conditon」的問題了。

因此，解決的方法便是將修改程式間共同變數之間的指令，

視為「critical section」，

若程式在執行「critical section」，

其它程式都不該進入它們的「critical section」。

## critical section requirement

一般來說，要解決「critical section」(以下簡稱CS)問題應滿足三個條件為理想:

1. **mutual exclusion**: 若程式在執行「critical section」，
    其它程式都不該進入它們的「critical section」。
    
2. **progress**: 若沒有程式在執行CS，並且有程式想要執行CS，則必須有程式去執行CS。

3. **bounded waiting**: 若一支程式想執行CS，等待的process數量必須是有限的，不能無限被插隊。

## Critical section problem的各種解法

## (軟體解)Peterson's solution

設```int turn;```，```boolean flag[2]```是共享變數。

(初始值```turn=0; flag[0]=flag[1]=false```)

P0的程式如下:
```
/* Process 0*/
do{
    flag[0]=true;
    turn = 1;
    while(flag[1] && turn==1);
        critical section
    flag[0]=false;
        remainder section
}while(1)
```
P1的程式如下:
```
/* Process 1*/
do{
    flag[1]=true;
    turn = 0;
    while(flag[0] && turn==0);
        critical section
    flag[1]=false;
        remainder section
}while(1)
```
