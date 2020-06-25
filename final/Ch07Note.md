Ch7 Deadlock(死結)
===

## deadlock圖解

![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/IMG/Ch0701.png)

## deadlock的四個必要條件

1. **Mutual exclusion**: 同一時間同個資源只能被一個process所用

2. **Hold and wait**: process手上可以握有資源並等待其它process的資源

3. **No preemption**: process手上的資源只能是自願放掉的，不能被其它process搶走

4. **Circular wait**: 存在多個process(P0, P1, ..., Pn)互相等待資源的情形(P0等P1的資源，P1等P2的資源，…，Pn等P0的資源)

我們可以用上方的「交通阻塞圖」，

生活化地解釋這四個必要條件:

**mutual exclusion**: 路寬只能有一輛車通行

**Hold and wait**: 車子占領十字路口，並等待橫向車輛先過去

**No preemption**: 車子不准插隊(不像現實中救護車、消防車可以直接插隊)

**Circular wait**: 不同路段的車子循環的等待

## 如何預防deadlock?

這裡介紹三種預防deadlock的方法，分別是:

## 1.Deadlock prevention

Deadlock prevention指的是透過避開Deadlock的四個必要條件，來阻止deadlock的發生。

1. 避開 **Mutual exclusion**：在一些允許共享資源的程式上，可不需要Mutual exclusion，例如read-only file。

2. 避開 **Hold and wait**：所有程式不允許手上占著資源，程式必須一次拿走工作所需的資源。但缺點是資源利用率會很底。

3. 避開 **Preemption**: 讓process手上的資源是允許被其它程式搶走的。

4. 避開 **Circular waiting**：假設我們有R0，R1，…，Rn這n+1種資源，process在申請要資源Rk之前，必須把編號大於k的資源都放掉，避免循環等待。

## 2.Deadlock avoidance

Deadlock avoidance又細分為以下兩種:

## Resource-allocation graph algorithm

我們可以將程式之間擁有資源及等待資源的關係，

以圖示視覺化的畫出來。

![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/IMG/Ch0702.png)

這張圖表示有三個process P1,P2,P3，

有四種資源 R1,R2,R3,R4，

其中R1與R3各有一個，

R2有兩個，

R4有三個。


箭頭P->R(圖中的紫色箭頭)表示P在等待資源R，

箭頭R->P(圖中的綠色箭頭)表示P擁有資源R。


Resource-allocation graph algorithm指的便是檢查這張graph是否有形成cycle，

只有沒有cycle，即確定不可能產生deadlock。

以本圖為例是沒有cycle的。

## Banker's algorithm

當程式提出資源的申請時，透過一個叫做「Banker's algorithm」的演算法檢查程式是否會進入「unsafe state」，

![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/IMG/Ch0703.png)

safe state是絕對不可能發生deadlock的情形，

unsafe state是有可能發生deadlock的情形，

若允許程式提出資源的申請會進入「unsafe state」，

便拒絕該程式的申請。
