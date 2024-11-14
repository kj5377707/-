## 1-1
### 2
在`image_file_header`可以看到`time date stamp`，代表編譯的時間
![[Pasted image 20241113131531.png]]
### 3
在`image_section_header`中的值看起來並沒有過多差距(`data`正常)，因此判斷沒有加殼的跡象
### 4
*exe* : 
	看起來是一些有關file的操作和創建一個map(對process可見)
	有一個奇怪的kerne132.dll

*dll* : 
	對process的一些操作(`mutux, OpenProcess`)，還是`send, recv`等網路操作
	string那邊有一個ip addr，可能是聯線的目標?
### 5
或許可以看有沒有創造一些file? -> ==kerne132會是個好的對象==
### 6
有一些socket相關的操作，可以從這裡入手 -> ==找到的ip位置==
### 7
打開file，並把裡面的資料傳出去?

### 總結
*記得檢查string*
在對一隻malware分析時，編譯時間，在disk跟memory的大小，導入/出表，字串可以做為初步檢查的目標(resource section在這裡沒有)
## 1-2
### 2
在section_header可以看到`raw`是0但`virtual`是1000，可能有packer
==透過section的名字可以看出來這是用UPX packer，解開後可以看到更多==
### 3
有一個createServiceA，可能會創建服務
==可以看到Service name為MalwareService==
### 4
InternetOpenA : 和https/ftp互動
==InternetOpenUrl : 打開一個url==

### 總結 
看到section的名字可以嘗試用upx解看看，解完能看到更多
## 1-3(ch18回來做)
![[Pasted image 20241114155637.png]]
## 1-4
### 2
看起來沒有加殼
### 3
2019(被改過了)(書是2012出版的)
### 4
1. 提權
2. resource相關操作(等下檢查resource section)
3. `CreateFile, WriteFile`
4. `OpenProcess`
把resource裡的東西開成新的process(要先把東西寫成一個文件), 並提權

### 5
有一個更新的程式名字
![[Pasted image 20241114161347.png]]
![[Pasted image 20241114161604.png]]
可以看到section header是一個PE檔

### resource section
![[Pasted image 20241114161958.png]]會從網路上抓一個檔案下來

### 總結
自己是一個launcher，把rsrc裡的文件抓下來後其會抓檔案下來(可能執行，因為有winexec)

