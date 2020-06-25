Ch3程式生命週期-new, ready, running, waiting, terminated
===


![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/IMG/Ch0301.png)

## 程式的生命週期

這張圖展示了一個程式(process)的生命週期，主要會有五個狀態，大概就是描述有多個程式的互動關係了。

**new**: 程式剛創建的階段

**ready**: 程式準備就緒，就等OS安排執行了

**running**: 程式正在執行中

**waiting**: 等候system call完成

**terminated**: 程式已經執行完畢

1. **new -> ready** : 有新的程式加入會由「new state」切換到「ready state」。

2. **ready -> running**:在OS上實際上有支叫做scheduler的程式進行程式的排程，CPU一次只會執行一支程式，scheduler會決定下一個給CPU執行的程式是哪個。

    (ready, running state間互換稱為「context switch」)
    
3. **running -> ready**:透過interrupt打擾CPU，interrupt除了程式有特別需求去打斷CPU以外，更多的interrupt可能是來自「Timer」的打斷。

4. **running -> waiting**:就是程式要做一件耗時的事情時，OS就會先把CPU的資源讓出來給其它程式使用，程式便進入 waiting state。

5. **waiting -> ready**:當程式將耗時的事情做完時，程式就會回到ready state等候OS安排給CPU執行。

6. **running -> terminated**:程式執行完畢。

## scheduler的種類

依據不同的程式狀態切換，scheduler可以分為三種。

**Short-term scheduler**: 又稱 CPU scheduler，在Ready state中選擇哪支程式可以進入running state，被CPU執行。

**Medium-Term Scheduler**: 決定哪支程式要請出記憶體，由running state 進入waiting state等待。用意是當程式要做IO, sleep這類耗時長的事情時，先將程式移出記憶體，等待它做完，再將程式搬回記憶體，節省記憶體空間。

**Long-term scheduler**: 又稱 job scheduler，選擇哪些程式可以從new state進入ready state，決定的是當前可以有多少支程式同時執行。
