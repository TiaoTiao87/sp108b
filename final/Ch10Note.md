Ch10 File System Interface(檔案系統)
===

##什麼是檔案?

檔案(file)是由OS所創建的數據資料(logical storage unit)，

若是存放在光碟(disk)內的資料稱為physical storage unit。

記錄一個檔案的屬性為Metadata，比如說name(檔名), size(檔案大小), type(檔案型別)，

以location(檔案位置), protection(保護) 兩個資訊尤其重要。

要知道檔案的確切地址，

File system會做一些location的映射，類似memory的page table。

##檔案操作

我們可以對檔案進行的操作如下:

1.創建檔案

2.寫入檔案

3.讀取檔案

4.重置擋案 (file seek)

5.刪除檔案

6.截短檔案 (Truncating file): 將檔案尾巴的資料切掉

##open-file table

在process的PCB(process control block)裡，

會有一個地方指向open-file，

必須要記錄哪支程式開了這個檔案，

以及開過檔案的資訊。

依檔案屬性是否為processes之間共用又分為兩種:

Per-process table: 記錄與process獨立的資訊，比如讀檔位置、開檔權限，每支程式讀檔位置可不相同，比如P1從開頭讀、P2想從中間讀; 開檔權限也可不同，比如P1用read開，P2用write開。

System-wide table: 所有process共用的資訊 (當檔案被修改時，對所有process要同步更新的資訊，如檔名、檔案內容)

以下介紹三種常見存取檔案的方式:

##sequential access (連續的存取)

檔案讀寫每次只能從上一次的讀寫位置繼續做，

例如C語言的fread, fwrite。

**檔案設計上常用這種方式，因為檔案讀寫通常是連續的。**

##Direct(relative) access (任意位置開始的存取)

可以從檔案的任意位置進行存取，

但缺點是需要額外用一個變數存位置資訊，

讀檔的程式碼會變複雜。

##Index access method

此種方式與Direct(relative) access的差別在於，

Direct(relative) access是用「檔案的位置」去做為查找，

比如說可以指定讀取檔行的第5行內容。

Index access method則是更彈性的存取，

可以用自訂資訊來查找檔案內容。

##檔案目錄結構(Directory Structure)

##Single-Level Directory

https://ithelp.ithome.com.tw/upload/images/20200103/20117114E0iekdw7wH.png

將所有檔案放在同一個目錄下，

而沒有資料夾(folder)的概念，

缺點是檔名必須是唯一的，

並且若檔案數量多，找到檔案的效率會下降。

##Two-Level Directory

https://ithelp.ithome.com.tw/upload/images/20200103/20117114JX8aqx4UnL.png

Single-Level Directory的改良版，

以不同的user來區分檔案，

檔案的路徑= 使用者名稱+檔案名稱。

但是對於每個user來說，

仍會有Single-Level Directory的缺點。

##Tree-structured Directory

https://ithelp.ithome.com.tw/upload/images/20200103/20117114q0OgVw5uUG.png

這個結構已經非常接近平時使用電腦的感覺了，

檔案的路徑又可分為「絕對路徑」與「相對路徑」兩種。

##Acyclic Graph Directory(沒有cycle的graph)

https://ithelp.ithome.com.tw/upload/images/20200103/201171142uUfkwiI3e.png

現代電腦的真實系統多是做成這樣的，可以不同路徑但指到同一個檔案。

##檔案權限管理

每個檔案有三種屬性owner, group, other，

判斷這個檔案由不同人開啟的權限。

每個file會有一個唯一的owner，表示誰創建了這個檔案。

group是一個管理檔案的群

other是不在owner也不在group裡的
