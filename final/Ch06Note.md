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

```flag[i]```代表process 1 想執行了，

```turn```代表輪到誰執行了。

**證明 progress**:

在Peterson's solution中改良為```flag[i]```，

```turn```兩個變數去控制，

如果P0想執行但P1不想執行，即便是輪到P1執行

也能夠因為P1不想執行而使P0順利進入它的CS。

因為擋住P0進入它的CS的條件是```while(flag[1] && turn==1);```

因此只要```flag[1]=false```，P0就能進入它的CS。

**證明 bounded waiting**:

因為P0進CS前做了```flag[0]=TRUE;```，```turn = 1```

做的是「我想執行了，但先讓給對方執行」，

因此對兩支process來說，

都不會無限插隊。

## (軟體解)Bakery algorithm (n processes)

假設有P0, P1, ..., P(n-1) 共n支程式(Pi的id為i)，

共享變數為```boolean choose[i]```，```int num[i]```，

Pi的程式如下:
```
/* Process i*/
do{
    choose[i]=true;
    num[i] = max(num[0], num[1], ..., num[n-1])+1 //抽號碼牌
    choose[i]=false;
    for(j=0;j<n;j++){
        while(choose[j]); //等待所有想領號碼牌的程式抽號碼牌
        while((num[j]!=0) && (num[j],j)<(num[i],i)); // 等待號碼牌小的程式先執行，若號碼牌相同則程式id小的優先
    }
        critical section
    num[i] = 0; //重置號碼牌為0，待下次重新排隊再抽號碼牌
        remainder section
}while(1)
```
這個解一樣可滿足 mutual exclusion, progress, bounded waiting。

## (硬體解)Atomic TestAndSet

atomic的意思是原子的、不可分割的，

用來形容一段程式碼便是這段程式碼在執行的過程中，

保證不會被其它程式打斷。

解法如下:
```
bool TestAndSet(bool &lock){
    bool value = lock;
    lock = true;
    return value
}
```

假設我們只有兩支程式(process)P0, P1，

```bool lock;```是共享變數，

則P0, P1的程式碼都是相同的，如下:
```
do{
    while( TestAndSet(bool &lock));
        critical section
    lock = false;
        remainder section
}while(1)
```
## (硬體解)Atomic Swap

假設Swap()函數是不可分割地執行，

假設我們只有兩支程式(process)P0, P1，

```bool lock;```是共享變數，

則P0, P1的程式碼都是相同的，如下:
```
do{
    key = true; //key是私有變數而非共享變數
    while(key)
        Swap(key,lock);
        critical section
    lock = false;
        remainder section
}while(1)
```
<分析>對於硬體解Atomic TestAndSet和Atomic Swap來說，

程式碼雖然簡單，

能滿足mutual exclusion和progress，

但是因為沒有限制程式執行的順序而會違反bounded waiting。
