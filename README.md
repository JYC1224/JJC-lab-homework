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

創建文字檔方法見10-1

我們要傳送一名為hello.txt的檔案

使用sftp user1@127.0.0.1

目的是用 SFTP 傳檔工具，登入「user1」帳號在 127.0.0.1（也就是自己的機器）上。

<img width="640" height="159" alt="image" src="https://github.com/user-attachments/assets/b96efd13-dd8f-4cc3-8705-ef9c571c6fb1" />

用put hello.txt把你現在這台電腦上的 hello.txt上傳到遠端使用者 user1 的家目錄

輸入ls後即可看到hello.txt

輸入exit後即可跳出sftp模式

8-4. file transfer by scp

scp hello.txt user2@127.0.0.1:~/.

目的是從目前使用者（本機）將檔案 hello.txt 傳送到同一台機器上 user2 使用者的家目錄 (~) 中。

這樣就完成了

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

輸入vim ~/.screenrc

加入這些設定：

caption always "%{= kw}Window %n: %t"

hardstatus alwayslastline "%{= kG}[%n] %t %H"

儲存後，下次開啟 screen 就會自動載入這些設定。


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


# part2

## 1.process

1-1. explain ps aux command all field

此指令能列出目前系統中所有的行程資訊（process），每一列就是一個行程。

|  欄位名稱   | 說明  |
|  ----  | ----  |
| USER | 該行程的擁有者（哪個使用者啟動的) |
| PID | 行程的唯一 ID（Process ID) |
| %CPU | 行程使用的 CPU 百分比 |
| %MEM |	行程使用的記憶體百分比 |
| VSZ |	虛擬記憶體大小（單位 KB) |
| RSS |	實體記憶體使用量（常駐記憶體，單位 KB) |
| TTY |	終端機類型，若是 ? 表示非終端啟動的行程（如背景服務) |
| STAT |	行程狀態，例如：R（執行中）、S（休眠）、Z（殭屍）等 |
| START |	行程的啟動時間或日期 |
| TIME |	行程使用 CPU 的總時間 |
| COMMAND |	啟動行程時所下的指令與參數 |


1-2. explain kill -l command all signal number

| 編號 | 名稱      | 說明              |
| -- | ------- | --------------- |
| 1  | SIGHUP  | 關閉終端或重新載入設定     |
| 2  | SIGINT  | 中斷（Ctrl + C）    |
| 9  | SIGKILL | 立即強制終止行程（不能被忽略） |
| 15 | SIGTERM | 結束行程（可被攔截）      |
| 18 | SIGCONT | 繼續（如果之前有暫停）     |
| 19 | SIGSTOP | 暫停（不能被攔截）       |

1-3-1. run vi a.txt command to background

你可以輸入：vi a.txt

然後按下：Ctrl + Z

這樣就會暫停 vi 並將它送到背景，變成一個背景作業（Stopped）。

系統會顯示類似：[1]+  Stopped                 vi a.txt

1-3-2. call %1 to frontground

背景作業的編號是 %1，你可以用 fg 指令將它帶回前景執行：

輸入指令fg %1 這樣 vi a.txt 就會重新在畫面上打開，回到你剛剛編輯的狀態。

## 2.hardware

2-1. explain lscpu command all column

使用:lscpu 顯示 CPU 架構與核心的相關資訊

| 欄位名稱                   | 說明                                |
| ---------------------- | --------------------------------- |
| **Architecture**       | 處理器架構（例如 x86\_64 表示 64 位元）        |
| **CPU(s)**             | 總邏輯核心數（含多執行緒）                     |
| **Thread(s) per core** | 每個實體核心有幾個執行緒（HT）                  |
| **Core(s) per socket** | 每個 CPU 插槽的實體核心數                   |
| **Socket(s)**          | 插槽數（即有幾顆實體 CPU）                   |
| **Vendor ID**          | 廠牌（如 GenuineIntel 或 AuthenticAMD） |
| **Model name**         | CPU 的商品名稱（例如 Intel i7-9750H）      |
| **CPU MHz**            | 當前 CPU 頻率                         |
| **Virtualization**     | 是否支援虛擬化（VT-x, AMD-V）              |
| **NUMA node(s)**       | NUMA 架構中的節點數                      |
| **Flags**              | 支援的 CPU 指令集，例如 SSE, AVX 等         |

2-2. explain free command all column

使用free -h來查看記憶體與 swap 的使用情況，加 -h 會顯示為人類可讀格式（如 MB、GB）。

| 欄位名稱           | 說明                   |
| -------------- | -------------------- |
| **total**      | 總記憶體大小               |
| **used**       | 已使用的記憶體（含快取與 buffer） |
| **free**       | 尚未使用的記憶體             |
| **shared**     | 多個行程共用的記憶體區段         |
| **buff/cache** | 作為快取與 buffer 使用的記憶體  |
| **available**  | 真正可用於新行程的記憶體（不會影響系統） |


2-3. explain lsblk command all column

使用lsblk列出所有區塊設備（硬碟、分割區、USB 裝置），並用樹狀圖表示主從關係。

| 欄位名稱           | 說明                                 |
| -------------- | ---------------------------------- |
| **NAME**       | 設備名稱，例如 `sda`、`sda1`（分割區）          |
| **MAJ\:MIN**   | 主/次設備號碼（供系統識別）                     |
| **RM**         | 是否為可移除裝置（如 USB）                    |
| **SIZE**       | 裝置或分割區的大小                          |
| **RO**         | 是否為唯讀裝置                            |
| **TYPE**       | 類型，例如 disk（整顆磁碟）、part（分割區）、rom（光碟） |
| **MOUNTPOINT** | 掛載點位置，例如 `/` 或 `/home`             |

2-4. explain df command all column

使用df -h顯示所有掛載的檔案系統與其磁碟使用情況。

| 欄位名稱           | 說明                               |
| -------------- | -------------------------------- |
| **Filesystem** | 檔案系統的名稱或來源設備，例如 `/dev/sda2`      |
| **Size**       | 該掛載點的總容量                         |
| **Used**       | 已使用的容量                           |
| **Avail**      | 尚可用的容量                           |
| **Use%**       | 已使用的比例                           |
| **Mounted on** | 掛載點（目錄），例如 `/`、`/home`、`/boot` 等 |

2-5-1. use du count file space usage in current directory

使用指令du -sh * 可以計算目前目錄中各檔案/目錄佔用的空間(如下圖)

<img width="173" height="138" alt="image" src="https://github.com/user-attachments/assets/8f71e7e8-1052-4226-83f4-b14255f89d34" />

指令解釋:

| 說明                               |
| -------------------------------- |
| `du`（Disk Usage）會顯示檔案或資料夾的磁碟使用量。 |
| `-s` 表示摘要（summary），只顯示每個項目的總和。   |
| `-h` 表示「人類可讀格式」，例如 KB、MB、GB。     |
| `*` 表示列出目前目錄下所有的檔案與子目錄的磁碟空間使用量。  |

2-5-2. use du sort file size in current directory

使用du -sh * | sort -h 來依檔案大小排序目前目錄下的項目(如下圖)

<img width="177" height="139" alt="image" src="https://github.com/user-attachments/assets/f5acfbd7-a186-40b2-aa4c-47278c16ad62" />

| 組合說明                                               |
| -------------------------------------------------- |
| `du -sh *`：列出目前目錄下每個檔案/資料夾的磁碟使用量（如上）               |
| `sort -h`：根據人類可讀格式進行排序（`-h` 是 human-readable sort） |


2-6-1. explain ip link command all column

使用ip link指令會出現

<img width="861" height="138" alt="image" src="https://github.com/user-attachments/assets/5bee6de9-6c35-4a36-b6f7-bde88c011f3d" />

| 欄位              | 解釋                                 |
| --------------- | ---------------------------------- |
| `1:`            | 介面編號                               |
| `lo:` / `eth0:` | 網路介面名稱（lo 是 loopback, eth0 是實體網卡）  |
| `<FLAGS>`       | 介面狀態（例如 UP 表示啟用中）                  |
| `mtu`           | 最大傳輸單元 (Maximum Transmission Unit) |
| `qdisc`         | 佇列管理器 (排程策略)                       |
| `state`         | 介面狀態，例如 UP, DOWN, UNKNOWN          |
| `link/ether`    | 網卡的 MAC 位址                         |
| `brd`           | 廣播位址 (broadcast address)           |

2-6-2. explain ip address command all column

輸入ip address 來顯示每個介面的 IP 位址資訊

<img width="1019" height="343" alt="image" src="https://github.com/user-attachments/assets/83e318fa-2677-4695-ab63-8fbac8e2f0d3" />

| 欄位                 | 解釋                                 |
| ------------------ | ---------------------------------- |
| `inet`             | IPv4 位址                            |
| `192.168.1.100/24` | IP 位址與子網遮罩                         |
| `brd`              | 廣播位址                               |
| `scope`            | IP 位址的範圍（`global`, `link`, `host`） |
| `dynamic`          | 由 DHCP 自動分配                        |
| `inet6`            | IPv6 位址                            |

2-6-3. explain ip route command all column

輸入ip route會出現

default via 10.0.2.2 dev eth0 proto dhcp

10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15

| 欄位             | 解釋                  |
| -------------- | ------------------- |
| `default`      | 預設路由                |
| `via`          | 下一跳（gateway）        |
| `dev`          | 哪個介面發送封包（如 eth0）    |
| `proto kernel` | 路由來源（kernel 是自動加入）  |
| `scope link`   | 路由範圍（這裡是 link，表示直連） |
| `src`          | 本機 IP 位址            |

2-6-4. explain ip neighbor command all column'

輸入ip neighbor

10.0.2.2 dev eth0 lladdr 52:55:0a:00:02:02 REACHABLE
fe80::2 dev eth0 lladdr 52:56:00:00:00:02 router STALE

| 欄位            | 解釋                                   |
| ------------- | ------------------------------------ |
| `192.168.1.1` | 鄰居主機 IP 位址（通常是 Gateway）              |
| `dev eth0`    | 本機發送封包的介面                            |
| `lladdr`      | 鄰居的 MAC 位址                           |
| `REACHABLE`   | 鄰居狀態（如：REACHABLE、STALE、DELAY、FAILED） |

2-7-1. use dmidecode show bios vendor

輸入dmidecode -t bios 會出現(如下圖)

<img width="629" height="418" alt="image" src="https://github.com/user-attachments/assets/ef1db9fa-3948-4a6d-9dc1-bad6ad5d6dff" />

| 欄位             | 說明                     |
| -------------- | ---------------------- |
| `Vendor`       | BIOS 製造商，如 AMI、innotek GmbH |
| `Version`      | BIOS 版本號               |
| `Release Date` | BIOS 發布日期              |
| `ROM Size`     | BIOS ROM 大小            |

2-7-2. use dmidecode show processor

輸入dmidecode -t processor 會出現下圖

<img width="312" height="65" alt="image" src="https://github.com/user-attachments/assets/d05ed020-d716-42a2-a6a7-87a8c9cbfdcc" />

理論上不該是這樣 這是我使用虛擬機所造成的結果

實際應該會出現以下資訊

| 欄位                   | 說明                       |
| -------------------- | ------------------------ |
| `Socket Designation` | 插槽位置                     |
| `Type`               | 類型（例如：Central Processor） |
| `Family`             | 處理器系列（例如：Core i7）        |
| `Manufacturer`       | 製造商（如 Intel、AMD）         |
| `Version`            | 詳細型號與時脈資訊                |

2-7-3. use dmidecode show memory/ram

輸入dmidecode -t memory 會出現與2-7-2一樣的圖

實際應該會出現以下資訊

| 欄位              | 說明                |
| --------------- | ----------------- |
| `Size`          | 單條記憶體容量           |
| `Form Factor`   | 實體類型（如：DIMM）      |
| `Locator`       | 插槽名稱              |
| `Type`          | 記憶體類型（如：DDR4）     |
| `Speed`         | 記憶體速度（如 2400 MHz） |
| `Manufacturer`  | 廠牌名稱（如 Kingston）  |
| `Serial Number` | 製造序號              |
| `Part Number`   | 零件號碼              |

2-7-4. use dmidecode show harddisk



2-7-5. use dmidecode show baseboard

使用dmidecode -t baseboard 會看到主機板資訊(如下圖)

<img width="425" height="241" alt="image" src="https://github.com/user-attachments/assets/4f78e39e-246e-4c73-9154-e47712ae71e3" />

2-7-6. use dmidecode show serial

使用dmidecode -t system 會看到(如下圖)

<img width="701" height="182" alt="image" src="https://github.com/user-attachments/assets/15663b5b-c64d-4df4-92da-43bfe92a5430" />


## 3.moniter

3-1-1. install procps

輸入指令sudo zypper install procps

跑出'procps' is already installed 代表我們已經安裝了

3-1-2. explain top command all column

輸入top後會出現以下畫面

<img width="1005" height="476" alt="image" src="https://github.com/user-attachments/assets/b798ef7b-37fe-4ad0-a7aa-a10432577efa" />

你會看到上下分為兩區 首先解釋上區

| 欄位                    | 說明                                        |
| --------------------- | ----------------------------------------- |
| `uptime`              | 系統開機時間                                    |
| `users`               | 登入使用者人數                                   |
| `load average`        | 系統平均負載（1分鐘、5分鐘、15分鐘）                      |
| `Tasks`               | 任務總數                                      |
| `running`             | 執行中的行程數                                   |
| `sleeping`            | 睡眠中的行程數                                   |
| `stopped`             | 停止的行程                                     |
| `zombie`              | 殭屍行程（已結束但未清除）                             |
| `%Cpu(s)`             | 各類 CPU 使用情況（us:使用者、sy:系統、id:閒置、wa:等待 I/O） |
| `KiB Mem` / `GiB Mem` | 實體記憶體使用情況（total, used, free）              |
| `Swap`                | 交換記憶體使用情況                                 |

下區:

| 欄位        | 說明                    |
| --------- | --------------------- |
| `PID`     | 行程 ID                 |
| `USER`    | 所有者帳號                 |
| `%CPU`    | CPU 使用率               |
| `%MEM`    | 記憶體使用率                |
| `VIRT`    | 虛擬記憶體使用量              |
| `RES`     | 常駐記憶體（實際用到的 RAM）      |
| `SHR`     | 共用記憶體大小               |
| `S`       | 行程狀態（R:執行中、S:睡眠、Z:殭屍） |
| `TIME+`   | 使用 CPU 累積時間           |
| `COMMAND` | 啟動的命令                 |

3-1-3. change top setting and save to config

進入top後使用以下快捷鍵可以開始設定

| 快捷鍵   | 作用                             |
| ----- | ------------------------------ |
| `f`   | 編輯顯示欄位                         |
| `↑ ↓` | 選擇欄位，按 `d` 開關是否顯示              |
| `o`   | 改變排序欄位                         |
| `q`   | 離開欄位編輯                         |
| `z`   | 開/關 彩色顯示                       |
| `M`   | 按記憶體使用率排序                      |
| `P`   | 按 CPU 使用率排序                    |
| `W`   | **儲存設定為預設**（寫入 `$HOME/.toprc`） |

3-1-4. use top batch mode then save n time and delay m sec

使用top -b -n -d > output.txt

| 參數   | 說明                       |
| ---- | ------------------------ |
| `-b` | batch mode（非互動模式，適合用來儲存） |
| `-n` | 重複次數                     |
| `-d` | 每次間隔時間（秒）                |
| `>`  | 將輸出存到檔案                  |

3-2-1. install iotop

使用 zypper in iotop 來安裝iotop

3-2-2. explain iotop command all column

使用指令iotop會出現(如下圖)

<img width="1147" height="401" alt="image" src="https://github.com/user-attachments/assets/2e1b5c56-9dcf-465e-866b-3cb14e8b301b" />

| 欄位             | 說明                       |
| -------------- | ------------------------ |
| **TID**        | Thread ID（行程或執行緒的 ID）    |
| **PRIO**       | 優先順序（nice 值）             |
| **USER**       | 該行程的使用者                  |
| **DISK READ**  | 磁碟讀取速度（KB/s、MB/s）        |
| **DISK WRITE** | 磁碟寫入速度                   |
| **SWAPIN**     | 記憶體交換進入比率 (%)            |
| **IO**         | I/O 等待時間百分比（高代表 I/O 等很久） |
| **COMMAND**    | 該行程的啟動指令                 |

3-2-3. use iotop batch mode then save n time and delay m sec

使用指令iotop -b -n <次數N> -d <秒數M> > iotop_output.txt

| 參數   | 說明                |
| ---- | ----------------- |
| `-b` | 啟用 batch 模式（非互動式） |
| `-n` | 執行次數              |
| `-d` | 間隔秒數              |
| `>`  | 將輸出存到檔案           |

例如:要執行 10 次，每次間隔 1 秒，結果輸出到 iotop_log.txt：

就要使用iotop -b -n 10 -d 1 > iotop_log.txt

再使用less iotop_log.txt看結果 就會出現下圖:

<img width="1100" height="302" alt="image" src="https://github.com/user-attachments/assets/bfea5f77-4f05-40e4-b0dc-42fd280576a6" />

3-3-1. install iftop

使用指令zypper in iftop 來安裝iftop

3-3-2. explain iftop command all column

使用指令iftop -i eth0 會出現以下畫面

<img width="809" height="515" alt="image" src="https://github.com/user-attachments/assets/ac927a23-29da-4a69-b0d8-cdad7e629bb6" />

| 區域          | 說明                             |
| ----------- | ------------------------------ |
| 左邊 IP       | Host A 和 Host B（正在通訊的來源與目的主機）  |
| `=>` / `<=` | 代表**傳送（Tx）** 或 **接收（Rx）** 資料方向 |
| 右邊三欄        | 分別是最近 2 秒、10 秒、40 秒內的平均流量      |
| 最上方橫列       | 顯示總體頻寬使用（TX、RX、TOTAL）          |
| 最底部橫列       | 可顯示過濾器、排序、總結等資訊                |

3-4-1. install sysstat

使用指令zypper in sysstat來安裝

3-4-2. enable and run sysstat service

首先使用systemctl enable sysstat來啟用

再使用systemctl start sysstat來啟動

使用systemctl status sysstat驗證是否啟動成功

如果看到：Active: active (running)即表示成功

3-4-3. use sar show cpu usage

使用指令sar -u 1 5                   # 每 1 秒報告一次 CPU 使用率，共 5 次 (如下圖:)

<img width="800" height="185" alt="image" src="https://github.com/user-attachments/assets/e80bdd40-1396-4334-9fb5-b79f75c9678f" />

| 欄位        | 意義                 |
| --------- | ------------------ |
| `%user`   | 使用者程式佔用的 CPU 時間    |
| `%system` | 核心程式佔用的 CPU 時間     |
| `%iowait` | 等待 I/O 時間（越高可能有瓶頸） |
| `%idle`   | 閒置時間（越高越好）         |

3-4-4. use sar show memory usage

使用指令sar -r 1 5 (如下圖:)

<img width="1215" height="178" alt="image" src="https://github.com/user-attachments/assets/38a8a66b-6429-4677-a0ca-62989f70bf35" />

| 欄位          | 意義                      |
| ----------- | ----------------------- |
| `kbmemfree` | 剩餘記憶體 (kB)              |
| `kbmemused` | 已用記憶體 (kB)              |
| `%memused`  | 記憶體使用百分比                |
| `kbbuffers` | I/O 缓存                  |
| `kbcached`  | Page cache（已使用但可回收的記憶體） |

3-4-5. use sar show network interface stats

使用指令sar -n DEV 1 5 (如下圖:)

<img width="1021" height="502" alt="image" src="https://github.com/user-attachments/assets/0014cc5e-506d-4cd9-91ba-a7d3b63bd2bc" />

| 欄位        | 說明                      |
| --------- | ----------------------- |
| `rxpck/s` | 每秒接收封包數                 |
| `txpck/s` | 每秒傳送封包數                 |
| `rxkB/s`  | 每秒接收資料量（kB）             |
| `txkB/s`  | 每秒傳送資料量（kB）             |
| `iface`   | 網路介面名稱（如 eth0, ens33 等） |

3-4-6. use sar run queue from the saved data file

使用指令sar -u -f /var/log/sa/sa05

錯誤


3-4-7. explain vmstat command all column

使用指令vmstat 2 5          # 每 2 秒報告一次，共 5 次 (如下圖:)

<img width="826" height="135" alt="image" src="https://github.com/user-attachments/assets/7fde77f0-a799-4d23-a835-2a5dc086900c" />

| 欄位  | 說明                                     |
| --- | -------------------------------------- |
| `r` | run queue 中等待 CPU 的行程數（大於 0 表示 CPU 繁忙） |
| `b` | 進入 uninterruptible sleep 的行程（通常在等 I/O） |

| 欄位      | 說明                         |
| ------- | -------------------------- |
| `swpd`  | 使用的 swap 空間大小              |
| `free`  | 可用記憶體                      |
| `buff`  | buffer 快取（通常給 block I/O 用） |
| `cache` | page cache 記憶體             |

| 欄位   | 說明                            |
| ---- | ----------------------------- |
| `si` | 每秒從 swap 区交換進記憶體的量            |
| `so` | 每秒從記憶體交換到 swap 的量（持續大代表記憶體不足） |

| 欄位   | 說明                      |
| ---- | ----------------------- |
| `bi` | 每秒從 block device 讀入的區塊數 |
| `bo` | 每秒寫入到 block device 的區塊數 |

| 欄位   | 說明                        |
| ---- | ------------------------- |
| `in` | 每秒中斷數（interrupt）          |
| `cs` | 每秒上下文切換次數（context switch） |

| 欄位   | 說明                          |
| ---- | --------------------------- |
| `us` | user space CPU 使用率          |
| `sy` | kernel space CPU 使用率        |
| `id` | CPU 閒置率                     |
| `wa` | CPU 等待 I/O 的時間              |
| `st` | 虛擬化系統中被偷走的 CPU（stolen time） |

3-4-8. explain iostat command all column

使用指令iostat 2 3       # 每 2 秒報告一次，共 3 次 (如下圖:)

<img width="791" height="442" alt="image" src="https://github.com/user-attachments/assets/7f902d5e-eeaa-453f-a01e-b53a5f47ad45" />

| 欄位        | 說明                                |
| --------- | --------------------------------- |
| `%user`   | 使用者空間佔用的 CPU 百分比（執行應用程式）          |
| `%nice`   | nice 值不為 0 的程序使用 CPU 的百分比（低優先權程式） |
| `%system` | 核心空間佔用的 CPU 百分比（作業系統）             |
| `%iowait` | CPU 等待 I/O 時間的百分比（表示磁碟慢或忙）        |
| `%steal`  | 被其他 VM 搶走的 CPU 百分比（虛擬化環境）         |
| `%idle`   | CPU 閒置百分比（越高代表系統越閒）               |

| 欄位             | 說明                  |
| -------------- | ------------------- |
| **Device**     | 裝置名稱（例如 `sda`）      |
| **tps**        | 每秒傳輸次數（包括讀寫），越高表示越忙 |
| **kB\_read/s** | 每秒讀取資料量（KB）         |
| **kB\_wrtn/s** | 每秒寫入資料量（KB）         |
| **kB\_read**   | 自開機以來總共讀取多少 KB      |
| **kB\_wrtn**   | 自開機以來總共寫入多少 KB      |

3-4-9. explain mpstat command all column

使用指令mpstat -P ALL 1 5  # 每 1 秒報告所有核心的 CPU 使用率，共 5 次

<img width="971" height="640" alt="image" src="https://github.com/user-attachments/assets/5aa31570-72fb-420b-b391-0c32473beb39" />

| 欄位        | 說明                |
| --------- | ----------------- |
| `%usr`    | user space 使用率    |
| `%nice`   | nice 值較高的行程所用 CPU |
| `%sys`    | kernel 使用率        |
| `%iowait` | 等待 I/O 的 CPU 時間   |
| `%irq`    | 處理硬體中斷            |
| `%soft`   | 處理軟體中斷            |
| `%steal`  | 虛擬化時被其他 VM 搶走的時間  |
| `%idle`   | 閒置時間              |

3-4-10. explain pidstat command all column

使用指令pidstat -u -r -d 1 5 每秒列出所有行程的 CPU、記憶體與 I/O 狀況，共 5 次。

<img width="1003" height="965" alt="image" src="https://github.com/user-attachments/assets/d059b548-a469-4515-924f-d1815400a653" />

| 欄位                    | 說明                    |
| --------------------- | --------------------- |
| `UID`                 | 該行程的使用者               |
| `PID`                 | 行程 ID                 |
| `%usr / %system`      | user/kernel CPU 使用率   |
| `%CPU`                | 總 CPU 使用率             |
| `minflt/s`            | 次要 page fault（不需磁碟存取） |
| `majflt/s`            | 主要 page fault（需存取磁碟）  |
| `VSZ`                 | 虛擬記憶體大小（KB）           |
| `RSS`                 | 實體記憶體大小（KB）           |
| `kB_rd/s` / `kB_wr/s` | 每秒讀寫磁碟的 KB 數          |
| `Command`             | 行程名稱                  |

# 4.network troubleshoot tool

4-1-1. install iputils

使用指令zypper install iputils 來安裝

4-1-2. use ping to check connect 8.8.8.8

使用ping 8.8.8.8

有一直跑出來就算成功

4-1-3. install traceroute

使用指令zypper install traceroute

4-1-4. use traceroute / tracepath

使用指令traceroute google.com

不知為何一堆***

<img width="717" height="618" alt="image" src="https://github.com/user-attachments/assets/084a2cc8-dfa1-43ba-9ae0-a027405ccaf8" />

使用指令tracepath google.com

不知為何一堆no reply

<img width="668" height="682" alt="image" src="https://github.com/user-attachments/assets/63e03985-15ca-4351-b47f-5a05676db391" />

4-2-1. use ip turn on/off nic

使用指令ip link set eth0 down 來關閉網卡（停用 NIC）

使用指令p link set eth0 up 來開啟網卡（啟用 NIC)

關閉網卡後putty直接連不上

4-2-2. use ip to set static ip address

使用指令ip addr add 192.168.1.100/24 dev eth0

4-2-3. use ip to show current ip address

使用 ip a

4-2-4. use ip to set router

??????????ip route add default via 192.168.56.1

4-2-5. use ip to show current router

使用指令ip route show (如下圖:)

<img width="521" height="64" alt="image" src="https://github.com/user-attachments/assets/14f6da21-fe45-4865-9ff7-7bbb3d95cee1" />

4-3. use ss to check port

使用指令ss -tunlpa       # 查看活躍的 TCP/UDP 連線、監聽埠、數字 IP、行程、所有連線

<img width="1242" height="181" alt="image" src="https://github.com/user-attachments/assets/5abaf720-4e43-4754-9197-9ee9aa299b05" />

4-4-1. use dig resolve www.google.com

使用dig www.google.com

<img width="548" height="321" alt="image" src="https://github.com/user-attachments/assets/d9b1ddac-0588-4df3-babf-78c2d8747074" />

4-4-2. use nslockup resolve www.google.com

使用指令nslookup www.google.com

<img width="261" height="144" alt="image" src="https://github.com/user-attachments/assets/fe2a5251-3daa-45d6-a5a4-c69a6bcd753c" />


4-5-1. install netcat-openbsd

使用zypper install netcat-openbsd 來安裝

4-5-2. use nc to check port

使用指令nc -vz www.google.com 80

<img width="467" height="32" alt="image" src="https://github.com/user-attachments/assets/834f642f-0e11-4589-996c-e4d5c5869213" />

# 5.ssh

5-1-1. install openssh-server

使用 zypper install openssh 來安裝

5-1-2. enable and run sshd

依序輸入以下指令

systemctl enable sshd      # 開機自動啟動

systemctl start sshd       # 立即啟動

systemctl status sshd      # 查看狀態（確認有沒有 active (running)）

<img width="1165" height="354" alt="image" src="https://github.com/user-attachments/assets/2f275a4e-6e74-481f-98d4-68ad8982c723" />

5-1-3. setup firewall to allow sshd

依序輸入圖片中的指令

<img width="412" height="112" alt="image" src="https://github.com/user-attachments/assets/3f33f955-0084-4495-a5d2-6b6744f0d4c5" />

5-1-4. check ssh port

5-2-1. setup sshd only authentication (private key /no password) login

使用指令 ssh-keygen -t rsa -b 4096

並使用ssh-copy-id username@server_ip_address 將公鑰上傳到伺服器

username為 你在 SUSE 伺服器上的帳號名稱
server_ip_address為 你的 SUSE 虛擬機的 IP 位址

接下來在伺服器上修改 sshd_config 檔案 使用vi /etc/ssh/sshd_config

找到並修改以下兩行（如果前面有 # 符號，請將其移除）：

PubkeyAuthentication yes

PasswordAuthentication no

接著儲存並退出

5-2-2. setup sshd disble root login

使用vi /etc/ssh/sshd_config 進行編輯

找到 PermitRootLogin 這行，將其設定為 no。

5-2-3. setup sshd allow user

使用vi /etc/ssh/sshd_config 進行編輯

在檔案的最後，新增 AllowUsers 指令，並列出所有允許透過 SSH 登入的使用者，用空格隔開。

我使用AllowUsers user1 user2

現在，只有 user1 和 user2 兩個使用者可以透過 SSH 登入，其他帳號都會被拒絕

5-2-4. setup sshd deny user

使用vi /etc/ssh/sshd_config 進行編輯

使用DenyUsers user1 user2

這條指令會阻止 user1 和 user2 兩個帳號透過 SSH 登入

5-2-5. setup sshd allow & deny user at same time

AllowUsers的優先級更高，若同時使用會使得 DenyUsers 失去作用

5-3-1. setup ssh config with alias hostname and username

使用vi ~/.ssh/config來編輯 ~/.ssh/config 檔案

並在文件中加入以下內容，為你的虛擬機設定一個別名。

Host test #設定別名
  HostName 10.0.2.15  # 替換成你的 SUSE 虛擬機 IP
  
  User user1             # 替換成你的使用者名稱
  
  Port 22              # 替換成你在 sshd_config 中設定的埠號
  
  IdentityFile ~/.ssh/id_rsa # 指定金鑰檔案路徑

使用ssh VM名稱 SSH 客戶端就會自動使用你設定的所有參數進行連線

輸入密碼即可

5-3-2. use ssh with other port

若要手動指定埠號 使用ssh username@server_ip_address -p 22

如:ssh user1@10.0.2.15 -p 22

之後輸入密碼即可

5-3-3. use ssh execute command (active mode)

使用ssh <user>@<host> "<command>"

<host>:遠端主機可以是同一台 VM 的另一個 user或另一台 VM兩種都可以，但教學通常是用後者

<command>:command為要執行的命令用單引號包起來，這是你希望在遠端機器上運行的指令。

常見的有

| 指令 | 作用 |
| :--- | :--- |
| `hostname` | 顯示伺服器的名稱。 |
| `date` | 顯示伺服器的日期和時間。 |
| `uptime` | 顯示伺服器已經開機多久了。 |
| `ls -l` | 列出伺服器家目錄下的檔案列表。 |
| `whoami` | 顯示你目前在伺服器上使用的使用者名稱。 |

5-4-1. use sftp resume file

# 6.ntp
6-1-1. install chrony

使用指令zypper install chrony來安裝

6-1-2. enable and run chronyd

使用指令systemctl start chronyd來啟動chronyd

也可以用systemctl enable chronyd設定開機自啟動

使用systemctl status chronyd檢查服務狀態

看到 Active: active (running) 的訊息，這表示服務已成功啟動

6-1-3. setup firewall to allow ntp

使用指令firewall-cmd --permanent --add-service=ntp來永久允許 NTP 服務(收到success回覆)

使用指令firewall-cmd --reload來重新載入防火牆設定(收到success回覆)

使用指令firewall-cmd --list-services來驗證是否有開放

系統回覆dhcpv6-client ntp ssh 確認輸出裡有 ntp 表示成功。

6-1-4. check ntp port

使用指令ss -uapn | grep chronyd

-u: 顯示 UDP 連線

-a: 顯示所有連線

-p: 顯示使用該連線的程式名稱

-n: 以數字格式顯示埠號

會出現<img width="931" height="58" alt="image" src="https://github.com/user-attachments/assets/b9879111-f185-4c27-85e1-628fcbfc925f" />

顯示 chronyd 正在監聽 UDP 323 埠

6-2-1. setup chronyd ntp server (time.google.com, time.hinet.net, time.iis.sinica.edu.tw)

使用指令vi /etc/chrony.conf來編輯 chrony.conf 檔案

找到文件中所有以 pool 或 server 開頭的行，並在它們前面加上 # 來註解掉 

然後，將你想要使用的伺服器新增到文件中 如以下

# Use public NTP servers from Taiwan
server time.google.com iburst
server time.hinet.net iburst
server time.iis.sinica.edu.tw iburst

# The default pool line should be commented out or removed
# pool 0.pool.ntp.org iburst

# Ensure this line exists to save the clock drift rate
driftfile /var/lib/chrony/drift

????????????????????????????????????

6-2-2. setup chronyd ntp server allow subnet host

使用指令vi /etc/chrony.conf進行編輯

在檔案裡加入 允許哪些客戶端可以用你的機器做NTP校正時間

允許 192.168.1.0/24 子網的所有主機來連線:allow 192.168.1.0/24     或者只允許單一主機allow 192.168.1.50

??????????????????????????

6-2-3. setup chronyd ntp server deny all host

使用指令vi /etc/chrony.conf進行編輯

使用deny all 或 deny 0.0.0.0/0 來拒絕所有連線

????????如何驗證

6-3-1. use chronyc show time sources

使用指令chronyc sources查看 chronyd 目前正在與哪些時間伺服器同步，以及它們的同步狀態

<img width="797" height="180" alt="image" src="https://github.com/user-attachments/assets/a98bcacf-626d-4247-913a-c23653a0bd5c" />

輸出解釋

| 欄位 / 符號 | 說明                                                               |
| :---------- | :----------------------------------------------------------------- |
| **`*` (星號)** | 你的系統目前正在與此時間源同步。這是主要的時間來源。               |
| **`^` (上標)** | 此時間源是**候選者**，可用於同步，但目前不是主要來源。             |
| **`?`** | 連線有問題，可能無法取得時間數據。                                 |
| `Name/IP address`      | 時間伺服器的名稱或 IP 位址。                                       |
| `Stratum`     | 伺服器的**層級**。數字越低，時間源越精確。                        |
| `Poll`        | 下一次向該伺服器發送請求的**時間間隔**（單位：秒）。              |
| `Reach`       | 成功從此時間源接收到的數據包次數。最大值是 377。                 |
| `LastRx`      | 上次從此時間源收到數據的時間間隔（單位：秒）。                    |
| `Last`      | 你的系統時鐘與此時間源的**時間差異**。單位是毫秒（ms），越接近零越好。 |

6-3-2. use chronyc synchronize with time source

使用指令chronyc makestep強制 chronyd 立刻把系統時鐘跟時間來源對齊

系統回應200 OK 也就是 chronyd 的回應碼，表示指令已經成功執行

6-3-3. use chronyc show how far the system clock

使用指令chronyc tracking了解系統時鐘與精確時間源之間的詳細差異(如下圖:)

<img width="551" height="280" alt="image" src="https://github.com/user-attachments/assets/0d980023-aef2-419e-8552-40f846d836d8" />

| 欄位名稱                | 範例值                           | 解釋                                                                       |
| ------------------- | ----------------------------- | ------------------------------------------------------------------------ |
| **Reference ID**    | `D8EF2308 (time3.google.com)` | 當前同步的上游時間伺服器 ID 與名稱。這裡代表正在跟 Google 的 NTP 伺服器同步。                          |
| **Stratum**         | `2`                           | 時間源的層級。Stratum 1 = 直接連接原子鐘/GPS，Stratum 2 = 向 Stratum 1 同步，以此類推。值越小精確度越高。 |
| **Ref time (UTC)**  | `Tue Aug 19 06:38:26 2025`    | 最後一次從時間源接收到的時間（UTC）。                                                     |
| **System time**     | `0.000001013 seconds fast`    | 系統時間比 NTP 標準時間快了 \~1 微秒（非常準）。                                            |
| **Last offset**     | `+0.000306891 seconds`        | 最近一次校正時，測得的時間差（約 0.3 毫秒）。                                                |
| **RMS offset**      | `0.000417445 seconds`         | 時鐘偏差的「均方根值」，反映平均誤差（約 0.4 毫秒）。                                            |
| **Frequency**       | `10.896 ppm slow`             | 系統時鐘的頻率偏差，每秒慢 \~10.896 微秒。chrony 會調整它來修正漂移。                              |
| **Residual freq**   | `+0.037 ppm`                  | 剩餘尚未完全修正的頻率誤差（幾乎可以忽略）。                                                   |
| **Skew**            | `0.686 ppm`                   | 頻率調整值的不確定性範圍，表示 chrony 還有多少「信心」需要觀察時間源來進一步修正。                            |
| **Root delay**      | `0.006741246 seconds`         | 系統到時間源的網路往返延遲（約 6.7 毫秒）。                                                 |
| **Root dispersion** | `0.001524317 seconds`         | 時鐘不確定度（約 1.5 毫秒），隨時間會慢慢增加直到下一次校正。                                        |
| **Update interval** | `516.5 seconds`               | 兩次校正的間隔時間（這裡大約每 8.6 分鐘會向 NTP 伺服器同步一次）。                                   |
| **Leap status**     | `Normal`                      | 閏秒狀態。`Normal` 代表目前沒有即將插入/刪除閏秒。                                           |

# 7.nis
7-1-1. run rpcinfo

若rpcbind 服務沒有運行則需要先啟動 rpcbind 服務

使用指令systemctl start rpcbind來啟動服務

也能使用指令systemctl enable rpcbind設定開機自啟動

完成後使用指令rpcinfo -p(如下圖:)

<img width="414" height="162" alt="image" src="https://github.com/user-attachments/assets/164bf950-e9cd-446c-b5f6-f9e69151a7ab" />

列出了 RPC 服務的程式編號、版本、協議和埠號

7-1-2. install ypserv

使用指令zypper install ypserv來安裝

7-1-3. enable and run ypserv service

使用指令systemctl start ypserv

使用指令systemctl enable ypserv

使用指令systemctl status ypserv

這樣 ypserv 就會作為 NIS Server 常駐在你的 VM 上

7-1-4. setup firewall to allow nis

NIS 需要 RPC (rpcbind) 和 NIS 本身 兩個服務才能運作 

使用指令sudo firewall-cmd --permanent --add-service=rpc-bind

使用指令sudo firewall-cmd --permanent --add-service=nis--------->失敗

7-1-5. check nis port

使用指令rpcinfo -p | grep ypserv (如下圖)

<img width="333" height="82" alt="image" src="https://github.com/user-attachments/assets/190e1471-62cd-47f3-b0b7-333c3eb1e200" />

這代表 ypserv 用 UDP 607、TCP 607 來監聽

7-2-1. enable and run yppasswdd service

使用指令sudo systemctl start yppasswdd來啟動服務

使用指令sudo systemctl enable yppasswdd來設定開機自動啟動

使用指令sudo systemctl status yppasswdd來檢查狀態

看到active(running)表示成功

7-2-2. setup firewall to allow nis

????????????????不知道是不是這樣

開放 port

sudo firewall-cmd --permanent --add-port=607/tcp

sudo firewall-cmd --permanent --add-port=607/udp

sudo firewall-cmd --reload

7-2-3. check nis port

使用指令rpcinfo -p(出現下圖)

<img width="410" height="262" alt="image" src="https://github.com/user-attachments/assets/45466eaf-1e99-4784-94c2-9d3efbfa1a8e" />

| 服務                                  | Program | Port          | 說明                                                                 |
| ----------------------------------- | ------- | ------------- | ------------------------------------------------------------------ |
| **portmapper (rpcbind)**            | 100000  | 111 (TCP/UDP) | **RPC 的「黃頁」**，負責告訴 client 各 RPC 服務目前在哪個 port 上。所有 RPC client 會先問它。 |
| **ypserv** (NIS Server)             | 100004  | 607 (TCP/UDP) | **NIS 主伺服器**，提供帳號、群組等 NIS 資料庫查詢。                                   |
| **yppasswdd** (NIS Password Daemon) | 100009  | 613 (TCP/UDP) | **NIS 密碼修改服務**，讓用戶能透過 `yppasswd` 指令更新自己的密碼。                        |

7-3-1. run getent?????????

使用指令getent passwd

7-3-2. run ypcat

使用指令ypcat passwd????????

7-3-3. run ypwhich????????

使用指令ypwhich

# 8.nfs
8-1-1. run rpcinfo

使用指令sudo rpcinfo -p來列出目前伺服器上所有的 RPC 服務

8-1-2. install nfs-server

使用指令zypper install nfs-kernel-server來安裝

8-1-3. enable and run nfs-server service

使用指令sudo systemctl start nfs-server

使用指令sudo systemctl enable nfs-server

使用指令sudo systemctl status nfs-server

看到active (exited)表示運行中

8-1-4. setup firewall to allow nfs

NFS 需要開啟以下服務：nfs → NFS 本身/rpc-bind → RPC 綁定器 (portmapper, port 111)/mountd → 負責處理掛載請求

使用指令firewall-cmd --permanent --add-service=nfs新增服務

使用指令firewall-cmd --permanent --add-service=rpc-bind新增服務

使用指令firewall-cmd --permanent --add-service=mountd新增服務

使用指令firewall-cmd --reload重新載入防火牆

使用指令firewall-cmd --list-services確認服務有加進去

8-1-5. check nfs port

使用指令rpcinfo -p看系統有沒有在監聽

nfs 服務會固定在 2049 埠，mountd 則可能使用不同的埠號，只要它們出現在列表中，就表示防火牆已為它們開啟了正確的埠，NFS 服務也已準備就緒
