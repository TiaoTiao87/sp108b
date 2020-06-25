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

![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/Ch0201.png)
