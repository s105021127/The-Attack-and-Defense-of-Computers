## adder
首先將adder轉成執行檔:
```
chmod +x adder
```
執行後會出現下列結果:

![image](https://user-images.githubusercontent.com/22366572/138789434-9189c3fd-1629-419a-83f6-508496d86d6d.png)



為了瞭解程式碼，我們使用**radare 2**來做逆向分析
## 逆向分析
用 radare2 做 static analysis
```
r2 pass
aa
afl
s.main
VV
```
實作結果:

**<Main組語>**
![image](https://user-images.githubusercontent.com/22366572/138788140-f977a32c-436a-48dd-aa95-1380b48002fc.png)
從倒數幾行可以看到 var_ch var_10h var_14h 相加以後會跟"1337"判斷相不相等，從這裡可以知道這三個變數是儲存使用者輸入的3個數字，如果這三個數字相加不等於1337，則會跳到這組組語:

**<數字和!=1337>**
![image](https://user-images.githubusercontent.com/22366572/138788714-48eaf671-7a03-4be5-a4bd-0c2ed43950ce.png)
可以發現，如果不相等的話系統會輸出"nope"

如果這三個數字相加等於"1337"，則會跳到這組組語:

**<數字和==1337>**
![image](https://user-images.githubusercontent.com/22366572/138788946-08fc0436-2488-420b-a840-44e991758774.png)
可以發現就會輸出成功訊息"easyctf"

所以只要將3個數字和設為0x539(1337)，就可以達成目標!

## 撰寫exploit code
```
from pwn import *

r = process('./adder')

r.sendline('1337 0 0')

r.interactive()
```
實作結果:

![image](https://user-images.githubusercontent.com/22366572/138793480-64502571-708c-4d0f-a952-fd7b7088867f.png)

