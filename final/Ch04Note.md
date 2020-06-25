Ch4 thread - 比process更輕量的程式
===

## thread - 比process更輕量的程式

簡單來說，thread可以視為輕量的process，

一支process裡面可以包含很多支thread，

在同一支process底下的所有thread共享相同的資源。

#### **thread,process,program的差異**

**thread** 是OS分配 CPU 時間的對象

**process** 是OS分配資源(資源指: 記憶體, 變數…等)的對象。

整理後歸納出以下四點:

1. OS 可以管理很多支process

2. 一支process裡面可以包含很多支thread

3. thread 是 OS 分配 CPU 時間的對象

4. process 是 OS 分配資源的對象

#### **什麼時候用process?什麼時候用thread**

舉例:「聊天室」是一支process，可以讓你跟你朋友互傳訊息聊天，

那麼這支process便可以設計兩支thread負責不同功能，

一支thread負責「將你的訊息發送出去」，

一支thread負責「接收別人傳來的訊息」。

## **user thread 與 kernel thread**

簡單來說，kernel thread直接歸OS所管，

user thread則是歸使用者程式所管。

1. OS並不會知道使用者寫了程式去創建user thread，

    不會介入user thread的管理。

2. 但OS會知道有幾個kernel thread存在，

    kernel thread就在kernel mode之下執行，

    由OS直接管理。
    
#### **Multi-threading models 介紹**

由於OS不直接管理user thread，

使用者寫程式要user thread完成的工作，

會連接到kernel thread來完成。

由user thread 的 kernel thread的連接關係來看，

又可以分為三種模型:

1. **Many-to-One model**

![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/IMG/Ch0401.png)


