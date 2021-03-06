# [docker运行oracle11g](https://www.cnblogs.com/davygeek/p/7510578.html)





## image

```
docker pull registry.cn-hangzhou.aliyuncs.com/qida/oracle-xe-11g
```

或者自己自动添加表

```
create role test_role;
grant create any table, alter any table, drop any table,
  insert any table, update any table, delete any table,
  create any index, alter any index, drop any index,
  create any sequence, alter any sequence, drop any sequence, select any sequence,
  create any view, drop any view
  to test_role;

create tablespace testdb datafile '/u01/app/oracle/oradata/XE/testdb.dbf' size 300m autoextend on next 1m maxsize unlimited extent management local;
create user test identified by test default tablespace testdb temporary tablespace temp;
grant connect, resource to test;
alter user test quota unlimited on testdb;
grant test_role to test;
```

构建镜像

```
FROM registry.cn-hangzhou.aliyuncs.com/qida/oracle-xe-11g
ADD init.sql /docker-entrypoint-initdb.d/
```

## 启动

```
docker run -d -p 49160:22 -p 49161:1521 -e ORACLE_ALLOW_REMOTE=true registry.cn-hangzhou.aliyuncs.com/qida/oracle-xe-11g
```

## 连接

```
hostname: 192.168.99.100
port: 49161
sid: xe
username: system
password: oracle
Password for SYS & SYSTEM
```

## jdbc

- maven

```
        <dependency>
            <groupId>com.oracle</groupId>
            <artifactId>ojdbc6</artifactId>
            <version>11.2.0.3</version>
        </dependency>
```

- repository

```
<repositories>
        <!-- for ORACLE ojdbc6. -->
        <repository>
            <id>codelds</id>
            <url>https://code.lds.org/nexus/content/groups/main-repo</url>
        </repository>
        <repository>
            <id>spring-snapshots</id>
            <name>Spring Snapshots</name>
            <url>https://repo.spring.io/snapshot</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </repository>
        <repository>
            <id>spring-milestones</id>
            <name>Spring Milestones</name>
            <url>https://repo.spring.io/milestone</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
    </repositories>
```

- 配置

```
spring.datasource.driver-class-name=oracle.jdbc.driver.OracleDriver
spring.datasource.url=jdbc:oracle:thin:@192.168.99.100:49161:xe
spring.datasource.username=test
spring.datasource.password=test
spring.jpa.hibernate.ddl-auto=update
spring.jpa.database-platform=org.hibernate.dialect.Oracle10gDialect
```

## doc

- [wnameless/oracle-xe-11g](https://hub.docker.com/r/wnameless/oracle-xe-11g/)
- [qida/oracle-xe-11g](https://dev.aliyun.com/detail.html?spm=5176.1972343.2.16.mwrZbe&repoId=5682)
- [在 Docker 上配置 Oracle](http://blog.csdn.net/zhousenshan/article/details/53072537)
- [docker-oracle-xe-11g-demo](https://github.com/muumin/docker-oracle-xe-11g-demo)



\##安装

docker shell 下：

```
docker pull wnameless/oracle-xe-11g
```

运行，并开放 49160 和 49161 端口，分别对应 22 端口和 oracle 端口（SSH 和 oracle 数据库）

```
docker run -d -p 49160:22 -p 49161:1521 wnameless/oracle-xe-11g
```

数据库信息如下：

```
hostname: localhost
port: 49161
sid: xe
username: system
password: oracle
```

SYSTEM和SYS的初始密码都为 `oracle`

Container SSH 的 root 密码为`admin`。

需要说明的是，在 OS X 上通过 SSH 连接 docker container 使用的是 docker 的 ip 而不是 localhost。

安装完毕之后，就可以通过客户端配置上述数据库信息来连接数据库了。图为 OS X 下 SQL Developer 连接 Docker 中数据库的配置界面。