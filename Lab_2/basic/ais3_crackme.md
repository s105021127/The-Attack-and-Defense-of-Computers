## ais3_crackme
首先將ais3_crackme轉成執行檔:
```
chmod +x ais3_crackme
```
執行後會出現下列結果:

![image](https://user-images.githubusercontent.com/22366572/138836735-9745e7f1-804c-4884-81e9-ea1600378724.png)

為了瞭解程式碼，我們使用**radare 2**來做逆向分析
## 逆向分析
用 radare2 做 static analysis
```
r2 ais3_crackme
aa
afl
s.main
VV
```
實作結果:

**<Main組語>**

![image](https://user-images.githubusercontent.com/22366572/138836103-e44b91be-c2ae-4c90-bc5e-bd19824fefba.png)


## 撰寫exploit code
```
from pwn import *

r = process('./ais3_crackme')

r.sendline('1337 0 0')

r.interactive()
```
實作結果:
