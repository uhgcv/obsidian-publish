
#docker容器数据库连接设置
- DB_TYPE=postgresdb 
- DB_POSTGRESDB_HOST=postgres 
- DB_POSTGRESDB_PORT=5432 
- DB_POSTGRESDB_DATABASE=n8n 
- DB_POSTGRESDB_USER=n8n 
- DB_POSTGRESDB_PASSWORD=YourStrongPassword_ChangeMe
*HOST：地址*

-这是容器连接到其外部 PostgreSQL 数据库时使用的认证密码。
-同一数据库连接不同容器的认证可以且宜不同。
-postgres可以下属任意数量数据库，POSTGRESDB_DATABASE=n8n是其中一个，密码--也仅对n8n及USER负责
-DATABASE和USER在PostgreSQL是相互独立的存在，不依赖另一方为条件。但从认证顺序上，用户名和密码先验证，才能访问数据库。所以虚拟认证以虚拟用户名为先

#用户名和密码
-拟用户名：