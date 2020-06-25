Ch5 程式排程問題
===

## 什麼是排程?

CPU只負責執行程式，

選擇接下來換哪支程式給CPU執行的角色為scheduler。

排程問題有分「可搶先(preemptive)」與「不可搶先(non-preemptive)」兩種。

## 常見的scheduling algorithm

## 先來先到排程法(first-come-first-served, FCFS)

process1(24分鐘)、process2(3分鐘)、process3(3分鐘)，在時間點0時，依序來了2、3、1號。

![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/IMG/Ch0501.png)

那麼對於每個process來說，

1號等待時間為6分鐘，

2號等待時間為0分鐘，

3號等待時間為3分鐘，

平均等待時間(AWT)為(6+0+3)/3=3分鐘。

**潛在問題: convoy effect(護衛效應)，指的是如果執行時間短的程式排在執行程式長的程式後面，造成的平均等待時間很長。**

例子: process1(24分鐘)、process2(3分鐘)、process3(3分鐘)，在時間點0時，依序來了1、2、3號。

![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/IMG/Ch0502.png)

1號等待時間為0分鐘，

2號等待時間為24分鐘，

3號等待時間為27分鐘，

AMT= (0+24+27)/3=17分鐘，平均等待時間則高達17分鐘，

因此，先來先到排程法很可能為了等執行時間長的程式，對效能有不良的影響。

## 最短工作優先排程法(shortest job first，簡稱SJF)

優先執行可以最快結束的程式，改良convoy effect的問題，

亦分為「可搶先(preemptive)」與「不可搶先(non-preemptive)」兩種。

## 優先等級排程法(priority scheduling)

亦分為「可搶先(preemptive)」與「不可搶先(non-preemptive)」兩種。

**可搶先**(preemptive):程式可以在CPU burst time結束前被其它程式搶先。

**不可搶先**(non-preemptive):程式不可以在CPU burst time結束前被其它程式搶先。

**潛在問題**: starvation(餓死)，指的是程式一直沒有被執行到的狀況。原因是若優先權高的程式一直不斷的來，那麼優先權低的程式就可能永遠執行不到。

**解決方法**: aging(年齡增長)，意思是每隔一段時間，就增加沒有被執行程式的優先權，避免有些程式永遠等不到CPU執行。

## 知更鳥式排程法(Round-Robin，簡稱RR)

scheduler每隔一段Timer就會告訴CPU，讓CPU先換下一個process。
