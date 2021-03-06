Ch1 簡介作業系統演進:interrupt與TimeSharing系統
===
## 作業系統概念

作業系統是協助管理你電腦上執行的程式的特別程式，
並且它可以讓你的電腦有多支程式「同時」執行。

#### 觀念一: 什麼是電腦「同時」執行程式?

電腦上就只有一個CPU，CPU快速交替執行你的程式，
快速交替的時間可以儘儘幾毫秒就輪流一次，
讓你感覺你電腦上的程式被「同時」執行了。

作業系統就是可以不斷的交替執行你的程式，達到幾乎"同時"執行的效果。

#### 觀念二: 什麼是interrupt?

意思是說可能程式會發出「interrupt」去打斷正在工作的CPU。

對程式來說，如果有什麼特殊需求，會主動發送interrupt給CPU，讓CPU停止現在的工作。

#### 觀念三: 作業系統的演進

早期作業系統的演進分成三個階段:
Batch system-> Multiprogramming system-> Multi-tasking system (又稱為Time sharing system)

**區別:**

Batch system: CPU一次只能執行一個工作，使用者跟電腦沒有互動。

Multiprogramming system: 希望CPU不要閒置，當有程式需要執行時不要發呆。

Time sharing system: 希望每支程式都要被公平的執行到，現代作業系統多屬此類型。

#### 觀念四: interrupt跟Time sharing system的關係?

由硬體每隔一段固定的時間，發送一個interrupt，再透過scheduler告訴CPU該換程式執行了，以達到每支程式都要被公平的執行的目的。


**scheduler**:作業系統內的排程器，用來安排什麼時間CPU執行哪支程式。


## 特權指令與系統保謢

在作業系統上，用了Dual mode來保護系統，

Dual mode上包括了兩種模式 --「kernal mode」及「user mode」，

對電腦系統會造成重大影響的指令我們則稱為「privileged instruction」，中文翻作「特權指令」，只能在「kernal mode」底下執行。

privileged instruction 舉例:I/O指令、關掉interrupt、context switch

**I/O指令**:Input/Output

**context switch**:CPU快速交替執行不同的程式

#### kernal mode/user mode差別

在kernel mode 下執行的是可以存取硬體支援的指令動作，而在user mode 底下的程式，則是只能透過作業系統所提供的系統呼叫常式(system call)，切換到kernel mode下取得資源。

