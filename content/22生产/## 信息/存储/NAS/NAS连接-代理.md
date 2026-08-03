
#代理 
-方案：NAS上配置clash，VPN升级至多设备


#clash 
-可行性：连接可行，配置容器（可行但很麻烦）、配置调用代理的程序的环境（行但需个别配置）
-难点：解析订阅，dns配置，规则配置
-工作流：Nexi5设备+的clash订阅链接>转URL，subconverter编辑链接（规则ACL4SSR），浏览器访问下载，转config.yml>修改参数，dns和UI，保存并备份>导入路径>创建容器，调整配置（存储空间映射、重启、接口），启动>UI测试，SSH curl测试，容器日志观察，浏览器调用测试>完成
-改进：自动拉取解析订阅，自动更新规则，本地部署URL转换
*注意编辑中的各种小错误，语法、格式上的*

-容器接口：7890、7891、9090。对应HTTP、HTTPS、UI
-NAS接口：宜与容器接口保持一致

-外部地址：http://192.168.0.200:7890
-内部地址：http://172.17.0.2:7890

#subconverter
-目的：转换代理链接
-部署：NAS的docker容器。docker compose通过监听端口，可以运行了。镜像市场的也可以，但不首选。

-URL在线转换[URL Encode and Decode - Online](https://www.urlencoder.org/)
~~~
-模板
http://192.168.1.200:25500/sub?target=clash&url=
{订阅链接转URL}
&config=https%3A%2F%2Fraw.githubusercontent.com%2FACL4SSR%2FACL4SSR%2Fmaster%2FClash%2Fconfig%2FACL4SSR_Online.ini

http://192.168.1.200:58080/sub?target=clash&url=https%3A%2F%2Fcdn.metaglide.org%2F%3FL2Rvd25sb2FkQ29uZmlnL0NsYXNoLmFzcHg%2FZXE9d2luZG93cyZ1cms9OTcxNGUwZjctM2VkZS00OTZjLTg3MjMtZTA4MzlhOTk0MmY2Jm1tPTM1NTkyNSZrdG1tPSUyZmttdkZLbFZXRyUyYnNySjNha3Bta2x3JTNkJTNkJjM3MjQ%3D&insert=false&config=https%3A%2F%2Fraw.githubusercontent.com%2FACL4SSR%2FACL4SSR%2Fmaster%2FClash%2Fconfig%2FACL4SSR_Online.ini&emoji=true&list=false&tfo=false&scv=true&fdn=false&expand=true&sort=false&new_name=true

-链接示例,sub-web转发
http://192.168.1.200:58080/sub?target=clash&url=https%3A%2F%2Fcdn.metaglide.org%2F%3FL2Rvd25sb2FkQ29uZmlnL0NsYXNoLmFzcHg%2FZXE9d2luZG93cyZ1cms9OTcxNGUwZjctM2VkZS00OTZjLTg3MjMtZTA4MzlhOTk0MmY2Jm1tPTM1NTkyNSZrdG1tPSUyZmttdkZLbFZXRyUyYnNySjNha3Bta2x3JTNkJTNkJjM3MjQ%3D&insert=false&emoji=true&list=false&tfo=false&scv=true&fdn=false&expand=true&sort=false&new_name=true

http://192.168.1.200:46347/sub?target=clash&url=https%3A%2F%2Fcdn.metaglide.org%2F%3FL2Rvd25sb2FkQ29uZmlnL0NsYXNoLmFzcHg%2FZXE9d2luZG93cyZ1cms9OTcxNGUwZjctM2VkZS00OTZjLTg3MjMtZTA4MzlhOTk0MmY2Jm1tPTM1NTkyNSZrdG1tPSUyZmttdkZLbFZXRzlpZnFaVmJjQUkzZyUzZCUzZCYwMTliN2E1NDE3NDM0%2FACL4SSR%2FACL4SSR%2Fmaster%2FClash%2Fconfig%2FACL4SSR_Online.ini


-链接示例,直连
http://192.168.1.200:25500/sub?target=clash&url=https%3A%2F%2Fsub.nexconvert.com%2F%3FL2Rvd25sb2FkQ29uZmlnL0NsYXNoLmFzcHg%2FZXE9d2luZG93cyZ1cms9OTcxNGUwZjctM2VkZS00OTZjLTg3MjMtZTA4MzlhOTk0MmY2Jm1tPTM1NTkyNSZmOGE3YWU4NDkxNTU0M2I5YjM%3D&config=https%3A%2F%2Fraw.githubusercontent.com%2FACL4SSR%2FACL4SSR%2Fmaster%2FClash%2Fconfig%2FACL4SSR_Online.ini


https://a.mjsub.org/api/v1/client/subscribe?token=8bdf770b1acc25666835f33c302bcca6

http://192.168.1.200:25500/sub?target=clash&url=https%3A%2F%2Fa.mjsub.org%2Fapi%2Fv1%2Fclient%2Fsubscribe%3Ftoken%3D8bdf770b1acc25666835f33c302bcca6&config=https%3A%2F%2Fraw.githubusercontent.com%2FACL4SSR%2FACL4SSR%2Fmaster%2FClash%2Fconfig%2FACL4SSR_Online.ini

http://192.168.1.200:25500/sub?target=clash&url=https%3A%2F%2Fa.mjsub.org%2Fapi%2Fv1%2Fclient%2Fsubscribe%3Ftoken%3D8bdf770b1acc25666835f33c302bcca6&config=https%3A%2F%2Fraw.githubusercontent.com%2FACL4SSR%2FACL4SSR%2Fmaster%2FClash%2Fconfig%2FACL4SSR_Online.ini

http://192.168.1.200:25500/sub?target=clash&url=https%3A%2F%2Fcdn.metaglide.org%2F%3FL2Rvd25sb2FkQ29uZmlnL0NsYXNoLmFzcHg%2FZXE9d2luZG93cyZ1cms9OTcxNGUwZjctM2VkZS00OTZjLTg3MjMtZTA4MzlhOTk0MmY2Jm1tPTM1NTkyNSZrdG1tPW4yaGMlMmJjRDN3ZVZvWHluZnVaU2c5ZyUzZCUzZCY2MDRlMmFiZWUzMjc0NTQ2YmM0NWIwZDk%3D&insert=false&config=https%3A%2F%2Foss.wcc.best%2F2025%2F07%2F16%2Ffa4ccc88-2611-4f3c-8367-593210c4c1e4.txt&emoji=true&list=false&tfo=false&scv=true&fdn=false&expand=true&sort=false&new_name=true



http://192.168.1.200:25500/sub?target=clash&url=https%3A%2F%2Fcdn.metaglide.org%2F%3FL2Rvd25sb2FkQ29uZmlnL0NsYXNoLmFzcHg%2FZXE9d2luZG93cyZ1cms9OTcxNGUwZjctM2VkZS00OTZjLTg3MjMtZTA4MzlhOTk0MmY2Jm1tPTM1NTkyNSZrdG1tPWx5ayUyYnBYJTJiOWl3UG9qVFFRZVglMmJ1TVElM2QlM2QmODM5MWRlMWM2NDY0NGM0Mg%3D%3D&insert=false&filename=config.yaml&emoji=true&list=false&tfo=false&scv=true&fdn=false&expand=true&sort=false&new_name=true&config=https%3A%2F%2Fraw.githubusercontent.com%2FACL4SSR%2FACL4SSR%2Fmaster%2FClash%2Fconfig%2FACL4SSR_Online.ini

port: 7890
socks-port: 7891
allow-lan: true
mode: Rule
log-level: info
external-controller: 0.0.0.0:9090
external-ui: public
secret: "" 


dns:
  enable: true
  # 我们回到最稳定、兼容性最强的 fake-ip 模式
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  # 关键：我们只使用【一个】最可靠的国内公共DNS作为基础
  nameserver:
    - 223.5.5.5
  # 关键：fallback DNS也只保留最不可能被封的IP形式DNS
  fallback:
    - 1.0.0.1
    - 8.8.4.4
  # fallback-filter 保持原样
  fallback-filter:
    geoip: true
    ipcidr:
      - 240.0.0.0/4

~~~

是docker compose部署subconverter容器自身的问题
我用绿联GUI上，再docker镜像仓库部署的subconverter，端口46347，可以输出
我用docker compose部署的容器，端口25500，访问不了

镜像仓库部署环境变量更完善，与这个有关系吗
PATH=usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
UID=1029
USER_ID=1029
PUID=1029
GROUP_ID=10
PGID=10
GID=10



用的这个链接，端口分别对应
http://192.168.1.200:25500/sub?target=clash&url=https%3A%2F%2Fa.mjsub.org%2Fapi%2Fv1%2Fclient%2Fsubscribe%3Ftoken%3D8bdf770b1acc25666835f33c302bcca6&config=https%3A%2F%2Fraw.githubusercontent.com%2FACL4SSR%2FACL4SSR%2Fmaster%2FClash%2Fconfig%2FACL4SSR_Online.ini

#sub-web转发
-目的：subweb作为前端，subconverter作为后端。提供GUI，能自动转URL，自动配置参数
-远程配置=规则模板


---


#ssh代理中转 
-docker代理中转的前置，以用电脑的clash为例
-ssh中输入：
export HTTP_PROXY="http://192.168.1.210:7890"
export HTTPS_PROXY="http://192.168.1.210:7890"

#docker代理中转
-含义：从电脑地址（210）,中转流量
-可用。图形界面上不显示修改了代理
-为Docker守护进程配置HTTP代理。
创建或编辑Docker的代理配置文件：
<BASH>
# 创建目录（如果不存在）
sudo mkdir -p /etc/systemd/system/docker.service.d
# 编辑配置文件
sudo vi /etc/systemd/system/docker.service.d/http-proxy.conf
在 vi 编辑器中，粘贴以下内容，并把IP和端口换成你电脑的：
<INI>
[Service]
Environment="HTTP_PROXY=http://192.168.1.210:7890"
Environment="HTTPS_PROXY=http://192.168.1.210:7890"
Environment="NO_PROXY=localhost,127.0.0.1,192.168.0.0/16" 
(NO_PROXY 确保访问局域网设备时不走代理)
重启Docker服务使配置生效：

<BASH>
sudo systemctl daemon-reload
sudo systemctl restart docker
  