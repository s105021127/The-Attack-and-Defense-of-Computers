# Fawn
## 測試連結
From Hackthebox -> Labs -> Starting Point -> Meow

https://app.hackthebox.com/starting-point
![image](https://user-images.githubusercontent.com/22366572/148695705-c3c35ab6-c129-4047-93b9-33f5606927b3.png)

## Step 1 -- Connect
Connect to Starting Point VPN before starting the machine

![image](https://user-images.githubusercontent.com/22366572/148696551-29dc1991-d3dc-4fc3-8c36-dc349909f713.png)

1. 點選 Connect to HTB

![image](https://user-images.githubusercontent.com/22366572/148696702-d99f7f6b-b1f8-4ff2-8e87-707a38c1e36e.png)

2. 點選 Staring Point

![image](https://user-images.githubusercontent.com/22366572/148696738-acfc5d81-2ad1-4519-b373-0d6ba1afd48a.png)

3. 點選 OpenVPN

![image](https://user-images.githubusercontent.com/22366572/148696764-4ccb65d0-2eff-4f3e-8317-f19989d84ddc.png)

4. 設定並下載

![image](https://user-images.githubusercontent.com/22366572/148696831-f32ca261-f3aa-4b23-8cbb-8243156b97a8.png)

5. 開啟 Kali，並將剛剛下載的檔案放到想要的資料夾

![image](https://user-images.githubusercontent.com/22366572/148697292-eb54bb14-b93d-4616-9155-cbffe5758a43.png)

6. 連結

指令: sudo openvpn starting_point_woowater.opvn

![image](https://user-images.githubusercontent.com/22366572/148697374-179b7595-78f5-402f-af67-38d4604328c4.png)

成功畫面:

![image](https://user-images.githubusercontent.com/22366572/148698322-74fd58e3-0b7e-49a1-84a6-7524e640a059.png)


## Step 2 -- Online

按下 block 裡的按鈕就完成了
![image](https://user-images.githubusercontent.com/22366572/148715363-5eeecb57-d31f-4697-a098-024f914e482a.png)

## Step 3 -- TASK 1

![image](https://user-images.githubusercontent.com/22366572/148715461-3e1c9ff1-1e5a-4172-8204-fc263b07a339.png)

## Step 4 -- TASK 2

![image](https://user-images.githubusercontent.com/22366572/148715755-f38c2e85-9d92-4e06-aead-2b3a054a577a.png)

## Step 5 -- TASK 3

![image](https://user-images.githubusercontent.com/22366572/148715908-d9557060-65be-4dc2-b51e-d4a61d145c55.png)

## Step 6 -- TASK 4

![image](https://user-images.githubusercontent.com/22366572/148716657-dcc0934c-80c2-4d3a-b845-525b57a8bd0c.png)

## Step 7 -- TASK 5

![image](https://user-images.githubusercontent.com/22366572/148716763-613c73f8-92dc-4c6a-9049-fa403af190dc.png)

## Step 8 -- TASK 6

![image](https://user-images.githubusercontent.com/22366572/148716835-df424e5c-965b-43e4-b305-b227ac83a065.png)

## Step 9 -- TASK 7

![image](https://user-images.githubusercontent.com/22366572/148716944-7bd28cc6-fa2b-4ae4-a9dc-7714848b378e.png)

## Step 10 -- TASK 8

![image](https://user-images.githubusercontent.com/22366572/148717009-a5bb4ad4-8d66-4a97-a171-93b8c6bae970.png)

## Step 11 -- Submit flag

### Introduction

有時，當我們被要求 Enumerate 客戶端網絡上特定主機的服務時，我們會遇到可能配置不當的文件傳輸服務。本練習的目的是讓您熟悉文件傳輸協議 (FTP)，這是一種適用於所有主機操作系統的本地協議，長期以來用於簡單的文件傳輸任務，無論是自動還是手動。如果沒有正確理解，FTP 很容易被錯誤配置。在某些情況下，我們正在評估的客戶公司的員工可能希望繞過文件檢查或防火牆規則，以便將文件從他們自己傳輸到他們的同事。考慮到控制和監視數據流的許多不同機制
在今天的企業網絡中，這種情況成為我們可能在野外遇到的一個實質性且可行的案例。同時，FTP 可用於將日誌文件從一個網絡設備傳輸到另一個網絡設備或日誌
採集服務器。假設負責處理配置的網絡工程師忘記正確保護接收 FTP 服務器或沒有足夠重視日誌中包含的信息，並決定故意使 FTP 服務不安全。在這種情況下，攻擊者可以利用日誌並從中提取各種信息，這些信息以後可用於繪製網絡、枚舉用戶名、檢測活動服務等。
讓我們看看 FTP 是什麼，根據維基百科上的定義：

![image](https://user-images.githubusercontent.com/22366572/148717465-78f75b74-17bc-4519-82bb-192745aad61d.png)



