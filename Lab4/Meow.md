# Meow
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

![image](https://user-images.githubusercontent.com/22366572/148697497-df0168b2-62ee-43bf-b563-b7c17d8dba2c.png)

export ip=10.129.189.222

![image](https://user-images.githubusercontent.com/22366572/148698477-043cf303-708f-452e-8c04-573b5423341e.png)


## Step 3 -- TASK 1

![image](https://user-images.githubusercontent.com/22366572/148698526-93459e9a-993a-4d21-a1f0-43473ea07adb.png)

## Step 4 -- TASK 2

![image](https://user-images.githubusercontent.com/22366572/148698582-4277edb3-eb1e-4688-bfab-d43aa1949dba.png)

## Step 5 -- TASK 3

![image](https://user-images.githubusercontent.com/22366572/148698760-65bcbf6b-ea9e-487d-b008-f0087b718602.png)

## Step 6 -- TASK 4

![image](https://user-images.githubusercontent.com/22366572/148698868-7a289c43-4ece-45a2-b97e-e900674868b0.png)

## Step 7 -- TASK 5

![image](https://user-images.githubusercontent.com/22366572/148698932-49ab881f-6ae4-4a02-b7b9-dbb33454d0a1.png)

## Step 8 -- TASK 6

![image](https://user-images.githubusercontent.com/22366572/148699005-8de9152a-196a-41c7-a6e4-4cb11b0d8031.png)

指令: nmap -vvvv $ip

![image](https://user-images.githubusercontent.com/22366572/148699175-995b2a8e-9f57-4281-953d-ab0432d38bc7.png)

## Step 9 -- TASK 7

![image](https://user-images.githubusercontent.com/22366572/148699639-179eb7e8-8b5a-4bf7-ab75-82fb53c82843.png)

## Step 10 -- TASK 8

![image](https://user-images.githubusercontent.com/22366572/148699834-fd2ffed4-b7bf-4271-b75d-247c902ee242.png)

## Step 11 -- TASK 9

### Introduction

當第一次開始對目標進行滲透測試或任何安全評估時，主要步驟被稱為
Enumeration。此步驟包括記錄目標的當前狀態，以便盡可能多地學習
可能的。

每個服務器都使用ports向其他客戶端提供數據。Enmeration階段的第一步
涉及掃描這些開放端口以查看網絡上目標的用途以及在其上運行的服務可能出現的潛在漏洞。為了快速掃描端口，我們可以使用名為 nmap 的工具。

找到目標上的開放端口後，我們可以使用不同的工具手動訪問它們中的每一個，以確定我們是否可以訪問它們的內容。不同的服務將使用不同的工具或腳本來訪問。

### Enumeration

1. 指令: ping {target_IP}

成功建立 VPN 連接後，我們可以 ping 目標的 IP 地址以查看我們的數據包是否到達目的地。 您可以從起點實驗室頁面獲取當前目標的 IP 地址，然後在鍵入 ping 命令後將其粘貼到終端中，如下圖所示。

![image](https://user-images.githubusercontent.com/22366572/148714349-7e28ec38-766f-47f4-ae0c-e1be39da0ed5.png)

實驗結果:

![image](https://user-images.githubusercontent.com/22366572/148708774-42fbfb3b-ab40-43a7-acd4-55cb90758e21.png)

2. 指令: sudo nmap -sV {target_IP}

繼續下一步 - 掃描目標的所有打開端口以確定
在其上運行的服務。為了開始掃描過程，我們可以使用以下命令和
nmap 腳本。 nmap 代表 Network Mapper，它會向目標端口發送請求，希望收到回复，從而確定所述端口是否打開。某些服務默認使用某些端口。其他可能是非標準的，這就是為什麼我們將使用服務檢測標誌 -sV 來確定已識別服務的名稱和描述。

![image](https://user-images.githubusercontent.com/22366572/148714406-dd4ab6a6-176e-44d8-8252-6d13db8e3d90.png)

實驗結果:

![image](https://user-images.githubusercontent.com/22366572/148712930-80754a96-dd40-4dd4-8660-0595460347b0.png)

3. 指令: telnet {target_IP}

掃描完成後，我們發現端口 23/tcp 處於打開狀態，正在運行 telnet 服務。 通過對該協議的 Google 快速搜索，我們發現 telnet 是一種用於遠程管理網絡上其他主機的舊服務。 由於目標正在運行該服務，它可以接收來自網絡中其他主機（例如我們自己）的 telnet 連接請求。 通常，通過 telnet 的連接請求配置了用戶名/密碼組合以提高安全性。 我們可以看到我們的目標就是這種情況，因為我們遇到了 Hack The Box 橫幅和來自目標的請求，以在被允許繼續對目標主機進行遠程管理之前對自己進行身份驗證。

![image](https://user-images.githubusercontent.com/22366572/148714454-c6be7c5c-db50-487d-8ffe-eda9c557756f.png)

實驗結果:

![image](https://user-images.githubusercontent.com/22366572/148714583-494d832b-6aaf-490e-a73b-f8e1b9bc516b.png)

我們需要找到一些可以繼續工作的憑據，因為在我們可以探索的目標上沒有其他端口打開。

### Foothold

有時，由於配置錯誤，**一些重要的帳戶可以留下空白密碼以方便訪問**。 這對於某些網絡設備或主機來說是一個重大問題，使它們容易受到簡單的暴力破解攻擊，攻擊者可以嘗試使用不輸入密碼的用戶名列表按順序登錄。
一些典型的重要賬戶具有不言自明的名稱，例如：
* admin 
* administrator
* root
嘗試使用這些憑據登錄以希望其中一個存在且密碼為空的直接方法是在主機請求它們時在終端中手動輸入它們。 如果列表更長，我們可以使用一個腳本來自動化這個過程，為它提供一個用於用戶名的單詞表和一個用於密碼的單詞表。
通常，用於此任務的詞表由典型的人名、縮寫或來自先前數據庫洩漏的數據組成。 現在，我們可以求助於手動嘗試上面的這三個主要用戶名。

![image](https://user-images.githubusercontent.com/22366572/148714864-d2a7e40e-84e8-43f1-a1bb-e108b68704b4.png)

前兩個對我們來說並不那麼幸運。 當事情往下看時，必須繼續前進，堅持不懈。 除非我們嘗試所有的可能性，否則我們無法成功。 讓我們試試最後一個。

![image](https://user-images.githubusercontent.com/22366572/148714898-a6a1ba6f-2b9a-4f4f-871f-9b06ee8e9ea1.png)

成功！ 我們已經登錄到目標系統。 我們現在可以繼續查看使用 ls 命令進入的目錄。 我們有可能找到我們正在尋找的東西。

![image](https://user-images.githubusercontent.com/22366572/148714947-663958b5-ca0d-47f7-ac3a-ad5239a0420b.png)

在這種情況下，flag.txt 文件是我們的目標。大多數 Hack The Box 的目標都會有這些文件之一，
它將包含一個稱為 flag 的哈希值。這些目標文件的命名約定因實驗室而異。例如，每周和退役機器將有兩個標誌，即 user.txt 和 root.txt 。 CTF 目標和其他實驗室將具有 flag.txt 。大多數情況下，挑戰將不包含實際文件，而是在您解決它時為您提供標誌的片段，各個部分更均勻地嵌入到挑戰中（隱藏在圖像中的文本或其他示例）。
您可以使用 cat 命令讀取文件以在終端中顯示哈希值。複製標誌並將其粘貼到起點實驗室的頁面將授予您這台機器的所有權，從而完成您的第一個任務。
恭喜！

![image](https://user-images.githubusercontent.com/22366572/148715135-9619e8cc-5ee1-4b0a-9e0a-6e8efacaab64.png)


## 問題
不知道為甚麼突然沒辦法 ping
![image](https://user-images.githubusercontent.com/22366572/152700873-a0b54308-b053-4dc4-a05b-c12cd1f8633d.png)
