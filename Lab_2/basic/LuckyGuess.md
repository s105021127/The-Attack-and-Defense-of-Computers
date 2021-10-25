## LuckyGuess
首先將LuckyGuess轉成執行檔:
```
chmod +x LuckyGuess
```
執行後會出現下列結果:

![image](https://user-images.githubusercontent.com/22366572/138693179-6ffdaed5-3a37-438b-957e-6d33ba292282.png)

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


