Ch2常見的OS結構
===
**OS(operating system)是作業系統的簡稱。**

## API(application program interface)介紹- 什麼是API?

Interface是「介面」，
大致上可以想成是函數的長相，

```
int power (int x, int y);
```

API就很像是函式庫的感覺，比如說我想讓程式幫我計算「2的100次方」，我可能就會去呼叫數學運算的API來幫我做這件事。

#### system call 與API的區別

API是使用者介面(user mode)可以使用的函式庫。

System call是OS介面(kernal mode)的函式庫。

#### 訊息傳送方式: message passing v.s. shared memory

在OS的世界裡，可以允許同時有很多支程式(process)在執行，程式之間也可能會需要互相傳遞訊息，而message passing 和 shared memory是最常見的兩種方式。


![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/IMG/Ch0201.png)

**message passing**: 程式傳遞訊息要先傳送到kernal，再傳給另一支程式。
**shared memory**: 兩個程式之間共享一個記憶體，A要傳訊息給B，就把訊息寫在這個記憶體中，B就會看到。

## 常見OS架構

#### Layerd OS Architecture

![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/IMG/Ch0202.png)

Layered OS level分成一層一層的，level N只能從levelN-1去呼叫函數，較容易維護與除錯(debug)，但困難點是較難去定義layer。

#### Microkernal OS

![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/IMG/Ch0203.png)

Microkernal則是節省核心功能，將一些非必要的OS部分移至user space，以message passing 做為process間的溝通，process間的溝通效率較差但kernal結構比較簡單。
