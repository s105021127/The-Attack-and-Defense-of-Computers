## LuckyGuess
首先將LuckyGuess轉成執行檔:
```
chmod +x LuckyGuess
```
並執行:
```
./LuckyGuess
```

執行後會出現下列結果:

![image](https://user-images.githubusercontent.com/22366572/138750903-aa6849b2-248e-4cb5-af2a-e42750777406.png)


為了瞭解程式碼，我們使用**radare 2**來做逆向分析
## 逆向分析
用 radare2 做 static analysisw
```
r2 LuckyGuess
aa
afl
s.main
VV
```
實作結果:

**<Main 組語>**

![image](https://user-images.githubusercontent.com/22366572/138873761-a4bbaaa4-f8e0-4184-a546-1eae64d5b3d6.png)

