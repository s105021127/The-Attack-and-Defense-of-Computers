# ret2shellcode
## 分析步驟
### Step 1 : 獲得檔案的基本資料

指令: ```file [執行檔名]```

![image](https://user-images.githubusercontent.com/22366572/148719601-e694ff03-15c6-41e8-91e0-63f9528e3fc8.png)

可以看到它是在 GNU/Linux 2.6.24 上編譯的，dynamically linked (動態連接的)，ELF 32-bit LSB executable

### Step 2 : 看開啟了哪些保護機制

指令: ```checksec [執行檔名]```

![image](https://user-images.githubusercontent.com/22366572/148719750-61ca71ad-4ce4-4565-ac46-e310794b4f09.png)

可以看到這個執行檔沒有開啟保護機制

### Step 3 : 開啟 IDA 做逆向工程
