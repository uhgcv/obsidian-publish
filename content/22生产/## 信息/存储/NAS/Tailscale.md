#进度 
网络层认证
服务层认证
流量、VPN
传输和同步
DNS
阅读文档、完善其他功能

#Tailscale_IP
```
##绿联NAS（dxp4800plus-989）
(旧）100.90.185.41
100.89.202.91

HTTP/HTTPS=https://100.90.185.41
SMB=\\100.90.185.41\
WebDAV=http://100.90.185.41

网页9999
HTTP5005
HTTPS5006

##联想电脑（laptop-edmoe6ka）
100.68.46.36

##MATE50pro(huawei-cet-al00)
100.88.196.104
```
*地址是用户内部的，因此用户地址非常充裕*

#连接条件 
-访问设备上需要安装Tailscale并加入组网，才能分配用户内部地址，每次访问两端还得同时保持Tailscale打开

-无需安装Tailscale访问：
--子网路由（Subnet Router）：
若NAS或路由器开启了子网路由（比如共享家庭局域网 192.168.1.0/24），即使某台电脑未安装Tailscale，只要它在家里的局域网中，其他组网设备可以通过Tailscale访问它的IP（如 192.168.1.100）。
--出口节点（Exit Node）：
若海外VPS开启了出口节点功能，手机连接Tailscale后可以选择“通过VPS上网”，此时手机无需安装Clash等其他VPN。

#连接
-信息中枢需要保持Tailscale常开，NAS很适合作为被访问端，而终端无需也不能总保持Tailscale常开、需要访问时再打开即可


-手机：默认情况下不提供SMB/WebDAV服务，因此其他设备无法直接访问手机上的文件。
例外：如果你在手机上安装了支持SMB或WebDAV的App（比如Solid Explorer），并主动开启共享功能，则可以被其他设备访问。

-即使设备在Tailscale组网内，未授权的服务访问依然会被拦截。
例如：
你手机登录了Tailscale，但没有在NAS上设置匿名访问权限，手机依然需要输入NAS的用户名和密码才能访问文件。
如果NAS的某个共享文件夹设置为“公开访问”，手机可以无需密码访问（但这是NAS本身的设置，与Tailscale无关）。

#用户认证 
-双层嵌套：网络层认证（Tailscale负责）、服务层认证（设备操作系统负责）
##网络层认证
-认证顺序：网络、用户、设备。网络和用户各为一层嵌套
--同一实体设备不同认证层网络可实现，方法：节点共享，多账号切换，多容器并行。
-默认认证规则：
--同一用户下的设备可以互相访问。
--不同用户下的设备不能互相访问
-ACL（自定义认证规则）

##服务层认证
--关闭匿名访问
--为你所有的网络服务（SMB, WebDAV, SSH等）都设置强密码


#连接方式 
-  支持 HTTP/HTTPS（网页管理）
-  支持 SMB 文件共享
-  支持 WebDAV
-  支持 SSH/FTP
-  支持任何基于 IP 的协议（如 Plex、Jellyfin 等）
*经验之谈：网页端访问适合用HTTP/HTTPS，直接访问或传输文件适合用SMB，挂载固定位置适合用WebDAV

#传输和同步 
-既然能访问，就能传输和同步
-传输：所有终端之间，比如NAS与任何，手机和电脑，手机和手机，也可以通过Tailscale传输。代替其他软件传输文件的功能。
--内置Taildrop文件共享功能，相比Tailscale可跨平台、跨网络。
-同步：配合同步软件，凭借特定传输协议


#手机VPN 
-与clash冲突
-解决方法：
绥靖：暂时不同时使用，手机端作为client，tailscale不必常开
根除：通用方法，其一是配置clash代理规则，其二是经过其他同时兼容并部署tailscale和clash的终端（例如NAS）转发流量。固定地点更适合使用路由部署clash

#容器配置
-总是重启
-网络模式host
-启用高级权限
-映射文件夹
-（操作面板获取密钥）
~~~
#方法1
添加变量TS_AUTHKEY，变量值即为密钥

#方法1
-启动命令：
/usr/local/bin/containerboot
tailscale up --authkey=tskey-auth-kkuPHMgjJ121CNTRL-7dR9FxP78AhNoZ969t8h9hh3uD7qoYce

-auth key
tskey-auth-kkuPHMgjJ121CNTRL-7dR9FxP78AhNoZ969t8h9hh3uD7qoYce
~~~

-注意：密钥现在只生效90天


---
### 组网方式
- 根本区别：权限需求不同。
- 方式：Node Sharing（最简单）；ACL规则；多开tailsclae容器0