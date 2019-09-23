# <strong>SAN Switch</strong>
## 2019/9/23 上午9:49:05

## Link：<a href="#1">SAN</a>;<a href="#2">Fibre Channel定址</a>;<a href="#3">分區(Zoning)</a>;<a href="#4">別名(Alases)</a>;<a href="#5">補充</a>;<a href="#6">Command Line(CLI)未初始化</a>;<a href="#7">CLI已初始化</a>;<a href="#8">其他補充</a>;

### <span id="1">SAN</span>
+ Stroage Attachment Network : 儲存區域網路，儲存設備所構成的區域網路。

### <span id="2">Fibre Channel定址</span>
+ Fibre Channel的介面，HBA，Host Bus Adapter(HBA)。類似Ethernet網路卡的MAC Address，稱為<strong>W</strong>orld <strong>W</strong>ide <strong>N</strong>ame(WWN)
+ WWN使用上有兩種不同意義，World Wide Node Name(WWNN指定設備)，World Wide Port Name(WWPN指定存取port位址)。

### <span id="3">分區(Zoning)</span>
+ Zoning by initiator port
+ 最佳實務的方式是創建分區對每一個各別的initiator ports(通常是主機的一個端口)和一個或多個target ports(目標端口)。

### <span id="4">別名(Alases)</span>
+ 別名將人類可讀的名稱與WWPN相關聯。
+ 別名將允許併入<strong>路徑集</strong>，或將有限數量的路徑分配給單個別名。

## <span id="5">補充</span>
+ 別名將允許併入路徑集，或將有限數量的路徑分配給單個別名。

## <span id="6">Command Line(CLI)未初始化</span>
+ 修改IP位址：ipaddrset
+ IP位址查詢：ipaddrshow
+ 修改主機名稱：switchname
+ 顯示default分區：defzone --show
+ 關閉default分區：defzone --noaccess

## <span id="7">CLI已初始化</span>
+ 檢視交換機本地的名稱服務：nsshow
+ 創建別名/增加成員/移除成員/刪除alias：</br>alicreate "aliName","member[;member]"</br>aliadd "aliName","member[;member]"</br>aliremove "aliName","member[;member]"</br>alidelete "aliName"
+ 創建分區/增加成員(別名)/移除成員(別名)/刪除zone：</br>zonecreate "zoneName","member[;member]"</br>zoneadd "zoneName","member[;member]"</br>zoneremove "zoneName","member[;member]"</br>zonedelete "zoneName"
+ 建議每個zone裡只放一個initiator(主機、Vplex的BE埠等)。
+ 創建設定檔/增加成員(分區)/移除成員(分區)/刪除cfg：</br>cfgcreate "cfgName","member[;member]"</br>cfgadd "cfgName","member[;member]"</br>cfgremove "cfgName","member[;member]"</br>cfgdelete "cfgName","member[;member]"
+ 儲存設定檔：cfgsave
+ 啟動設定檔：cfgenable
- 啟動某個cfg會使其他正被使用cfg停止工作，一個fabric裡同時只能有一個cfg處於工作狀態。

## <span id="8">其他補充</span>
+ 檢視SW健康狀態：switchStatusShow
+ 查看SW基本配置：switchShow
+ 查看端口狀態：portshow portnumber
+ 查看zoning：zoneshow
+ 查看風扇狀態：fanshow
+ 查看電源狀態：psshow
+ 查看溫度：tempshow
+ 查看日誌：errDump、errShow、errClear
+ 檢查韌體版本：firmwareShow
+ 查看FC網路連接所有交換機系統：fabricshow
+ 查看版本：version
+ 更新韌體：firmwaredownload
+ 查看交換機機箱訊息(含序號)：chassisshow
+ 查看授權：licenseshow
+ 備份還原：
