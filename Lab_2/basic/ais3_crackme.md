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

可以發現如果 **cmp dword [var_4h], 2** 不相等，就會跳到左下角的組語，會印出"You need to enter the secret key!"後結束程式。

那如果 **cmp dword [var_4h], 2** 不相等的話，就會跳到右下角的組語，可以發現這裡也有一個判斷式:

![image](https://user-images.githubusercontent.com/22366572/138869911-b6327a2d-e7ed-4af0-9379-1201bf9ba2c5.png)


如果 **test eax, eax** 相等，就會跳到右下角組語，會印出"I'm sorry, that's the wrong secret key!"
<p> 但如果 <b>test eax, eax</b> 不相等，就會跳到右下角組語，會印出"Correct! that is the secret key!"
  
所以我們的目標是想辦法印出這行字!

---
<補充常見的跳躍指令>

  <p>指令	功能
  <p>je	相等則跳 ==
  <p>jne	不相等則跳 !=
  <p>jg	大於則跳 >
  <p>jge	大於等於則跳 >=
  <p>jl	小於則跳 <
  <p>jle	小於等於則跳 <=
  <p>jmp	無條件跳 goto
  <p>jz	等於零則跳 ==0
  <p>jnz	不等於零則跳 !=0
    
|指令 |功能|
|-----|--------|
|je   |相等則跳 ==|
|jne  |不相等則跳 !=|
|jg   |大於則跳 >|
|jge  |大於等於則跳 >=|
|jl   |小於則跳 <|
|jle  |小於等於則跳 <=|
|jmp  |無條件跳 goto|
|jz   |等於零則跳 ==0|
|jnz  |不等於零則跳 !=0|    
---

## 撰寫exploit code
```
from pwn import *
r = process('./ais3_crackme')

payload = b'A'*6

r.sendline(payload + '2')

r.interactive()
```
實作結果:
