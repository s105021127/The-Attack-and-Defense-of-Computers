# ret2libc1
## 執行結果

會自動印出 " RET2LIBC >_<"

接著輸入任意字串

結束執行

![image](https://user-images.githubusercontent.com/22366572/148839660-8d8a26ce-cc93-417c-8cb0-e9650c19ffdb.png)

## 分析步驟
### Step 1 : 獲得檔案的基本資料

指令: ```file [執行檔名]```

![image](https://user-images.githubusercontent.com/22366572/148839990-0c63cbd9-9962-4996-ab85-9c7076b701e3.png)

可以看到它是在 GNU/Linux 2.6.24 上編譯的，dynamically linked (動態連接的)，ELF 32-bit LSB executable

### Step 2 : 看開啟了哪些保護機制

指令: ```checksec [執行檔名]```

![image](https://user-images.githubusercontent.com/22366572/148840299-0e68c03b-1981-44d4-aca7-99bba7bc32e1.png)

可以看到 **NX 是開啟的**

NX enabled 如果開啟就代表 **stack 中的數據沒有執行權限**，所以當攻擊者在 stack 上部屬自己的 shellcode 並觸發時，只會直接造成程式崩潰，**但是可以利用 ROP 這種方式繞過**

### Step 3 : 做逆向工程


## 參考資料
