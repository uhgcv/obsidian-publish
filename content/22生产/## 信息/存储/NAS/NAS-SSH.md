#Linux系统 
-Debian




#路径 
个人文件夹ls -l /volume2/@home
-例如：drwx------ 1 FMJKM000UNAS admin 144 Jun 16 11:10 FMJKM000UNAS

#用户、组、权限位 
-对应数据系统中的虚拟部分
-权限判定规则（简化版）：
首先判断当前用户是否是 文件拥有者 (User)，如果是，使用User权限。
如果不是User，判断当前用户是否在 Group 中，如果是，使用Group权限。
如果都不是，使用 Others 的权限
-ACL默认是drwxr-xr-x
ACL修改后是d--------- 
775是drwxrwxr-x
-绿联NAS的组包括：用户：admin、users；系统：root、postgres等


#root 
最高管理员，默认管理员组可通过sudo借用，但不建议直接使用root，太危险

#ACL 
-GUI的权限层通常是基于 ACL (Access Control List, 访问控制列表)凌驾于基础Linux权限之上的权限层
-会自动修改权限位，但不会修改用户和组
-执行chown -Rv root:root /volume1，沉默了ACL，可能因为更改了路径下的FlieManager

##用户组
-用户组权限优先级：用户组冲突时，禁止>读写>只读。设置不太合理，因此用户组需要精简，减少冲突。采用全包括且不重合的方案。
-JK：我的账号。实体仅归属我一人，全部读写
-M：其他管理员。目前为空，临时更改管理员，同时调整入此分组，并调整我的个人文件如个人文件夹。禁止访问我的共享和系统s\d\docker文件，其他都读写。
-U0：爸妈的账号。
*R只读、W读写仅特别对于普通用户

##应用权限
-特殊用户和权限，需要特殊设置，在/volume1/@appstore
-对于绿联NAS，很多核心应用的目录所有者和组就是 root:root，并且权限是 755 (rwxr-xr-x)，正是默认权限

#权限设置 
-目的：
--ACL权限高于Linux
--Linux：SSH的sudo可读写执行，GUI的admin组可读写执行
--精确分配权限的可以在GUI上更改，其优先级更高。我认为SSH上保持一致性、开放性更好，便于操作
-默认权限：目录775 rwxrwxr-x FMJKM000UNAS:admin
文件 664 rw-rw-r--
-特殊权限：/volume2/@home
/volume1/@appstore
/volume1/@syncbackup/cache
*应避开，若不慎更改，home特殊脚本运行，appstore部分可脚本，部分需重装，syncbackup/cache改drwxr-xr-x root root。其中docker需重装，连带容器也没了。应用重装前应备份，或文字记录其配置。*

-目前没发现其他问题

#[[卷1、2权限记录]]


