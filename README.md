# zabbix-docker

## Criar mysql
copie o conteudo abaixo, crie um arquivo no linux chamado mysql-zabbix.sh e cole o conteudo



```sh
docker run -d --name zabbix-mysql \
--restart always \
-p 3306:3306 \
-v /docker/zabbix/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=secret \
-e MYSQL_DATABASE=zabbix \
-e MYSQL_USER=zabbix \
-e MYSQL_PASSWORD=zabbix \
--network=zabbix \
mysql:8 \
--default-authentication-plugin=mysql_native_password \
--character-set-server=utf8 \
--collation-server=utf8_bin
```

em seguida execute o comando abaixo

```sh
chmod +x mysql-zabbix.sh
```


copie o conteudo abaixo, crie um arquivo no linux chamado server-zabbix.sh e cole o conteudo
```sh
docker run -d --name zabbix-server \
--restart always \
-p 10051:10051 \
-e DB_SERVER_HOST="zabbix-mysql" \
-e DB_SERVER_PORT="3306" \
-e MYSQL_ROOT_PASSWORD="secret" \
-e MYSQL_DATABASE="zabbix" \
-e MYSQL_USER="zabbix" \
-e MYSQL_PASSWORD="zabbix" \
-e ZBX_ENABLE_SNMP_TRAPS="true" \
--network=zabbix \
--volumes-from zabbix-snmptraps \
zabbix/zabbix-server-mysql

```
em seguida execute o comando abaixo

```sh
chmod +x server-zabbix.sh
```

copie o conteudo abaixo, crie um arquivo no linux chamado web-zabbix.sh e cole o conteudo

```sh
docker run -d --name zabbix-web \
--restart always \
-p 80:8080 \
-e ZBX_SERVER_HOST="zabbix-server" \
-e DB_SERVER_HOST="zabbix-mysql" \
-e DB_SERVER_PORT="3306" \
-e MYSQL_ROOT_PASSWORD="secret" \
-e MYSQL_DATABASE="zabbix" \
-e MYSQL_USER="zabbix" \
-e MYSQL_PASSWORD="zabbix" \
-e PHP_TZ="America/Sao_Paulo" \
--network=zabbix \
zabbix/zabbix-web-nginx-mysql

```
em seguida execute o comando abaixo

```sh
chmod +x web-zabbix.sh
```


Por fim execute os comandos abaixo um por vez

```sh
./mysql-zabbix.sh
./server-zabbix.sh
./web-zabbix.sh
```
