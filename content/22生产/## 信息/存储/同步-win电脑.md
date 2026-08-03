#方案
（系统映像+文件历史记录）+（用户数据文件备份+应用配置备份）

#系统映像
-每月一次，1日4时

# 同步路径
*绿联云只能手动同步
##任务C：
C:\ProgramData
C:\Users\86188
(主要包括：C:\Users\86188\Desktop，Documents，Downloads，AppData\Roaming，.开头的开发文件)
不包括C:\Users\86188\AppData\Local和LocalLow

##任务Local：
C:\Users\86188\AppData\Local\Microsoft\Edge\User Data\Default C:\Users\86188\AppData\Local\Google\Chrome\User Data\Default

##任务D：
D:\01 mod
D:\00 data\下载
D:\00 data\00 backup
D:\00 data\00 Download