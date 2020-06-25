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

1. new-> ready : 有新的程式加入會由「new state」切換到「ready state」。

2. ready->running:在OS上實際上有支叫做scheduler的程式進行程式的排程，CPU一次只會執行一支程式，scheduler會決定下一個給CPU執行的程式是哪個。

(ready, running state間互換稱為「context switch」)
