---
date : 2023-06-05 10:42 Mon
alias : []
---

---

## Overview

### Non-volatile

+ hard disk drives (HDDs)
	+ 利用帶有磁性物質的磁盤旋轉，再利用讀寫頭改變磁盤上的極性用以紀錄資料
	+ 因為利用讀寫頭及磁盤，是精密的機械操作，對於震動、搖晃敏感，且會有磁性物質隨時間失效的問題
+ nonvolatile memory (NVM)
	+ `solid-state disks(SSDs)``
		+ 主要由控制器、FLASH Memory組成
		+ Read in page , write in block (2^n pages)
		+ garbage collection
		+ overprovisioning
		+ 每個cell都有一定的生命週期，需要用`wear leveling`的方式來平均讀寫各個cell
	+ 比HDD更reliable
	+ 較貴
	+ 空間較少，但也較快
	+ 使用上有次數限制
		+ Can only be erased a limited number of times before worn out – ~ 100,000
	+ 生命週期取決於DWPD(drive writes per day)
	
+ Transfer rate
	+ data flow between drive and computer
+ Positioning time(random-access time)
	+ Time to move disk from arm to desired cylinder(`seek time`)
	+ Time for desired sector to rotate under the disk head(`rotational latency`)

### Volatile

+ DRAM
+ RAM drives
	+ as raw block devices, `commonly file system formatted`
	+ under user control
	+ 作為存取速度快的storage

## Disk Attachment

+ Disk drives 可以被視為一個`logical blocks`的一維陣列，block是transfer的最小單位
+ Host-attached storage藉由I/O ports 來跟I/O busses溝通
	+ advanced technology attachment(ATA) 
	+ serial ATA (SATA), eSATA 
	+ serial attached SCSI(SAS)
	+ universal serial bus (USB)
+ 因為NVM比HDD快
	+ 新的NVM介面 -> NVM express

### HDD Scheduling

+ OS 負責
	+ Minimize seek time
	+ Bandwidth is the total number of bytes transfered 
+ OS維護每個disk或device的queue
	+ 當有閒置的disk時就可以馬上做I/O request
	+ busy disk must queue
+ Disk drive head scheduling for queue management
	+ 過去由作業系統負責
	+ 現在則被整合進storage devicce, controller中

## Disk Scheduling

+ Drive有一個buffer且可以管理一個I/O request的queue

+ FCFS
	+ 先到先做，但因為每個request之間可能距離很大，造成seek time長
+ SSTF
	+ 選一個離現在讀寫頭最近的位置做讀取
	+ 可能會starvation
+ SCAN
	+ 從disk中的一端開始，延伸讀寫頭到另外一端，並服務路徑上經過的cylinder，直到所有queue中的request都被滿足
	+ 剛被讀取過的cylinder若要被重新存取就必須要再等讀寫頭回程
	+ 儘管queue中沒有需要的cylinder，讀寫頭仍會經過
+ C-SCAN
	+ 一樣是從disk的其中一端移動到另一端，但當讀寫頭到達另一端點時，會直接回到disk最一開始的地方(cylinder 0)，而且在回程的路上不會進行讀寫。
	+ 需要比SCAN做更多的movement

### Selecting a Dis-Scheduling Algorithm

+ SSTF is common and `has natural appeal`
+ SCAN跟C-SCAN更適合heavy load on disk的系統，比較不會有starvation
+ Linux 利用`deadline scheduler`，來避免starvation
	+ PPT 16

## NVM Scheduling

+ No disk heads or rotational latency but still room for optimization
	+ 讀取的時間是統一的，寫入的時間是不統一的
	+ 
+ PPT 17

## Error Detection and Correction

+ Error Dectection 
	+ 如果檢查到error ，可以中斷operation
	+ 通常藉由parity bit來執行
	+ Checksum
		+ 用mod來運算、儲存及比較
	+ Cyclic
	+ Redundancy check(CRC)
		+ 用hash function來check mutiple-bit的錯誤
+ Error-correction code (ECC)

*parity bit : 指用來作為檢查作用的二進制bit*


## Storage Device Management

+ Low-level formatting, or physical formatting - 將磁碟分為controller可以進行讀寫的磁區
	+ 磁區可以包含data, ECC
	+ 通常是512byte
+ Partition
	+ 將disk分成cylinder group，並將他視為logical disk
	+ `Logical formatting` -> make a file system
	+ 為了提高存取效率
		+ file system通常會把block group成cluster
			+ Disk I/O done in blocks
			+ File I/O done in cluster
+ Root partition
	+ 包含OS系統
	+ 在開機時被掛載

## Boot and Mount

系統在開機時會先讀取韌體中的程式碼，而這個程式碼會將系統導向讀取MBR
+ MBR中紀錄了系統的bott partiton在哪裡，首先會先將kernel載入，再來就會陸陸續續載入整個系統
+ Sector sparing
	+ 用來處理bad block，將壞軌的區域map到備用區域

## UEFI

+ 一種技術規範
	+ 定義一個讓OS及韌體做溝通的軟體介面

+ 包含
	+ Datatable ，包含platform的資訊
	+ Boot and runtime services for OS loader and OS

+ 優點
	+ 對64bit及硬碟空間大的系統支援性更佳
	+ 是一個單一且完整的boot manager
		+ 且比mutistage的BIOS更快

## Swap-Space Management

在DRAM不夠時用來做pageing或swapping(移動整個process)

+ OS提供Swap space management
	+ 因為存取速度比DRAM慢，所以優化效能是很重要的
	+ 通常會有多個swap space，來降低IO的負擔
	+ Best to have dedicated devices

## Storage Attachment

+ computers access storage in three ways
	+ host-attached
		+ High-end system 使用 fibre channel(FC)
	+ network-attached(IP)
		+ network-attached storage (NAS)
			+ 可以遠端存取資料系統
			+ NFS and CIFS are common protocols
			+ 在host跟storage實作RPC(Remote procedure calls)，在IP的基礎上使用TCP或UDP
			+ `iSCSI`
			+ `FCoE`
	+ cloud(HTTP)
		+ cloud是一個基於API的program，利用API來提供存取，而NAS只是另一個遠端的FS

## Storage Area Network

+ `LUN Masking`

## RAID Structure

+ 將多個硬碟組成一個磁碟陣列
+ 利用NVRAM來提高寫入效能
+ 延長了平均故障壽命
	+ 平均維修壽命
	+ 平均資料遺失
+ 其他功能
	+ 系統快照
	+ 複製
+ JBOD (Just a Bunch Of Disks)
	+ 多個硬碟組成一個獨立裝置

+ RAID Level
	+ 分為六種level

+ Extension
+ ppt31


## Object Storage


I/O ports , I/O busses, sector, High-end system

