
- BIOS设置
- win安装：我没有internet连接
- 磁盘分区计划
- linux安装
- 主板驱动安装
- 显卡驱动安装

- 结论：每个系统独占一个硬盘，并定期自动备份。避免出错收到波及，安装更省事�?- 自动备份：包括驱动、软件、软件设置等

### 测试流程

- 显卡验真：GPU-Z
- 显卡压力测试：FurMark
- CPU验真：CPU-Z
- 传感器监控：HWiNFO64
- 内存健康：MemTest86
- 硬盘健康：CrystalDiskInfo
- 硬盘分区：DiskGenius
- 整机、电源测试：OCCT


- 显卡健康�?DMark.收费，可不用�?- CPU跑分：Cinebench R23。软件太大，可不用�?- 整机压力测试：AIDA64。收费，可不用�?

---

### 优化经验
- LINUX系统版本可能不兼容主板固件，导致无法安装。很可能是系统版本太新没适配老硬件，尝试用同时期发布的老系统，多半能解�?- 硬盘分区必须先规划好需求，再开始装系统，串行内存修改分区非常麻烦。修改系统盘需要装额外系统操作
- 系统崩溃重启，根本原因是系统损坏，折腾装系统的时候搞错了。是主板没有自动适配内存频率、超频设置�?- 系统损坏修复：没法自身修复。只能从别的系统启动修复
    - 用win11安装U盘。U盘启动，选命令行。dir找到windows、program files所在的盘符�?    - dism /image:C:\ /cleanup-image /restorehealth /Source:WIM:D:\sources\install.wim:4 /LimitAccess
    - 离线修复，指定源为U盘中的指定版本系统，需要加WIM防止系统看不�?    - 失败，系统补丁后版本号不同，拒绝dism修复，只能重装系�?妈的，气笑了
- 重装系统：没法不勾选删除所有数据。只能备�?太扯了。Windows 这种“系统坏了只能全盘格式化”的毛病，在 2026 年依然存在，简直离谱。即使备份了再安装，岂不是每次修复系统都要备份重装再恢复，然后再牵连重装另一个linux系统
- 旁路安装：找到C盘，ren修改原Windows文件夹名称为Windows_old。mkdir创建新Windows文件夹名称为Windows_new�?    - diskpart 的attributes volume权限确认readonly为否
    - 绕开访问权限，安装到这里来dism /apply-image /imagefile:D:\sources\install.wim /index:4 /applydir:C:\Windows_new
    - 引导文件 bcdboot C:\Windows_new\Windows /l zh-cn
    - 报错找不到启动文�?        - 找到FAT32的系统盘，清除原来的启动文件
        `cd EFI
        `rmdir /s Microsoft
        - 写入新的启动文件
        `bcdboot C:\Windows_New\Windows /l zh-cn /s S: /f UEFI
    - 启动后黑屏，因为路径变更，显卡驱动没有初始化
黑屏，多次尝试，不行，四次重启或强制注入连安全模式都进不去。气笑了
    - 改正Windows路径，搞不清楚，重新apply-image到C:\Windows
    - 也不�?bcdboot复制失败
算了，彻底重装一遍罢�?- 安装中断重启，可能是U盘读取太多次坏道了。换个U盘好�?- 被迫全盘
- 全盘格式化后：不能直接创建分区，只能默认全盘。那么单盘双系统，仍然需要再用虚拟系统分割系统盘。很麻烦。或者干脆每个系统独占一个硬件得了。懒得重新划分了

以后重装win系统，备份什么数据可以最大程度减少重复工作。唉，还好是还在调试阶段没放重要数据。但各种驱动、软件、软件设置等也要花半天时间重新部署都是重复劳�?

- 用本地agent定位问题很高�?- 系统崩溃问题大概率指向内存。硬件条件不�?
---
- 硬盘分区是大问题。是串行的，没法单独扩容夹在中间的分区。最好先计划好，装完系统再调整就麻烦了。还是以win为主就全部C盘化。共享数据放NAS。省的管理这么多盘�?    - 全做系统盘是对的，主系统win�?00(一次性给大点方便缩小)，副系统�?00最末尾可调