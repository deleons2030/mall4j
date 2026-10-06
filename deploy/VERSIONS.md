# mall4j v2.4 运行环境

基线：官方 v2.4，提交 `07f919b8ab51f4c350da42e703340f8d627243de`。

| 组件 | 配置版本 | 配置位置 |
| --- | --- | --- |
| Spring Boot | 2.7.0 | 根 pom.xml |
| Spring Framework | 5.3.20 | 显式导入 spring-framework-bom |
| Spring Security | 5.7.0 | 显式导入 spring-security-bom，优先于 Boot 默认的 5.7.1 |
| Java | 8 / 1.8 | pom.xml；未指定 8u 更新号和厂商，保留原 Docker 镜像 |
| MySQL 服务端 | 8.0.32 | db/Dockerfile |
| Redis 服务端 | 5.0.4（原仓库版本） | docker-compose.yml；用户尚未指定版本 |
| Nginx | 1.20.1 | docker-compose.yml |

Java 8 编译目标并不能固定实际运行时的 8u 更新号。原镜像
`anapsix/alpine-java:8_server-jre_unlimited` 没有在名称中标注精确更新号；
如以后指定 JDK 厂商及 8u 更新号，再替换或固定镜像摘要。

MySQL 8 使用独立的 `mall4j-mysql8-data` 数据卷。不要直接复用 MySQL 5.7 的数据目录；
已有数据需要单独导出、导入并验证。这次配置不迁移已有数据。

运行前先使用 Java 8 与 Maven 打包：`mvn clean package -DskipTests`，
然后运行 `docker compose up -d --build`。默认仍需按项目原说明配置支付、对象存储、
小程序、XXL-JOB 等外部服务。

Nginx 入口为 `http://127.0.0.1:8080`，`/apis/` 转发后台服务，
`/api/` 转发商城 API。当前配置仅代理接口；前端构建产物、域名、HTTPS 证书另行配置。

当前仅准备版本配置；未构建镜像、未启动服务、未验证数据库导入或业务端到端流程。
