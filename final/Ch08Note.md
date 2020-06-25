Ch8 記憶體管理(memory management)
===

## 為什麼要記憶體管理?

「記憶體(memory)」為電腦暫時存放資料的地方。

當你在電腦上執行程式時，OS會將程式讀進記憶體中，你的電腦必須知道你的資料存放在什麼位置，這就是需要記憶體管理的原因。

## 變數地址的綁定時機

當一個電腦使用者寫了一支程式，

要執行這支程式大致上有三個步驟:

1. 由compiler編譯程式

2. 將程式讀進記憶體中

3. 執行程式

一支程式裡會有許多變數，

變數放在記憶體的位置稱為變數的地址。

決定變數地址的行為稱為綁定(address binding)，

那麼可以綁定變數地址的時機也就是上述說的三個時間點了:

1. **Compile time** (compiler編譯程式時即決定地址)

    在Compile time 就綁定地址，可能綁定到已經正在使用中的記憶體導致無法綁定。

2. **Load time** (將程式讀進記憶體時才決定地址)

    在Load time 綁定地址，如果綁定到已經正在使用中的記憶體，會改至其他位置，缺點是一旦確認位置後無法更改，除非程式重新讀取。

3. **Execution time** (程式的時間動態決定地址)

    在Execution time綁定地址是最有彈性的做法，位置可以隨時更改。
    
因為在「Compile time」和「Load time」綁定地址是沒有彈性的做法，

現在的OS幾乎是採用在Execution time綁定地址的做法。

## 虛擬地址的產生

因為OS是採用在Execution time綁定地址的做法，

所以使用者在寫程式時所看到的地址也僅僅是「虛擬地址」，

並不會看到變數真正放在記憶體的位置。


實際看一段C語言的程式碼，

程式碼相當簡單，

查看變數b的值和變數b的地址:
```
#include <stdio.h>

int main()
{
    int b = 2;

    printf("變數 b 的值：%d\n", b);
    printf("變數 b 的記憶體位址：%p\n", &b); //%p為印出地址的16進位表示法

    return 0;
}
```
例如結果印出:

```變數 b 的值：2```

```變數 b 的記憶體位址：0060FEFC```

但使用者看到的地址```0060FEFC```僅僅是虛擬地址，

b放在真實記憶體的其他位置，

我們無從得知變數b實際放在哪裡。

使用者視角所看到變數b的記憶體位置我們稱為「logical address」，

該變數存放的真實記憶體位置為「physical address」。

## Paging- 連結logical memory與physical memory的機制

對於作業系統來說，怎麼知道在使用者程式在「logical memory」實際上放在「physical memory」哪裡呢?

**OS將logical memory的空間切成相同大小的空間，稱為「page」**

**OS將physical memory的空間切成相同大小的空間，稱為「frame」**

OS會有一張「page table」，用來將logical memory和physical memory做對應的關係，

![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/IMG/Ch0801.png)

假設PageTable說Page0實際對應到frame編號1的位置，

那麼使用者在logical memory看到的page0實際上便放在physical memory的frame1上了。

## logical address的表示方法

一張page的大小可能很大，

裡面不會只有存一個地址，

所以光是知道Page對應到哪個frame還不足以得到完整的地址。

事實上，logical address由兩個部分組成:

**page number(p)**:表示第幾個page

**page offset(d)**:有點像是變數放在這個page的第幾個字的感覺，若d有n個bit表示page size為2的n次方個byte這麼大。

剛剛看到的那張page table，

實際上查的便是page number(p)對應到哪一個frame number(f)，

那麼真實記憶體位置(physical address)即是f串接d得到的值。

## 二次存取記憶體的時間浪費

然而，Page Table它也是放在記憶體裡面的，

如果我們想透過logical address去到physical address的話，

就需要兩次的記憶體存取，

一次用來查詢Page Table，

另一次用來進入physical memory的地址。

## 快速查詢表-TLB

因為TLB屬於cache(快取記憶體)的一種，

查詢TLB的時間比查詢記憶體上的page table還要來的快。

## TLB在context switch後必順刷新

對不同的process來說，它們有各自的page table，但TLB是共用的，

TLB的存在是為了使查找process裡的logical address到physical address的轉換可以快一點。

因為TLB是共用的，如果process交換了，應該先清空TLB，把下一個要執行的process需要的資料(他的page table)跟著拿進來，

否則便會讀到別人page table的資訊，而查詢到錯誤的地址。

## Effective Memory Access Time(EMAT)

EMAT計算平均一次查page而存取記憶體的時間。

1. 假設每次TLB search的時間為20ns (ns是奈秒，時間單位，為10^(-9)秒)

2. 假設每次memory access的時間為100ns

3. 假設TLB hit ratio為70%

查詢時間會有下列兩種情況:

1. 如果透過TLB查到資料了，那麼我們就直接去physical memory就好，

    此時需要的時間為 20+100=120ns。
    
    EMAT= 0.7x120ns + (1-0.7)x220ns= 84+66 = 150ns

2. 如果透過TLB查不到資料，我們要額外花一次時間查page table再去physical memory，

    此時需要的時間為 20+100+100=220ns。
    
如果沒有TLB時，則查詢速度皆為100+100=200ns。

## 碎片化(fragmentaion)

![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/IMG/Ch0802.png)

## 以電腦語言來定義fragmentaion

**internal fragmentaion**: 作業系統配置給 process 的 memory 空間大於 process 真正所需的空間，這些多出來的空間該 process 用不到，而且也沒辦法供其他 process 使用，形成浪費。

**external fragmentaion**: 系統中，所有可用空間總和大於某個 process 所需要，但因為這些空間不連續而無法配給該 process 使用，造成 memory 空間閒置的浪費。

## 真實系統在paging前做segmentation

系列文前兩篇我們一直在講paging，透過page table將使用者視角的logical address映射到 physical address，

然而在真實的電腦作業系統中，實際上還會再多一層轉換，叫做segmentation，

![image](https://github.com/TiaoTiao87/sp108b/blob/master/final/IMG/Ch0802.png)

首先，使用者寫程式所看到的變數地址稱為「logical address」，

先透過segmentation將logical address映射至「linear address」，

再透過paging 將「linear address」映射至「physical address」。

## 為什麼不能只有paging?

對於記憶體管理來說，paging只能解決「外部碎片化」的問題，

因為paging將使用者的地址切成一塊塊固定大小，

segmentation則是解決內部碎片的問題，較符合使用者的視角，

比如說將使用者程式相近的地址打包成一個page啦。

以解決內部碎片的問題。
