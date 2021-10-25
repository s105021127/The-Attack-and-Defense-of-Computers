## adder
首先將adder轉成執行檔:
```
chmod +x adder
```
執行後會出現下列結果:



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

