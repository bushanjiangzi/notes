## 添加环境变量

1. path 路径添加环境变量

`D:\mysql-8.0.21-winx64\mysql-8.0.21-winx64\bin`

2. 新建 data 文件夹和 my.ini 文件

编辑 my.ini 文件

```
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[mysqld]
#设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=D:\mysql-8.0.21-winx64\mysql-8.0.21-winx64\
# 设置mysql数据库的数据的存放目录
datadir=D:\mysql-8.0.21-winx64\mysql-8.0.21-winx64\data\
# 允许最大连接数
max_connections=20
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```

3. 以管理员打开 cmd

```
mysqld --install
mysqld --initialize-insecure
net start mysql
mysql -u root -p
```

### 安装失败重新安装

1. 查看 mysql

- `sc query mysql`

2. 删除 mysql

- `sc delete mysql`
- `mysqld --remove mysql`

3. 安装

- `mysqld --install`

4. 启动服务

- `net start mysql`

5. 登录

- `mysql -u root -p`

### 其他命令

- `use mysql`
- `select user,host from user;`

- 刷新：
  `flush privileges;`

- 修改 host：
- `ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'password';`
