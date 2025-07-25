# SUSE 15 安裝示範

## 步驟 1：下載安裝檔

前往 SUSE 官網，下載對應硬體設備之安裝檔。

本次教學以下載15 SP7為例

![如圖](image/suse17.jpg)

![如圖](image/suse17_2.jpg)

接下來準備開始安裝虛擬機

---

## 步驟 2：安裝virtual box與建立VM

### 安裝virtual box

將SUSE安裝在VM之前我們必須在實體機器上創建一台VM

◎ 本次安裝VM設置為(CPUS：2 / RAM：4196MB / Disk：60GB)

本次選用Virtual Box來進行安裝

並依照電腦系統安裝正確的安裝包

![如圖](image/VB1.jpg)

然後直接打開安裝包

接下來三張圖直接點選下一步

![如圖](image/VB2.jpg)

![如圖](image/VB3.jpg)

![如圖](image/VB4.jpg)

接下來兩步皆點擊是

![如圖](image/VB5.jpg)

即可正式進入virtual box的介面

### 建立VM

點擊新增(N)

![如圖](image/VB6.jpg)

填寫此VM的基本資料

![如圖](image/VB7.jpg)

依照我們對VM的配置填入正確的設定

![如圖](image/VB8.jpg)

如此一來VM即設置完成

### 安裝SUSE

接下來要將SUSE系統裝入虛擬機內

在虛擬機上點擊設定並進入存儲裝置 點擊控制器:IDE 點擊空的

並在右側的光碟機choose a disk file

找出安裝的SUSE檔

![如圖](image/VB10.jpg)

便可以打開VM正式為其安裝SUSE

接下來只需要照著以下幾張圖片進行就可以

![如圖](image/SU1.jpg)

點選SUSE Linux Enterprise Sever 15 SP7

![如圖](image/SU2.jpg)

同意後點next

![如圖](image/SU3.jpg)

skip registration

![如圖](image/SU4.jpg)

自行選擇要不要額外裝設

![如圖](image/SU5.jpg)

next

![如圖](image/SU6.jpg)

minimal

![如圖](image/SU7.jpg)

expert partitioner----->start with correct proposal

![如圖](image/SU8.jpg)

將sda1-sda4全數刪除

![如圖](image/SU9.jpg)

照下列步驟自行設置

![如圖](image/SU10.jpg)

![如圖](image/SU11.jpg)

![如圖](image/SU12.jpg)

![如圖](image/SU13.jpg)

![如圖](image/SU14.jpg)

![如圖](image/SU15.jpg)

![如圖](image/SU16.jpg)

設置時區

![如圖](image/SU17.jpg)

skip the creation

![如圖](image/SU18.jpg)

設密碼

![如圖](image/SU19.jpg)

![如圖](image/SU20.jpg)

![如圖](image/SU21.jpg)

![如圖](image/SU22.jpg)

---

## 步驟 3：



## 2.network
2-1 DHCP

打開 Yast→ 選擇 System → Network Settings

![如圖](image/DHCPyast.jpg)

輸入指令確認有無連上網路

![如圖](image/ping888.jpg)

2-2 static IP

輸入vim /etc/sysconfig/network/ifcfg-eth0

調整內容

BOOTPROTO='static'

STARTMODE='auto'

IPADDR='192.168.1.100/24'

GATEWAY='192.168.1.1'

NAME='eth0'

![如圖](image/static.jpg)

2-3 show ip, subnet mask, routing/gate way

查看 IP----->使用ip a

![如圖](image/ipa.jpg)

查看 gateway----->ip route

![如圖](image/gateway.jpg)

查詢Subnet Mask---->輸入ip addr

![如圖](image/subnestmask.jpg)

2-4 2 vm access each other

## 3.setup ssh server and access it

3-1 enable ssh server

照以下順序輸入指令

1.zypper in openssh   2.systemctl start sshd  3.systemctl enable sshd  4.systemctl status sshd

最後若出現Active: active (running) 這樣就代表你已成功啟動 SSH Server

<img width="746" height="419" alt="image" src="https://github.com/user-attachments/assets/d26cf510-b05d-4e12-81c8-41de21d594e6" />


3-2 setup firewall

依序輸入

1.firewall-cmd --permanent --add-service=ssh

2.firewall-cmd --reload

3.firewall-cmd --list-all

<img width="414" height="332" alt="image" src="https://github.com/user-attachments/assets/ce526b27-c6f5-46e9-a058-667ecff4bd08" />



## 4.change hostname

1.開啟yast

2.選擇 System → Network Settings

3.修改Hostname欄位並儲存

<img width="1276" height="797" alt="image" src="https://github.com/user-attachments/assets/88661b62-0eb6-442e-8eac-e2a8a2a79052" />


## 5.repo & package

5-1 setup DVD or image iso repo

使用zypper ar file:///mnt/.....  名稱可以自己決定

再用zypper lr  檢查是否新增成功

<img width="1280" height="148" alt="image" src="https://github.com/user-attachments/assets/f2faa7c4-9381-4879-948a-521aa86418ed" />


5-2 install mlocate, apache

回到 YaST 主畫面，選擇：Software Management

搜尋 mlocate → 勾選並安裝

搜尋 apache2 → 勾選並安裝

按Accept開始安裝

5-3 find package to include /usr/bin/chsh

輸入zypper wp /usr/bin/chsh

<img width="1280" height="195" alt="image" src="https://github.com/user-attachments/assets/bdd576d5-3d26-44d7-ae89-fe003c889035" />


5-4 list all file in mlocate



## 6.user management

6-1 create user account

使用useradd [-s /bin/bash] [-m] user1

使用passwd user1設定密碼


6-2 user join to wheel group

使用usermod -aG wheel user1 將成員加入群組

可用cat /etc/group查看系統有哪些群組

使用id user1便能確認user1有哪些群組


6-3 force change user password when next login

使用以下指令(chage [option] newuser)

-d LAST_DAY: 設定上次密碼變更的日期

若輸入chage -d 0 student01

會把「上次變更密碼」設定成 0 系統會強迫該使用者下次登入時重設密碼


## 7.sudo

7-1 wheel group could run all commonad

使用visudo指令進入vi編輯模式

在字串中找到   # %wheel ALL=(ALL) ALL

把# 刪掉變成%wheel ALL=(ALL) ALL

要離開vi編輯模式只需按下 Esc -----> 輸入 :wq 然後按 Enter

輸入usermod -aG wheel user1


## 8.ssh

8-1. generate ssh key

使用ssh-keygen 產生一組金鑰

會出現下代碼 代表已經產生公鑰與私鑰

<img width="476" height="499" alt="image" src="https://github.com/user-attachments/assets/f499244b-1738-4756-8bdd-77ec8a457a1e" />


8-2. ssh login without password

使用ssh-copy-id user1@VM1的ip 目的是複製公鑰到遠端伺服器

再次輸入ssh user1@VM1的ip，就不需要輸入密碼了

<img width="1180" height="804" alt="image" src="https://github.com/user-attachments/assets/2f3ac59a-594c-4800-a0e7-9a8e2fb2d86c" />


8-3. file transfer by sftp


8-4. file transfer by scp


## 9.screen

9-1. create screen with session name

使用screen [-S session_name]指令 可以順便命名(此處命名為hw)


9-2. create & switch window

建立新視窗:Ctrl + a 然後按 c

Ctrl+a 然後按 '    可以切換 window

Ctrl + a 然後按 "  可以列出所有視窗清單


9-3. deattach & re-attach screen

使用Ctrl + a 然後按 d來離線

會顯示:detached from xxxx.hw

使用screen -r hw可以重新連線


9-4. split vertical & horizontal screen

Ctrl+a 然後按 S     # 建立新區域 (水平分割)
Ctrl+a 然後按 |     # 建立新區域 (垂直分割)


9-5. setup caption & hardstatus*

## 10.vim

10-1. create text file "Hello Vim!"

輸入vim hello.txt來建立一個檔案

按i進入插入模式並輸入：Hello Vim!

按 Esc 回到一般模式並輸入 :wq 存檔並離開


10-2. split vertical & horizontal screen

水平分割：輸入:split hello.txt

<img width="356" height="979" alt="image" src="https://github.com/user-attachments/assets/4cfa6f53-56a7-4c47-be02-42362b35da6c" />

垂直分割：:vsplit hello.txt

<img width="1895" height="982" alt="image" src="https://github.com/user-attachments/assets/f95e5414-664b-46c1-ab66-103e2b6fe2d0" />


10-3. go to normal mode, insert mode, command mode, visual mode

Normal Mode:預設模式，按 Esc 回到一般模式

Insert Mode:按i

Command-Line Mode:按 : 進入

Visual Mode按 v：進入文字選取模式,按 V：選取整行,按 Ctrl-v：選取區塊


10-4. on-line help

在 Vim 中輸入：:help

也能指定特定主題的幫助

:help insert

:help visual

:help write


10-5. compare file

先創立兩個檔案(file1 file2)並用vim編輯

輸入vimdiff file1.txt file2.txt 即可對兩個檔案進行比較

<img width="1896" height="987" alt="image" src="https://github.com/user-attachments/assets/ae04ca7c-c254-4408-8b1d-90d55fa0ed51" />
