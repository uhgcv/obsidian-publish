

#工作流 
-找到官方github或dockerhub
-阅读简介
-找到适合的版本代码，下载
-解压，阅读、理解参数
-AI指导，将本地文档作为附件、或网页文档fetch
-通用是配置路径，指定路径，导入代码，修改.env，
-修改docker-compose.yaml，注意端口映射
-docker compose up -d


#配置 
-存储空间映射、权限
--config
--其他配置
-自动重启
-网络模式
--端口映射
-环境变量
--路由内部clash地址
HTTP_PROXY http://172.17.0.2:7890
HTTPS_PROXY http://172.17.0.2:7890
NO_PROXY localhost,127.0.0.1,192.168.0.0/16,*.lan


#命令行 
-NAS以SSH协议连接后，需要root权限以执行docker命令，输入 sudo -i ，再次密码，命令提示符会从 $ 变为 # 即可

#Docker-compose 
-用于预设容器配置
*绿联云安装的版本是Docker Compose V2，注意指令集差别
