# confluence

新的Confluence/Jira版本仅支持数据中心许可证

并支持绿联nas的docker部署

---

[README](README_en.md) | [中文文档](README.md)

默认端口: 8090

+ 最新版本(amd64): v9(9.2.1)
+ 绿联dxp2800测试通过

## 环境要求
- docker-compose: 17.09.0+

## 使用 docker-compose 启动

- docker-compose.yml 文件内容

```yaml
version: '3.4'
services:
  confluence:
    # 只需要修改这行
    image: ghcr.io/misaka10843/confluence-ugnas:9.2.1-fixed
    container_name: confluence-srv
    environment:
      - TZ=Asia/Shanghai
    #      - JVM_MINIMUM_MEMORY=1g
    #      - JVM_MAXIMUM_MEMORY=12g
    #      - JVM_CODE_CACHE_ARGS='-XX:InitialCodeCacheSize=1g -XX:ReservedCodeCacheSize=8g'
    depends_on:
      - mysql
    ports:
      - "8090:8090"
    volumes:
      - home_data:/var/confluence
    restart: always
    networks:
      - network-bridge

  mysql:
    image: mysql:8.0
    container_name: mysql-confluence
    environment:
      - TZ=Asia/Shanghai
      - MYSQL_DATABASE=confluence
      - MYSQL_ROOT_PASSWORD=123456
      - MYSQL_USER=confluence
      - MYSQL_PASSWORD=123123
    command: ['mysqld', '--character-set-server=utf8mb4', '--collation-server=utf8mb4_bin', '--transaction-isolation=READ-COMMITTED', '--innodb_log_file_size=256M', '--max_allowed_packet=256M','--log_bin_trust_function_creators=1']
    volumes:
      - mysql_data:/var/lib/mysql
    restart: always
    networks:
      - network-bridge

networks:
  network-bridge:
    driver: bridge

volumes:
  home_data:
    external: false
  mysql_data:
    external: false
```

其余操作与上游仓库相同

