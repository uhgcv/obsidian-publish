

#容器组网
-代理网络名：clash
容器名：dreamacro_clash-1
docker network create clash
docker network connect clash <compose>

-连接容器：clash、subconverter、sub-web


#镜像仓库配置
*无dockercompose，自定义程度低，不推荐了*
-clash：映射了路径

#Docker-compose配置
